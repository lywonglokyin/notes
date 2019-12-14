# Splunk config files

## Editing the config files directly

### Default files

The default directory contains preconfigured versions of config files, located in `$SPLUNK_HOME/etc/system/default`.

Note that changes should **NOT** be directly applied to the default config files. On upgrade, splunk would overwrite these files, therefore any changes is lost. Changes should be make in:

1. `$SPLUNK_HOME/etc/system/local`
2. `$SPLUNK_HOME/etc/apps/<app_name>/local`

where the changes would persist through upgrades

### Changing default values

Changing default values is easy, by creating a new version of the file in a non-default directory and modifying the values there.

Note that you should **ONLY** add the attributes you need to change. Do **NOT** copy the whole attribute file then apply changes.

As mentioned, changes should be made in the two places specified. More specifically:

`$SPLUNK_HOME/etc/system/local`: Local changes on a site-wide basis. For example, settings that is avaiable to all app.

---

`$SPLUNK_HOME/etc/slave-app/[_cluster|<app_name>]/[local|default]`: For cluster peer nodes only. The subdirectory under `$SPLUNK_HOME/etc/slave-apps` contains config files that are common across all peer nodes.

Note that you should **ONLY** change these files in the cluster master. Do **NOT** edit the cluster peer itself.

`_cluster` directory would contains config files that are not part of the real app but still need to be identical across all peers.

---

`$SPLUNK_HOME/etc/apps/<app_name>/[local|default]`: When config only need to be applied to a specific app, the config file is put here.

e.g. To edit serach-time setting for the Search app: `$SPLUNK_HOME/etc/apps/search/local/`

---

`$SPLUNK_HOME/etc/users`: Contains user-specific config changes.

---

`$SPLUNK_HOME/etc/system/README`: Contains supporting reference documentation. Usually would have two: `.spec` and `.example`.

E.g. `inputs.conf.spec` and `inputs.conf.example`.

---

# Configuration file structures

## Stanza format

Config files are built by stanzas. Stanza begin with a stanza header in square brackets. Basic structure:
```
[stanza1_header]
<attribute1> = <val1>
# comment
<attrubute2> = <val2>
...

[stanze2_header]
<attribute1> = <val1>
...
```

Note that attributes are case-sensitive.

### Scope

The stanza header defines the scope of the configuration. See https://docs.splunk.com/Documentation/Splunk/8.0.1/Admin/Configurationfilestructureandsyntax for more details.

