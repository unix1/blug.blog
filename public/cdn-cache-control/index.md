---
title: CDN cache control
date: 2026-09-07
---

[blug](https://github.com/unix1/blug) can now set `Cache-Control` on objects when you deploy. Cloudflare uses that header for how long the CDN keeps a copy at the edge.

Without it, R2 objects had no cache metadata. HTML stayed `DYNAMIC` and every request hit the bucket.

#### Set it in `.env`

Quote the value. Deploy sources `.env` as a shell file, so commas in an unquoted value get parsed as a command.

```
CACHE_CONTROL="public, max-age=3600"
```

Leave it empty to omit the header. `npm run deploy` passes it to rclone as `--header-upload Cache-Control: ...`, and R2 stores it as object metadata.

#### Re-upload after you change it

rclone skips objects that look unchanged. After you change `CACHE_CONTROL`, set this once, deploy, then turn it off:

```
CACHE_CONTROL_REUPLOAD=1
```

That adds `--ignore-times` so existing objects get the new header.

#### Cache HTML at the edge

`Cache-Control` on the object is not enough for HTML. Cloudflare does not cache HTML by default. Add a Cache Rule with **Eligible for cache** (the Cache everything template works) so those responses are not `DYNAMIC`.

Setup steps for the rewrite, cache rule, and R2 are in [SETUP.md](https://github.com/unix1/blug/blob/main/SETUP.md) on the blug project.
