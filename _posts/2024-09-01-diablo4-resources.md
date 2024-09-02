---
date: Sun Sep 01 2024 17:09:17 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Resources in Diablo 4, a 40-hour impression
previous_teaser: Benchmarking the eight most powerful cake cutters
previous_first: true
---
<!--
digraph {
"helltide active?" [shape=diamond]
"wait 5 minutes" [shape=box]
"helltide active?" -> helltide [label="yes"]
"helltide active?" -> "wait 5 minutes" [label="no"]
"wait 5 minutes" -> helltide
helltide [shape=box]
"woa there's a treasure goblin" [shape=box]
"omg that's three of them" [shape=box]
"disappears after getting sidetracked" [shape=box]
helltide -> "abberent cinder" -> "disappears after getting sidetracked"
helltide -> "woa there's a treasure goblin" -> "omg that's three of them"
"wait 5 minutes" -> "disappears after getting sidetracked" [dir=back]
"omg that's three of them" -> "legendary item"
"omg that's three of them" -> "unique item"

equipment [shape=box]

"put in stash because what if I want it later" [shape=box]
"unique item" -> "put in stash because what if I want it later"
"unique item" -> equipment
"unique item" -> "at least four more dimensions of improving an item" [dir=both, color=red]

"salvage (l)" [shape=box]
"legendary item" -> "salvage (l)" -> "legendary aspect"
"salvage (l)" -> "about a half dozen different crafting materials"
"legendary item" -> equipment
"imprint aspect" [shape=box]
"legendary item" -> "imprint aspect" [dir=both, color=red]
"legendary aspect" -> "imprint aspect"
"about a half dozen different crafting materials" -> "imprint aspect"
"at least four more dimensions of improving an item" [shape=box]
"legendary item" -> "at least four more dimensions of improving an item" [dir=both, color=red]
"never looked into what they need" -> "at least four more dimensions of improving an item"

"I found some one time but generally don't have many" [shape=box]
"nothing needs them yet" [shape=box]
"I found some one time but generally don't have many" -> "about a half dozen other different crafting materials" -> "nothing needs them yet"
"about a half dozen other different crafting materials" -> "at least four more dimensions of improving an item" [label="probably", style=dotted]

glyph -> equipment
"don't remember how I got these" [shape=box]
"don't remember how I got these" -> glyph
"level up glyph" [shape=box]
"something I don't have yet" -> "level up glyph"
glyph -> "level up glyph" [dir=both, color=red]

event [shape=box]
event -> "murmuring obol"
"buy whispering key" [shape=box]
"open silent chest" [shape=box]
"murmuring obol" -> "buy whispering key" -> "whispering key" -> "open silent chest"
"open silent chest" -> "lower quality item"
"open silent chest" -> "legendary item"
"murmuring obol" -> gamble
gamble -> "lower quality item"

"salvage (j)" [shape=box]
"lower quality item" -> "salvage (j)" -> "about a half dozen different crafting materials"

foraging [shape=box]
foraging -> "bundled herbs"
foraging -> "angelbreath"
"upgrade potion" [shape=box]
"bundled herbs" -> "upgrade potion" -> "potion level"
"angelbreath" -> "upgrade potion"
foraging -> "about a half dozen different crafting materials"
"potion level" -> equipment

"craft gem" [shape=box]
"gem fragments" -> "craft gem" -> gem -> equipment

"haven't tried them yet" [shape=box]
"sigil" -> "haven't tried them yet"
"summoning item" -> "haven't tried them yet"
"some pvp thing" [shape=box]
"some pvp thing" -> "red dust" -> "haven't tried them yet"

indeterminate [shape=box]
gold -> indeterminate
}
-->
![](/assets/2024/diablo4-resources-graph.svg)
