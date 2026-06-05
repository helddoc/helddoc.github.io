---
title: "A Cube Draft Calculator for better Cube Drafts!"
date: 2026-06-04
layout: single
categories:
  - Blog
tags:
  - mtg
---

Recently, I have been enjoying MTG Cube drafts. However, a frequent problem we encounter is that we are rarely 8 people and people run non-standard configurations of their cube (e.g,. having 426 cards). We always began discussing:

- How many packs per player should we run?
- What pack size makes sense for a given cube?
- How much of the cube is actually seen or played?
- How do different draft configurations affect pool size?

We used to simply run 3x15 or 4x12 drafts but I began to wonder if there is a way to explore different options to figure out the optimal configuration. Turns out, someone built a tool for this: [MTG-Draft-Calculator](https://www.westrope.dev/mtg-draft-calculator). My only gripe with this was that burn is non-optional and you have to manually enter configurations as opposed to using a slider. So I forked the repo and added these two functions.

Instead of guessing or relying on rules of thumb, you can adjust sliders for:
- Number of players
- Cube size
- Desired draft pool size
- Pack size limits
- Number of packs per player

The tool then computes an optimized draft configuration and shows key metrics like:
- Cards seen vs. cube size
- Cards used vs. cube size
- Burn rate per pack
- Final pool size accuracy

It’s designed for cube designers who want to quickly iterate on draft structure without spreadsheets or manual math.

👉 Try it here:  
[Cube Calculator](/cubecalculator/)
