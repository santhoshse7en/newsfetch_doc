# API Reference

## `Newspaper`

```python
from newsfetch.news import Newspaper
```

### `Newspaper(url: str)`

Scrapes and extracts information from a single news article. Raises `ValueError` if the page can't be reached or parsed at all.

**Attributes**

| Attribute | Type | Description |
| --- | --- | --- |
| `url` | `str` | The URL passed in. |
| `headline` | `str \| None` | Article title. |
| `article` | `str \| None` | Cleaned article body text. |
| `authors` | `list[str]` | Author name(s). |
| `date_publish` | `str \| None` | Publication date/time. |
| `date_modify` | `str \| None` | Last-modified date/time, from JSON-LD. |
| `image_url` | `str \| None` | Top article image URL. |
| `language` | `str \| None` | Detected language code. |
| `publication` | `str \| None` | Publication / site name. |
| `category` | `str \| None` | Article category or section. |
| `keywords` | `list[str]` | Up to 10 keywords, from newspaper4k's own extraction or a stdlib fallback. |
| `summary` | `str \| None` | NLP-generated summary of the article. |
| `description` | `str \| None` | The page's own meta description tag; falls back to `summary` if absent. |
| `source_domain` | `str \| None` | Netloc of the article's source URL. |
| `source_favicon_url` | `str \| None` | Favicon URL for the source site. |
| `word_count` | `int` | Word count of `article`. |
| `reading_time_minutes` | `int` | Estimated reading time at 200 words/minute. |
| `get_dict` | `dict` | All of the above as a single flat dictionary (see below for exact keys). |

`get_dict` keys: `headline`, `author`, `date_publish`, `date_modify`, `language`, `image_url`, `description`, `publication`, `category`, `source_domain`, `source_favicon_url`, `article`, `summary`, `keyword`, `word_count`, `reading_time_minutes`, `url`.

Note that a few dict keys don't match their attribute name one-to-one — `author` maps to `authors`, and `keyword` maps to `keywords` — kept this way for backward compatibility with existing consumers of `get_dict`.

### `Newspaper.from_urls(urls: list[str], max_workers: int = 5) -> list[Newspaper | None]`

Classmethod. Scrapes multiple URLs concurrently with a `ThreadPoolExecutor`. Returns a list the same length and order as `urls`; any URL that fails (invalid page, network error, no extractable content, etc.) comes back as `None` in its place instead of raising, so one bad URL doesn't abort the whole batch.

## `NewsSiteURLExtractor`

```python
from newsfetch.discovery import NewsSiteURLExtractor
```

### `NewsSiteURLExtractor(news_domain: str, limit: int | None = 50)`

Discovers recent article URLs for a news site via its sitemaps and RSS/Atom feeds — `robots.txt` `Sitemap:` directives, Google News sitemaps, and homepage `<link>` autodiscovery — without any browser automation.

**Parameters**

- `news_domain` — the site's homepage URL, e.g. `"https://www.bbc.com"`.
- `limit` — maximum number of articles to return. Pass `None` for no limit; `0` returns an empty list.

**Attributes**

| Attribute | Type | Description |
| --- | --- | --- |
| `news_domain` | `str` | The homepage URL passed in (trailing slash stripped). |
| `articles` | `list[dict]` | Each entry has `url`, `title` (may be `None`), and `date` (may be `None`). |
| `urls` | `list[str]` | Just the URLs from `articles`, in the same order. |
