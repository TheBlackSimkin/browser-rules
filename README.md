# browser-rules

Personal mobile browser configuration.

This repository contains a small set of conservative content-filtering rules intended for a specific Android browsing setup.

The public documentation deliberately uses neutral aliases. The purpose is not secrecy; it is simply to keep the repository description discreet.

## Scope

This project is:

* mobile-only;
* intended for Vivaldi on Android;
* focused on removing unwanted page elements, unsolicited navigation and short inserted content;
* designed to preserve normal page navigation, previews, playback and player controls.

It is **not** intended to be a giant general-purpose blocklist.

The rule philosophy is:

> narrow rule first, broad rule only with evidence.

## Files

### `mobile-filters.txt`

The only file that Vivaldi should import.

It contains the active filtering rules.

### `MANUAL.MD`

Beginner-oriented operating manual.

It explains how to edit the repository, load the list in Vivaldi, test changes and recover from a bad rule.

Do **not** import this file into Vivaldi.

### `CURRENT-STATE.MD`

Maintenance handoff.

It records the current device, browser assumptions, previous failures, testing status, aliases and project preferences so a later maintenance session can resume without reconstructing the entire history.

Do **not** import this file into Vivaldi.

## Import

Only the RAW form of `mobile-filters.txt` is a blocker source.

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/main/mobile-filters.txt
```

The normal GitHub page for the file is **not** the blocker-list address.

## Maintenance rule

If a new rule removes an unwanted element but also damages normal content, the rule is considered wrong.

Never solve a filtering problem by knowingly breaking the underlying page.

## Alias convention

Public maintenance notes use:

```text
A
B
R1
R2
```

The maintenance handoff contains the codebook in a deliberately lightweight encoded form.

## Project state

Read `CURRENT-STATE.MD` before changing active rules.

That file is the source of truth for what has already been tried and what must not be repeated.
