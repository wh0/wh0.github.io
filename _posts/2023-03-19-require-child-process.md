---
date: Sun Mar 19 2023 22:29:17 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Names for require('child_process')
previous_teaser: A salsa made of tempura instead of raw vegetables
previous_first: false
---
![
child_process 0.3321;
childProcess 0.2675;
cp 0.2619;
child 0.0515;
child_process_1 0.0174;
proc 0.0147;
ChildProcess 0.0121;
process 0.0083;
data 0.0065;
Womb 0.0038;
exports 0.0036;
ch 0.0032;
s 0.0032;
_child_process 0.0031;
exec 0.0027;
cproc 0.0017;
r 0.0016;
n 0.0016;
cProcess 0.0015;
childprocess 0.0008;
Child 0.0005;
subprocess 0.0003;
p 0.0003;
ncp 0.0002;
cprocess 0.0001
](/assets/2023/require-child-process-names.png)

From a search for `\w+\s*=\s*require\(['"]child_process['"]\)(;|$) count:2000` on [Sourcegraph](https://sourcegraph.com/search), weighted by repository stars.
For example, if a repository had 12 stars and had three matches in the search results, twice with `child_process = ...` and once with `cp = ...`, it would contribute 8 votes for `child_process` and 4 votes for `cp`.

I realized afterwards that people could be doing `require('node:child_process')` now, but I had to load the search results manually, and I don't want to do that again.

Here's some other data that I also collected from the same search results.

![
const 0.7311;
var 0.2313;
(nothing) 0.0321;
let 0.0055
](/assets/2023/require-child-process-decls.png)

_Presence of `var` or `const` or `let` on the line where the match occurs.
That little green slice at the top is `let`._

![
";" 1;
(nothing) 0
](/assets/2023/require-child-process-semis.png)

_Presence of `;` at the end of the match. I double checked, and the query can match no-semicolon lines, and such results exist, only they didn't include any._
