---
title: Site titles and description
date: 2026-10-25
---

[blug](https://github.com/unix1/blug) now lets you set the listing page title, post and page title suffix, and meta description in `scripts/config.js`.

```javascript
export const SITE_HEADER = "blug";
export const SITE_TITLE = "blug - simple static blog generator";
export const SITE_SUBTITLE = "blug";
export const SITE_DESCRIPTION = "A small static blog generator.";
```

Let's demystify what they are:

* `SITE_HEADER` is the visible header that shows up at the top of each page
* `SITE_TITLE` is the home page title tag value.
* Posts and pages use `<entry title> - SITE_SUBTITLE` as their title tag value.
* `SITE_DESCRIPTION` fills the meta description tag.

For more info, see the [blug README](https://github.com/unix1/blug/blob/main/README.md).
