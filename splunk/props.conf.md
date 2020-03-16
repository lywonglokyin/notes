# `props.conf`

## When is `props.conf` involved?

From the data pipeline described in https://docs.splunk.com/Documentation/Splunk/latest/Admin/Configurationparametersandthedatapipeline, `props.conf` will be used in (inexhausive):

1. Input phase
2. Structured parsing phase
3. Parsing phase
4. Indexing phase
5. Search phase

This file is commonly used for:

- Configuring line breaking for multi-line events
- Setting up character set encoding
- Allowing processing of binary files
- Configuring timestamp recognition.
- Configuring event segmentation.
- Overriding automated host and source type matching:
    - Conifgure advanced (regex) host and source type override
    - Override source type matching for data from a particular source
    - Set up rule-based source type recognition.
    - Rename source types
- Anonymizing certain types of sensitive incoming data
- Routing specific events to a particular index
- **Creating new index-time field extraction, including header-based field extractions**
- **Defining new search-time field extraction.** `transforms.conf` is required when one or more of the following is involved:
    - Reuse of the same field-extracting regex across multiple sources, source types, or hosts
    - **Application of more than one regex to the same source, source type, or host**
    - **Delimiter-based field extraction**
    - Extraction of multiple values for the same field
    - Extraction of field with names that begin with numbers or underscore
- Setting up lookup tables that look up fields from external sources.
- **Creating field aliases.**

## Stanza

### `[default]`

Use `[default]` stanza to define global settings.

### `[<spec>]`

This enable properties for a given `<spec>`, which could be:
1. `<sourcetype>`: the source type of an event.
2. `host::<host>`: where `<host>` is the host, or a host-matching pattern, for an event.
3. `source::<source>`: where `<source>` is the source, or a source-matching pattern, for an event.
4. `delayedrule::<rulename>`: Not covered here. Check `props.conf.spec` for details.

Settings that are matched by multiple categories, the precedence is: `sourcetype` < `host` < `source`.

Example of using "regex" for the stanza can be seen in `props.conf.spec`.

Problem of multiple-matched regex priority is not covered here and can also be seen in the `.spec` file.

## Setting/pair pairs

### General settings

---

#### `priority`

Used to deal with multiple-matched regex priorities.

---

#### `CHARSET`

Used to define the charset of input. Usually leaving it as default would be ok.

---

### Line breaking setting

---

#### `TRUNCATE`

Define the maximum line length, in byte. Usually leaving it as default (`10000`) would be ok.

---

#### `LINE_BREAKER`

Specifies a regex that defines how the raw text stream is broken into initial events, before linemerging takes place. Usually leaving it as default (event for each line, `([\r\n]+)`).

---

#### `LINE_BREAKER_LOOKBEHIND`

Specifies the number of bytes before the end of the raw data chunk to apply `LINE_BREAKER` regex. Increase only for large or multi-line events. Usually leaving it as deault (`100`).

---

#### `SHOULD_LINEMERGE`

Specifies whether to combine several lines of data into a single multi-line event. Default: `true`

If set to `true`, usually have to specifiy one or more of further settings below:

`BREAK_ONLY_BEFORE_DATE = <boolean>`

`BREAK_ONLY_BEFORE = <regular expression>`

`MUST_BREAK_AFTER = <regular expression>`

`MUST_NOT_BREAK_AFTER = <regular expression>`

`MUST_NOT_BREAK_BEFORE = <regular expression>`

`MAX_EVENTS = <integer>`

---

#### `EVENT_BREAKER_ENABLE`

This is a setting only related to *Universal Forwarders*. Default: `false`

- If true, then UF would split data wit a light-weight chunked line breaking processor, so data is distributed fairly evenly amongst multiple indexers.
- Else, UF uses standard load-balancing method to send events to indexers

---

#### `EVENT_BREAKER`

Specifies regex for multiline events. Only useful for multiline events.

---

### Timestamp extraction configuration

---

#### `DATETIME_CONFIG = [<filename relative to $SPLUNK_HOME> | CURRENT | NONE]`

Specifies which file configures the timestamp extractor, or

`NONE` to diable timestamp extractor and leave the event time set to whatever time was selected by the input layer, or

`CURRENT` to set it as the time it passed through the aggreator processor.

default: `/etc/datetime.xml`

---

#### `TIME_PREFIX`

Specifies a regex to scan a the event text before attempting to extract a timestamp.

default: empty string

---

#### `MAX_TIMESTAMP_LOOKAHEAD`

The number of characters into an event the software should look for a timestamp.

Default: `128`

---

#### `TIME_FORMAT`

Specifis a "strptime" format string to extract the date

Default: empty string

---

#### `TZ`

Specifies the timezone for the text. It follows the predecence (descending) of:

1. Timezone in the raw text (from `TIME_FORMAT`)
2. `TZ` value
3. Timezone provided by forwarder (for forwarder-indexer 6.0 or above)
4. The timezone of the system running splunkd.

---

#### `TZ_ALIAS`

Usually not useful. Is used to clarify ambiguity between timezone names with the same acronyms. E.g. EST can represent both Eastern US or Easter Australia timezone.

---

#### `MAX_DAYS_AGO`

Specifies the maximum number of days in the past, from current date provided by input layer (e.g. forwarder current time, modtime for files), that an extracted date can be valid.

Default: `2000`

---

#### `MAX_DAYS_HENCE`

Similar to `MAX_DAYS_AGO` but specifies days in the future.

Default: `2`

---

#### `MAX_DIFF_SECS_AGO`

Specifies the maximum difference allowed such that splunk could reject events with timestamps that are out of order.

Note that this setting should not be used for filtering, as splunk can only guarantee hour granuarity for this setting.

Default: `3600` (one hour)

---

#### `MAX_DIFF_SECS_HENCE`

Similar to `MAX_DIFF_SECS_AGO`, but in the future.

Default: `604800` (one week)

---

#### `ADD_EXTRA_TIME_FIELDS`

Specifies whether to drop specific info about the time. Usually default setting is fine.

Default: `true`

---

### Structured Data Header Extraction and configuration

Note that such index time extraction is not recommended. Use search time field extraction instead, which is mentioned in the next section.

---

#### `FIELD_DELIMITER`

This setting is appllied at input time, when data is first read by Splunk, such as on a frwarder that has configured inputs acquiring the data.

Note that only single character is allowed.

e.g.

`FIELD_DELIMITER=space`

- `space`
- `tab`
- `fs`: ASCII file separator
- `gs`: ASCII group separator
- `rs`: ASCII record separator
- `us`: ASCII unit separator
- `\xHH`: HH is two hexadecimal digits used as separator
- `none`: null termination character separator
- `whitespace`: any number of space and tabs as single delimiter

---

#### `INDEXED_EXTRACTIONS`

Specifies the type of file that splunk should expect, and also the extraction method used.

Note that you should only use this for input with guaranteed format.

---

#### `METRICS_PROTOCOL = <STATSD|COLLECTD_HTTP>`

#### `STATSD-DIM-TRANSFORMS = <statsd_dim_stanza_name1>,<statsd_dim_stanza_name2>..`

#### `METRIC-SCHEMA-TRANSFORMS = <metric-schema:stanza_name>[,<metric-schema:stanza_name>]...`

#### `PREAMBLE_REGEX = <regex>`

#### `FIELD_HEADER_REGEX = <regex>`

#### `HEADER_FIELD_LINE_NUMBER = <integer>`

#### `FIELD_DELIMITER = <character>`

#### `HEADER_FIELD_DELIMITER = <character>`

#### `HEADER_FIELD_ACCEPTABLE_SPECIAL_CHARACTERS = <string>`

#### `FIELD_QUOTE = <character>`

#### `HEADER_FIELD_QUOTE = <character>`

#### `TIMESTAMP_FIELDS = [ <string>,..., <string>]`

#### `FIELD_NAMES = [ <string>,..., <string>]`

#### `MISSING_VALUE_REGEX = <regex>`

#### `JSON_TRIM_BRACES_IN_ARRAY_NAMES = <boolean>`

---

### Field extraction configuration

There are two main point to be concerned in this part:

1. Whether indexed fields or extracted fields are created
2. Whether they include a reference to an additional component called "field transform", which is separately defined in `transforms.conf`.

For (1), in general, it is always better to create extracted field (search-time extraction) to minimize performance impact during indexing. In specific circumstances it would be good to include it as indexed fields.

For (2), field transforms contain a field-extracting regular expression, and some settings that govern the way the transform extracts fields.

There are 3 method of extraction, they are:

`EXTRACT`: which creates extracted field, and does not require field transforms (inline).

`TRANSFORMS`: which creates indexed field, and require field transforms.

`REPORT`: which creates extracted field, and require field transforms.

#### When to use `REPORT` over `EXTRACT`

1. You wish reuse the regex for multiple sources.
2. Applying multiple regex to same source, which have two or more very different event patterns.
3. Set up delimiter-based field extraction, useful if data presents field-value pairs.
4. Extraction of multi-valued fields.
5. Extraction of field beginning with numbers or underscores.
6. Manage formatting of extracted fields, when extracting both the field name and values.

---

#### `TRANSFORMS-<class> = <transform_stanza_name>, <transform_stanza_name2>,...`

#### `REPORT-<class> = <transform_stanza_name>, <transform_stanza_name2>,...`

The required format for `TRANSFORM` and `REPORT`. e.g.:

```
[source::color_logs]
TRANSFORMS-colorchange = yellow, blue, red
```

this sequence ensures that the `[yellow]` transform stanza gets
  applied first, then `[blue]`, and then `[red]`

---

`EXTRACT-<class> = [<regex>|<regex> in <src_field>]`

- Used to create extracted fields (search-time field extractions) that do
  not reference transforms.conf stanzas.
- `<class>` is a unique literal string that identifies the namespace of the
  field you're extracting.
- 