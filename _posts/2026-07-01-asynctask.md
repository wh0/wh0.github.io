---
date: Wed Jul 01 2026 22:22:31 -0700 (Pacific Daylight Time)
qualitative_time: 
title: AsyncTask's ThreadPoolExecutor in different versions of Android
previous_teaser: An AR app that simulates being short
previous_first: true
has_highlighting: true
---
The AsyncTask class from the Android API internally sets up an Executor for running AsyncTask objects off the main thread.
AsyncTask is deprecated (as of a long time ago), and the advice from the Android API docs is to use an Executor or similar on your own.
The Executor implementation that Android uses in AsyncTask is the ThreadPoolExecutor class.
This post is about what ThreadPoolExecutor lets you configure and how Android had this ThreadPoolExecutor configured for use in AsyncTask.

First, let's go through what you can choose about how the ThreadPoolExecutor behaves.
Here's one of ThreadPoolExecutor's constructors with most of the customization:

```java
public ThreadPoolExecutor (int corePoolSize, 
                           int maximumPoolSize, 
                           long keepAliveTime, 
                           TimeUnit unit, 
                           BlockingQueue<Runnable> workQueue, 
                           RejectedExecutionHandler handler)
```

And there's this method:

```java
public void allowCoreThreadTimeOut(boolean value)
```

For `workQueue`, you can make an unbounded queue, or a bounded queue that can hold up to some number of tasks, including zero.

```java
public LinkedBlockingQueue() // for unbounded
public LinkedBlockingQueue(int capacity)
public SynchronousQueue() // for zero capacity
```

(There are other implementations of BlockingQueue that do more.)

Actually the order of the ThreadPoolExecutor constructor parameters is counterproductive to explaining the behavior of the class.
Let's instead visit them in this order:

![Core pool size, queue size, maximum pool size, rejected execution handler, keep alive time, and allow core thread timeout.](/assets/2026/asynctask-param-order.drawio.svg)

In the normal state, there's the core pool size number of threads idle; the queue is empty; and the rejected execution handler is not in use.

![Core pool idle;](/assets/2026/asynctask-pool-idle.svg)
![queue empty;](/assets/2026/asynctask-queue-empty.svg)
![excess pool empty;](/assets/2026/asynctask-pool-empty.svg)
![rejected execution handler not in use.](/assets/2026/asynctask-trash-closed.svg)

(I read on the internet that "pool" as in the collection of resources comes from a French term "poule," which had to do with either some kind of race for baby horses or a chicken.
It's an etymology unrelated to the "pool" with the water.)

If more tasks arrive than the pool can finish executing, all the threads in the core pool become busy.

![Core pool busy;](/assets/2026/asynctask-pool-full.svg)
![queue empty;](/assets/2026/asynctask-queue-empty.svg)
![excess pool empty;](/assets/2026/asynctask-pool-empty.svg)
![rejected execution handler not in use.](/assets/2026/asynctask-trash-closed.svg)

Then the queue becomes full.

![Core pool busy;](/assets/2026/asynctask-pool-full.svg)
![queue full;](/assets/2026/asynctask-queue-full.svg)
![excess pool empty;](/assets/2026/asynctask-pool-empty.svg)
![rejected execution handler not in use.](/assets/2026/asynctask-trash-closed.svg)

Then the pool creates more threads, until the pool has the maximum pool size number of threads.
These aren't kept separate; I've only drawn it this way to visualize that the pool has more than the core pool size number of threads.
At this point, any more tasks coming in go to the rejected execution handler.

![Core pool busy;](/assets/2026/asynctask-pool-full.svg)
![queue full;](/assets/2026/asynctask-queue-full.svg)
![excess pool busy;](/assets/2026/asynctask-pool-full.svg)
![rejected execution handler in use.](/assets/2026/asynctask-trash-open.svg)

If the pool then finishes executing the tasks, then the queue will become empty and the pool will become idle.
The pool doesn't clean up threads in excess of the core pool size right away.

![Core pool idle;](/assets/2026/asynctask-pool-idle.svg)
![queue empty;](/assets/2026/asynctask-queue-empty.svg)
![excess pool idle;](/assets/2026/asynctask-pool-idle.svg)
![rejected execution handler not in use.](/assets/2026/asynctask-trash-closed.svg)

![Keep alive time passes.](/assets/2026/asynctask-hourglass.svg)
When a thread is idle for the keep alive time, the pool removes it.
If the "allow core thread timeout" option is false (the default), the pool removes idle threads this way until it's back down to the core pool size number of threads.
Otherwise, the pool removes idle threads until it's down to zero threads.

![Core pool idle;](/assets/2026/asynctask-pool-idle.svg)
![queue empty;](/assets/2026/asynctask-queue-empty.svg)
![excess pool empty;](/assets/2026/asynctask-pool-empty.svg)
![rejected execution handler not in use.](/assets/2026/asynctask-trash-closed.svg)

Now let's look at how the AsyncTask class set up its ThreadPoolExecutor in various versions of Android.

<table>
  <thead>
    <tr>
      <th>Android version</th>
      <th>core pool size</th>
      <th>queue size</th>
      <th>maximum pool size</th>
      <th>backup pool size</th>
      <th>keep alive time</th>
      <th>allow core thread timeout</th>
      <th><code>execute</code> uses</th>
      <th>ThreadPoolExecutor name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Android 11</td>
      <td colspan="8">deprecated (although no removal and no changes to any of this data)</td>
    </tr>
    <tr>
      <td>Android 10</td>
      <td>1</td>
      <td>0</td>
      <td>20</td>
      <td>5</td>
      <td>3</td>
      <td>false</td>
      <td>serial</td>
      <td>THREAD_POOL_EXECUTOR</td>
    </tr>
    <tr>
      <td>Android 7</td>
      <td>max(2, min(cpu_count-1, 4))</td>
      <td>128</td>
      <td>cpu_count*2+1</td>
      <td></td>
      <td>30</td>
      <td>true</td>
      <td>serial</td>
      <td>THREAD_POOL_EXECUTOR</td>
    </tr>
    <tr>
      <td>Android 4.4</td>
      <td>cpu_count+1</td>
      <td>128</td>
      <td>cpu_count*2+1</td>
      <td></td>
      <td>1</td>
      <td>false</td>
      <td>serial</td>
      <td>THREAD_POOL_EXECUTOR</td>
    </tr>
    <tr>
      <td>Android 4.0.1</td>
      <td>5</td>
      <td>10</td>
      <td>128</td>
      <td></td>
      <td>1</td>
      <td>false</td>
      <td>serial</td>
      <td>THREAD_POOL_EXECUTOR</td>
    </tr>
    <tr>
      <td>Android 2.3</td>
      <td>5</td>
      <td>10</td>
      <td>128</td>
      <td></td>
      <td>1</td>
      <td>false</td>
      <td>thread pool</td>
      <td>sExecutor</td>
    </tr>
    <tr>
      <td>Android 1.6</td>
      <td>5</td>
      <td>10</td>
      <td>128</td>
      <td></td>
      <td>10</td>
      <td>false</td>
      <td>thread pool</td>
      <td>sExecutor</td>
    </tr>
    <tr>
      <td>Android 1.5</td>
      <td>1</td>
      <td>10</td>
      <td>10</td>
      <td></td>
      <td>10</td>
      <td>false</td>
      <td>thread pool</td>
      <td>sExecutor</td>
    </tr>
    <tr>
      <td>Earlier</td>
      <td colspan="8">didn't exist</td>
    </tr>
  </tbody>
</table>

In Android 10 and up, AsyncTask's thread pool has a custom rejected execution handler that submits the task to a separate "backup" ThreadPoolExecutor with an unbounded queue.
A ThreadPoolExecutor with an unbounded queue will never have the queue full, so it doesn't meaningfully have a separate maximum pool size to increase to after the queue would be full, so only a single size is shown in the table.

I've put a few extra columns in this table.

Since Android 4, AsyncTask offers two executors: its internal ThreadPoolExecutor and a "serial" wrapper that collects tasks in its own queue and submits them to the thread pool one at a time.
The **`execute` uses** column specifies what executor is the default one that the `execute` method submits a task to.

The AsyncTask class keeps its ThreadPoolExecutor object in a static field.
The **ThreadPoolExecutor name** column specifies the name of this field.
In early versions this field is private though, and some libraries use reflection to get at it.

Other note, Android 3.x "Honeycomb" was not open source.
The Git commit history that later came out with Android 4.0.1 I assume has some shared history with what Android 3.x used, but we don't know what releases were made from when in the commit history.
Actually the selection of default executor flipped back and forth before Android 4.0.1's release tag.
The API reference indicates that the switch to a serial executor by default was released as early as Android 3.0.

Bonus note, AsyncTask's serial executor doesn't run things on a single thread.
It waits for each task to finish before starting another, but whatever thread from the pool is available can run it, which usually alternates.
So don't go and assume anything thread local will be shared across tasks.

Bonus note 2, when a ThreadPoolExecutor adds threads past the core pool size when the queue is full, those new threads start off with the most recently submitted task rather than the oldest queued task.

Bonus note 3, when tasks go to the AsyncTask's backup thread pool's queue, those tasks are stuck there even if the main pool later becomes less busy.

Bonus note 4, the change to using a serial executor by default in Android 4's AsyncTask cited that earlier versions had it that way.
But we know from the configuration that it was only serialized into a single thread if you didn't fill up the capacity-10 queue and cause the pool to increase the number of threads.

Bonus note 5, in AsyncTask from Android 10 and up, the rejected execution handler that forwards tasks to the backup pool is named sRunOnSerialPolicy, but the backup pool allows some parallelism.

Bonus note 6, the Espresso instrumentation testing library tries to wait for the AsyncTask thread pool to be idle between interactions.
To avoid spin waiting, it submits a batch of tasks to occupy the core pool size number of threads between each idle check.
But this still spin waits if the pool has more than the core pool size number of threads.
It also doesn't check if the backup pool is idle.
