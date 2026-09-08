---
title: CDN cache control
date: 2026-09-07
---

[blug](https://github.com/unix1/blug) now allows you to set how long the objects will be cached on the CDN edge. It does so by setting the `Cache-Control` header on those objects when you deploy your site. Cloudflare uses that header to control the CDN caching behavior.

Without this header, R2 objects have no cache metadata, HTML would stay as a `DYNAMIC` resource and every request would go through to the R2 bucket.

#### Set it in `.env`

Here's an example in your `.env` configuration that caches objects for one hour:

```
CACHE_CONTROL="public, max-age=3600"
```

Remember to quote the value. To use the previous behavior (no caching), leave this value empty.

#### Re-upload after you change it

If you just set the above configuration, the setting will only apply to newly copied objects in R2. To change the setting on all objects, they must be re-uploaded. For this purpose, you can tell `blug` to force objects to be re-uploaded in `.env`:

```
CACHE_CONTROL_REUPLOAD=1
```

Set it before running `npm run deploy`. Don't forget to unset it (or change it to `0`) when done, so you don't unintentionally re-upload all objects with subsequent runs.

#### Publish

To make the change effective (either for newly copied objects or all) you must run

```
npm run deploy
```

In the background this makes `rclone` send the necessary headers to R2.

#### Cache HTML at the edge

One last note: `Cache-Control` on the object by itself is not enough for Cloudflare CDN to start caching HTML files. Cloudflare does not cache HTML by default. Add a Cache Rule with **Eligible for cache** (the Cache everything template works) so those responses are not treated as `DYNAMIC`.

If you (or your agent) previously followed the steps in [SETUP.md](https://github.com/unix1/blug/blob/main/SETUP.md) you should be all set.

Enjoy blazing low latency on your site worldwide and fewer requests to R2!
