# Item pipeline

Item pipeline is a way to prcoess scraped file. Functions including:

- cleansing HTML data
- validating scraped data
- checking for duplicates (and dropping them)
- store the item in database

## Required methods:

a) `process_item(self, item, spider)`
Called for every item pipeline component. Return either: a dict with data, an `Item`, a Twisted Deferred or raise `DropItem` exception.

Parameter:
  -  item (`Item` object or a dict) – the item scraped
  -  spider (`Spider` object) – the spider which scraped the item


b) `open_spider(self, spider)`
Called when spider is opened.

c) `close_spider(self, spider)`
Called when spider is closed.

d) `from_crawler(cls, crawler)`
If present, this classmethod is called to create a pipeline instance from a Crawler. It must return a new instance of the pipeline. Crawler object provides access to all Scrapy core components like settings and signals; it is a way for pipeline to access them and hook its functionality into Scrapy.

## Define the pipeline

Add the related class to the `ITEM_PIPELINE` setting.