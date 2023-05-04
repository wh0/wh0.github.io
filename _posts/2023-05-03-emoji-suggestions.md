---
date: Wed May 03 2023 19:51:01 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Emoji suggestions
previous_teaser: People who open a pile of pistachios before eating any of them
previous_first: false
---
My phone keyboard gives emoji suggestions.
I've only seen it documented as letting you type a word and giving you the emoji for it, as long as there's a relatively direct mapping.
But I'm pretty sure it can read at least a whole sentence at a time and reason about it.

![](/assets/2023/emoji-suggestions-3up.png)

_Emoji suggestions while typing.
Left: suggestions for a directly related word, "confused" giving "😕" and "🤔."
Center: a word "tonight" without an obviously associated emoji with no suggsted emoji.
Right: a sentence with suggested emojis that reasonably go with it._

And I haven't seen this suggestion-from-whole-sentence behavior documented, so here are my own experiments.
Here, I've opened a notes app (Google Keep) and typed a sentence, with a space at the end.
I've written each sentence to end with the word "tonight" to make sure this rules out suggestions based on the last word alone.

## Nice/mean
| Input | Suggestion 1 | Suggestion 2|
|--|:--:|:--:|
|have a good rest tonight|💕|😙|
|I hope you stub your toe tonight|☺️|😉|

## Feelings
| Input | Suggestion 1 | Suggestion 2|
|--|:--:|:--:|
|I'm tired tonight|😞|💤|
|I'm excited for tonight|😁|🙂|

## Going/not going
| Input | Suggestion 1 | Suggestion 2|
|--|:--:|:--:|
|I'll be there tonight|😁|🙂|
|count me out for tonight|😁|🙂|
|I'm not going tonight|😕|😔|
|I'm staying home tonight|-|-|
|I want to stay home tonight|😿|😞|
|I don't want to go tonight|😢|😔|
|I can't go tonight|😭|😔|

## People
| Input | Suggestion 1 | Suggestion 2|
|--|:--:|:--:|
|your boss will be there tonight|😀|☺️|
|your professor will be there tonight|😺|☺️|
|your ex will be there tonight|-|-|

## Opinions on a common situation
| Input | Suggestion 1 | Suggestion 2|
|--|:--:|:--:|
|there are so many customers tonight|😁|☺️|
|why are there so many customers tonight|😒|😂|

## Pizza
| Input | Suggestion 1 | Suggestion 2|
|--|:--:|:--:|
|we're getting pizza tonight|😁|☺️|
|we're not getting pizza tonight|😭|😔|
|we're all getting pizza tonight|😺|🐱|

## Experiment details
Collected on Gboard 12.8.12.514733254-release-arm54-v8a, "Personalize for you" off.
I have not texted most of these things before, I swear.
