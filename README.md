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

## Reordering

Every category and every job has **▲ ▼** buttons. Categories move within the list,
jobs move within their category. The arrows grey out at the top and bottom, and the
order is exactly what customers see on the price guide.

## Hiding prices from customers

Every category and every job in the editor has a **Hide** tick.

- Tick **Hide all** on a category and the whole thing disappears for customers.
- Tick **Hide** on a single job and just that job goes, with its category still shown.
- A category with all its jobs hidden is left out entirely rather than shown empty.

Hidden things come back for you when the unlock code is typed into the Name and Phone
boxes on the price guide, or with `#edit` on the end of the address. Once revealed they
look and behave exactly like any other job — nothing on the page marks them out, so the
guide can be shown to a customer with the code entered.

In the file this is a `"hidden": true` flag on a category or a job. Older lists kept a
separate `hiddenCategories` array; the editor folds those into the main list on load, so
the next save clears the old format automatically.

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
