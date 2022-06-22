---
date: Thu Jun 16 2022 00:05:30 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Resources in Diablo Immortal
previous_teaser: How fast you have to spin a banana for it to peel itself
previous_first: false
---
<!--
digraph {

# shop
"free daily rewards" [shape=box]
"daily login" -> "free daily rewards"
// "free daily rewards" -> "gold"
// "free daily rewards" -> "lower quality crafting material"
"free daily rewards" -> "glowing shard"

// "money" [color=purple]
// "buy eternal orb" [shape=box, color=purple]
// "money" -> "buy eternal orb" [color=purple]
// "eternal orb" [color=purple]
// "buy eternal orb" -> "eternal orb" [color=purple]
// "buy platinum" [shape=box, color=purple]
// "eternal orb" -> "buy platinum" [color=purple]
// "buy platinum" -> "platinum" [color=purple]

// "buy crest" [shape=box, color=purple]
// "eternal orb" -> "buy crest" [color=purple]
// "buy crest" -> "legendary crest" [color=purple]
p2w_legendary_crest [label="pay", shape=box, color=purple]
p2w_legendary_crest -> "legendary crest" [color=purple]

// "buy bundle" [shape=box, color=purple]
// "money" -> "buy bundle" [color=purple]
// "buy bundle" -> "eternal orb" [color=purple]
// "buy bundle" -> "cosmetics" [color=purple]
p2w_cosmetics [label="pay", shape=box, color=purple]
p2w_cosmetics -> "cosmetics" [color=purple]
// "buy bundle" -> "legendary crest" [color=purple]
// "buy bundle" -> "crest" [color=purple]
p2w_crest [label="pay", shape=box, color=purple]
p2w_crest -> "crest" [color=purple]
// "buy bundle" -> "gem" [color=purple]
p2w_gem [label="pay", shape=box, color=purple]
p2w_gem -> "gem" [color=purple]
// "buy bundle" -> "scoria" [color=purple]
p2w_scoria [label="pay", shape=box, color=purple]
p2w_scoria -> "scoria" [color=purple]

// "buy cosmetics" [shape=box, color=purple]
// "eternal orb" -> "buy cosmetics" [color=purple]
// "buy cosmetics" -> "cosmetics" [color=purple]

// "boon of plenty" [shape=box, color=purple]
// "money" -> "boon of plenty" [color=purple]
// "daily login" -> "boon of plenty" [color=purple]
// "boon of plenty" -> "legendary gem" [color=purple]
p2w_legendary_gem [label="pay", shape=box, color=purple]
p2w_legendary_gem -> "legendary gem" [color=purple]
// "boon of plenty" -> "legendary crest" [color=purple]
// "boon of plenty" -> "crest" [color=purple]
// "boon of plenty" -> "gem" [color=purple]

// "buy reforge stone" [shape=box, color=purple]
// "eternal orb" -> "buy reforge stone" [color=purple]
// "buy reforge stone" -> "specialized reforge stone" [color=purple]
p2w_specialized_reforge_stone [label="pay", shape=box, color=purple]
p2w_specialized_reforge_stone -> "specialized reforge stone" [color=purple]
// "buy dawning echo" [shape=box, color=purple]
// "eternal orb" -> "buy dawning echo" [color=purple]
// "buy dawning echo" -> "dawning echo" [color=purple]
p2w_dawning_echo [label="pay", shape=box, color=purple]
p2w_dawning_echo -> "dawning echo" [color=purple]

# battle pass
"battle pass quest" [shape=box]
"battle pass quest" -> "hilts"
"battle pass (f)" [shape=box]
"battle points" -> "battle pass (f)"
// "battle pass (f)" -> "lower quality crafting material"
"battle pass (f)" -> "hilts"
// "battle pass (f)" -> "crest"
// "battle pass (f)" -> "gem"
// "battle pass (f)" -> "legendary item"
"battle pass (f)" -> "legendary gem"
// "battle pass (f)" -> "charm"
"battle pass (f)" -> "legendary crest"
"battle pass (f)" -> "reforge stone"
"battle pass (f)" -> "specialized reforge stone"

// "battle pass (e)" [shape=box, color=purple]
// "money" -> "battle pass (e)" [color=purple]
// "battle points" -> "battle pass (e)" [color=purple]
// "battle pass (e)" -> "cosmetics" [color=purple]
// "battle pass (e)" -> "legendary gem" [color=purple]
// "battle pass (e)" -> "scoria" [color=purple]
// "battle pass (e)" -> "gem" [color=purple]
// "battle pass (e)" -> "crest" [color=purple]
// "battle pass (e)" -> "aspirant's key" [color=purple]
p2w_aspirants_key [label="pay", shape=box, color=purple]
p2w_aspirants_key -> "aspirant's key" [color=purple]
// "battle pass (e)" -> "legendary crest" [color=purple]

# first kill of the day
"first kill of the day" [shape=box]
"daily login" -> "first kill of the day"
// "first kill of the day" -> "gold"
// "first kill of the day" -> "charm"
// "first kill of the day" -> "crest"
// "first kill of the day" -> "scrap materials"
// "first kill of the day" -> "aspirant's key"
// "first kill of the day" -> "gem"
// "first kill of the day" -> "hilts"

# prodigy's path
"prodigy's path (a)" [shape=box]
// "prodigy's path (a)" -> "lower quality crafting material"
"prodigy's path (a)" -> "legendary item"
// "prodigy's path (a)" -> "glowing shard"
"prodigy's path (a)" -> "set item"
"prodigy's path (a)" -> "enigmatic crystal"

// "prodigy's path (p)" [shape=box, color=purple]
// "money" -> "prodigy's path (p)" [color=purple]
// "prodigy's path (p)" -> "legendary crest" [color=purple]
// "prodigy's path (p)" -> "scoria" [color=purple]

# codex
"daily activity rewards" [shape=box]
"battle points" -> "daily activity rewards"
"daily activity rewards" -> "hilts"
"daily activity rewards" -> "charm"
"daily activity rewards" -> "platinum"
"boss battle" [shape=box]
"boss battle" -> "battle points"

# bestiary
"bestiary unlock" [shape=box]
"monstrous essence" -> "bestiary unlock"
"bestiary unlock" -> "battle points"

# rifts
"elder rift daily reward" [shape=box]
"daily login" -> "elder rift daily reward"
"elder rift daily reward" -> "crest"
"elder rift (f)" [shape=box]
"elder rift (f)" -> "fading ember"
"elder rift (f)" -> "blessed chest"
"elder rift (f)" -> "battle points"
"elder rift (r)" [shape=box]
"crest" -> "elder rift (r)"
"elder rift (r)" -> "other rune"
"elder rift (r)" -> "ati rune"
"elder rift (l)" [shape=box]
"legendary crest" -> "elder rift (l)"
"elder rift (l)" -> "legendary gem"

"challenge rift" [shape=box]
"challenge rift" -> "enigmatic crystal"
"challenge rift first-clear rewards" [shape=box]
// "challenge rift first-clear rewards" -> "gold"
"challenge rift first-clear rewards" -> "hilts"

"fading embers trade" [shape=box]
"fading ember" -> "fading embers trade"
"fading embers trade" -> "fa rune"
"trade for ati runes" [shape=box]
"other rune" -> "trade for ati runes"
"trade for ati runes" -> "ati rune"

# blacksmith
"salvage (j)" [shape=box]
"lower quality item" -> "salvage (j)"
"salvage (j)" -> "lower quality crafting material"
"salvage (l)" [shape=box]
"legendary item" -> "salvage (l)"
"set item" -> "salvage (l)"
"salvage (l)" -> "glowing shard"

"upgrade item (p)" [shape=box]
"lower quality crafting material" -> "upgrade item (p)"
"glowing shard" -> "upgrade item (p)"
"legendary item" -> "upgrade item (p)" [dir=both, color=red]
"upgrade item (s)" [shape=box]
"lower quality crafting material" -> "upgrade item (s)"
"enigmatic crystal" -> "upgrade item (s)"
"set item" -> "upgrade item (s)" [dir=both, color=red]

"reforge item" [shape=box]
"reforge stone" -> "reforge item"
"specialized reforge stone" -> "reforge item"
"set item" -> "reforge item" [dir=both, color=red]
"legendary item" -> "reforge item" [dir=both, color=red]

"refine scoria" [shape=box]
"scoria" -> "refine scoria"
"refine scoria" -> "hellfire scoria"

"smithing materials vendor" [shape=box]
"lower quality crafting material" -> "smithing materials vendor"
"smithing materials vendor" -> "lower quality crafting material"
"glowing shard" -> "smithing materials vendor"
"smithing materials vendor" -> "enigmatic crystal"

# jeweler
"upgrade gem" [shape=box]
"gem" -> "upgrade gem"
"echo crystal" -> "upgrade gem"
"upgrade gem" -> "gem"
"upgrade legendary gem" [shape=box]
"gem fragment" -> "upgrade legendary gem"
"legendary gem" -> "upgrade legendary gem"
"legendary gem" -> "upgrade legendary gem" [dir=both, color=red]
"upgrade legendary gem" -> "gem fragment"

"craft legendary gem" [shape=box]
"ati rune" -> "craft legendary gem"
"other rune" -> "craft legendary gem"
"craft legendary gem" -> "legendary gem"
"craft random legendary gem" [shape=box]
"fa rune" -> "craft random legendary gem"
"craft random legendary gem" -> "legendary gem"

"buy crystal" [shape=box]
"platinum" -> "buy crystal"
"buy crystal" -> "echo crystal"

"awaken item" [shape=box]
"dawning echo" -> "awaken item"
"legendary item" -> "awaken item" [dir=both, color=red]

"add resonance" [shape=box]
"dawning echo" -> "add resonance"
"legendary gem" -> "add resonance"
"legendary item" -> "add resonance"  [dir=both, color=red]

# hilts trader
"hilts" [color=green3]
"hilts trader" [shape=box, color=green3]
"hilts" -> "hilts trader" [color=green3]
"hilts trader" -> "legendary crest" [color=green3]
"hilts trader" -> "legendary item" [color=green3]
"hilts trader" -> "legendary gem" [color=green3]
"hilts trader" -> "crest" [color=green3]
"hilts trader" -> "gem" [color=green3]
"hilts trader" -> "reforge stone" [color=green3]
"hilts trader" -> "aspirant's key" [color=green3]
"hilts trader" -> "charm" [color=green3]

# bounties
"bounty" [shape=box]
"bounty" -> "battle points"

# clan stuff
"create clan" [shape=box]
"platinum" -> "create clan"

# helliquary
"helliquary raid" [shape=box]
"helliquary raid" -> "trophy"
"helliquary raid" -> "scoria"
"upgrade helliquary" [shape=box]
"hellfire scoria" -> "upgrade helliquary"
"upgrade helliquary" -> "helliquary bonus"
"helliquary seal" [shape=box]
"trophy" -> "helliquary seal"
"helliquary seal" -> "helliquary special attributes"

# market
"platinum" [color=blue]
// "buy on market" [shape=box, color=blue]
// "platinum" -> "buy on market" [color=blue]
// "sell on market" [shape=box, color=blue]
// "sell on market" -> "platinum" [color=blue]
// "buy on market" -> "legendary gem" -> "sell on market" [color=blue]
market_legendary_gem [label="market", shape=box, color=blue]
market_legendary_gem -> "legendary gem" [dir=both, color=blue]
// "buy on market" -> "gem" -> "sell on market" [color=blue]
market_gem [label="market", shape=box, color=blue]
market_gem -> "gem" [dir=both, color=blue]
// "buy on market" -> "skill stone" -> "sell on market" [color=blue]
market_skill_stone [label="market", shape=box, color=blue]
market_skill_stone -> "skill stone" [dir=both, color=blue]

# gambling
"rarities & antiques" [shape=box]
"gold" -> "rarities & antiques"
"rarities & antiques" -> "lower quality item"
"rarities & antiques" -> "set item"
"rarities & antiques" -> "legendary item"

# adventure seekder
"side quest" [shape=box]
"side quest" -> "adventure journal"
"elite quest" [shape=box]
"adventure journal" -> "elite quest"

# cycles stuff
"shadow lottery" [shape=box]
"daily login" -> "shadow lottery"
"shadow lottery" -> "akeba's signet"

"shadow rank up" [shape=box]
"mark" -> "shadow rank up"
"shadow rank up" -> "cycle bonus"

"contract" [shape=box]
"contract" -> "mark"
"path of blood" [shape=box]
"path of blood" -> "mark"
"vault raid" [shape=box, color=orange]
"vault raid" -> "hilts" [color=orange]
"battleground" [shape=box, color=orange]
"battleground" -> "glowing shard" [color=orange]
"battleground" -> "aspirant's key" [color=orange]
"battleground" -> "reforge stone" [color=orange]
"assembly" [shape=box]
"assembly" -> "aspirant's key"
"assembly" -> "mark"
"rite of exile" [shape=box, color=orange]
"rite of exile" -> "mark" [color=orange]
"rite of exile" -> "hilts" [color=orange]
"shadow war" [shape=box, color=orange]
"shadow war" -> "legendary crest" [color=orange]
"shadow war" -> "aspirant's key" [color=orange]

"open blessed chest" [shape=box]
"blessed chest" -> "open blessed chest"
"open blessed chest" -> "aspirant's key"

"buy cycle's turn chest" [shape=box]
"hilts" -> "buy cycle's turn chest"
"daily login" -> "buy cycle's turn chest"

# essence
"extract essence" [shape=box]
"legendary item" -> "extract essence"
"extract essence" -> "essence"
"inherit essence" [shape=box]
"essence" -> "inherit essence"
"legendary item" -> "inherit essence" [dir=both, color=red]

# legacy of the horadrim
"sanctum depths" [shape=box]
"sanctum depths" -> "aspirant's key"
"open sanctum chest (f)" [shape=box]
"daily login" -> "open sanctum chest (f)"
"open sanctum chest (f)" -> "culling stone"
"open sanctum chest (k)" [shape=box]
"aspirant's key" -> "open sanctum chest (k)"
"open sanctum chest (k)" -> "culling stone"
// "open sanctum chest (p)" [shape=box]
// "platinum" -> "open sanctum chest (p)"
// "open sanctum chest (p)" -> "culling stone"
"upgrade vessel" [shape=box]
"culling stone" -> "upgrade vessel"
"upgrade vessel" -> "vessel bonus"

# charms
"salvage charm" [shape=box]
"charm" -> "salvage charm"
"salvage charm" -> "alchemical powder"
"upgrade charm" [shape=box]
"alchemical powder" -> "upgrade charm"
"upgrade charm" -> "charm" [dir=both, color=red]
"extract charm" [shape=box]
"charm" -> "extract charm"
"extract charm" -> "skill stone"
"imbue charm" [shape=box]
"skill stone" -> "imbue charm"
"charm" -> "imbue charm" [dir=both, color=red]

# warband
"deliver warband chest" [shape=box]
"sealed warband chest" -> "deliver warband chest"
"deliver warband chest" -> "warband stash item"
"borrow" [shape=box]
"warband stash item" -> "borrow"
"borrow" -> "legendary item"

# sinks
"equipment" [shape=box]
"legendary item" -> "equipment"
"legendary gem" -> "equipment"
"set item" -> "equipment"
"gem" -> "equipment"
"charm" -> "equipment"
"cosmetics" -> "equipment"
"helliquary bonus" -> "equipment"
"helliquary special attributes" -> "equipment"
"cycle bonus" -> "equipment"
"vessel bonus" -> "equipment"

}
-->
![](/assets/2022/diablo-immortal-resources-graph.svg)
