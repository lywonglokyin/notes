# Settings

Scrapy settings are important to define spider behaviour.

## Hieracy

1. Command line options (most precedence)
2. Settings per-spider
3. Project settings module
4. Default settings per-command
5. Default global settings (less precedence)

### Command line option

Specify settings using the `-s` flag. E.g.
```python
scrapy crawl spider1 -s LOG_FILE=scrapy.log
```

### Setting per spider

Specify settings as class attribute. E.g.
```python
class Spider1(scrapy.Spider):
  custom_setting = {
    'SOME_SETTING' : 'SOME_VALUE',
  }
```

### Project settings module

Specify setting at `setting.py`.

### Default settings per-command

Command settings are specified in the `default_settings` attribute of the command class.

### Default global settings

Located at `scrapy.settings.default_settings`.


## Important Settings

- 

