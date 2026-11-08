# Agent notes

This repo is the blug.blog site. Generate, preview, and deploy with the Go [blug](https://github.com/unix1/blug) CLI. Do not add a Node generator, templating languages besides the existing `html/template` files, `<base>` tags, a custom HTTP server, or recursive post discovery unless asked.

## Layout

- `public/` is the site root and the rclone/R2 upload tree.
- Posts and pages are one level deep: `public/<slug>/index.md` plus any media. Skip `public/assets/`.
- Pages use frontmatter `type: page` (no `date`). They share the same slug/URL layout as posts but are excluded from the home listing and render without a `<time>`.
- `blug.yaml` holds `site_header`, `site_title`, `site_subtitle`, `site_description`, `site_footer`, and `listing_heading`.
- `public/index.md` is the home intro (markdown above the post list). It is required. An empty file omits the intro.
- `templates/header.html` and `templates/footer.html` wrap every page. Fields are `{{.Title}}`, `{{.Description}}`, `{{.SiteHeader}}`, and `{{.SiteFooter}}`.
- `templates/listing.html` is the home page body. Fields are `{{.Body}}`, `{{.ListingHeading}}`, and `{{range .Posts}}` with `{{.Date}}`, `{{.Slug}}`, and `{{.Title}}`.
- `templates/article.html` is the post/page body. Fields are `{{.EntryTitle}}`, `{{.Date}}`, and `{{.Body}}`. Empty `{{.Date}}` omits `<time>` on pages.
- `.env` holds S3 provider, region, endpoint, access keys, bucket, and optional `CACHE_CONTROL`. Generate does not read it. Deploy does.

## Generate

`blug generate` (run in this directory):

1. Scan `public/*/` for `index.md`.
2. If `type: page`, require YAML frontmatter `title` only. Otherwise require `title` and `date`.
3. Convert markdown and write `public/<slug>/index.html`. Posts include a `<time>`; pages do not. Document titles are `<entry title> - site_subtitle`. Skip the write when the file is already up to date.
4. Write `public/index.html` from `public/index.md` plus posts only (newest date first), using `site_title` as the document title.

Leave media files untouched. Markdown relative links (`./photo.jpg`) must stay relative in the HTML.

## URLs

- Listing links use a trailing slash: `hello-world/`, not `hello-world` and not `/hello-world/`.
- Site chrome uses root-absolute paths (`/`, `/assets/style.css`).
- Do not introduce a blog-base config. Posts live at the site root until someone moves them on purpose.

## Hosting (R2)

R2 does not serve `index.html` for directory paths. Production needs a Cloudflare **URL Rewrite** on the zone (proxied custom domain, not `r2.dev`): paths ending in `/` rewrite to `concat(http.request.uri.path, "index.html")`. That includes `/`.

Without the trailing slash in the browser URL, `./photo.jpg` resolves to `/photo.jpg`. Do not "fix" that with `<base href>`.

`blug deploy` runs generate, then rclone sync of `public/` using R2 credentials from `.env` (flags, not `rclone.conf`). The rclone script is embedded in the `blug` binary. If `CACHE_CONTROL` is set, rclone passes `--header-upload Cache-Control: ...` so R2 stores it as object metadata. Set `CACHE_CONTROL_REUPLOAD=1` once after changing it so unchanged objects are rewritten. HTML and extensionless URLs also need a Cloudflare Cache Rule with Eligible for cache; otherwise they stay `DYNAMIC` and every request hits R2.

Setup steps: [SETUP.md](https://github.com/unix1/blug/blob/main/SETUP.md).

## Preview

`blug serve` serves `public/`. It binds `:3000` first; if that port is taken it tries another port. It always prints the URL.
