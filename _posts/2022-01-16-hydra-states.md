---
date: Sun Jan 16 2022 12:54:15 -0800 (Pacific Standard Time)
qualitative_time: 
title: Hydra (Diablo 3 Season 25), simplified
previous_teaser: The most expensive meal you can eat with a toothpick
previous_first: true
---
_Mobile users be advised: this post starts a few screens to the right._

<!--
digraph {
    ranksep=0
    node [shape=box]
    start [shape=none]
    start -> h0
    subgraph cluster_out_of_combat {
        label="out of combat"
        labeljust=l
        h0 -> h5 [label="hydra", color="orangered", fontcolor="orangered"]
        h5 -> h10 [label="hydra", color="orangered", fontcolor="orangered"]
        h10 -> h10_fi2 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        h10_fi2 -> h10_fi4 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        h10_fi4 -> h10_fi5 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]

        h10 -> h10_fi5 [label="hydra", color="orangered", fontcolor="orangered", dir=back]

        h5_fi5 -> h10_fi5 [label="hydra timeout (30s)", dir=back]
        h5_fi5 -> h10 [label="hydra", color="orangered", fontcolor="orangered"]
    }
    subgraph cluster_in_combat {
        label="in combat"
        labeljust=l
        // 321.6
        H5h5 [style=filled, fillcolor="#fcfed3"]
        h10_fi5 -> H5h5 [label="hydra", color="orangered", fontcolor="orangered"]
        // 804
        H5h5_b [style=filled, fillcolor="#f7fcb9"]
        H5h5 -> H5h5_b [label="blizzard", color="royalblue", fontcolor="royalblue"]
        // 1768.8
        H5h5_b_fi2_s4 [style=filled, fillcolor="#d0ec9f"]
        H5h5_b -> H5h5_b_fi2_s4 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        // 2733.6
        H5h5_b_fi4_s8 [style=filled, fillcolor="#98d486"]
        H5h5_b_fi2_s4 -> H5h5_b_fi4_s8 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        // 3216
        H5h5_b_fi5_s10 [style=filled, fillcolor="#78c679"]
        H5h5_b_fi4_s8 -> H5h5_b_fi5_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        // 6432
        H10_b_s10 [style=filled, fillcolor="#004529", fontcolor=white]
        H5h5_b_fi5_s10 -> H10_b_s10 [label="hydra", color="orangered", fontcolor="orangered"]
        // 6432
        H10_b_fi2_s10 [style=filled, fillcolor="#004529", fontcolor=white]
        H10_b_s10 -> H10_b_fi2_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        // 6432
        H10_b_fi4_s10 [style=filled, fillcolor="#004529", fontcolor=white]
        H10_b_fi2_s10 -> H10_b_fi4_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        // 6432
        H10_b_fi5_s10 [style=filled, fillcolor="#004529", fontcolor=white]
        H10_b_fi4_s10 -> H10_b_fi5_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]

        H10_b_fi5_s10 -> H10_b_fi5_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid", dir=back]
        H10_b_s10 -> H10_b_fi5_s10 [label="hydra", color="orangered", fontcolor="orangered", dir=back]
        H10_b_s10 -> H10_b_s10 [label="blizzard", color="royalblue", fontcolor="royalblue", dir=back]
        H10_b_s10 -> H10_b_fi5_s10 [label="blizzard", color="royalblue", fontcolor="royalblue", dir=back]

        // 5212.8
        H5H4_b_fi5_s10 [style=filled, fillcolor="#12763d", fontcolor=white]
        H5H4_b_fi5_s10 -> H10_b_fi5_s10 [label="take damage", dir=back]
        H5H4_b_fi5_s10 -> H10_b_s10 [label="hydra", color="orangered", fontcolor="orangered"]
        // 5212.8
        H4H5_b_fi5_s10 [style=filled, fillcolor="#12763d", fontcolor=white]
        H4H5_b_fi5_s10 -> H10_b_fi5_s10 [label="take damage", dir=back]
        H5H4_b_s10 [style=filled, fillcolor="#12763d", fontcolor=white]
        H4H5_b_fi5_s10 -> H5H4_b_s10 [label="hydra", color="orangered", fontcolor="orangered"]
        H5H4_b_s10 -> H5H4_b_s10 [label="blizzard", color="royalblue", fontcolor="royalblue", dir=back]
        H5H4_b_fi2_s10 [style=filled, fillcolor="#12763d", fontcolor=white]
        H5H4_b_s10 -> H5H4_b_fi2_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        H5H4_b_fi4_s10 [style=filled, fillcolor="#12763d", fontcolor=white]
        H5H4_b_fi2_s10 -> H5H4_b_fi4_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        H5H4_b_fi5_s10 [style=filled, fillcolor="#12763d", fontcolor=white]
        H5H4_b_fi4_s10 -> H5H4_b_fi5_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]

        // 1616
        H5_b_fi5_s10 [style=filled, fillcolor="#d9f0a3"]
        H5_b_fi5_s10 -> H10_b_fi5_s10 [label="hydra timeout (30s)", dir=back]
        H5_b_fi5_s10 -> H10_b_s10 [label="hydra", color="orangered", fontcolor="orangered"]

        // 2572.8
        H10_fi5_s10 [style=filled, fillcolor="#a2d88a"]
        H10_fi5_s10 -> H10_b_fi5_s10 [label="blizzard timeout (6s)", dir=back]
        H10_fi5_s10 -> H10_b_s10 [label="blizzard", color="royalblue", fontcolor="royalblue"]

        // 1608
        H10_b_fi5 [style=filled, fillcolor="#d9f0a3"]
        H10_b_fi5 -> H10_b_fi5_s10 [label="spectral timeout (6s)", dir=back]
        // 3537.6
        H10_b_fi5_s4 [style=filled, fillcolor="#62bb6e"]
        H10_b_fi5 -> H10_b_fi5_s4 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        // 5453.6
        H10_b_fi5_s8 [style=filled, fillcolor="#086e3a", fontcolor=white]
        H10_b_fi5_s4 -> H10_b_fi5_s8 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]
        H10_b_fi5_s8 -> H10_b_fi5_s10 [label="electrocute", color="darkorchid", fontcolor="darkorchid"]

        h10_fi5 -> H10_b_fi5_s10 [label="end combat", dir=back]
    }
}
-->

![](/assets/2022/hydra-states-graph.svg)

_State machine for a Hydra build (Wizard) in Diablo 3, Season 25.
Player actions are colored edges; other events are black edges.
See below for description of state names.
States in the "in combat" section range from light yellow for low damage output to dark green for high damage output.
Damage output colormap is YlGn from ColorBrewer._

Used in this build:

- [Hydra](https://us.diablo3.com/en-us/class/wizard/active/hydra):
  "Summon a multi-headed Hydra for 15 seconds that attacks enemies ...
  Only one Hydra may be active at a time."
  [multi- i.e. 3-]
- [Electrocute](https://us.diablo3.com/en-us/class/wizard/active/electrocute):
  "This is a Signature spell. ...
  Lightning arcs from your fingertips ...
  The lightning can jump, hitting up to 2 additional enemies."
  It seems that this always casts twice per click.
  I don't think I've ever been able to cast it a single time only.
  Thus I use a single state transition that covers the effects of both casts.
- [Blizzard](https://us.diablo3.com/en-us/class/wizard/active/blizzard):
  "Call down shards of ice ... in a 12 yard radius."
- [Serpent's Sparker](https://us.diablo3.com/en-us/item/serpents-sparker-P68_Unique_Wand_102):
  "You may have one extra Hydra active at a time ..."
  Casting a Hydra when two are already active replaces the less recently cast one.
- [The Typhon's Veil](https://us.diablo3.com/en-us/item/typhons-thorax-P68_Unique_Chest_Set_03):
  "Double the duration of Hydras and increase the number of heads on multi-headed Hydras by two. ...
  Each time you take damage, a head dies. ...
  Hydras deal 2000% increased damage for each Hydra head alive."
  When two Hydras are active, a head from one of them dies, in a way that I can't predict.
- [Arcane Dynamo](https://us.diablo3.com/en-us/class/wizard/passive/arcane-dynamo):
  "When you cast a Signature spell, you gain a Flash of Insight.
  After 5 Flashes of Insight, your next non-Signature spell deals 60% additional damage."
  [Signature spell e.g. Electrocute; non-Signature spell e.g. Hydra, Blizzard]
- [Fragment of Destiny](https://us.diablo3.com/en-us/item/fragment-of-destiny-P610_Unique_Wand_010):
  "You gain a Spectral buff whenever you land an attack with a Signature Spell.
  Hydras deal 30% increased damage for each Spectral stack.
  Max 10 stacks."
  [Signature Spell e.g. Electrocute]
  If the spell hits multiple enemies, you get multiple stacks.
  The stacks last 6 seconds, according to some unofficial webpage, which feels about right.
  Common to many stackable effects in Diablo 3, there's only one timer for removing all stacks simultaneously, and triggering the effect (even if the stacks are maxed out and do not increase) resets the one timer.
- [Winter Flurry](https://us.diablo3.com/en-us/item/winter-flurry-P610_Unique_Orb_005):
  "... Your Hydras deals 150% increased damage to enemies in a Blizzard."
  [sic]
  I read this to my brother, and he's pretty sure that it means that the enemies have to be in the Blizzard, not the Hydras.

<!--
H10 -> 201x * 2 = 402
H9 -> 181x * 1.8 = 325.8
H5h5 -> 201x * 1 = 201
H5 -> 101x * 1 = 101
H0 -> 1x * 0 = 0

fi5 -> 1.6x

b -> 2.5x

s10 -> 4x
s8 -> 3.4x
s4 -> 2.2x
s0 -> 1x
-->

Numerous other things also strengthen Hydra in a typical build, but they don't affect this state machine (e.g. flat buffs to Hydra damage).

State names indicate:

1. The number of Hydra heads alive, with an "H" prefix for Hydras in combat and "h" for Hydras out of combat.
   In cases where two Hydras are in different states or have different numbers of heads, they are listed separately with the most recently cast Hydra first.
2. Whether the enemies are in a Blizzard, with a "b."
   In this simplified model, casting Blizzard always leads to such a state.
   Just don't miss.
   You'll be fine, Blizzard is pretty big.
3. The number of Flashes of Insight, with an "fi" prefix.
   These come from Arcane Dynamo, and I assume they're more often called Arcane Dynamo stacks.
4. The number of Spectral stacks, with an "s" prefix.
   In this simplified model, it increases as if you hit exactly 2 enemies with each cast of Electrocute while in combat (and you cast Electrocute twice per click).

Notable things not modeled:

- What happens if you cast a Hydra without 5 Flashes of Insight (you do 62.5% damage, or 81.25% damage if you cast one with and one without)
- What happens if you kite away and various Hydras and Blizzards go out of range (you gotta cast them again)
- What happens if more timeouts happen and/or you take damage while recovering from something else (you just fix everything somehow)
- What happens if you lose multiple Hydra heads from taking damage (your damage goes down further)
- What happens if both Hydras time out (you die because you lose a big chunk of damage reduction)
- What happens if Electrocute hits more or fewer than 2 enemies (you cast Electrocute more to get enough Spectral stacks, but probably not fewer because you still need Flashes of Insight)
- What happens if you manage to cast Electrocute an odd number of times (you need five, so it's actually better this way. and please post a video if you do)
- Most tactical things, such as area damage, crowd control,
[Endless Walk](https://us.diablo3.com/en-us/item/the-travelers-pledge-Unique_Amulet_008_x1),
[Oculus Ring](https://us.diablo3.com/en-us/item/oculus-ring-Unique_Ring_017_p4),
and damage increase based on player character's distance to enemy (they all make sense, they just don't fit well in this kind of model)
