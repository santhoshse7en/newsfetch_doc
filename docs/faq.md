# FAQ

### Do I need to download NLTK data first?

No. newspaper4k's own summary extraction (`.nlp()`) needs an NLTK corpus download, which routinely fails behind corporate proxies or strict SSL setups. news-fetch falls back to a dependency-free, pure-stdlib summarizer and keyword extractor whenever that's unavailable, so `summary`/`keywords` are populated either way.

### Why are `description` and `summary` sometimes identical?

`description` comes from the page's own `<meta name="description">` tag. Not every page has one — when it's missing, `description` falls back to the same value as `summary` (the NLP-generated summary of the article text).

### Why is `date_publish`, `category`, or `publication` sometimes `None`?

These are resolved by trying, in order: newspaper4k's own Open Graph parsing, then a JSON-LD `schema.org/NewsArticle` block on the page. If a site exposes neither for that field, it comes back `None` rather than a guess.

### Does this work on non-English news sites?

Yes, in the sense that nothing in news-fetch itself is English-specific — extraction relies on standard HTML metadata (Open Graph, JSON-LD) and newspaper4k's own text extraction, both of which are language-agnostic. Summary/keyword quality for the pure-stdlib fallback (sentence splitting, stopword filtering) is tuned for English text, though, and will be weaker on other languages.

### Is it okay to scrape any site with this?

`NewsSiteURLExtractor` discovers URLs via `robots.txt` `Sitemap:` directives and RSS/Atom feeds — the same mechanisms real news aggregators use — rather than scraping a search engine. It does not check a site's `Disallow` rules before fetching individual pages. You're responsible for respecting the target site's terms of service and adding your own rate limiting for large batches; `Newspaper.from_urls`'s `max_workers` controls concurrency but not request pacing.

### Why did `NewsSiteURLExtractor` return fewer articles than my `limit`?

It only returns what it actually found across the site's sitemaps/feeds (up to 8 feeds, deduplicated by URL). If the site exposes fewer articles than `limit` through those mechanisms, that's all you'll get.

### Something isn't working — where do I report it?

Open an issue on [GitHub](https://github.com/santhoshse7en/news-fetch/issues) with the URL you tried (if it's not paywalled/private) and what you expected vs. got.
