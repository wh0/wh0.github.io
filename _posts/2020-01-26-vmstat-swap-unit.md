---
date: Sun Jan 26 2020 12:22:53 -0800 (Pacific Standard Time)
qualitative_time: 
title: vmstat(8) swap unit
previous_teaser: The opposite of nonpareils
previous_first: false
---
When you run `vmstat`, you get a bunch of numbers without units:

```
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 ...
 1  2 889856 150008  44788 406152   47 5145  3693  5173 2696 4030 42  3 22 33  0
```

You can look up the units in the man page.
The `si` and `so` columns under `swap` are described as:

> **Swap**  
> si: Amount of memory swapped in from disk (/s).  
> so: Amount of memory swapped to disk (/s).  

Additional information of _what_ per second for most fields is above:

> **-S**, **--unit** _character_  
> Switches outputs between 1000 (_k_), 1024 (_K_), 1000000 (_m_), or 1048576 (_M_) bytes.
> Note this does not change the swap (si/so) or block (bi/bo) fields.

Thus, it ultimately goes unspecified what the "amount" per second of swap activity is measured in.

But after this version of the man page was published, about a year and a half ago, [it was discovered](https://gitlab.com/procps-ng/procps/merge_requests/69) that this description was wrong.
The `-S` option _does_ change the `si`/`so` fields, and the amount is measured in that unit (with the default being `K`, or 1024 bytes).

A corrected version reads:

> **Swap**  
> These are affected by the --unit option.  
> si: Amount of memory swapped in from disk (/s).  
> so: Amount of memory swapped to disk (/s).  

and

> **-S**, **--unit** _character_  
> Switches outputs between 1000 (_k_), 1024 (_K_), 1000000 (_m_), or 1048576 (_M_) bytes.
> Note this does not change the block (bi/bo) fields, which are always measured in blocks.

Here's a listing of where the fixed version is distributed:

| Source                                                                                                  | Status                      |
|:--------------------------------------------------------------------------------------------------------|:----------------------------|
| Debian unstable                                                                                         | Not updated                 |
| [linux.die.net](https://linux.die.net/man/8/vmstat)                                                     | (different procps provider) |
| [man7.org](http://man7.org/linux/man-pages/man8/vmstat.8.html)                                          | Updated                     |
| [manpages.debian.org](https://manpages.debian.org/buster/procps/vmstat.8.en.html) (Debian buster)       | Not updated                 |
| [manpages.debian.org](https://manpages.debian.org/unstable/procps/vmstat.8.en.html) (Debian unstable)   | Not updated                 |
| [manpages.ubuntu.com](https://manpages.ubuntu.com/manpages/bionic/en/man8/vmstat.8.html) (Ubuntu 18.04) | Not updated                 |
| [manpages.ubuntu.com](https://manpages.ubuntu.com/manpages/eoan/en/man8/vmstat.8.html) (Ubuntu 19.10)   | Not updated                 |
| [jlk.fjfi.cvut.cz](https://jlk.fjfi.cvut.cz/arch/manpages/man/core/procps-ng/vmstat.8.en) (Arch)        | Not updated                 |
| [manned.org](https://manned.org/vmstat.8)                                                               | Not updated                 |
| [skrenta.com](http://www.skrenta.com/rt/man/vmstat.8.html)                                              | (different procps provider) |
| [linux.org](https://www.linux.org/docs/man8/vmstat.html)                                                | Not updated                 |
| [man.he.net](http://man.he.net/?topic=vmstat&section=all)                                               | Not updated                 |
| [man.cx](https://man.cx/vmstat(8))                                                                      | Not updated                 |
