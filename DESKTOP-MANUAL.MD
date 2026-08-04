# Vivaldi Desktop Filter Manual

This manual explains how to install, use, test, troubleshoot, and maintain:

```text
desktop-filters.txt
```

This manual is intended for:

- Vivaldi Desktop on Windows, Linux, or macOS
- Vivaldi 7.6 or newer
- The built-in Vivaldi Tracker and Ad Blocker
- The filter list in this repository

The filter file is:

```text
desktop-filters.txt
```

The manual file is:

```text
DESKTOP-MANUAL.MD
```

> Do not import `DESKTOP-MANUAL.MD` into Vivaldi.  
> Import only the Raw GitHub URL of `desktop-filters.txt`.

---

# 1. What This Desktop Filter List Does

The list contains rules for:

- Blocking advertising network requests
- Blocking advertising scripts
- Blocking VAST and preroll-ad requests
- Blocking popup and popunder tabs
- Hiding visible advertising containers
- Blocking selected tracking requests
- Completely blocking Jugabet domains
- Preserving normal video playback whenever possible

The current list contains sections for:

- XVideos
- HentaiHaven
- Pornhub
- Jugabet

The desktop list is separate from:

```text
mobile-filters.txt
```

Do not replace the mobile list with the desktop list unless you specifically want the desktop-compatible behavior on that device.

---

# 2. Requirements

Use:

```text
Vivaldi Desktop 7.6 or newer
```

Vivaldi 7.6 introduced improved support for popup-blocking filter rules.

To check your Vivaldi version:

1. Open the Vivaldi menu.
2. Select **Help**.
3. Select **About**.
4. Check the version number.
5. Update Vivaldi when necessary.

---

# 3. Raw Filter URL

Use this address when importing the list:

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/refs/heads/main/desktop-filters.txt
```

The address must contain:

```text
raw.githubusercontent.com
```

Do not import the normal GitHub page address:

```text
https://github.com/TheBlackSimkin/browser-rules/blob/main/desktop-filters.txt
```

The normal GitHub page contains GitHub’s interface and is not the raw filter file.

---

# 4. Installing the List in Vivaldi Desktop

## Step 1 — Open Settings

Press:

```text
Ctrl + F12
```

On macOS, open Vivaldi Settings from the application menu.

## Step 2 — Open the Blocker Settings

Go to:

```text
Privacy and Security
→ Tracker and Ad Blocking
```

## Step 3 — Select the Blocking Level

Choose:

```text
Block Trackers and Ads
```

The custom ad-blocking list will not be fully useful when ad blocking is disabled.

## Step 4 — Open the Sources

Click:

```text
Manage Sources
```

Find:

```text
Ad Blocking Sources
```

## Step 5 — Add the List

Click:

```text
Add List
```

Paste:

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/refs/heads/main/desktop-filters.txt
```

## Step 6 — Select the Import Options

Use these settings:

```text
[ ] Allow the list to use ABP snippet rules
[x] Treat simple host names as pure host-name rules
[ ] Allow document exception rules to load all resources
```

In Spanish, the options may appear similar to:

```text
[ ] Permitir que la lista use las reglas para snippets de ABP
[x] Tratar los nombres de host simples como una regla de nombre de host puro
[ ] Permitir reglas de excepción de documento para cargar todos los recursos
```

Only the middle option should be checked.

## Step 7 — Import

Click:

```text
Import
```

Confirm that the list appears under the ad-blocking sources and that its checkbox is enabled.

## Step 8 — Reload Websites

Close and reopen affected tabs, or press:

```text
Ctrl + Shift + R
```

This performs a stronger page reload.

---

# 5. Confirming That the List Is Active

Open one of the supported websites.

Click the shield icon beside the address field.

Confirm that the selected level is:

```text
Block Trackers and Ads
```

The shield panel can also show the number of requests blocked on the current page.

Remember:

- Network rules stop requests.
- Cosmetic rules hide page elements.
- Popup rules target unwanted tabs or windows.
- A blocked counter does not always show every hidden cosmetic element.

---

# 6. What the XVideos Rules Do

The XVideos section blocks several types of advertising behavior.

## First-Party Advertising Paths

Examples:

```text
||xvideos.com/zoneload/$domain=xvideos.com
||xvideos.com/picserror/$domain=xvideos.com
||xvideos.com/lvcsm/abck-banners/
```

These rules block known advertising or tracking paths hosted on the site itself.

## Third-Party Advertising Provider

Examples:

```text
||orbsrv.com^$third-party,domain=xvideos.com
||a.orbsrv.com/ad-provider.js$script,third-party,domain=xvideos.com
```

These rules block advertising resources loaded from Orbsrv while visiting XVideos.

## VAST Preroll Request

```text
/vast.php?idzone=$domain=xvideos.com
```

A VAST request can tell the player which advertisement to play.

Blocking the VAST request may prevent the player from receiving the advertisement address.

## Third-Party MP4 Rule

```text
*.mp4$media,third-party,domain=xvideos.com
```

This blocks third-party MP4 media while visiting XVideos.

This rule is effective against some preroll videos, but it is aggressive.

It may also block:

- Legitimate externally hosted videos
- Video previews
- Alternate video servers
- Some normal playback files

If normal videos stop working, disable this rule by adding `! ` before it:

```text
! *.mp4$media,third-party,domain=xvideos.com
```

## Popup and Popunder Rules

```text
*$popup,domain=xvideos.com|xvideos2.com
*$popunder,domain=xvideos.com|xvideos2.com
```

These rules target tabs or windows opened as popup-style advertising navigation.

## Cosmetic Rules

Examples:

```text
xvideos.com###video-sponsor-links
xvideos.com###ad-footer
xvideos.com##.thumb-ad
xvideos.com##.video-ad
```

These rules hide visible advertising containers.

They do not necessarily stop the resource from downloading unless a network rule blocks it too.

---

# 7. What the HentaiHaven Rules Do

The HentaiHaven section blocks known external advertising and redirect services.

Examples:

```text
||a.adtng.com^$third-party,domain=hentaihaven.xxx
||havenclick.com^$third-party,domain=hentaihaven.xxx
||ads-twitter.com^$third-party,domain=hentaihaven.xxx
```

These rules apply only while visiting:

```text
hentaihaven.xxx
```

The VAST or advertising-delivery rule is:

```text
||havenclick.com/www/delivery/$xhr,domain=hentaihaven.xxx
```

This targets advertising instructions requested through XHR.

Popup protection is provided by:

```text
*$popup,domain=hentaihaven.xxx
*$popunder,domain=hentaihaven.xxx
```

Cosmetic rules hide advertising frames, player-ad containers, overlays, and promotional blocks.

Examples:

```text
hentaihaven.xxx##iframe[src^="https://a.adtng.com/"]
hentaihaven.xxx##.script_manager_video_master
hentaihaven.xxx##.popecrazy
```

---

# 8. What the Pornhub Rules Do

The Pornhub section uses conservative rules intended to avoid breaking normal videos.

## Advertising Networks

```text
||trafficjunky.net^$third-party,domain=pornhub.com|pornhub.org|pornhub.net
||trafficjunky.com^$third-party,domain=pornhub.com|pornhub.org|pornhub.net
```

These rules block TrafficJunky requests only while visiting the listed Pornhub domains.

## Category Tracking

Examples:

```text
||pornhub.com/video/category_tracker?e=$domain=pornhub.com|pornhub.org|pornhub.net
```

These rules block matching first-party category-tracking requests.

## Popup and Popunder Protection

```text
*$popup,domain=pornhub.com|pornhub.org|pornhub.net
*$popunder,domain=pornhub.com|pornhub.org|pornhub.net
```

## Cosmetic Rules

Examples:

```text
pornhub.com,pornhub.org,pornhub.net##a[href*="trafficjunky"]
pornhub.com,pornhub.org,pornhub.net##iframe[src*="trafficjunky"]
pornhub.com,pornhub.org,pornhub.net##.adContainer
```

These rules hide advertising links, frames, and containers.

## Important Playback Protection

Do not broadly block:

```text
phncdn.com
```

Do not add a generic rule such as:

```text
*.mp4$media,domain=pornhub.com
```

Those resources may be used for:

- Normal video playback
- Video previews
- Thumbnails
- Player resources

The Pornhub section intentionally avoids broad video-file blocking.

---

# 9. What the Jugabet Rules Do

The Jugabet section is a complete domain block.

It does not merely hide advertisements.

These rules block connections to the domains and their subdomains:

```text
||jugabet.com^
||jugabet.cl^
||jugabet.mx^
```

These rules block direct document navigation:

```text
||jugabet.com^$document
||jugabet.cl^$document
||jugabet.mx^$document
```

These rules target popup and popunder tabs leading to the domains:

```text
||jugabet.com^$popup
||jugabet.com^$popunder
```

Equivalent rules are included for `.cl` and `.mx`.

---

# 10. Basic Filter-Rule Syntax

## `!` — Comment or Disabled Rule

```text
! This is a comment.
```

Vivaldi ignores lines beginning with `!`.

A rule can be disabled without deleting it:

```text
! ||ads.example.com^
```

Remove `! ` to enable it again.

---

## `||` — Match a Hostname

```text
||ads.example.com^
```

This matches the hostname and its subdomains.

It can match:

```text
ads.example.com
cdn.ads.example.com
video.ads.example.com
```

---

## `^` — Hostname End or Separator

```text
||ads.example.com^
```

The `^` prevents accidental matches against unrelated hostnames such as:

```text
ads.example.com.fake-domain.net
```

---

## `$` — Start of Rule Options

```text
||ads.example.com^$script,third-party,domain=website.com
```

Everything after `$` adds conditions to the rule.

---

## `domain=`

```text
domain=website.com
```

This limits the rule to a particular website.

Safer:

```text
||ads.example.com^$third-party,domain=website.com
```

Broader:

```text
||ads.example.com^
```

Use `domain=` whenever a global block is unnecessary.

---

## Multiple Domains

Vivaldi-style network rules separate domains with `|`:

```text
domain=website.com|website.net|website.org
```

Cosmetic rules separate domains with commas:

```text
website.com,website.net,website.org##.advertisement
```

Do not mix the two formats.

---

## `third-party`

```text
||ads.example.net^$third-party,domain=website.com
```

This applies when the requested resource comes from a different domain than the page being visited.

---

## `script`

```text
||ads.example.net/ad-loader.js$script,third-party,domain=website.com
```

This blocks a JavaScript resource.

---

## `media`

```text
||video-ads.example.net/*.mp4$media,third-party,domain=website.com
```

This blocks matching media requests.

Use media rules carefully because normal video playback can use the same file formats as advertising.

---

## `xhr`

```text
||ads.example.net/vast/$xhr,third-party,domain=website.com
```

This blocks matching XMLHttpRequest-style requests.

These requests may contain:

- Advertising instructions
- Tracking data
- Video metadata
- Page data
- Login data

Do not block all XHR requests from a website.

---

## `popup` and `popunder`

```text
*$popup,domain=website.com
*$popunder,domain=website.com
```

These rules target popup-style navigation originating from the specified site.

---

## `document`

```text
||blocked-site.example^$document
```

This blocks navigation to the page itself.

Use it when the intention is to block the entire website.

---

## `##` — Cosmetic Element Hiding

```text
website.com##.advertisement
```

Everything after `##` is a CSS selector.

This rule hides elements with the class:

```text
advertisement
```

---

## `###` — Hide an Element by ID

```text
website.com###top-ad
```

This looks like three `#` characters because:

```text
website.com##  #top-ad
```

The first two are the filter syntax.

The third belongs to the CSS ID selector.

---

# 11. CSS Classes and IDs

Example HTML with a class:

```html
<div class="advertisement">
    Advertising banner
</div>
```

The corresponding rule is:

```text
website.com##.advertisement
```

Example HTML with an ID:

```html
<div id="top-ad">
    Advertising banner
</div>
```

The corresponding rule is:

```text
website.com###top-ad
```

Easy reminder:

```text
.class-name
#id-name
```

A class uses a dot.

An ID uses a hash.

---

# 12. Finding an Advertising Element on Desktop

Desktop Vivaldi includes Developer Tools.

## Inspecting an Element

1. Open the website.
2. Find the visible advertisement.
3. Right-click directly on it.
4. Select **Inspect**.
5. Developer Tools will open.
6. Look at the highlighted HTML element.

Example:

```html
<div class="video-ad-container">
```

Possible rule:

```text
website.com##.video-ad-container
```

Another example:

```html
<div id="preroll-ad">
```

Possible rule:

```text
website.com###preroll-ad
```

Do not automatically block the first large container.

For example:

```html
<div id="video-player">
```

may contain both the advertisement and the real video.

Hiding it could remove the entire player.

---

# 13. Testing a Cosmetic Rule

Before permanently adding a cosmetic rule:

1. Open Developer Tools.
2. Inspect the suspected element.
3. Right-click the highlighted HTML line.
4. Select **Delete element**.
5. Check what disappeared.
6. Test the rest of the page.

The deletion is temporary.

Reloading the page restores the element.

A good cosmetic candidate removes only the unwanted content.

A bad candidate may remove:

- The video player
- Navigation controls
- Login buttons
- Comments
- Download controls
- The complete page layout

---

# 14. Using the Network Panel

Open Developer Tools with:

```text
F12
```

Select:

```text
Network
```

Reload the page.

The Network panel shows files and requests loaded by the website.

Useful filters include:

```text
JS
Media
Fetch/XHR
Img
Doc
```

You can also type text into the Network search field, such as:

```text
ad
ads
advert
banner
campaign
click
popup
popunder
preroll
redirect
sponsor
track
vast
vmap
zone
```

These words are clues, not proof.

Always test a suspected request before adding a permanent rule.

---

# 15. Finding an Advertising Script

In Developer Tools:

1. Open **Network**.
2. Select **JS**.
3. Reload the page.
4. Look for suspicious files or domains.
5. Test one candidate at a time.

Example request:

```text
https://ads-company.example/scripts/popunder.js
```

Possible narrow rule:

```text
||ads-company.example/scripts/popunder.js$script,third-party,domain=website.com
```

Possible broader rule:

```text
||ads-company.example^$script,third-party,domain=website.com
```

Start with the narrow rule.

A broad script rule may block several unrelated site features.

---

# 16. Finding a Preroll Video Request

In Developer Tools:

1. Open **Network**.
2. Select **Media**.
3. Clear the current Network log.
4. Start opening a video.
5. Observe requests appearing during the advertisement.
6. Wait for the normal video to begin.
7. Compare the advertising and normal-video requests.

Example advertising request:

```text
https://video-ads.example/library/casino-123.mp4
```

Example normal video:

```text
https://cdn.website.com/videos/main-video-456.mp4
```

A narrow rule could be:

```text
||video-ads.example/library/*.mp4$media,third-party,domain=website.com
```

Do not block the normal content CDN.

---

# 17. Blocking VAST Instead of Video Files

A player may first request an advertising instruction document.

Common terms include:

```text
vast
vmap
adtag
adserver
campaign
preroll
zone
```

Example:

```text
https://ads.example/vast.php?zone=123
```

Possible rule:

```text
/vast.php?zone=$domain=website.com
```

Another example:

```text
||ads.example/vast/$xhr,third-party,domain=website.com
```

Blocking the advertising instruction is often safer than blocking every MP4 file.

However, the player may wait or show an error if it expects a response.

Always test normal playback.

---

# 18. Finding a Popup or Popunder Source

Start with:

```text
*$popup,domain=website.com
*$popunder,domain=website.com
```

When an unwanted tab still opens:

1. Open Developer Tools.
2. Select **Network**.
3. Select **JS**.
4. Reload the page.
5. Look for click, redirect, popup, or advertising scripts.
6. Test blocking one candidate.
7. Confirm that normal links still work.

Also record the destination domain of the unwanted tab.

A destination block may look like:

```text
||unwanted-casino.example^$popup
||unwanted-casino.example^$popunder
```

A complete destination block may look like:

```text
||unwanted-casino.example^
```

Use a complete domain block only when you never want that domain to load.

---

# 19. Unsupported Rule Types

This desktop list intentionally avoids uBlock Origin-specific rules such as:

```text
example.com##+js(...)
```

It also avoids:

```text
redirect-rule=
```

Examples that should not be added to this Vivaldi desktop list:

```text
example.com##+js(nowoif)
*.mp4$redirect-rule=noopmp4-1s
```

Vivaldi may reject or ignore such rules.

Use compatible alternatives:

- Normal network blocking
- Popup and popunder rules
- Cosmetic element hiding
- Narrow script blocking
- Narrow media blocking
- VAST or XHR blocking

ABP snippet rules use a different syntax, but this list currently does not use them.

For that reason, the ABP snippet import option remains disabled.

---

# 20. Adding a New Website Section

Use this template near the bottom of `desktop-filters.txt`:

```text
! =====================================================
! WEBSITE.COM — ads, tracking, popups and clutter
! =====================================================

! Known advertising domains

! Advertising scripts

! VAST, XHR or advertising-delivery requests

! Popup and popunder protection
*$popup,domain=website.com
*$popunder,domain=website.com

! Cosmetic element-hiding rules

! Experimental rules — disabled by default
```

Example:

```text
! =====================================================
! WEBSITE.COM — ads, tracking, popups and clutter
! =====================================================

! Known advertising domains
||ads-company.example^$third-party,domain=website.com

! Advertising scripts
||ads-company.example/js/popunder.js$script,third-party,domain=website.com

! VAST or advertising-delivery requests
||ads-company.example/vast/$xhr,third-party,domain=website.com

! Popup and popunder protection
*$popup,domain=website.com
*$popunder,domain=website.com

! Cosmetic rules
website.com##.sponsored-banner
website.com###preroll-ad

! Experimental broad media rule — disabled
! *.mp4$media,third-party,domain=website.com
```

---

# 21. Safe Rule-Development Order

When adding protection for a new website, use this order:

1. Hide a specific visible ad container.
2. Block a specific known advertising hostname.
3. Block a specific advertising script.
4. Block a specific VAST or advertising-delivery request.
5. Add popup and popunder rules.
6. Block a narrow advertising-media path.
7. Use broad MP4 or media rules only as a last resort.

This order reduces the chance of breaking the website.

---

# 22. Updating the GitHub Filter

After editing `desktop-filters.txt`:

1. Increase the version number.
2. Update the modification date.
3. Commit the changes to the `main` branch.
4. Confirm that the Raw URL displays the new contents.
5. Reload or reopen Vivaldi.
6. Reload the affected website.

Example header:

```text
! Version: 20260804.2
! Last modified: 2026-08-04
```

A useful version format is:

```text
YYYYMMDD.REVISION
```

Examples:

```text
20260804.1
20260804.2
20260805.1
```

---

# 23. Making Vivaldi Retrieve an Updated List

Vivaldi periodically retrieves URL-based lists.

When a recent GitHub edit does not appear immediately:

1. Confirm that the change exists in the Raw GitHub file.
2. Fully close Vivaldi.
3. Reopen Vivaldi.
4. Reload the affected website.
5. Check that the custom source remains enabled.

When it still appears stale:

1. Open **Settings**.
2. Open **Privacy and Security**.
3. Open **Tracker and Ad Blocking**.
4. Select **Manage Sources**.
5. Remove the custom desktop source.
6. Add the same Raw URL again.
7. Use the same import options.
8. Reload the website.

Raw URL:

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/refs/heads/main/desktop-filters.txt
```

---

# 24. Troubleshooting: A Video Does Not Play

First, temporarily disable the custom source.

If playback returns, one of the custom rules is responsible.

For XVideos, test this rule first:

```text
*.mp4$media,third-party,domain=xvideos.com
```

Disable it:

```text
! *.mp4$media,third-party,domain=xvideos.com
```

Commit the change, reload the source, and test again.

Do not immediately disable all cosmetic rules because cosmetic rules normally do not block video downloads.

Also inspect blocked requests for:

```text
media
m3u8
mp4
webm
xhr
fetch
```

---

# 25. Troubleshooting: The Website Layout Is Broken

A cosmetic rule may be hiding a container that includes legitimate content.

Temporarily disable cosmetic rules one at a time by adding `! `.

Example:

```text
! website.com##.suspected-container
```

Reload after each change.

Look especially for broad selectors such as:

```text
website.com##[class*="ad"]
website.com##div[class]
website.com##iframe
```

Broad cosmetic selectors can hide unrelated elements.

Prefer specific selectors.

---

# 26. Troubleshooting: A Popup Still Opens

Check all of the following:

```text
*$popup,domain=website.com
*$popunder,domain=website.com
```

Also check Vivaldi’s normal website permission:

1. Click the site-information icon beside the address.
2. Open site settings.
3. Find **Pop-ups and redirects**.
4. Confirm that they are blocked.

Then inspect:

- The destination hostname
- Click-handler scripts
- Redirect scripts
- Advertising network requests
- Newly opened tab addresses

The tab may be created by normal JavaScript rather than a request that clearly contains the word `popup`.

---

# 27. Troubleshooting: Rules Were Not Admitted

When Vivaldi reports that rules were not admitted, search for unsupported syntax.

Common causes include:

```text
##+js(
redirect-rule=
redirect=
removeparam=
```

Some advanced modifiers may belong to another blocker engine.

Also check for:

- Missing commas
- Incorrect domain separators
- Invalid CSS selectors
- Accidental spaces inside modifiers
- Copied typographic quotation marks
- Broken line wrapping

For this desktop list:

- Network-rule domains use `|`
- Cosmetic-rule domains use `,`
- ABP snippet rules are not used
- uBlock scriptlet rules are not used
- Redirect replacement rules are not used

---

# 28. Temporarily Disabling Protection for One Website

When a page does not work correctly:

1. Open the page.
2. Click the shield icon beside the address field.
3. Change the site to **No Blocking**.
4. Reload the page.
5. Test the missing feature.

If the feature works with blocking disabled, a filter rule or another blocking source may be responsible.

Restore:

```text
Block Trackers and Ads
```

after testing.

---

# 29. Testing Rules Without Breaking the Main List

Before adding an aggressive rule:

1. Copy the filter list to a temporary local file.
2. Add the experimental rule there.
3. Import the local file as a temporary source.
4. Disable the normal GitHub source while testing.
5. Test several pages and several videos.
6. Remove the temporary source afterward.

This avoids publishing an untested rule to every Vivaldi installation using the GitHub list.

---

# 30. Information to Record When an Ad Escapes

Record:

- Website and exact page
- Date and time
- Vivaldi version
- Operating system
- Type of advertisement
- Banner, overlay, preroll, popup, popunder, or redirect
- Destination hostname of an unwanted tab
- Advertising request URL
- Advertising script URL
- VAST or XHR request URL
- Media file URL
- Whether it occurred on the first click
- Whether normal playback still worked
- Which rule stopped it during testing

Do not record:

- Passwords
- Session cookies
- Authorization headers
- Account tokens
- Private messages
- Personal identifying information

Network logs can contain sensitive information.

---

# 31. Rule Safety Checklist

Before committing a new rule, confirm:

- The advertisement disappears.
- Normal video playback still works.
- Normal links still work.
- Login still works.
- Search still works.
- Page navigation still works.
- The rule is limited with `domain=` where possible.
- The rule does not globally block a common CDN.
- The rule does not block all scripts.
- The rule does not block all XHR requests.
- The rule does not block all MP4 files unless necessary.
- The rule uses syntax accepted by Vivaldi.
- The version and date were updated.

---

# 32. Quick Reference

## Block a Domain Everywhere

```text
||ads.example.com^
```

## Block a Third-Party Domain on One Site

```text
||ads.example.com^$third-party,domain=website.com
```

## Block a Script

```text
||ads.example.com/ad.js$script,third-party,domain=website.com
```

## Block an XHR Advertising Request

```text
||ads.example.com/vast/$xhr,third-party,domain=website.com
```

## Block Narrow Media

```text
||video-ads.example.com/path/*.mp4$media,third-party,domain=website.com
```

## Block Popups and Popunders

```text
*$popup,domain=website.com
*$popunder,domain=website.com
```

## Hide a Class

```text
website.com##.advertisement
```

## Hide an ID

```text
website.com###advertisement
```

## Hide a Matching Link

```text
website.com##a[href*="advertiser"]
```

## Block a Complete Website

```text
||blocked-site.example^
||blocked-site.example^$document
||blocked-site.example^$popup
||blocked-site.example^$popunder
```

## Disable a Rule

```text
! ||ads.example.com^
```

---

# 33. Current Repository Layout

Recommended repository structure:

```text
browser-rules/
├── README.md
├── MANUAL.MD
├── DESKTOP-MANUAL.MD
├── mobile-filters.txt
└── desktop-filters.txt
```

Suggested purpose:

```text
MANUAL.MD
```

Mobile-filter documentation.

```text
DESKTOP-MANUAL.MD
```

Desktop-filter documentation.

```text
mobile-filters.txt
```

Vivaldi Mobile filter source.

```text
desktop-filters.txt
```

Vivaldi Desktop filter source.

---

# 34. Official References

Vivaldi Desktop Tracker and Ad Blocker help:

```text
https://help.vivaldi.com/desktop/privacy/tracking-and-ad-blocking/
```

Vivaldi Desktop 7.6 popup-rule announcement:

```text
https://vivaldi.com/blog/vivaldi-on-desktop-7-6/
```

Repository:

```text
https://github.com/TheBlackSimkin/browser-rules
```

Desktop Raw filter:

```text
https://raw.githubusercontent.com/TheBlackSimkin/browser-rules/refs/heads/main/desktop-filters.txt
```

---

# 35. Final Reminder

Import this:

```text
desktop-filters.txt
```

Do not import this:

```text
DESKTOP-MANUAL.MD
```

Use these import options:

```text
[ ] Allow the list to use ABP snippet rules
[x] Treat simple host names as pure host-name rules
[ ] Allow document exception rules to load all resources
```

Keep aggressive media rules narrow and test normal playback after every change.
