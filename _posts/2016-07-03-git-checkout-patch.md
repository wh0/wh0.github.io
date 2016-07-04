---
date: 2016-07-03 22:51:32
qualitative_time: 
title: Hunk editing in git checkout --patch
previous_teaser: Setting all your clocks at once
previous_first: true
---
When you run `git checkout --patch`, you get to discard changes in the working directory interactively.
The `e` command, which lets you edit the hunk before discarding it, is the trickiest one for me.
This week I've finally gone and learned how to work the darned thing.

<!--
digraph {
	label="git checkout --patch"
	labeljust=l
	rankdir=LR
	node [shape=rect]
	work [label="workdir"]
	index -> work [label="you start out looking at this"]
	desired [label="desired workdir"]
	desired -> work [label="edit it into this"]
	desired -> work [dir=back, label="git then applies this"]
	index -> desired [style=dotted, dir=none, label="(kept unstaged)"]
}
-->
![]({% include asset-path.txt name="checkout.svg" %})

You start with a diff that would transform the state of the index into the state of the working directory.
The third state, in your head, is what you want to have in the working directory.
Edit the diff into so that it would transform the desired state into the state of the working directory.
Then, git applies the reverse of the patch to the working directory, bringing it to the desired state.

It's different from what you do for `git add --patch`, so don't get confused 😉.

<!--
digraph {
	label="git add --patch"
	labeljust=l
	rankdir=LR
	node [shape=rect]
	work [label="workdir"]
	index -> work [label="you start out looking at this"]
	desired [label="desired index"]
	index -> desired [label="edit it into this"]
	index -> desired [label="git then applies this"]
	desired -> work [style=dotted, dir=none, label="(kept unstaged)"]
}
-->
![]({% include asset-path.txt name="add.svg" %})
