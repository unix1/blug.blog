---
title: CDN cache control
date: 2026-09-07
---

[blug](https://github.com/unix1/blug) can now set `Cache-Control` on objects when you deploy. Cloudflare uses that header for how long the CDN keeps a copy at the edge.

Without it, R2 objects had no cache metadata. HTML stayed `DYNAMIC` and every request hit the R2 bucket.

#### Set it in `.env`

Here's an example that caches for one hour:

```
CACHE_CONTROL="public, max-age=3600"
```

Quote the value. The deploy script sources `.env` as a shell file, so commas in an unquoted value get parsed as a command. Leave it empty to omit the header.

#### Re-upload after you change it

rclone skips objects that look unchanged. After you change `CACHE_CONTROL`, set this once, then turn it off:

```
CACHE_CONTROL_REUPLOAD=1
```

That forces rclone to re-upload all objects so it can set the new Cache-Control attributes.

#### Publish

`npm run deploy` generates the site and uploads `public/`. rclone sends `Cache-Control` as `--header-upload`, and R2 stores it as object metadata.

#### Cache HTML at the edge

`Cache-Control` on the object is not enough for HTML. Cloudflare does not cache HTML by default. Add a Cache Rule with **Eligible for cache** (the Cache everything template works) so those responses are not `DYNAMIC`.

Setup steps for the cache rule are in [SETUP.md](https://github.com/unix1/blug/blob/main/SETUP.md) on the blug project.
