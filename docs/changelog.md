# Changelog

All notable changes to this project are documented in this file.

## [0.4.1]

### Fixed
- `SoupHandler` now unwraps the `{"@context": ..., "@graph": [...]}` JSON-LD shape used by sites like BBC, and a bare top-level JSON-LD array — previously the JSON-LD fallback for `publication`, `category`, `date_publish`, `date_modify`, `authors`, and `publisher` silently returned nothing (or, for authors/publisher, missed data even on plain-array JSON-LD) on any site using these shapes.
- `ArticleHandler.date_publish` now also checks the nested `article.published_time` key that Open Graph article tags actually populate, matching how `category` already looked up `article.section`.
- `Newspaper.authors` no longer returns `None` when neither the article engine nor the JSON-LD fallback has authors; it now consistently returns `[]`, matching its declared type.
- `helpers.extract_keywords` (the stdlib fallback used when newspaper4k's own keyword extraction is empty) returned `(word, count)` tuples instead of plain strings, and returned every non-stopword in the article instead of a curated list. Now returns a capped, deduplicated `list[str]` of the top 10 keywords.
- `description` was always identical to `summary` — it now uses the article's actual meta description tag (`ArticleHandler.meta_description`), falling back to `summary` only when no meta description is present.
- `NewsSiteURLExtractor(..., limit=0)` was treated the same as `limit=None` (unlimited) because `0` is falsy in Python; it now correctly returns an empty list.
- Removed dead `max_keywords` parameter from `ArticleHandler.__process_keywords` (never called with a value).
- Deduplicated the identical `__safe_execute` helper that was copy-pasted across `ArticleHandler` and `SoupHandler` into a shared `newsfetch.helpers.safe_execute`.

### Changed
- `pyproject.toml`'s `Homepage` and `Documentation` project URLs pointed at a GitHub Pages site that returns 404; `Homepage` now points at the GitHub repository and the dead `Documentation` entry was removed.
- Bumped `newspaper4k` to `0.9.6` and `twine` (dev) to `7.0.0` in the pinned requirements files.
- Added `Typing :: Typed` classifier (the package ships `py.typed`).
- README: fixed a dependency-list omission (`lxml-html-clean`), a mislabeled "Repository" link, and stale sample output; added GitHub stats badges, a table of contents, and a feature comparison table.

## [0.4.0] and earlier

See the [GitHub release history](https://github.com/santhoshse7en/news-fetch/commits/master) — changelog tracking starts at 0.4.1.
