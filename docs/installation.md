# Installation

news-fetch requires **Python 3.10+**.

## From PyPI

```bash
pip install news-fetch
```

## From source

```bash
git clone https://github.com/santhoshse7en/news-fetch.git
cd news-fetch
pip install .
```

## Dependencies

news-fetch is intentionally lightweight (~50MB): [newspaper4k](https://pypi.org/project/newspaper4k/), [beautifulsoup4](https://pypi.org/project/beautifulsoup4/), [requests](https://pypi.org/project/requests/), [lxml-html-clean](https://pypi.org/project/lxml-html-clean/), and [Unidecode](https://pypi.org/project/Unidecode/). No optional extras, no Scrapy, no boto3, no Selenium.
