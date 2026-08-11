# Vivaldi Mobile Filter Manual

This manual is written for a non-developer maintaining a small personal Vivaldi Android filter list.

You do **not** need to understand programming to use it.

The goal is simple:

```text
remove unwanted material
without breaking normal browsing or video playback
```

The project is mobile-only.

---

# 1. The Four Files

The repository should contain only:

```text
README.md
MANUAL.MD
CURRENT-STATE.MD
mobile-filters.txt
```

## `mobile-filters.txt`

This is the real blocker list.

This is the **only repository file imported into Vivaldi**.

## `README.md`

Short public description of the repository.

Do not import it into Vivaldi.

## `MANUAL.MD`

This file.

It explains how to operate the project.

Do not import it into Vivaldi.

## `CURRENT-STATE.MD`

The maintenance memory.

It records what was tried, what failed, the target environment and how future changes should be made.

Do not import it into Vivaldi.

---

# 2. Files We No Longer Use

Delete these if they still exist:

```text
desktop-filters.txt
DESKTOP-MANUAL.MD
```

This project is no longer trying to maintain separate desktop and mobile lists.

---

# 3. The Most Important Safety Rule

Never assume:

```text
third-party = advertisement
```

and never assume:

```text
MP4 = advertisement
```

A website can load its legitimate video or preview from another server.

That means a rule such as:

```text
*.mp4$media,third-party,domain=example.com
```

can block good content together with bad content.

For this project, broad MP4 blocking is forbidden as a normal solution.

---

# 4. What a Filter Can Do

There are several useful kinds of filter.

## Network rule

Example:

```text
||ads.example.net^
```

Meaning:

```text
block requests to ads.example.net
```

## Site-limited network rule

Example:

```text
||ads.example.net^$third-party,domain=example.com
```

Meaning:

```text
while visiting example.com,
block third-party requests to ads.example.net
```

This is safer because it affects fewer websites.

## Cosmetic rule

Example:

```text
example.com##.advertisement
```

Meaning:

```text
on example.com,
hide elements whose CSS class is advertisement
```

## ID cosmetic rule

Example:

```text
example.com###advertisement-box
```

The three `#` symbols look strange, but they are:

```text
##   filter syntax
#    CSS ID
```

So this means:

```text
hide #advertisement-box on example.com
```

## Popup rule

Example:

```text
*$popup,domain=example.com
```

Meaning:

```text
block popup-style navigation initiated from example.com
```

## Popunder rule

Example:

```text
*$popunder,domain=example.com
```

Meaning:

```text
block popunder-style navigation initiated from example.com
```

---

# 5. What `!` Means

A line beginning with:

```text
!
```

is a comment.

Example:

```text
! This is a note.
```

Vivaldi does not treat that line as an active blocking rule.

Comments are useful for documenting why a rule exists.

---

# 6. Why We Prefer Narrow Rules

Suppose an advertising request is:

```text
https://ads.example.net/video/ad-123.mp4
```

A dangerous reaction is:

```text
*.mp4$media,domain=example.com
```

That may block every MP4.

A better rule is:

```text
||ads.example.net^$third-party,domain=example.com
```

An even narrower rule could be:

```text
||ads.example.net/video/$media,third-party,domain=example.com
```

The principle is:

```text
block the smallest identifiable bad thing
```

not:

```text
block everything that resembles it
```

---

# 7. GitHub Page Versus RAW File

This distinction is extremely important.

The normal GitHub page looks approximately like:

```text
github.com/.../blob/main/mobile-filters.txt
```

That is a web page for humans.

Do **not** give that page to Vivaldi as the filter source.

Vivaldi needs the RAW text file:

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/main/mobile-filters.txt
```

That address returns the filter text directly.

---

# 8. How to Replace a Repository File

When ChatGPT gives you a complete replacement file:

1. Open your GitHub repository.
2. Open the file you need to replace.
3. Enter edit mode using the pencil/edit button.
4. Select all existing text.
5. Delete it.
6. Paste the complete replacement text from ChatGPT.
7. Check the beginning and end to make sure nothing was accidentally omitted.
8. Commit the change.

You do not need to manually merge old and new text when ChatGPT explicitly says:

```text
complete replacement
```

Replace the whole file.

---

# 9. How to Create `CURRENT-STATE.MD`

If the file does not exist:

1. Open the repository.
2. Choose the option to add/create a new file.
3. Name it exactly:

```text
CURRENT-STATE.MD
```

4. Paste the complete supplied content.
5. Commit it.

Capitalization matters for keeping the repository organized consistently.

---

# 10. How to Delete the Desktop Files

Delete:

```text
desktop-filters.txt
DESKTOP-MANUAL.MD
```

For each one:

1. Open the file in GitHub.
2. Use the file actions menu.
3. Choose the delete-file option.
4. Commit the deletion.

Afterward, check that the repository contains only:

```text
README.md
MANUAL.MD
CURRENT-STATE.MD
mobile-filters.txt
```

---

# 11. Preparing Vivaldi

Use the current stable Vivaldi Android version whenever practical.

Your browser interface is in Spanish, so the relevant names should look similar to:

```text
Configuración
Privacidad
Bloqueo de rastreadores y anuncios
Nivel de bloqueo predeterminado
Fuentes
Administrar fuentes de bloqueo de anuncios
Añadir nueva fuente
Importar
```

The exact layout or wording can change slightly between versions.

---

# 12. Set the Blocking Level

In Vivaldi Android:

```text
Menú Vivaldi
→ Configuración
→ Privacidad
→ Bloqueo de rastreadores y anuncios
```

Find:

```text
Nivel de bloqueo predeterminado
```

Choose:

```text
Bloquear rastreadores y anuncios
```

Do not choose:

```text
No bloquear
```

and for this project do not use only:

```text
Bloquear rastreadores
```

because we specifically need advertising filtering too.

---

# 13. Check Per-Site Blocking

Vivaldi can have a global setting and a different setting for one particular website.

While the target page is open, tap the shield / content blocker icon near the address bar.

Make sure that site is also using:

```text
Bloquear rastreadores y anuncios
```

If it says:

```text
No bloquear
```

the custom list will appear to be broken even when the rules themselves are correct.

---

# 14. Add the Custom Source

Go to:

```text
Menú Vivaldi
→ Configuración
→ Privacidad
→ Bloqueo de rastreadores y anuncios
→ Fuentes
```

Look for the advertising-blocking sources.

Choose:

```text
Añadir nueva fuente
```

Paste:

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/main/mobile-filters.txt
```

Then choose:

```text
Importar
```

After importing, make sure the new source is enabled / checked.

---

# 15. Partner Advertising Setting

While diagnosing this project, it is preferable not to have a separate advertising exception confusing the result.

Under the advertising blocking sources, look for a setting corresponding to:

```text
Permitir anuncios de nuestros socios
```

Disable/uncheck it for the test configuration.

---

# 16. If the Source Was Already Added

Editing GitHub does not mean an already open page instantly starts using the new rules.

First:

1. Commit the new `mobile-filters.txt`.
2. Close the affected website tabs.
3. Reopen the site.

If Vivaldi still appears to use an older copy of your custom list and you cannot find a manual refresh option, the simple diagnostic method is:

```text
remove the custom source
then add the RAW URL again
```

This forces you to know exactly which list is being tested.

Do not repeatedly change rules while you are uncertain whether Vivaldi has loaded the new file.

---

# 17. First Test: Group A

Do not immediately jump between many pages.

Test systematically.

Check:

```text
1. Page opens.
2. Thumbnails appear.
3. Preview behavior appears normally.
4. Open one normal video.
5. Video starts.
6. Play/pause works.
7. Seek/timeline works.
8. Volume works.
9. Fullscreen works.
10. Check whether unwanted material appears before playback.
11. Check whether unwanted material appears after playback.
12. Tap ordinary page controls/links.
13. Check whether an unwanted new tab opens.
```

The first priority is proving that normal video functionality still works.

---

# 18. Second Test: Group B

After A has been tested, repeat the same basic checks on B.

Pay special attention to:

```text
player loads
player does not enter an anti-block loop
normal video plays
first click does not open another site
later clicks do not open another site
```

B may be harder because some of its behavior is controlled directly by page JavaScript.

---

# 19. How to Use Vivaldi's Shield

Vivaldi shows a shield/content-blocker icon near the address bar.

Tap it.

It can show information about blocked trackers and advertisements for the page.

When a problem remains, this information may help determine whether:

```text
Vivaldi saw the advertising request
Vivaldi blocked something
the remaining behavior is script-driven
```

If you can send ChatGPT a screenshot of this information, do so.

---

# 20. If a Popup Still Opens

Do not add:

```text
*.js
```

or:

```text
*$script
```

for the entire website.

That can destroy the player and page navigation.

Instead record:

```text
which site you were on
what you tapped
which hostname opened
whether the original page changed
whether the normal video still played
```

The unwanted destination hostname is particularly useful.

It may allow a very narrow rule such as:

```text
||bad-destination.example^
```

---

# 21. If a Banner Still Appears

A banner can be either:

```text
a network-loaded resource
```

or:

```text
a page element that needs hiding
```

If you can take a screenshot, send it.

Describe where it appears:

```text
top of page
below player
inside player
between thumbnails
bottom of page
floating over page
```

Do not hide the entire surrounding section without identifying the banner more precisely.

---

# 22. If a Video Advertisement Still Appears

Do not respond by blocking all MP4 files.

The preferred order is:

```text
1. advertising instruction / VAST request
2. dedicated advertising domain
3. dedicated advertising path
4. specific proven advertising media URL pattern
```

Generic media blocking is the last possible experiment, not the first solution.

For this project it should normally remain disabled.

---

# 23. If Thumbnails Disappear

Assume a new filter caused damage until proven otherwise.

Check the most recently added rules.

Rules most suspicious for disappearing previews include:

```text
generic media rules
generic image rules
broad cosmetic selectors
large-container cosmetic selectors
```

Revert the newest suspicious rule and retest.

Do not try to fix missing thumbnails by adding more filters.

---

# 24. If Player Controls Disappear

Look first for cosmetic rules targeting:

```text
player
video
controls
overlay
container
```

A cosmetic rule can hide a parent element that contains both advertising and legitimate controls.

Remove or disable the suspicious rule.

The correct result is:

```text
advertising gone
controls still present
```

Not:

```text
advertising gone because the entire player is gone
```

---

# 25. About `##+js(...)`

You may see rules online resembling:

```text
example.com##+js(...)
```

These are uBlock-style scriptlet rules.

They can modify JavaScript behavior in ways ordinary network filters cannot.

Do not automatically copy them into this Vivaldi list.

A line accepted by uBlock Origin is not automatically equivalent to a rule that Vivaldi's native blocker can execute.

For this repository, active rules should remain within Vivaldi-compatible behavior unless support for a particular syntax has been verified.

---

# 26. About Upstream Filter Lists

Large projects such as EasyList, uBlock filters and AdGuard filters are useful evidence.

They tell us what experienced maintainers are currently targeting.

But this project should not blindly copy everything.

Why?

Because:

```text
different blockers support different syntax
large lists have different compatibility goals
a rule useful for millions of users may be unnecessary here
some rules depend on other rules in the larger list
```

Use upstream filters as evidence, then choose the smallest rule appropriate for this Vivaldi setup.

---

# 27. The Rule Change Procedure

Whenever adding a new rule:

1. Start from the currently working baseline.
2. Add the smallest plausible rule.
3. Increase the version number.
4. Update the modification date.
5. Commit.
6. Make sure Vivaldi is using the updated list.
7. Test the unwanted behavior.
8. Test thumbnails.
9. Test playback.
10. Test controls.
11. Keep the rule only if both sides of the test pass.

This makes it possible to know which rule caused a regression.

---

# 28. Version Numbers

The file uses:

```text
YYYYMMDD.revision
```

Example:

```text
20260811.1
```

A second change on the same day becomes:

```text
20260811.2
```

A third:

```text
20260811.3
```

The next calendar day starts again at:

```text
YYYYMMDD.1
```

---

# 29. How to Ask ChatGPT for the Next Fix

A useful report looks like:

```text
Current filter version: 20260811.1

Site: A
Page: video page

Normal video: works
Thumbnails: work
Controls: work
Fullscreen: works

Problem:
A new tab still opens after the first tap on Play.

Destination shown:
example-domain.com

The original video still begins normally.

Vivaldi shield:
12 items blocked.
```

That gives enough information to make a narrow next change.

A less useful report is:

```text
ads are still there
```

because several completely different mechanisms can all look like "an ad."

---

# 30. What Not to Do

Do not solve problems by repeatedly adding rules like:

```text
*.mp4
*.js
*$script
*$media
##video
##iframe
##.player
```

These are extremely broad in the context of a video website.

Do not add five speculative changes at once.

If the result improves, we would not know which change helped.

If the site breaks, we would not know which change caused it.

---

# 31. Recovery Procedure

If a new version breaks normal functionality:

1. Do not panic and do not add another rule.
2. Return to the previous known-good version.
3. Confirm normal functionality returns.
4. Identify which newly added rule caused the problem.
5. Remove that rule.
6. Design a narrower alternative.

Git history is useful precisely because older versions can be recovered.

---

# 32. Project Philosophy

The objective is not to create the world's largest filter list.

The objective is:

```text
quiet browsing
no unsolicited destinations
minimal unwanted inserted content
normal site functionality
easy maintenance
```

A precise list is easier to understand, test and repair.

---

# 33. Final Quick Reference

File imported into Vivaldi:

```text
mobile-filters.txt
```

RAW source:

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/main/mobile-filters.txt
```

Vivaldi setting:

```text
Configuración
→ Privacidad
→ Bloqueo de rastreadores y anuncios
→ Bloquear rastreadores y anuncios
```

Custom sources:

```text
Fuentes
→ Añadir nueva fuente
→ paste RAW URL
→ Importar
```

Never restore as a normal rule:

```text
*.mp4$media,third-party,domain=...
```

When something survives:

```text
identify it
block it narrowly
retest normal functionality
```

When something breaks:

```text
remove the last suspicious rule
do not add more rules
```

---

# 34. Before Every Future Maintenance Session

Read:

```text
CURRENT-STATE.MD
```

Then inspect the current:

```text
mobile-filters.txt
```

Only after that should new rules be proposed.
