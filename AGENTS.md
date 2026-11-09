# Agent notes

This repository is the [blug.blog](https://blug.blog) site, not the blug CLI.

Follow [blug's agent notes](https://github.com/unix1/blug/blob/main/AGENTS.md) for generate, templates, URLs, preview, and deploy.

Site-specific:

- Write posts and pages in `public/<slug>/index.md`. Site text is `blug.yaml` and `public/index.md`. Presentation is `templates/` and `public/assets/`.
- Run `blug generate` and `blug serve` from this directory.
- Never run `blug deploy` unless the user explicitly confirms it in the current conversation.
- Leave published post markdown as written unless asked to change it.
- Do not add a local generator or copy the upstream README or setup guide into this repo.
