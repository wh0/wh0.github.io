---
date: Thu Dec 14 2017 20:53:47 -0800 (Pacific Standard Time)
qualitative_time: 
title: Subduing Windows machinations with Resource Monitor
previous_teaser: What to do if your toothbrush is too small
previous_first: false
---
![]({% include asset-path.txt name="task-manager-processes.png" %})

Ah, dangit.

![]({% include asset-path.txt name="task-manager-performance.png" %})

![]({% include asset-path.txt name="active-time.png" %})

![]({% include asset-path.txt name="suspend.png" %})

![]({% include asset-path.txt name="drop.png" %})

![]({% include asset-path.txt name="hunter.gif" %})

You can suspend processes in Resource Monitor.
Even for some processes that you're not allowed to terminate, you can suspend them.
That's pretty OP.

Suspended processes can be paged out to disk, so they won't take up any resources.

Once it's suspended, you can find it in the CPU tab.

![]({% include asset-path.txt name="resume.png" %})
