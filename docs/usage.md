# Usage

## Scrape a single article

```python
from newsfetch.news import Newspaper

news = Newspaper(url='https://www.thehindu.com/news/cities/Madurai/aa-plays-a-pivotal-role-in-helping-people-escape-from-the-grip-of-alcoholism/article67716206.ece')
print(news.headline)
# Output: 'AA plays a pivotal role in helping people escape from the grip of alcoholism'
print(news.word_count, news.reading_time_minutes)
```

`news.get_dict` returns every extracted field as a single flat dictionary — see [API Reference](api-reference.md#newspaper) for the full field list.

## Discover article URLs from a news site

No browser automation — this uses the same sitemap/RSS discovery mechanisms real news aggregators rely on.

```python
from newsfetch.discovery import NewsSiteURLExtractor

site = NewsSiteURLExtractor(news_domain='https://www.bbc.com', limit=10)
for article in site.articles:
    print(article)
# Output: {'url': 'https://www.bbc.com/news/articles/...', 'title': '...', 'date': '2026-07-10T16:56:53Z'}
```

Pass `limit=None` to fetch every discovered article instead of capping the result count.

## Scrape many URLs concurrently

Failures are isolated per URL — one bad page doesn't abort the batch.

```python
from newsfetch.news import Newspaper

results = Newspaper.from_urls(site.urls, max_workers=5)
for result in results:
    if result is not None:
        print(result.headline)
```
