+++
title = "Mread, a minimal Markdown reader for Omarchy"
author = ["Katabex"]
date = 2026-11-24T00:00:00+01:00
tags = ["Omarchy"]
draft = false
+++

I read more Markdown than ever: notes, documentation, and the reports my AI agents write.
An editor is the wrong tool for comfortable reading, so I built a reader instead: [Mread](<https://github.com/katabex/mread>), a minimal Markdown reader for [Omarchy](<https://omarchy.org>).

Mread does one job: it opens a Markdown file and sets it in a comfortable column.
It never changes your files.

{{< figure src="/images/mread_screenshot.png" >}}

It is built for the keyboard.
Vim keys move through the text, with counts like `5j` and jumps between headings with `]]` and `[[`.
`/` searches as you type and highlights every match, and `o` opens a fuzzy finder that lists recent files first, then every Markdown file in your home directory.
Press `?` at any time to see all the keys.

The colors follow the Omarchy theme and change live when you switch themes; the screenshot shows [Legibilis]({{< relref "legibilis_a_readable_omarchy_theme" >}}).
You choose the text font, code font, font size and column width, and Mread remembers them.

It renders GitHub-flavored Markdown, including tables, task lists, code blocks and images.
Links to other Markdown files and to headings open inside Mread; other links open in your browser.

It also works well next to an editor.
Press `e` to open the file in your Omarchy default editor, and keep Mread beside it as a live preview: it reloads on every save and keeps your place.
When you start it again, it reopens the last file where you left off.

Like Omarchy's own apps, Mread is written in C++ and Qt Quick and installs as an Arch package, so it shows up in the app launcher.
Installing is three commands:

```bash
git clone https://github.com/katabex/mread.git
cd mread
bin/install
```

To make it the default app for Markdown files:

```bash
xdg-mime default mread.desktop text/markdown
```

Mread is MIT-licensed.
The theme watching follows [Omacut](<https://github.com/omacom/omacut>) by David Heinemeier Hansson.
