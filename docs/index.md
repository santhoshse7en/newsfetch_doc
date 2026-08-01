# news-fetch

[![PyPI version](https://img.shields.io/pypi/v/news-fetch.svg?style=flat-square)](https://pypi.org/project/news-fetch)
[![Python versions](https://img.shields.io/pypi/pyversions/news-fetch.svg?style=flat-square)](https://pypi.org/project/news-fetch)
[![License](https://img.shields.io/pypi/l/news-fetch.svg?style=flat-square)](https://pypi.python.org/pypi/news-fetch/)
[![GitHub stars](https://img.shields.io/github/stars/santhoshse7en/news-fetch.svg?style=flat-square)](https://github.com/santhoshse7en/news-fetch/stargazers)

**news-fetch** extracts structured data from a news article URL with one call: `Newspaper(url=...).get_dict`. Under the hood it's built on [newspaper4k](https://github.com/AndyTheFactory/newspaper4k), but it isn't just a wrapper around it — it exists to fix the gaps single-engine extraction leaves.

If this saves you time, [starring the repo](https://github.com/santhoshse7en/news-fetch) helps other people find it.

## Why news-fetch?

|  | `news-fetch` | Plain `newspaper4k` |
| --- | :---: | :---: |
| Publication / category / modified-date backfilled from JSON-LD when Open Graph tags are missing (e.g. BBC) | Yes | No |
| `summary` / `keywords` always populated, no NLTK corpus download needed | Yes | No (`.nlp()` requires one) |
| One flat dict, first non-empty value wins across engines | Yes | No (assemble it yourself) |
| Site-wide article discovery via sitemap/RSS, no browser automation | Yes | No |
| Concurrent batch scraping with per-URL failure isolation | Yes | No |
| Reading time & word count computed for free | Yes | No |
| Install footprint | ~50MB, no Scrapy/boto3/Selenium | — |

In detail:

* **JSON-LD backfill.** Many modern news sites (e.g. BBC) don't expose Open Graph tags that newspaper4k relies on for `publication`/`category`, but do embed a `schema.org/NewsArticle` JSON-LD block. news-fetch parses that block directly — including the common `{"@graph": [...]}` wrapper — and fills in `publication`, `category`, and `date_modify` whenever the primary engine comes up empty.
* **No NLTK download required.** newspaper4k's built-in summary extraction (`.nlp()`) needs an NLTK corpus download, which routinely fails behind corporate proxies or strict SSL setups. news-fetch falls back to a dependency-free, pure-stdlib summarizer/keyword extractor when that's unavailable, so `summary`/`keywords` are never empty.
* **`description` and `summary` are genuinely different fields** — `description` comes from the page's own meta description tag, `summary` is generated from the article text, with one falling back to the other only when needed.
* **Every field, one flat dict, first non-empty value wins.** Instead of learning three different libraries' inconsistent APIs, you get one object where each field is resolved by trying every available engine in priority order and returning the first real value.
* **Site-wide article discovery, no browser automation.** [`NewsSiteURLExtractor`](api-reference.md#newssiteurlextractor) finds a news site's recent article URLs (with title/date, when available) via its `robots.txt` sitemap directives, Google News sitemaps, and RSS/Atom feeds.
* **Reading time and word count**, computed for free from the extracted article text.
* **Concurrent batch scraping** via `Newspaper.from_urls([...])` — one bad URL doesn't take down the whole batch; it just comes back as `None`.
* **A minimal, honest dependency footprint.** The whole install is ~50MB: newspaper4k, beautifulsoup4, requests, lxml-html-clean, Unidecode — no Scrapy, no boto3, no Selenium.

## Project Links

| Source | Link |
| --- | --- |
| PyPI | [pypi.org/project/news-fetch](https://pypi.org/project/news-fetch/) |
| Repository | [github.com/santhoshse7en/news-fetch](https://github.com/santhoshse7en/news-fetch) |
| Issue tracker | [github.com/santhoshse7en/news-fetch/issues](https://github.com/santhoshse7en/news-fetch/issues) |

See [Installation](installation.md) to get started, or jump straight to [Usage](usage.md) for copy-pasteable examples.
