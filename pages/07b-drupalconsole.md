# DrupalConsole

DrupalConsole is a new command line tool built with [S] components.

- [config:debug](#configdebug)
- [config:delete](#configdelete)
- [config:diff](#configdiff)
- [config:edit](#configedit)
- [config:export](#configexport)
- [config:export:content:type](#configexportcontenttype)
- [config:export:single](#configexportsingle)
- [config:export:view](#configexportview)
- [config:import](#configimport)
- [config:import:single](#configimportsingle)
- [config:override](#configoverride)
- [config:settings:debug](#configsettingsdebug)

## config:debug

Show the current configuration.

*Usage:*

```bash
$ drupal config:debug [arguments]
$ cde
```

*Available arguments*

config-name
: Configuration name.

## config:delete

Delete configuration

*Usage:*

```bash
$ drupal config:delete [arguments]
```

*Available arguments*

name
: Configuration name.

## config:diff

Output configuration items that are different in active configuration compared with a directory.

*Usage:*

```bash
$ drupal config:diff [arguments] [options]
```

*Available options*

--reverse
: See the changes in reverse (i.e diff a directory to the active configuration).

*Available arguments*

directory
: The directory to diff against. If omitted, choose from Drupal config directories.

## config:edit

Edit the selected configuration.

*Usage:*

```bash
$ drupal config:edit [arguments]
$ cdit
```

*Available arguments*

config-name
: Configuration name.

editor
: Editor.

## config:export

Export current application configuration.

*Usage:*

```bash
$ drupal config:export [options]
$ ce
```

*Available options*

--directory
: Define the export directory to save the configuration output.

--tar
: If set, the configuration will be exported to an archive file.

## config:export:content:type

Export a specific content type and their fields.

*Usage:*

```bash
$ drupal config:export:content:type [arguments] [options]
$ cect
```

*Available options*

--module
: The Module name.

--optional-config
: Export content type as an optional YAML configuration in your module

*Available arguments*

content-type
: Content Type to be exported

## config:export:single

Export single configuration as yml file.

## config:export:view

Export a view in YAML format inside a provided module to reuse in other website.

## config:import

Import configuration to current application.

## config:import:single

Import the selected configuration.

## config:override

Override config value in active configuration.

## config:settings:debug

Displays current key:value on settings file.
