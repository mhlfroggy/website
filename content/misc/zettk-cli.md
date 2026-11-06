---
date: '2026-05-23T00:00:00-05:00'
draft: false
title: 'Zettk-CLI'
---
I find using GUI applications such as Obsidian to be cumbersome and not
optimal in some cases for how I take notes and utilize information gathering. A
great exmaple is if I just want to make a quick edit to a file. However, I did
like some aspects of Obsidian such as templates. I also wanted to develop a
system that utilized the Zettelkasten note taking system to it's fullest, while
also learning how to develop CLIs.

I achieved all of these objectives by writing a custom CLI utility in Go using
the [Cobra](https://cobra.dev) framework which allows note creation, template
parsing, and initalizing new note vaults as needed, while maintaining 100%
compatability with Obsidian and other note taking applications if need be.

The following commands are supported
- `zettk-cli new [args] [TITLE]` -> Creates a new note with the supplied TITLE,
  arg allows choice of custom template
  - `--template` or `-t` will allow us to specify a specific template
  - `--type` specifies if it's a reference, permanent note, etc. `zettk-cli
    daily` -> Opens the daily note for the current day, or creates it if it
    doesn't exist
- `zettk-cli sp` -> Opens the `/scratchpad/scratchpad.md` markdown for quick
  notes
- `zettk-cli find` -> Searches the settlekasten for note, and provides some
  basic data such as when it was created, when it was last edited, etc.
  - `--inbox` only searches the inbox
  - `--archive` only searches the archive
  - `--reference` only searches references
  - The info returned should also specify which directory the note is in (Inbox,
    Reference, Archive)
- `zettk-cli open` finds and opens the zettlekasten note using your choice of
  editor
- `zettk-cli init` initalizes a new zettlekasten with filesystem

### Notes
- Directory management has been a complete pain in the ass to manage, Go needs
  better defaults

You can find the repo [here](https://github.com/matthewlabrecque/zettk-cli).
