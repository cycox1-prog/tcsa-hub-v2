# TCSA Referral Hub — v2 (one-tap copy for a Facebook comment)

A copy of the original `tcsa-paste-test` hub, with one-tap copying added.
The original repo is untouched — this is a separate version to test.

A referral is a COMMENT reply on someone else's ask post, not a post
to the member's own Facebook page. The native share sheet cannot do
that — sharing to Facebook only makes a new post on your own timeline.
So v2 copies text + artwork to the clipboard, for pasting into a
comment in the Facebook app.

Same Supabase backend as the original (same `hub/config.js`), so the
same businesses, blurbs, referrals, members and alerts carry over.
Only the front-end in `hub/` changed.

## What changed — one-tap

1. **Copy text + image — for a Facebook comment, in 2 taps**
   - New `oneTapEl()` in `hub/app.js`.
   - Tested 2026-11-20 on iPhone: one clipboard item holding both
     text and image does NOT work — only the image pastes in
     Facebook. iOS keeps one thing on the clipboard at a time.
   - So Tap 1 copies the TEXT — paste it in the comment, do not
     submit yet. Tap 2 copies the IMAGE — paste it in the same
     comment, then submit.
   - The old Copy text / Copy image buttons are still there, under
     “Copy text / image separately instead”.

2. **Log it — 1 tap after commenting**
   - The Copy button remembers the business in session storage
     (`tcsa_pending`).
   - The “Log it” button goes to `#/log?business=<id>`.
   - The Log form now pre-fills from that deep link, and from
     `?business=<id>&post=<facebook url>` in the page URL.
   - So Muse (or anyone) can send a link like:
     `…/hub/?business=<business-id>&post=<facebook-post-url>`
     and the Log form opens with the business already chosen and
     the Facebook link already filled in. Paste / tap Log referral.
   - The pending entry is cleared after a successful log.

3. **Referral cards use the same one-tap block**
   - `#/r/<id>` now shows Copy text + image first, then
     Open Facebook post, then Mark done. Copy-separately is
     collapsed under the same details element.

4. **Cache busting**
   - `hub/index.html` now loads `styles.css?v=copy2`,
     `config.js?v=copy2`, `app.js?v=copy2`.

## What did NOT change

- Sign-in, Join the group, To do, Race, Stats, Admin, alerts,
  Slack webhook, race points, and the Supabase schema / functions.
- No new tables, no new Edge Functions, no new permissions.

## Deploy (GitHub Pages, same as the original)

1. Create a new GitHub repo, e.g. `tcsa-paste-test-v2`, and push
   this folder to its `main` branch.
2. In the new repo: Settings → Pages → deploy from `main`, root.
3. Open the new Pages URL, sign in with the same hub account.
   It talks to the same Supabase project, so the same data shows up.
4. Test on a phone: open a referral card, tap
   “Copy text + image — for a Facebook comment”, open the
   Facebook post in the Facebook app, tap Comment, paste,
   then tap “Log it” and confirm the business is pre-filled.

## Files changed vs the original

- `hub/app.js` — added `copyForComment` / `oneTapEl`, wired them into
  Log and referral-card views, added Log pre-fill, fixed the
  router to ignore `?…` after the hash, added `?business&post`
  handling in the boot redirect.
- `hub/index.html` — version strings `race2` → `copy2`.
- `hub/styles.css` — `.onetap` spacing for the big buttons.
- Everything else is an exact copy of the original.
