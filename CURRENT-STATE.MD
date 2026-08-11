# CURRENT STATE — MOBILE FILTER PROJECT

Last major rewrite: **2026-08-11**

This file exists so a future maintenance session can resume the project without reconstructing the earlier conversation.

Read this file **before changing `mobile-filters.txt`**.

---

# 1. Project Direction

The repository is now **mobile-only**.

Desktop-specific filters and desktop-specific manuals are intentionally removed.

Expected repository files:

```text
README.md
MANUAL.MD
CURRENT-STATE.MD
mobile-filters.txt
```

Files that should NOT exist anymore:

```text
desktop-filters.txt
DESKTOP-MANUAL.MD
```

---

# 2. User Environment

Primary device:

```text
Redmi Note 13 Pro 5G
Android 13
```

Primary browser:

```text
Vivaldi Android
```

Browser/device interface language:

```text
Spanish
```

Conversation/documentation preference:

```text
English
```

When explaining browser navigation, write the explanation in English but include the relevant Spanish UI labels.

Example:

```text
Open Settings (Configuración).
```

---

# 3. Communication / Maintenance Preferences

The user is not working as a software developer and does not want instructions written as though advanced development knowledge is assumed.

Instructions should therefore be:

```text
explicit
sequential
concrete
copy/paste friendly
```

Do not merely say:

```text
update the filter
```

Instead say which file to open, what to replace, what to paste and how to test it.

When providing a changed repository file, provide the **entire replacement file**, not only a patch or fragment, unless the user explicitly requests otherwise.

The project may be discussed from a temporary ChatGPT conversation.

Therefore:

```text
Do not depend on downloadable artifacts.
Prefer complete copy/paste blocks in the conversation.
```

The user prefers conversation in English even though Vivaldi and Android are configured in Spanish.

---

# 4. Discreet Alias Codebook

The public-facing documentation uses neutral aliases for modesty.

The mapping below uses ROT13.

To decode, apply ROT13 again.

```text
A  = kivqrbf.pbz
A2 = kivqrbf2.pbz

B  = uragnvunira.kkk

R1 = whtnorg family
R2 = orgnab family
```

This is not intended as cryptographic protection.

It is only a lightweight way to avoid spelling everything out in ordinary prose.

The active filter file necessarily contains real hostnames because the blocker requires them.

---

# 5. Actual Goal

For A and B, remove or suppress as much of the following as Vivaldi's native Android blocker safely allows:

```text
page banners
advertising containers
short inserted material before normal playback
short inserted material after normal playback
click-triggered advertising tabs
popups
popunders
unexpected redirections
R1/R2 destination openings
```

At the same time, preserve:

```text
normal thumbnails
hover/touch previews where supported
normal video loading
play/pause
timeline / seek control
volume
fullscreen
player controls
ordinary navigation
```

A rule that blocks advertising but destroys these normal functions is not an acceptable solution.

---

# 6. Important Previous Failure

A previous active version contained rules equivalent to:

```text
*.mp4$media,third-party,domain=...
```

and a redirect variant of the same idea.

This was too broad.

Third-party media on A is not automatically advertising.

Broad MP4 blocking can affect:

```text
normal video
preview clips
thumbnail previews
player behavior
```

DO NOT REINTRODUCE generic MP4 blocking unless a very specific delivery host/path has first been proven to serve only the unwanted media.

This is the most important historical warning in the project.

---

# 7. Other Rules We No Longer Want as Baseline Rules

Do not casually restore broad cosmetic selectors such as:

```text
##[href*="base64"]
##[href*="data:"]
```

even if a large upstream list uses them.

They are wider than necessary for this small personal list and make diagnosis harder if the page changes.

Do not casually hide large player containers.

Do not assume a class containing words such as:

```text
player
video
media
container
```

is advertising.

---

# 8. Scriptlet Limitation

Some maintained upstream fixes for B use browser-extension scriptlets that manipulate JavaScript behavior.

Examples of the type of upstream technique involved include:

```text
prevent window.open
prevent a specific fetch
change an anti-adblock property
replace or disable an advertising JavaScript function
```

uBlock Origin and AdGuard can have capabilities that Vivaldi's native blocker does not reproduce identically.

Therefore:

```text
Do NOT respond to a surviving B popup by blocking all JavaScript.
Do NOT respond by blocking all media.
Do NOT respond by hiding the entire player.
```

Instead, first identify the narrowest network request, redirect host or cosmetic element that Vivaldi can actually handle.

It is possible for the Vivaldi-safe solution to be slightly less complete than an equivalent uBlock Origin solution.

Preserving normal playback has priority over pretending the list achieved 100% blocking.

---

# 9. Current Active Design

The 2026-08-11 baseline intentionally uses:

```text
narrow network rules
known advertising paths
known advertising domains
VAST-request blocking
popup rules
popunder rules
narrow cosmetic selectors
explicit R1/R2 destination blocking
```

It intentionally DOES NOT use:

```text
generic *.mp4 blocking
generic media blocking
uBlock ##+js(...) scriptlets
generic JavaScript blocking
broad player removal
broad base64/data selectors
```

---

# 10. Current Test Status

The 2026-08-11 rewrite is a **new conservative baseline**.

It has been constructed from:

```text
the previous project history
current repository contents
current upstream EasyList rules
current uBlock maintained rules
current AdGuard maintained rules
current Vivaldi blocker capabilities/documentation
```

However, the exact rewritten baseline has **not yet been confirmed on the user's physical phone**.

Do not mark the project as solved until it has been tested on the target device.

---

# 11. Required Test Matrix

After importing the current list, test A first.

Test:

```text
A-01  Home/list thumbnails appear.
A-02  Preview behavior still works.
A-03  A normal video opens.
A-04  Play/pause works.
A-05  Timeline/seek works.
A-06  Volume works.
A-07  Fullscreen works.
A-08  Short material before playback is absent or reduced.
A-09  Short material after playback is absent or reduced.
A-10  First ordinary click does not create an unwanted tab.
A-11  Repeated ordinary clicks do not create unwanted tabs.
A-12  R1/R2 destinations do not open.
```

Then test B:

```text
B-01  Page loads normally.
B-02  Video/player appears.
B-03  Play/pause works.
B-04  Timeline/seek works.
B-05  Fullscreen works.
B-06  Visible banners are absent or reduced.
B-07  First ordinary click does not create an unwanted tab.
B-08  Repeated ordinary clicks do not create unwanted tabs.
B-09  R1/R2 destinations do not open.
B-10  No anti-block loop prevents normal playback.
```

---

# 12. How to Report a Failure

Do not report only:

```text
it doesn't work
```

Prefer:

```text
Site: A or B
Page: home / search / video page
Action: exactly what was tapped
Expected: what should have happened
Actual: what happened
New tab opened: yes/no
Destination hostname if visible: ...
Video still plays: yes/no
Controls still work: yes/no
Thumbnails still work: yes/no
```

If possible, also report what Vivaldi shows after tapping the shield icon / content blocker icon.

A screenshot can be useful when a visual element survives.

---

# 13. Rule Escalation Policy

When something survives:

STEP 1:

```text
Identify exactly what survived.
```

STEP 2:

```text
Determine whether it is:
network request
popup/popunder
redirect
cosmetic element
media request
script-driven behavior
```

STEP 3:

```text
Prefer the smallest rule that affects only that behavior.
```

STEP 4:

Retest all normal player functions.

STEP 5:

Only keep the rule if normal functionality remains intact.

Never stack increasingly broad rules simply because the previous broad rule failed.

---

# 14. A-Specific Maintenance Notes

Current maintained upstream lists provide evidence for A rules involving concepts such as:

```text
zoneload
abck-banners
/vast.php?idzone=
/go.php?...&ad=
video-right
video-sponsor-links
thumb-ad
video-ad
popup/window-open behavior
```

The custom Vivaldi list uses the subset that can be expressed conservatively without depending on uBlock-only scriptlets.

If a popup survives, investigate its destination or initiating request before changing media rules.

If preview thumbnails disappear, immediately suspect a recently added media or cosmetic rule.

---

# 15. B-Specific Maintenance Notes

B is the more difficult target.

Maintained upstream lists currently use both ordinary filters and script-level fixes.

Relevant concepts include:

```text
a.adtng
havenclick
ads-twitter
pornhaven advertising assets
popecrazy
Script_Manager
window.open
anti-adblock/player detection
```

The Vivaldi list should use ordinary network/cosmetic rules where possible.

Do not assume every upstream uBlock/AdGuard scriptlet can simply be pasted into Vivaldi.

---

# 16. R1 / R2 Policy

R1 and R2 are unwanted redirect destinations.

The project currently takes the simple position that these destination families may be blocked completely.

If a new unwanted destination appears:

```text
record its hostname
confirm it is actually an unwanted redirect
add the exact domain or family
avoid blocking unrelated domains
```

---

# 17. Versioning Convention

For `mobile-filters.txt` use:

```text
YYYYMMDD.revision
```

Example:

```text
20260811.1
20260811.2
20260811.3
```

Increase the final number for another revision made on the same day.

Update:

```text
! Version:
! Last modified:
```

together.

---

# 18. Definition of Success

Success is NOT:

```text
the blocked counter is very high
```

Success is:

```text
the unwanted behavior is gone
AND
the desired page behavior still works
```

A filter list with 20 precise rules can be better than a filter list with 500 speculative rules.

---

# 19. Future Assistant Instruction

If a later ChatGPT session is asked to continue this project:

1. Read this file first.
2. Inspect the current `mobile-filters.txt` from the repository.
3. Check current upstream rules before assuming old domains/selectors remain valid.
4. Check current Vivaldi blocker capabilities if syntax support matters.
5. Do not restore broad MP4 rules.
6. Do not replace the user's entire working baseline with an aggressive experimental list without explaining the risk.
7. Give complete copy/paste file contents when repository files are changed.
8. Explain steps in English and include Spanish Vivaldi UI labels.
9. Assume the user wants practical guidance rather than developer shorthand.
10. Preserve normal playback, controls and previews as a hard requirement.

---

# 20. Immediate Next Step

Import the 2026-08-11 conservative baseline into Vivaldi Android and run the test matrix.

Do not add more rules until the baseline result is known.
