# Simple Manual for Updating My Vivaldi Filters

This file explains how to add or change filtering rules without needing technical knowledge.

## Important Files

The active filter file is:

```text
mobile-filters.txt
```

This is the file Vivaldi uses.

This manual is only for instructions:

```text
MANUAL.MD
```

Do not import `MANUAL.MD` into Vivaldi.

---

## How to Edit the Filter File

1. Open the GitHub repository.
2. Click `mobile-filters.txt`.
3. Click the pencil icon named **Edit this file**.
4. Add the new rule in the correct website section.
5. Click **Commit changes**.
6. Use a simple message such as:

```text
Update browser filters
```

7. Open the Raw version of `mobile-filters.txt`.
8. Confirm that the new rule appears there.
9. Reload or reimport the list in Vivaldi if the change does not appear automatically.

---

## How to Add a New Website Section

At the bottom of `mobile-filters.txt`, add:

```text
! ==================================================
! EXAMPLE.COM
! ==================================================
```

Replace `EXAMPLE.COM` with the website name.

Lines starting with `!` are comments. They do not block anything.

---

## Block an Advertising Domain

Use this when you know the domain that serves the advertisement:

```text
||ads-example.com^$third-party,domain=example.com
```

Replace:

* `ads-example.com` with the advertising domain.
* `example.com` with the website where the ad appears.

Example:

```text
||casino-ad-example.com^$third-party,domain=example.com
```

---

## Block Popups and New Advertising Tabs

Add these rules under the website section:

```text
*$popup,domain=example.com
*$popunder,domain=example.com
```

Replace `example.com` with the website name.

These rules try to stop advertising tabs that open after tapping or clicking.

---

## Block an Advertising Script

Use this when the advertisement comes from a known JavaScript file:

```text
||ads-example.com/path/ad-script.js$script,third-party,domain=example.com
```

Do not guess the path unless you know it is correct.

---

## Block an Advertising Video

Use a narrow rule that targets only the advertising server:

```text
||ads-example.com/path/*.mp4$media,third-party,domain=example.com
```

Do not use this broad rule unless absolutely necessary:

```text
*.mp4$media,domain=example.com
```

A broad MP4 rule may block the normal video too.

---

## Hide an Advertising Box

To hide an element with a CSS class:

```text
example.com##.advertisement
```

To hide an element with a CSS ID:

```text
example.com###advertisement-container
```

The first rule uses a class.

The second rule uses an ID.

---

## Disable a Rule Without Deleting It

Add `!` and a space at the beginning:

```text
! ||ads-example.com^$third-party,domain=example.com
```

The rule becomes a comment and stops working.

To enable it again, remove `! `.

---

## Safe Testing Method

Add only one or two rules at a time.

After every change:

1. Commit the change on GitHub.
2. Confirm it appears in the Raw file.
3. Close all tabs from the affected website.
4. Reopen the website.
5. Test several videos or pages.
6. Confirm that normal navigation and playback still work.

If something breaks, disable the newest rule by adding `! ` at the beginning.

---

## Reload the List in Vivaldi

When a change does not appear:

1. Open Vivaldi.
2. Open **Settings**.
3. Open **Tracker and Ad Blocking**.
4. Open **Sources**.
5. Find the custom source.
6. Disable and enable it again.

If that does not work:

1. Remove the custom source.
2. Close Vivaldi.
3. Reopen Vivaldi.
4. Import the same GitHub Raw URL again.

---

## Raw URL Format

The address normally looks like this:

```text
https://raw.githubusercontent.com/USERNAME/browser-rules/main/mobile-filters.txt
```

Do not use a GitHub address containing:

```text
/blob/
```

The correct address must contain:

```text
raw.githubusercontent.com
```

---

## Basic Safety Rules

* Keep all active rules in `mobile-filters.txt`.
* Keep instructions in `MANUAL.MD`.
* Add the narrowest rule possible.
* Restrict rules to the affected website with `domain=`.
* Do not block common video domains globally.
* Test after every change.
* Keep experimental rules disabled until needed.
* Do not place passwords, tokens, emails, or private information in the repository.
* Keep filenames and commit messages neutral.
  ::: 
