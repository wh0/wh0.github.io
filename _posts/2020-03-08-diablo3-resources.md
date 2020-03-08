---
date: Sun Mar 08 2020 12:49:29 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Resources in Diablo 3
previous_teaser: Switches that are safe to operate with wet hands vs. switches that look good next to the sink
previous_first: true
---
<!--
digraph {
	// activities

	// "adventuring" [shape=box]
	// "adventuring" -> "gold"
	// "adventuring" -> "lower quality gem"
	// "adventuring" -> "lower quality crafting material"
	// "adventuring" -> "forgotten soul"
	// "adventuring" -> "lower quality item"
	// "adventuring" -> "legendary quality item"
	// "adventuring" -> "death's breath"

	"rift" [shape=box]
	"rift" -> "greater rift keystone"
	"rift" -> "gold"
	"rift" -> "legendary quality item"
	"rift" -> "death's breath"

	"greater rift" [shape=box]
	"greater rift keystone" -> "greater rift"
	"greater rift" -> "blood shard"
	"greater rift" -> "legendary gem"
	"greater rift" -> "upgrade attempt"

	"empower" [shape=box]
	"gold" -> "empower"
	"empower" -> "upgrade attempt"

	"upgrade legendary gem" [shape=box]
	"upgrade attempt" -> "upgrade legendary gem"
	"upgrade legendary gem" -> "legendary gem" [dir=both, color=red]

	"gamble" [shape=box]
	"blood shard" -> "gamble"
	"gamble" -> "lower quality item"
	"gamble" -> "legendary quality item"

	"bounty" [shape=box]
	"bounty" -> "bounty material"

	// kanai's cube

	"extract legendary power" [shape=box]
	"legendary quality item" -> "extract legendary power"
	"bounty material" -> "extract legendary power"
	"death's breath" -> "extract legendary power"
	"extract legendary power" -> "legendary power"

	"upgrade rare item" [shape=box]
	"lower quality item" -> "upgrade rare item"
	"death's breath" -> "upgrade rare item"
	"lower quality crafting material" -> "upgrade rare item"
	"upgrade rare item" -> "legendary quality item"

	"convert set item" [shape=box]
	"death's breath" -> "convert set item"
	"forgotten soul" -> "convert set item"
	// "convert set item" -> "forgotten soul" [dir=back, color=green]
	"convert set item" -> "legendary quality item" [dir=both, color=red]

	// "convert gems (j)" [shape=box]
	// // "essence" -> "convert gems (j)"
	// "convert gems (j)" -> "lower quality gem" [dir=both, color=red]

	"convert gems (fr)" [shape=box]
	// "essence" -> "convert gems (fr)"
	"convert gems (fr)" -> "flawless royal gem" [dir=both, color=red]

	"convert crafting materials" [shape=box]
	"lower quality item" -> "convert crafting materials"
	"convert crafting materials" -> "lower quality crafting material" [dir=both, color=red]

	// "remove level requirement on an item" [shape=box]
	// "legendary gem" -> "remove level requirement on an item"
	// "remove level requirement on an item" -> "legendary quality item" [dir=both, color=red]

	"reforge legendary quality item" [shape=box]
	"bounty material" -> "reforge legendary quality item"
	"forgotten soul" -> "reforge legendary quality item"
	// "reforge legendary quality item" -> "forgotten soul" [dir=back, color=green]
	"reforge legendary quality item" -> "legendary quality item" [dir=both, color=red]

	"augment ancient or primal ancient item" [shape=box]
	"legendary gem" -> "augment ancient or primal ancient item"
	"flawless royal gem" -> "augment ancient or primal ancient item"
	"augment ancient or primal ancient item" -> "legendary quality item" [dir=both, color=red]

	// blacksmith

	"salvage (j)" [shape=box]
	"lower quality item" -> "salvage (j)"
	"salvage (j)" -> "lower quality crafting material"
	// "salvage (j)" -> "lower quality item" [dir=back, color=green]
	// "lower quality crafting material" -> "salvage (j)" [dir=back, color=green]

	"salvage (l)" [shape=box]
	"legendary quality item" -> "salvage (l)"
	"salvage (l)" -> "forgotten soul"
	// "salvage (l)" -> "legendary quality item" [dir=back, color=green]
	// "forgotten soul" -> "salvage (l)" [dir=back, color=green]

	"craft lower quality item" [shape=box]
	"lower quality crafting material" -> "craft lower quality item"
	// "gold" -> "craft lower quality item"
	"craft lower quality item" -> "lower quality item"

	"craft legendary quality item" [shape=box]
	"lower quality crafting material" -> "craft legendary quality item"
	"bounty material" -> "craft legendary quality item"
	// "gold" -> "craft legendary quality item"
	"craft legendary quality item" -> "legendary quality item"

	// jeweler

	"combine gems" [shape=box]
	"lower quality gem" -> "combine gems"
	// "death's breath" -> "combine gems"
	// "gold" -> "combine gems"
	"combine gems" -> "flawless royal gem"

	"craft jewelry" [shape=box]
	"lower quality gem" -> "craft jewelry"
	// "gold" -> "craft jewelry"
	"craft jewelry" -> "lower quality item"

	// mystic

	// "enchant (j)" [shape=box]
	// "lower quality crafting material" -> "enchant (l)"
	// "gold" -> "enchant (j)"
	// "enchant (j)" -> "lower quality item" [dir=both, color=red]

	"enchant (l)" [shape=box]
	"death's breath" -> "enchant (l)"
	"gold" -> "enchant (l)"
	"lower quality gem" -> "enchant (l)"
	"enchant (l)" -> "legendary quality item" [dir=both, color=red]

	// "buy essence" [shape=box]
	// "gold" -> "buy essence"
	// "buy essence" -> "essence"

	"equipment" [shape=box]
	"legendary quality item" -> "equipment"
	"flawless royal gem" -> "equipment"
	"legendary power" -> "equipment"
	"legendary gem" -> "equipment"
}
-->
![](/assets/2020/diablo3-resources-graph.svg)
