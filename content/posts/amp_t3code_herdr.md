+++
title = "Amp, T3 Code and Herdr: three layers of the agent stack"
author = ["Katabex"]
date = 2026-11-16T00:00:00+01:00
tags = ["AI"]
draft = false
+++

New tools for working with AI coding agents seem to appear every week, and I keep trying them.
[Herdr](<https://herdr.dev>) was my first choice for running my agents, and it still is.
My workflow these days is mostly work on technical specifications and standards, and I am always looking for tools that improve it.
Two more tools keep coming up in my feeds: [Amp](<https://ampcode.com>) and [T3 Code](<https://t3.codes>).
They get compared to Herdr as if they were rivals, but they are not.
Each one sits at a different layer of the stack, and understanding that layer is the quickest way to decide whether you need it.
All three change weekly; the details below were checked against each project's own pages on November 16, 2026.

In short: Amp replaces the agent, T3 Code wraps the agent, and Herdr hosts the agent.

## Amp: the agent and the machine it runs on

Amp started inside Sourcegraph and became an independent company in December 2025.
Amp brings its own agent and its own cloud machines, called orbs, which you can also host yourself.
You pick the model, or leave it on the Amp team's Staff Pick, and Amp builds the agent around it, with a system prompt and tools tuned for each model.
An orb keeps working after you close your laptop, runs your app, and shows it through portals you can open on any device.
Orbs can also wake up on outside events: a new Linear issue, CI failing on the main branch, a webhook from another service, or a schedule the agent sets for itself.
Teammates can join the same orb to share the agent, the code and the running app.

If you prefer your own hardware, `amp --no-tui` turns your laptop or server into a runner, and you start threads on it from the web or your phone.
A runner can also share its terminals and portals, connecting directly where the network allows and through Amp's relay where it does not.

Pricing has two parts.
Model tokens bill to your own connections first - a ChatGPT or Claude subscription, or your API keys - and fall back to Amp credits, which Amp sells without markup.
Web search, reading web pages, voice and image generation with Amp's Painter tool always use Amp credits.
Hosted orb time is pay-as-you-go on the free Hobby tier, or comes from the $20 per month plan with 45,000 orb minutes.
Team workspaces are free, and so are runners and self-hosted orbs.

Amp moves fast: in one week it added boxes, always-on computers to deploy what you build (from $18 per 30 days), and spaces, where one agent coordinates the others.
It suits you if you want agents that live in the cloud, react to events and work with a team, without installing a server, configuring a terminal multiplexer or arranging remote access.
The trade-off is that you adopt Amp's agent and Amp's way of working, even when it runs on your own hardware.

## T3 Code: one interface for the agents you already have

T3 Code comes from T3 Tools and is free and MIT licensed.
It has no agent of its own.
Instead, it drives the coding CLIs you already have installed and signed in to - Claude Code, Codex, Cursor, OpenCode, pi, Grok Build, Antigravity and any agent that speaks ACP, the Agent Client Protocol - from one desktop, web or phone interface.
You can switch models in the middle of a thread, or let one agent hand a task to another.
The agents can run on your own computer or on a remote machine: the desktop app can start T3 Code on a server over SSH, and phones and other computers connect over the local network, Tailscale, or T3 Connect, an optional account-based service that is free for now and links up to three machines.
On my server, pairings with phones and browsers expired after 30 days; a desktop app connected over SSH pairs again on its own.

Its strength is the git workflow.
Every thread can get its own git worktree and branch, with an inline diff and one-click commit, push and pull request.
You can start a task at your desk and approve the next step from your phone.
Because it never resells tokens, your existing subscriptions keep working as they are.

T3 Code is young and changes almost daily, but in my use it is already pretty stable.
It is most useful for repository-heavy coding, where parallel threads on separate worktrees save real time.

## Herdr: where the agents live

Herdr is the layer underneath, and today the name covers two separate products.
Herdr TUI, the original, is a terminal multiplexer built for coding agents, in the spirit of tmux: real terminal panes that keep running in a background server when you close the window or lose your SSH connection, and that come back with their sessions after a reboot.
It adds what tmux never had - it knows, for agents that speak the open OSC 7501 status protocol, which are working, which are done, and which need you - plus a CLI and socket API that scripts and agents use to start, prompt and wait for each other.
The newer Herdr app is a native desktop app built on herdr-core, a small runtime that keeps the terminals alive while the app is closed, and you reshape the app itself by asking your agent to change its configuration.

Both work with anything that runs in a terminal, so Herdr does not care which agent you prefer.
Other machines join over SSH, and that also works across a private Tailscale network, so a home server is reachable from anywhere without opening it to the internet.
The iPhone, iPad and Android apps connect straight to herdr-core over SSH, with no account and no Herdr server in between; they do not see agents running in Herdr TUI.
For Herdr TUI there is [Sherpr](<https://codeberg.org/alevui/sherpr>), a community Android app that reaches the TUI server over SSH in the same way.
Herdr TUI, the Herdr app and herdr-core are free and open source under Apache 2.0; Sherpr is GPL-3.0.

## How they compare

| | Amp | T3 Code | Herdr |
|---|---|---|---|
| What it is | Agent platform | Front end for agent CLIs | Terminal runtime for agents (TUI and native app) |
| Own agent | Yes | No | No |
| Where agents run | Amp cloud, or your local or remote machine through an Amp runner (connects through Amp's service, no SSH or Tailscale setup) | Your local or remote machine, over SSH, Tailscale or T3 Connect | Your local or remote machine, over SSH (also across Tailscale) |
| Mobile access to remote machines | iPhone and iPad (TestFlight beta) and Android apps; portals show the running app on the phone | iOS and Android apps paired with your server over the local network or Tailscale (no account), or through T3 Connect (account) | iPhone, iPad and Android apps that reach herdr-core over SSH (Tailscale works); for Herdr TUI, the community Android app Sherpr |
| Strongest feature | Cloud orbs that start work on outside events (new issues, failing builds, schedules) | Worktrees, diffs and pull requests | Persistent panes with agent status |
| Teams | Yes | No | No |
| Price | Free tier, $20/month for hosted orbs, runners free | Free, MIT | Free, Apache 2.0 |

## What I use

My own setup is remote-first, and it runs on Herdr TUI.
Long-running agents go in Herdr TUI on a home server, which I have added as a machine in Herdr TUI on my desktop, so its workspaces sit in the same sidebar as my local ones.
On the phone I reach the same sessions with Sherpr.
Next to that production setup I test the newer Herdr app and the official phone app against herdr-core on the same server.
Both clients see the same herdr-core sessions, but not the Herdr TUI ones, so for now it stays a test bench.
I mainly use the [pi](<https://pi.dev>) coding agent, with my own extensions and skills, and bring in Claude Code, Copilot and Codex from time to time, for example to have them review the results.
I only work locally when a task needs the local machine itself, such as developing OS plugins and themes or maintaining the desktop.
Herdr is the part I would not give up - it keeps long-running agents alive and visible.
T3 Code runs as a background service on the same home server, with pi and OpenCode as its agents.
I reach it from the desktop app over SSH and from the T3 Code phone app over Tailscale, and I turn to it for pure repository work, when I want worktrees and pull requests in one place.
Amp is impressive, but for a single person who prefers to own the agent and the hardware, it duplicates more than it adds.

If you are choosing, start by asking which layer you are missing.
If your agent is fine but your terminals keep dying, look at Herdr.
If your agent is fine but juggling branches is painful, look at T3 Code.
If you want someone else to run the agent and the machine, or you work in a team that needs to share agents, threads and running apps, look at Amp.
