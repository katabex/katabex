+++
title = "Monitoring AI agents on Omarchy"
author = ["Katabex"]
date = 2026-10-04T00:00:00+02:00
tags = ["Omarchy", "AI"]
draft = false
+++

I spend my days working with AI coding agents. Claude Code, Codex, OpenCode, pi, Hermes - each has its strengths, and I switch between them depending on the task. They run on a mix of subscriptions (Anthropic, OpenAI, Z.ai, OpenRouter, Fireworks, Copilot) and prepaid credits, which makes one simple question surprisingly hard to answer: how much of my allowance is left, and where is it actually going?

[agents-monitor](<https://github.com/katabex/agents-monitor>) is my answer. It is a plugin for [Omarchy](<https://omarchy.org>), the Arch Linux and Hyprland desktop, that replaces the stock agents widget in the bar with a panel showing your usage per subscription and per agent.

The panel has three views. Subscriptions shows how full each allowance is and when it resets, or your remaining prepaid balance, ordered by actual use. By agent shows one tab per coding tool you used in the last seven days, with a day-by-day token chart for the week. Totals sums this week's usage by subscription and by model, across all tools.

It works out of the box with Claude Code, Codex, Copilot, OpenCode, pi and Hermes - no configuration needed, it discovers them on its own. OpenRouter, Z.ai and Fireworks only need an API key or a sign-in. A subscription that is not configured simply gets no tab.

Installing is one command:

```bash
git clone https://github.com/katabex/agents-monitor
cd agents-monitor
./install.sh
```

Uninstalling with `./uninstall.sh` restores the stock widget and removes the plugin without a trace.

It is written in QML and shell, MIT licensed, and [the code is on GitHub](<https://github.com/katabex/agents-monitor>). Issues and suggestions are welcome.
