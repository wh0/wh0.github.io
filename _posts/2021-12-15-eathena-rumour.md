---
date: Wed Dec 15 2021 21:51:07 -0800 (Pacific Standard Time)
qualitative_time: 
title: What it was like to play Ragnarok Online as a computer scientist
previous_teaser: People who get boba at less than 100% sugar, not even to save the calories or to protest how much sugar the country consumes, the people who genuinely find it to taste the best that way, the very existence of people like that, that they experience the world so, that they may have been among us this whole time, people we could have spoken to just the other day, or shared a meal with, or whom we rely on for our livelihood or who rely on us, that they would order their boba this way and be as happy with it as we are with ours
previous_first: true
---
Long ago, I played a game called Ragnarok Online.
But kind of not really: I played on a private server that ran an "emulator" of the official game server.
That emulator was an open source project called eAthena.

I found out, probably following an inopportune internet outage, that disconnecting during a dialogue sequence in the game could leave your game progress in an inconsistent state.
It appeared that smaller operations, such as adding or removing items from your inventory or granting you experience, were performed step by step throughout a dialogue.
And even though there would be no button other than to advance linearly through this kind of dialogue, disconnecting in the middle would abort any steps yet to be performed in the "script" of the game quest.
That is, some scripts that would act out events like trading with a non-player character (NPC) would not be done atomically.
And the way the server would wait for the player to click through dialogue between carrying out the steps meant that a player could abort a script at an exact step of their choosing.

I went to the eAthena source code to find places in the quest scripts that behaved this way, with dialogue inserted between other actions.
(I didn't care at all about spoilers to that game's story.)
One such place I found takes place in the [Rogue Guild Investigation Quest](https://irowiki.org/classic/Rogue_Guild_Investigation_Quest).
First, you would gather several odds and ends for some NPCs.

![Fabric](/assets/2021/eathena-rumour-1059.gif)
![Golden Hair](/assets/2021/eathena-rumour-1060.gif)
![Little Evil Horn](/assets/2021/eathena-rumour-1038.gif)
![Horseshoe](/assets/2021/eathena-rumour-944.gif)
![Jack o' Pumpkin](/assets/2021/eathena-rumour-1062.gif)
![Green Herb](/assets/2021/eathena-rumour-511.gif)
![Alcohol](/assets/2021/eathena-rumour-970.gif)
![Mr. Smile](/assets/2021/eathena-rumour-2278.gif)

Then at one point, an NPC gives you a food item.
Here's roughly how that part is written in the [quest script](https://github.com/eathena/eathena/blob/0cb07145ea1431528d0591b6a8706856915b3782/npc/quests/quests_nameless.txt#L13037-L13080):

```
// Some dialogue omitted and code simplified for brevity
else if (rumour_nd == 19) {
	// ...
	mes "[Agent]";
	mes "Oh, and take this food";
	mes "with my apologies, 'kay?";
	getitem 12064,1; //Dex_Dish04
	next;
	mes "[Agent]";
	mes "Hey, be safe on your";
	mes "way to Morroc, alright?";
	set rumour_nd,20;
	close;
}
```

This quest uses a variable (saved with your character) named `rumour_nd` to track its progress, and this NPC performs an action on step 19 of this quest.
He gives you a food item (actually one chosen at random from a set in the real code) and then advances the quest progress variable to the next value to indicate that you've received this food item.

![Herb Marinade Beef](/assets/2021/eathena-rumour-12044.gif)
![Lutie Cold Noodle](/assets/2021/eathena-rumour-12059.gif)
![Cream Sandwich](/assets/2021/eathena-rumour-12064.gif)
![Morocc Fruit Wine](/assets/2021/eathena-rumour-12049.gif)
![Seasoned Jellyfish](/assets/2021/eathena-rumour-12054.gif)
![Steamed Ancient Lips](/assets/2021/eathena-rumour-12069.gif)

However, a `next` instruction in between causes the whole game, server included, to wait for you to click through the "Oh, and take this ..." message before going on to update your quest progress.
Disconnecting at this point in the dialogue before you click the "next" button leaves the game in an inconsistent state where you've received the food, but you also "haven't" spoken to this NPC yet.
When you would log in again, you could speak to that NPC again and receive another food.
If you would stop and disconnect there again, you could then receive another food.
And another.
Until you got bored.

The author of this script, in all likeliness, didn't intend for an incomplete run through this script to be allowed.
Yet, as I found out perhaps hundreds of times back then, the emulator allowed it.

---

And if anyone wants to stay around to read about why I bothered with this specific case with the food items, it's because one of them increases your DEX stat by 4 for a while.

Back then, skill cast time was calculated as some variables times a factor of `(1 - DEX/150)`.
Reaching 150 DEX thus would turn your character into some kind of bizarrely powerful being that could skip the entire casting animation and bring down screenfuls of meteors all at once.
The game was balanced so that it would take a lot of grinding for gear plus cooperating with many other character classes to reach 150 DEX.

![Apple of Archer](/assets/2021/eathena-rumour-2285.gif)
![Valkyrian Armor](/assets/2021/eathena-rumour-2357.gif)
![Orleans's Glove](/assets/2021/eathena-rumour-2701.gif)

![Drops Card](/assets/2021/eathena-rumour-4004.gif)
![Rocker Card](/assets/2021/eathena-rumour-4021.gif)
![Zerom Card](/assets/2021/eathena-rumour-4064.gif)

So if you had endgame gear that already brought you pretty close to 150 DEX, being able to get this food that gives 4 extra DEX made a huge impact on how much you could do in battle or how many fewer people you had to ask for help or how many people you could bribe into helping you.
It was paramount, that is to say.
