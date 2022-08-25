---
date: Wed Aug 24 2022 22:38:38 -0700 (Pacific Daylight Time)
qualitative_time: 
title: One time when it really was a Docker command quoting issue
previous_teaser: The average number of slices of pizza a fork touches in its lifetime
previous_first: false
has_highlighting: true
---
A few days ago I was staring at a Docker command like this:

```sh
set -e
image=debian # set externally
tag=latest # derived from some command output
cd /tmp
docker pull "$image:$tag"
docker run --rm \
    --entrypoint echo \ 
    -v $PWD/a:/mnt/a \
    $image:$tag \
    "hello world"
```

But it would give:

```
latest: Pulling from library/debian
Digest: sha256:d52921d97310d0bd48dab928548ef539d5c88c743165754c57cfad003031386c
Status: Image is up to date for debian:latest
docker.io/library/debian:latest
docker: invalid reference format.
See 'docker run --help'.
```

And there were plenty of unquoted variables, which by now is well known to mess up Docker commands.
But we were able to confirm that they didn't contain any spaces.
(I think we later added quotes around stuff in order to be safer.)
So I was thinking, this couldn't be a quoting issue.

One interesting thing is that if any of `image` or `tag` were empty, it would give the exact error we're seeing:

```
$ docker run :latest
docker: invalid reference format.
See 'docker run --help'.
$ docker run debian:
docker: invalid reference format.
See 'docker run --help'.
$ docker run :
docker: invalid reference format.
See 'docker run --help'.
```

But the `docker pull` above it was working fine, so it couldn't have been those variables being empty.

At that point, I tried typing in the whole command in my terminal and found that I couldn't reproduce the issue:

```
...
$ docker run --rm \
> --entrypoint echo \
> -v $PWD/a:/mnt/a \
> $image:$tag \
> "hello world"
hello world
```

Could it have been some sneaky non-ASCII characters hiding in the original?
I checked with an online tool, and there was not.
No specialized dashes instead of hyphens, no smart quotes instead of the straight ones.

---

It turned out that there was an extra space after the backslash in the `--entrypoint` line.
(Apologies if anyone was reading a printed copy of this; you stood no chance.)

The result was that the backslash-space would give `docker run` a space character as the first positional parameter, i.e. the image name.
Then there would be a plain old newline marking the end of the command.

We never saw any junk about `-v` not being found as a program because the thing we were using to run it bailed on the first failed command (I've tried to capture that with the `set -e` above).

And I was looking up the terminology for this, and supposedly, escaping a character such as a space with a backslash in shell script is described under the "Quoting" section, so it was technically a quoting issue after all.
