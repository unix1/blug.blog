---
title: Static pages
date: 2026-09-26
---

[blug](https://github.com/unix1/blug) now supports static pages. Static pages are any page content that you don't want to appear in the list of posts. Examples are contents such as privacy policy, about, contact or anything you can think of.

Similar to posts, create a folder under `public/` with the slug you want, and put your content in `index.md` with `type: page` instead of a date. For example, `public/privacy-policy/index.md` could be:

```markdown
---
title: Privacy Policy
type: page
---

Your content here.
```

Then run `npm run generate`. The page will now be available at `/privacy-policy/`. Link to it from anywhere with `[privacy](/privacy-policy/)`.

For more info, see [Write a page](https://github.com/unix1/blug/blob/main/README.md#write-a-page) in the blug README.
