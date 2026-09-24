# square-callback

OAuth redirect page for **SquarePosSync**, the iMerchant → Square POS sync tool.

Live at: https://nicksalls-stack.github.io/square-callback/

## What it does

When a store owner authorizes SquarePosSync to access their Square account, Square sends them back to this page with a one-time authorization code in the URL. The page displays that code with a **Copy** button so it can be entered into SquarePosSync, which exchanges it for access tokens.

- If the owner clicks **Deny**, the page shows the error Square returned.
- Opened with no parameters, the page just says there is nothing to do.

## How authorization works

1. The store owner opens the authorize link:

   ```
   https://connect.squareup.com/oauth2/authorize?client_id=APP_ID&scope=ITEMS_READ+ITEMS_WRITE+INVENTORY_READ+INVENTORY_WRITE+ORDERS_READ+MERCHANT_PROFILE_READ&session=false&state=RANDOM
   ```

2. They sign in with their Square owner login and click **Allow**.
3. Square redirects to this page with `?code=...&state=...`.
4. The code is entered into `SquarePosSync.exe --authorize`, which exchanges it at `POST https://connect.squareup.com/oauth2/token`.

The code **expires 5 minutes** after it is issued, so complete step 4 promptly.

## Security

- This repo is public by design. The authorization code is single-use, short-lived, and cannot be exchanged without the application secret.
- **Never commit** the Square Application Secret, access tokens, or refresh tokens to this repo.
- The page uses no external scripts and stores nothing.

## Setup

- GitHub Pages: **Settings → Pages → Deploy from a branch → main / (root)**
- Square Developer Console: **App → Production → OAuth → Redirect URL** = `https://nicksalls-stack.github.io/square-callback/`

## Files

| File | Purpose |
|---|---|
| `index.html` | The callback page |
| `README.md` | This file |
