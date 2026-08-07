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

Prices live in a single `CONFIG` block near the top of `index.html`, between the
`/* === PRICE LIST START === */` and `/* === PRICE LIST END === */` markers.

Two ways to change them:

1. **In the browser.** Edit prices, job names, units and categories on the page,
   then use the export button to produce a replacement for that block. Paste it
   over the old block and commit.
2. **Directly on GitHub.** Open `index.html`, click the pencil icon, edit, commit.
   The live page updates about a minute later.

From this machine:

```powershell
git -C "D:\GamesFromAi\KitchenPriceGuide" add -A
git -C "D:\GamesFromAi\KitchenPriceGuide" commit -m "Update prices"
git -C "D:\GamesFromAi\KitchenPriceGuide" push origin main
```

## Sending estimates

The send button posts to [Web3Forms](https://web3forms.com/), so estimates arrive
by email without needing a server. The access key is in `CONFIG.formKey` and the
destination address in `CONFIG.ownerEmail`. If the key is ever removed, the button
falls back to opening a `mailto:` link instead.
