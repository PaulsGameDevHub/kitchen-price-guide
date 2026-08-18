# Kitchen Price Guide

A price guide and estimate builder for kitchen supplementary work.

**Live:** https://paulsgamedevhub.github.io/kitchen-price-guide/

A customer opens the link, ticks the jobs they need with quantities, and sees a
running total with a VAT breakdown. They can save it as a PDF, send the estimate
straight through by email, or copy it as plain text.

## How it's built

One file — `index.html`. Vanilla HTML, CSS and JavaScript. No framework, no build
step, no dependencies. Fonts come from Google Fonts; everything else is inline.
No browser storage is used.

## Changing the prices

**Easiest — the price editor:**
<https://paulsgamedevhub.github.io/kitchen-price-guide/edit.html>

Change prices, add or remove jobs and categories, press **Save to website**. It
commits the change to this repo for you; the live page follows about a minute
later. It reads the current prices with no key needed — a key is only required to
save. See "The editor's GitHub key" below.

The editor has two tabs:

- **Customer prices** (`CONFIG.categories`) — everyone sees these.
- **Hidden prices** (`CONFIG.hiddenCategories`) — these appear on the price guide
  only after the unlock code is typed into the Name and Phone boxes, or with
  `#edit` on the end of the address. Once revealed they behave like any other
  job: tickable, counted in the total, and included on the PDF and the email.

The unlock code lives in `CONFIG.editUnlock`. It is obfuscation, not security —
anyone reading the page source can find both the code and the hidden prices.

Prices live in a single `CONFIG` block near the top of `index.html`, between the
`/* === PRICE LIST START === */` and `/* === PRICE LIST END === */` markers. The
editor swaps that block and leaves the rest of the file untouched.

Other ways, if ever needed:

- **Directly on GitHub.** Open `index.html`, click the pencil icon, edit the price
  block, commit.

From this machine:

```powershell
git -C "D:\GamesFromAi\KitchenPriceGuide" add -A
git -C "D:\GamesFromAi\KitchenPriceGuide" commit -m "Update prices"
git -C "D:\GamesFromAi\KitchenPriceGuide" push origin main
```

## The editor's GitHub key

`edit.html` writes to this repo through the GitHub API using a fine-grained personal
access token, scoped to **this repository only** with **Contents: Read and write**.

The token is never in the source. It is typed in by hand and kept in that device's
browser storage under `kpg-github-key`. The editor page is publicly reachable, but
without a token it can only read what is already public. If a device is lost, delete
the token on GitHub and the page becomes read-only again immediately.

## Sending estimates

The send button posts to [Web3Forms](https://web3forms.com/), so estimates arrive
by email without needing a server. The access key is in `CONFIG.formKey` and the
destination address in `CONFIG.ownerEmail`. If the key is ever removed, the button
falls back to opening a `mailto:` link instead.
