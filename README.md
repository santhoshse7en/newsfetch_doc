# newsfetch_doc

MkDocs documentation source (and built output) for [news-fetch](https://github.com/santhoshse7en/news-fetch), intended to be published via GitHub Pages.

## Layout

- `mkdocs.yml` — site config (`readthedocs` theme).
- `docs/` — source pages (Markdown).
- `site/` — built static site (output of `mkdocs build`).

## Building locally

```bash
pip install -r requirements.txt
mkdocs serve   # live preview at http://127.0.0.1:8000
mkdocs build   # regenerate site/
```

## Publishing

GitHub Pages serves a repo's default branch root by default. To publish:

1. Create the `santhoshse7en/newsfetch_doc` repo on GitHub (name must match exactly — this is what determines the `https://santhoshse7en.github.io/newsfetch_doc/` URL).
2. Push this repo to it.
3. In the GitHub repo's Settings → Pages, set the source to the default branch, root folder.
4. Point `pyproject.toml`'s `Documentation` URL and the README back in `news-fetch` at `https://santhoshse7en.github.io/newsfetch_doc/` once it's live — don't add that link back before then, or it'll 404 like the old one did.

If you'd rather serve straight from the repo root instead of `/site/`, copy `site/*` to the repo root as a deploy step (that's what determines what Pages actually shows).
