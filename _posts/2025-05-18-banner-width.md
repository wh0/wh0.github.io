---
date: Sun May 18 2025 22:44:31 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Readme banner sizing
previous_teaser: Applications waiting for a counterexample to the Collatz conjecture
previous_first: true
---
| Rendition                                       | Width       | Overwide consequence | Notes                                                                                                 |
|-------------------------------------------------|-------------|----------------------|-------------------------------------------------------------------------------------------------------|
| Glitch editor markdown preview                  | 612px       | scale down           | relative file refs don't work                                                                         |
| Glitch project page embed readme                | 965px       | scale down           | relative file refs don't work; varies by scrollbar width                                              |
| GitHub readme in repo view                      | 830px       | scale down           |                                                                                                       |
| GitHub markdown view                            | 1012px      | scale down           |                                                                                                       |
| Visual Studio Code extension view readme        | 882px       | scale down           | on vscode.dev, needs crossorigin attribute on image tag or Cross-Origin-Resource-Policy: cross-origin |
| Visual Studio Marketplace extension view readme | 710.55px    | scale down           |                                                                                                       |
| Visual Studio Code markdown preview             | (unlimited) | scale down           |                                                                                                       |

It was only recently that a contributer proposed that I add a banner image to the readme file of one of my projects.

The table above shows how wide an image can be when rendered in various places.
The "Width" column is the maximum width when viewed on a large screen.
Viewing on a smaller screen or in a smaller window may reduce how wide these places allow an image to be.

If you're doing a lot vector graphics or other things that scale well, you can still use this information to guide how dense or detailed to make an image.

In my case, I was making [a Visual Studio Code extension with a pixelated style in mind](https://marketplace.visualstudio.com/items?itemName=wh0.fishcracker), so I went with 710px.
