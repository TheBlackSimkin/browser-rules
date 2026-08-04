# Vivaldi Filter Manual for Complete Beginners

This manual explains, in plain language, how to understand and add rules to:

```text
mobile-filters.txt
```

The instructions file itself is:

```text
MANUAL.MD
```

Do not import `MANUAL.MD` into Vivaldi. Only import the Raw URL of `mobile-filters.txt`.

---

# 1. What a Web Page Is Made Of

A web page normally contains several different things:

1. **HTML elements**  
   Boxes, buttons, images, links, video players, banners, menus, and text.

2. **CSS styles**  
   Instructions that control how those elements look and where they appear.

3. **JavaScript files**  
   Programs that make the page interactive. They can also open popups, load ads, track clicks, or start video advertisements.

4. **Network requests**  
   Files downloaded by the page, such as images, scripts, videos, fonts, and advertising data.

Different filter rules block different parts.

For example:

- A cosmetic rule hides an advertising box.
- A script rule blocks a JavaScript file.
- A media rule blocks an advertising video.
- A popup rule tries to stop a new advertising tab.
- A domain rule blocks everything loaded from a known advertising server.

---

# 2. The Basic Shape of a Filter Rule

Example:

```text
||ads.example.net^$third-party,domain=website.com
```

Meaning:

```text
||ads.example.net^
```

Block requests to `ads.example.net`.

```text
$
```

Everything after `$` contains extra conditions.

```text
third-party
```

Only block it when it comes from another website.

```text
domain=website.com
```

Only apply the rule while visiting `website.com`.

In plain English:

> While I am visiting `website.com`, block third-party requests to `ads.example.net`.

---

# 3. Meaning of Common Symbols

## `!` — Comment or Disabled Rule

Example:

```text
! This is a note
```

Vivaldi ignores lines beginning with `!`.

You can also disable a rule without deleting it:

```text
! ||ads.example.net^
```

To enable it again, remove `! `.

---

## `||` — Match a Domain

Example:

```text
||ads.example.net^
```

This blocks requests to:

```text
ads.example.net
cdn.ads.example.net
video.ads.example.net
```

It does not mean “two vertical lines” in a mathematical sense. In filter syntax, it means to match the beginning of a hostname.

---

## `^` — End or Separator

Example:

```text
||ads.example.net^
```

The `^` tells the blocker that the domain name ends there, or that a separator follows.

It helps prevent accidental matches such as:

```text
ads.example.net.fake-site.com
```

---

## `$` — Start of Options

Example:

```text
||ads.example.net^$script,third-party,domain=website.com
```

Everything after `$` describes how and where the rule should apply.

---

## `domain=`

Example:

```text
domain=website.com
```

This limits the rule to one website.

Without `domain=`, a rule may affect every site you visit.

Safer:

```text
||ads.example.net^$third-party,domain=website.com
```

Riskier:

```text
||ads.example.net^
```

Use `domain=` whenever possible.

---

## `third-party`

This means the requested file comes from a different domain than the page you opened.

Example:

You open:

```text
website.com
```

The page loads an ad from:

```text
ads-company.net
```

That advertising request is third-party.

---

## `script`

Example:

```text
||ads.example.net/ad-loader.js$script,third-party,domain=website.com
```

This blocks a JavaScript file.

---

## `media`

Example:

```text
||video-ads.example.net/*.mp4$media,third-party,domain=website.com
```

This blocks a media file, such as an MP4 advertising video.

---

## `popup` and `popunder`

Example:

```text
*$popup,domain=website.com
*$popunder,domain=website.com
```

- A **popup** opens a new tab or window in front of the current page.
- A **popunder** opens a new tab or window behind the current page.

These rules tell the blocker to stop popup-style navigation originating from that website.

---

## `##` — Hide a Page Element

Example:

```text
website.com##.advertisement
```

Everything after `##` is a CSS selector.

This kind of rule does not stop a download. It hides a visible part of the page.

---

# 4. What Is a CSS Class?

A **class** is a reusable label attached to one or more page elements.

Example HTML:

```html
<div class="advertisement">
    Casino banner
</div>
```

The class name is:

```text
advertisement
```

In a filter rule, a class begins with a dot:

```text
.advertisement
```

Complete rule:

```text
website.com##.advertisement
```

Meaning:

> On `website.com`, hide every element with the class `advertisement`.

A page may use the same class on several elements.

Example:

```html
<div class="advertisement">Top banner</div>
<div class="advertisement">Side banner</div>
```

The rule:

```text
website.com##.advertisement
```

would hide both.

---

# 5. What Is a CSS ID?

An **ID** is usually a unique label for one specific element.

Example HTML:

```html
<div id="top-ad-banner">
    Advertising banner
</div>
```

The ID name is:

```text
top-ad-banner
```

In a CSS selector, an ID begins with `#`:

```text
#top-ad-banner
```

Complete filter rule:

```text
website.com###top-ad-banner
```

This looks like three `#` symbols because:

- The first two belong to the filter syntax: `##`
- The third belongs to the CSS ID selector: `#top-ad-banner`

It can be mentally separated like this:

```text
website.com##  #top-ad-banner
```

Without the space:

```text
website.com###top-ad-banner
```

Meaning:

> On `website.com`, hide the element whose ID is `top-ad-banner`.

---

# 6. Class Versus ID

Example HTML:

```html
<div id="main-ad" class="advertisement large-banner">
```

This element has:

- ID: `main-ad`
- Class: `advertisement`
- Another class: `large-banner`

Possible rules:

```text
website.com###main-ad
```

Hide the element by ID.

```text
website.com##.advertisement
```

Hide every element with the class `advertisement`.

```text
website.com##.large-banner
```

Hide every element with the class `large-banner`.

## Easy Way to Remember

- A class uses a dot: `.class-name`
- An ID uses a hash: `#id-name`

Examples:

```text
.advertisement
```

is a class.

```text
#top-ad
```

is an ID.

---

# 7. How to Find a Class or ID

This is much easier on a computer than on a phone.

Use Vivaldi Desktop or another Chromium-based desktop browser.

## Step-by-Step

1. Open the website.
2. Find the visible advertisement.
3. Right-click directly on the advertisement.
4. Click **Inspect**.
5. Developer Tools opens.
6. Look for the highlighted HTML line.

You may see something like:

```html
<div class="video-ad-container">
```

The class is:

```text
video-ad-container
```

Possible rule:

```text
website.com##.video-ad-container
```

Or you may see:

```html
<div id="preroll-ad">
```

The ID is:

```text
preroll-ad
```

Possible rule:

```text
website.com###preroll-ad
```

## Important Warning

Do not automatically block the first large container you see.

For example:

```html
<div id="video-player">
```

may contain both the normal video and the advertisement.

Blocking it could remove the entire player.

Prefer names that clearly suggest advertising, such as:

```text
ad
ads
advert
advertisement
banner
promo
sponsor
preroll
popup
casino
```

These names are clues, not guarantees.

---

# 8. How to Test a Cosmetic Rule Before Saving It

In Developer Tools:

1. Find the suspected HTML element.
2. Right-click the highlighted line.
3. Choose **Delete element**.

If only the advertisement disappears and the page still works, the element may be a good candidate.

This deletion is temporary. Reloading the page brings it back.

Then add the matching filter rule to `mobile-filters.txt`.

Example:

HTML:

```html
<div class="casino-popup-banner">
```

Rule:

```text
website.com##.casino-popup-banner
```

---

# 9. What Is an Advertising Script?

A script is usually a JavaScript file ending in:

```text
.js
```

Examples:

```text
ad-loader.js
popunder.js
vast-player.js
tracking.js
campaign.js
```

A script can:

- Open a new advertising tab.
- Detect clicks.
- Insert banners.
- Request a video advertisement.
- Redirect the browser.
- Track what the user does.
- Detect ad blockers.

Example rule:

```text
||ads.example.net/js/popunder.js$script,third-party,domain=website.com
```

Meaning:

> While visiting `website.com`, block the JavaScript file `popunder.js` loaded from `ads.example.net`.

---

# 10. How to Find a Suspicious Script

Use Vivaldi Desktop.

## Step-by-Step

1. Open the website.
2. Press `F12`, or right-click the page and choose **Inspect**.
3. Open the **Network** tab.
4. Reload the page.
5. Click the filter named **JS**.

Now you should see JavaScript files.

Look for suspicious names or domains such as:

```text
ad
ads
advert
banner
campaign
click
pop
popup
popunder
redirect
sponsor
track
vast
```

Example request:

```text
https://ads-company.net/scripts/popunder.js
```

Possible rule:

```text
||ads-company.net/scripts/popunder.js$script,third-party,domain=website.com
```

A broader version would be:

```text
||ads-company.net^$script,third-party,domain=website.com
```

The broader rule blocks every script from that advertising domain.

Start with the narrow rule whenever possible.

---

# 11. How to Know Whether a Script Is Really the Ad Script

A filename is only a clue.

Use this safer testing method:

1. In Developer Tools, open **Network**.
2. Find the suspicious script.
3. Right-click it.
4. Choose **Block request URL** or **Block request domain**, when available.
5. Reload the page.
6. Test the advertisement again.
7. Confirm that normal video playback and navigation still work.

If the popup disappears and everything else works, the script is probably related to the popup.

If the entire player breaks, undo the block and choose a narrower rule.

Developer Tools blocking is temporary and used only for testing.

---

# 12. What Is an Advertising Video Request?

A preroll advertisement is often a separate video file or video stream loaded before the normal video.

Possible formats include:

```text
.mp4
.m3u8
.ts
.webm
```

The website may also request an advertising playlist using terms such as:

```text
vast
vmap
preroll
adtag
adserver
campaign
zone
```

Examples:

```text
https://video-ads.example.net/library/ad-12345.mp4
```

```text
https://ads.example.net/vast.php?zone=123
```

The first is the actual advertising video.

The second may be an instruction telling the video player which advertisement to load.

---

# 13. Meaning of an Advertising Video Rule

Example:

```text
||video-ads.example.net/library/*.mp4$media,third-party,domain=website.com
```

Breakdown:

```text
||video-ads.example.net
```

Match the advertising video domain.

```text
/library/
```

Only match files inside that folder.

```text
*.mp4
```

Match any MP4 filename.

```text
$media
```

Only apply to media requests.

```text
third-party
```

Only when the file comes from another domain.

```text
domain=website.com
```

Only while visiting `website.com`.

In plain English:

> On `website.com`, block third-party MP4 media files loaded from the `/library/` folder of `video-ads.example.net`.

---

# 14. Why a Narrow Video Rule Is Safer

Good narrow rule:

```text
||video-ads.example.net/library/*.mp4$media,third-party,domain=website.com
```

This targets a known advertising server and folder.

Dangerous broad rule:

```text
*.mp4$media,domain=website.com
```

This blocks every MP4 file on the website.

That may include:

- The five-second advertisement.
- The normal full video.
- Video previews.
- Thumbnails.
- Background clips.

Use a broad MP4 rule only as a temporary experiment.

---

# 15. How to Find the Advertising Video Request

Use Vivaldi Desktop.

## Step-by-Step

1. Open Developer Tools with `F12`.
2. Open the **Network** tab.
3. Select the **Media** filter.
4. Clear the Network list.
5. Start opening a video.
6. Watch which requests appear during the five-second advertisement.
7. Note the domain, path, and file type.
8. Wait for the normal video to start.
9. Compare the requests used by the advertisement and the normal video.

Example:

Advertising request:

```text
https://video-ads.example.net/library/casino-123.mp4
```

Normal video:

```text
https://cdn.website.com/videos/main-video-456.mp4
```

A good rule would target only:

```text
video-ads.example.net
```

Possible rule:

```text
||video-ads.example.net/library/*.mp4$media,third-party,domain=website.com
```

Do not block:

```text
cdn.website.com
```

because that may be the normal video server.

---

# 16. How to Tell the Ad Video From the Normal Video

Look for several clues together.

## Timing

The ad request appears immediately before the five-second advertisement.

The normal video request appears when the actual content begins.

## Duration

The advertisement is usually very short.

The normal video is much longer.

## Domain

The advertisement may come from a different company or unfamiliar domain.

The normal video often comes from the website's own CDN.

## Filename or Path

Advertising paths may contain:

```text
ad
ads
campaign
creative
preroll
vast
zone
promo
```

Normal video paths may contain:

```text
video
content
media
stream
hls
cdn
```

These are only clues. Always test.

## Request Type

The advertising request may be:

```text
media
fetch
xhr
```

The VAST instruction is often an `xhr` or `fetch` request, while the actual advertisement may appear under `media`.

---

# 17. Blocking the Ad Instructions Instead of the Video

Sometimes it is better to block the VAST or advertising instruction request.

Example:

```text
/vast.php?zone=$domain=website.com
```

Meaning:

> On `website.com`, block a request whose URL contains `/vast.php?zone=`.

Another example:

```text
||ads.example.net/vast/$xhr,third-party,domain=website.com
```

This blocks an advertising instruction request classified as XHR.

Why this may be better:

- The player never receives the advertisement address.
- You avoid blocking normal video media.
- The rule can be narrower.

Why it may fail:

- The site may use several VAST addresses.
- The request path may change.
- The player may wait because it expected a response.

Always test normal playback.

---

# 18. What Is XHR or Fetch?

XHR and Fetch are ways JavaScript asks a server for information.

They are often used to request:

- Advertising instructions.
- Video metadata.
- Tracking information.
- Recommended content.
- Login information.
- Comments.

Example rule:

```text
||ads.example.net/vast.php$xhr,third-party,domain=website.com
```

Do not block all XHR requests from the main website. That can break the entire page.

---

# 19. How to Block a Popup or Casino Tab

Start with:

```text
*$popup,domain=website.com
*$popunder,domain=website.com
```

These rules target popup-style navigation.

If the casino tab still opens, the page may be using a script.

Use Developer Tools:

1. Open **Network**.
2. Select **JS**.
3. Reload the page.
4. Look for scripts with names containing `click`, `popup`, `popunder`, `redirect`, or `ad`.
5. Test blocking one suspicious script.
6. Confirm that normal links and video playback still work.

Example:

```text
https://ads.example.net/js/click-popunder.js
```

Possible rule:

```text
||ads.example.net/js/click-popunder.js$script,third-party,domain=website.com
```

You may also block the advertising domain directly:

```text
||ads.example.net^$third-party,domain=website.com
```

This is broader and may block scripts, images, media, and other requests from that domain.

---

# 20. How to Read a Network Request

Example URL:

```text
https://ads.example.net/videos/casino-987.mp4?campaign=123
```

Breakdown:

```text
https://
```

Connection type.

```text
ads.example.net
```

Domain or hostname.

```text
/videos/
```

Folder or path.

```text
casino-987.mp4
```

Filename.

```text
?campaign=123
```

Extra query information.

Possible narrow rule:

```text
||ads.example.net/videos/*.mp4$media,third-party,domain=website.com
```

Possible broader rule:

```text
||ads.example.net^$third-party,domain=website.com
```

Start narrow.

---

# 21. Example: Hide a Banner

Developer Tools shows:

```html
<div class="sponsored-banner">
```

Rule:

```text
website.com##.sponsored-banner
```

What it does:

- Hides the banner.
- Does not necessarily stop the ad from downloading.
- Usually has low risk.

---

# 22. Example: Hide One Unique Ad Box

Developer Tools shows:

```html
<div id="casino-ad-box">
```

Rule:

```text
website.com###casino-ad-box
```

What it does:

- Hides the element with that exact ID.
- Usually affects only one element.

---

# 23. Example: Block an Ad Script

Network shows:

```text
https://ads-company.net/assets/popunder.js
```

Rule:

```text
||ads-company.net/assets/popunder.js$script,third-party,domain=website.com
```

What it does:

- Blocks that specific JavaScript file.
- May stop a popup.
- Could break something if the script also controls normal page behavior.

Test carefully.

---

# 24. Example: Block an Advertising Video

Network shows:

```text
https://video-ads.example.net/library/casino-456.mp4
```

Rule:

```text
||video-ads.example.net/library/*.mp4$media,third-party,domain=website.com
```

What it does:

- Blocks MP4 files in that advertising folder.
- Does not block every MP4 on the website.
- Is safer than a global MP4 rule.

---

# 25. Example: Block a VAST Request

Network shows:

```text
https://ads.example.net/vast.php?idzone=12345
```

Possible rule:

```text
||ads.example.net/vast.php$xhr,third-party,domain=website.com
```

Another possible path-only rule:

```text
/vast.php?idzone=$domain=website.com
```

What it does:

- Blocks the ad instruction request.
- May prevent the player from discovering the preroll video.
- May need adjustment if the player waits or fails.

---

# 26. Example: Block a Whole Advertising Domain

You repeatedly see requests to:

```text
casino-ads-network.example
```

Rule:

```text
||casino-ads-network.example^$third-party,domain=website.com
```

What it does:

- Blocks scripts, images, media, and other third-party requests from that domain.
- Only applies while visiting `website.com`.

Use this when the entire domain appears to be advertising-only.

---

# 27. Which Rule Should I Try First?

Use this order:

1. **Visible banner only**  
   Try a cosmetic class or ID rule.

2. **New casino tab**  
   Try `$popup` and `$popunder`.

3. **Popup continues**  
   Find and block the popup script or advertising domain.

4. **Short video ad before content**  
   Find the VAST request or the advertising media domain.

5. **Nothing else works**  
   Try a broader rule temporarily, then narrow it.

Do not start by blocking every script or every video file.

---

# 28. Safe Testing Method

Add only one or two new rules at a time.

After each change:

1. Commit the file on GitHub.
2. Open the Raw URL.
3. Confirm that the new rule is visible.
4. Reload or reimport the source in Vivaldi.
5. Close all tabs from the affected website.
6. Reopen the website.
7. Test several pages or videos.
8. Confirm that normal playback still works.
9. Confirm that links, buttons, and menus still work.

If something breaks, disable the newest rule:

```text
! ||ads.example.net^$third-party,domain=website.com
```

Then commit and test again.

---

# 29. How to Organize a New Website Section

Add this near the bottom of `mobile-filters.txt`:

```text
! ==================================================
! WEBSITE.COM
! ==================================================

! Popup and popunder rules

! Advertising domains

! Advertising scripts

! Advertising video or VAST requests

! Cosmetic rules

! Experimental rules — disabled by default
```

Then add rules under the correct heading.

Example:

```text
! ==================================================
! WEBSITE.COM
! ==================================================

! Popup and popunder rules
*$popup,domain=website.com
*$popunder,domain=website.com

! Advertising domains
||ads-company.net^$third-party,domain=website.com

! Advertising scripts
||ads-company.net/js/popunder.js$script,third-party,domain=website.com

! Advertising video or VAST requests
||video-ads.example.net/library/*.mp4$media,third-party,domain=website.com

! Cosmetic rules
website.com##.sponsored-banner

! Experimental rules — disabled by default
! *.mp4$media,domain=website.com
```

---

# 30. Reloading the Source in Vivaldi

When a change does not appear:

1. Open Vivaldi.
2. Open **Settings**.
3. Open **Tracker and Ad Blocking**.
4. Open **Sources**.
5. Find your custom source.
6. Disable and enable it again.

If that fails:

1. Remove the custom source.
2. Fully close Vivaldi.
3. Reopen Vivaldi.
4. Import the same Raw GitHub URL again.

---

# 31. Raw URL Reminder

Correct style:

```text
https://raw.githubusercontent.com/USERNAME/browser-rules/main/mobile-filters.txt
```

Incorrect style:

```text
https://github.com/USERNAME/browser-rules/blob/main/mobile-filters.txt
```

The correct address contains:

```text
raw.githubusercontent.com
```

The incorrect address contains:

```text
/blob/
```

---

# 32. Information to Save When an Ad Escapes

When a new advertisement appears, write down:

- Website where it happened.
- Whether it was a banner, popup, popunder, redirect, or video ad.
- Exact destination domain of the new tab.
- Whether it happened on the first tap or later.
- Advertising request domain, if found.
- Script filename, if found.
- Video or VAST request URL, if found.
- Whether normal playback still worked.
- Vivaldi version and operating system.

Example report:

```text
Website: website.com
Problem: New casino tab and five-second preroll
Casino domain: casino-example.net
Trigger: First tap on Play
Suspicious script: https://ads-example.net/js/popunder.js
Suspicious media: https://video-ads.example.net/library/ad-123.mp4
Normal video after ad: Works
Device: Android
```

This information makes it much easier to create a safe rule.

---

# 33. Final Safety Rules

- Use `domain=` whenever possible.
- Prefer a specific file or folder over an entire domain.
- Prefer an advertising domain over blocking every MP4.
- Do not block the website's main video CDN unless you are certain.
- Do not block every JavaScript file.
- Test one change at a time.
- Keep failed experiments commented out.
- Keep backups of working versions.
- Never include passwords, access tokens, email addresses, or private data.
