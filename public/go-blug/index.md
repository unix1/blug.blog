---
title: blug is now written in Go
date: 2026-11-08
---

Go [blug](https://github.com/unix1/blug)!

blug is now an executable written in Go. Install it from [GitHub releases](https://github.com/unix1/blug/releases) or, if you use Go, with `go install github.com/unix1/blug@latest`.

No more unversioned npm scripts. No more template repos.

How does it work?

#### Commands

* `blug init` writes a starter site.
* `blug generate` writes the HTML.
* `blug serve` previews it locally.
* `blug deploy` publishes `public/` to S3-compatible storage such as Cloudflare R2.

That's it! See [README.md](https://github.com/unix1/blug/blob/main/README.md) for more details.

#### What changed

* Site settings live in `blug.yaml` instead of `scripts/config.js`.
* The home page intro is in `public/index.md`.
* Templates use Go `html/template` syntax, including the new `templates/article.html`.

### What stayed the same

Content creation and editing remains the same. Write markdown, then use blug to generate HTML.

* Posts and pages are still `public/<slug>/index.md`.
* Static pages are still `type: page` with no date.
* The `public/` directory still holds all content to be deployed to the destination website.

For more details on usage and hosting steps, see the [blug on GitHub](https://github.com/unix1/blug).
