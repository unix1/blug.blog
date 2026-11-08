# blug.blog

The site at [blug.blog](https://blug.blog), generated with [blug](https://github.com/unix1/blug).

Posts and pages are markdown folders under `public/`. `blug generate` turns them into HTML.

## Layout

```
blug.yaml            # header, titles, description, footer, listing heading
.env                 # S3 credentials for rclone (not committed)
templates/           # header.html, footer.html, listing.html, article.html
public/
  index.md           # home page intro
  index.html         # generated listing
  assets/style.css
  hello-world/
    index.md         # you write this
    index.html       # generated post
    photo.jpg        # optional media
```

## Write a post

Create `public/<slug>/index.md`:

```markdown
---
title: Hello World
date: 2026-08-30
---

Text. Media next to this file: [photo](./photo.jpg).
```

`title` and `date` are required. Keep media in the same directory and use relative links.

## Write a page

Pages use the same folder layout as posts, but with `type: page` and no date. They are not listed on the home page.

Create `public/<slug>/index.md`:

```markdown
---
title: Privacy Policy
type: page
---

Your content here.
```

`title` and `type: page` are required. Link to pages the same way as posts (`/privacy-policy/`).

Edit `blug.yaml` for the site header, listing page title (`site_title`), post/page title suffix (`site_subtitle`), meta description (`site_description`), footer, and listing heading. The home intro is `public/index.md` (markdown shown above the post list). Posts and pages use `<title> - site_subtitle` as the document title. Leave `public/index.md` empty to omit the intro.

## Commands

Install `blug` from [GitHub Releases](https://github.com/unix1/blug/releases) or with `go install github.com/unix1/blug@latest`. See the [blug README](https://github.com/unix1/blug/blob/main/README.md) and [SETUP.md](https://github.com/unix1/blug/blob/main/SETUP.md).

```bash
blug generate   # write listing, post, and page HTML
blug serve      # serve public/ (port 3000, or another free port)
blug deploy     # generate, then rclone sync public/ to destination
```

## Hosting

This supports S3-compatible hosting, such as Cloudflare R2. Put `S3_*` values in `.env` (see `.env.example`). `blug deploy` passes those to rclone; no `rclone.conf` is needed. Set `CACHE_CONTROL` in `.env` to control Cache-Control on uploaded objects.

R2 serves exact object keys. It does not map `/hello-world/` to `hello-world/index.html`. Add one **URL Rewrite** on the domain (Rules → Overview → URL Rewrite Rule):

- If URI Path ends with `/`
- Rewrite path dynamically to `concat(http.request.uri.path, "index.html")`

That also covers `/` → `/index.html`.

Post links on the listing include a trailing slash (`hello-world/`) so the browser treats the page as a directory and relative media links resolve. The rewrite is internal: the address bar stays `/hello-world/`.

## License

[MIT](LICENSE)
