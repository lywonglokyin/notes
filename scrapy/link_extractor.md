# Link Extractor

Link extractor are objects that are responsible for extracting links in a page.

There are a built-in link extractor in `scrapy.linkextractors`. i.e.:

```python
from scrapy.linkextractors import LinkExtractor
```

## Notable Parameters

- allow: regular expression to match urls
- deny: regular expression to deny urls
