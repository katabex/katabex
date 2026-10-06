+++
title = "Legibilis, a readable Omarchy theme"
author = ["Katabex"]
date = 2026-10-06T20:45:00+02:00
tags = ["Omarchy"]
draft = false
+++

For me, readability should be the top priority of any theme.
I have been hopping between the themes that ship with Omarchy, but none came close enough to [Modus Vivendi](<https://github.com/protesilaos/modus-themes>), my favourite Emacs theme - a palette built around readability.

So I decided to build my own theme, and today I released it: [Legibilis](<https://github.com/katabex/omarchy-legibilis-theme>), a new dark theme for [Omarchy](<https://omarchy.org>).
The name is Latin for "readable".

Legibilis starts from the Modus Vivendi Tinted palette on a cobalt background, and then fine-tunes it: the fourteen Modus colors that read below 7:1 on that background are lifted, and the reds and blues are spread in hue, chroma and contrast so that lifting them does not merge them.
Every color now reads at 7:1 or more - beyond WCAG AAA - and the palette stays distinguishable for color-blind viewers.
The hues move only a few degrees from the originals, so it still looks like Modus; it is just readable.

The backgrounds are classical marble sculptures - the head of the Apollo Belvedere and a detail of Michelangelo's David - cropped and toned to match the palette.

{{< figure src="/images/legibilis_preview.png" >}}

Installing is one command:

```bash
omarchy theme install https://github.com/katabex/omarchy-legibilis-theme
```

The repo also has matching extras: an Emacs theme built with `modus-themes-theme` that carries the same fine-tuned palette to every face the Modus themes support, and a FreeTube stylesheet in the same colors.

The palette comes from the [Modus themes](<https://github.com/protesilaos/modus-themes>) by Protesilaos Stavrou.
