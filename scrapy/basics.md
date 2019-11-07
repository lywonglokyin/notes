# Scrapy basics

## Default structure of Scrapy

```
scrapy.cfg
myproject/
  __init__.py
  items.py
  middlewares.py
  pipelines.py
  settings.py
  spiders/
    __init__.py
    spider1.py
    spider2.py
```

## Basic command to run scrapy

`> scrapy startproject myproject [project_dir]`: Create a scrapy project.

`> scrapy crawl <scaper_name>` : Start the spider with specified name!

`> scrapy list` : List all available spiders.

`> scrapy fetch <url>` : Download the given URL. Useful to preview what content the spider would receive. Can specify a spider with `--spider=SPIDER` such that corresponding user agent is used. Or:

- `--headers`: Print the response header only
- `--no-redirect` : Do not follow HTTP 3xx redirect

`> scrapy parse <url> [options]` : Useful for running spider in specific pages

## Scrapy procedure

1. Generate request and use corresponding callback function.
2. Use *Selectors* to parse the page content.
3. Callback function returns dicts, `Item` objects, `Request` objects, or iterable of these.
4. Item returned will be persisted to a database in some *Pipeline* or written to a file usig feed exports.

## Spider class

Each scrapy spider is a subclass of `scrapy.Spider`. Check https://docs.scrapy.org/en/latest/topics/spiders.html. 

Notable attributes or functions:

`name` : Identifies the spider. Must be unique in project.

`start_requests()`: returns an iterable of `scrapy.Request`, which the spider will begin the crawl with. Will only be called once when the spider is opened.

`start_urls`: An alternative of `start_request()`. Specify the starting URL with list.

`allowed_domain`: Domains that the spider is allowed to crawl on.

`parse()`: Method to handle the response downloaded from the web

`logger`:


## More on `scrapy.Request`

## Recursive scrapping

One can invoke recursive scrape on links found in website using:

```python
next_page = <full url>
yield scrapy.Request(next_page, callback=self.parse)
```

Useful functions:

`response.urljoin`: Build a complete URL using the attribute of element, where `response` is the parameter object of `parse()`.

Another useful recursive scrapping, which can operate on partial URLs and `<a>` objects:

```python
next_page = <partial url>
response.follow(next_page, callback=self.parse)
```
 or
 ```python
 response.follow(a, callback=self.parse)
 ```

## Scrapy arguments

### Spider attributes

 ```
 scrapy crawl test_spider -o quotes.json -a tag=humor
 ```

 Attributes after `-a` tag is parse as the internal attributes of the spider object. In this case, `test_spider.tag = humor`.

 For example, if wish to define `start_url` through command line argument, will have to manually convert the parsed argument into list using `ast.literal_eval` or `json.loads`.

 Another useful use of attribute parsing is to pass the the `HttpAuthMiddleWare` and user-agent `UserAgentMiddleWare`. 
 ```python
 scrapy crawl myspider -a http_user=myuser -a http_pass=mypassword
 ```
 Another way to parse attrubute is through Scrapyd `schedule.json`.

## Selectors

Scrapy use either XPath or CSS to parse HTML elements. See xpath.md for more info.

Use `response.xpath(<your xpath>).get()` or `response.xpath(<your xpath>).getAll()` to get the corresponding element.

### Selector chaining

Sometimes it is easier to use CSS, but sometimes it is easier to use XPath. Why not both? 

E.g. XPath way to select element of some class is troublesome, but simple in CSS, we can chain them:
```python
ele.css('.shout').xpath('./time/@datetime').getall()
```

### Variable in XPath

Variable in XPath is allowed. E.g.:
```
response.xpath('//div[@id=$val]/a/text()', val='images').get()
```

### Namespace removal

It is useful when elements are obfuscated by namespaces (?). See https://docs.scrapy.org/en/latest/topics/selectors.html.



## Serialization

 Simplest way is to use -Item exporters-. 

(a) JSON

- `FEED_FORMAT` : `json`
-  Exporter: `JsonItemExporter`

(b) JSON lines

- `FEED_FORMAT` : `jsonlines`
- Exporter: `JsonLinesItemExporter`

(c) CSV

- `FEED_FORMAT`: `csv`
-  Exporter: `CsvItemExporter`
-  Specify columns to export and their order use `FEED_EXPORT_FIELDS`.

(d) XML

- `FEED_FORMAT`: `xml`
-  Exporter: `XmlItemExporter`

(e) Pickle

(f) Marshal

## Storage

Specified using URI through `FEED_URL` setting. Available storages:

- Local filesystem
- FTP
- S3
- Standard output

### Storage parameters

Specify special parameters when saving the file.

- `%(time)s` : replaced by timestamp when the feed is created
- `%(name)s` : replaced by spider name

E.g.:

FTP:
```
ftp://user:password@ftp.example.com/scraping/feeds/%(name)s/%(time)s.json
```

S3:
```
s3://mybucket/scraping/feeds/%(name)s/%(time)s.json
```

See https://docs.scrapy.org/en/latest/topics/feed-exports.html for more storage methods.

## Logging

Scrappy use Python's built-in logging system for event logging.

It is recommended to use a separate logger for each instance, common use:
```python
import logging
logger = logging.getLogger(__name__)
logger.warning('this is a warning.')
```

There is also a `logger` for each spider, accessed by `self.logger`.

Related logging settings can be seen at https://docs.scrapy.org/en/latest/topics/logging.html.

## Deploying spiders

- Scrapyd (open source)
- Scrapy Cloud (cloud-based)

## Middlewares

Useful to implement peripheral requests, e.g. cookies, header