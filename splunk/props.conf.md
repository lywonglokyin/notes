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

