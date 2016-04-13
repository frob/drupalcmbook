# Drush

These are the commands that allow drush to interact with the configuration management system. I will go over the command and its options one at a time. Much of this documentation comes directly from drush's help pages.

- config-edit
- config-export
- config-get
- config-import
- config-list
- config-pull
- config-set

## config-edit

Open a config file in a text editor. Edits are imported into active configration after closing editor.

```
Examples:
 drush config-edit image.style.large       Edit the image style configurations.
 drush config-edit                         Choose a config file to edit.
 drush config-edit --choice=2              Edit the second file in the choice list.
 drush --bg config-edit image.style.large  Return to shell prompt as soon as the editor window opens.

Arguments:
 config-name                               The config object name, for example "system.site".

Options:
 --bg                                      Run editor in the background. Does not work with editors such as `vi` that run in the terminal.
                                           Supresses config-import at the end.
 --editor=<vi>                             A string of bash which launches user's preferred text editor. Defaults to ${VISUAL-${EDITOR-vi}}.
 --file                                    Import from a file instead of interactively editing a given config.

Aliases: cedit
```

## config-export

Export configuration to a directory.

```
Examples:
 drush config-export --skip-modules=devel  Export configuration; do not include the devel module in the exported configuration, regardless of
                                           whether or not it is enabled in the site.
 drush config-export --destination         Export configuration; Save files in a backup directory named config-export.

Arguments:
 label                                     A config directory label (i.e. a key in $config_directories array in settings.php). Defaults to
                                           'sync'

Options:
 --add                                     Run `git add -p` after exporting. This lets you choose which config changes to sync for commit.
 --branch=<branchname>                     Make commit on provided working branch. Ignored if used without --commit or --push.
 --commit                                  Run `git add -A` and `git commit` after exporting.  This commits everything that was exported without
                                           prompting.
 --destination                             An arbitrary directory that should receive the exported files. An alternative to label argument.
 --message                                 Commit comment for the exported configuration.  Optional; may only be used with --commit or --push.
 --push                                    Run `git push` after committing.  Implies --commit.
 --remote=<origin>                         The remote git branch to use to push changes.  Defaults to "origin".
 --skip-modules                            A list of modules to ignore during export (e.g. to avoid listing dev-only modules in exported
                                           configuration).

Aliases: cex
```

## config-get

Display a config value, or a whole configuration object.

```
Examples:
 drush config-get system.site              Displays the system.site config.
 drush config-get system.site page.front   gets system.site:page.front value.

Arguments:
 config-name                               The config object name, for example "system.site".
 key                                       The config key, for example "page.front". Optional.

Options:
 --format=<json>                           Select output format. Available: yaml, csv, html, json, list, string, table, var_export. Default is
                                           yaml.
 --include-overridden                      Include overridden values.
 --pipe                                    Equivalent to --format=var_export.
 --source=<sync>                           The config storage source to read. Additional labels may be defined in settings.php

Topics:
 docs-output-formats                       Output formatting options selection and use.

Aliases: cget
```

## config-import

Import config from a config directory.

```
Examples:
 drush config-import --skip-modules=devel  Import configuration; do not enable or disable the devel module, regardless of whether or not it
                                           appears in the imported list of enabled modules.

Arguments:
 label                                     A config directory label (i.e. a key in $config_directories array in settings.php). Defaults to
                                           'sync'

Options:
 --cache-clear=<0>                         If 0, Drush skips normal cache clearing; the caller should then clear if needed.
 --partial                                 Allows for partial config imports from the source directory. Only updates and new configs will be
                                           processed with this flag (missing configs will not be deleted).
 --preview=<list>                          Format for displaying proposed changes. Recognized values: list, diff. Defaults to list
 --skip-modules                            A list of modules to ignore during import (e.g. to avoid disabling dev-only modules that are not
                                           enabled in the imported configuration).
 --source                                  An arbitrary directory that holds the configuration files. An alternative to label argument

Aliases: cim
```

## config-list

List config names by prefix.

```
Examples:
 drush config-list system                  Return a list of all system config names.
 drush config-list "image.style"           Return a list of all image styles.
 drush config-list --format="json"         Return all config names as json.

Arguments:
 prefix                                    The config prefix. For example, "system". No prefix will return all names in the system.

Options:
 --format=<json>                           Select output format. Available: list, json, var_export, yaml. Default is list.
 --pipe                                    Equivalent to --format=var_export.

Topics:
 docs-output-formats                       Output formatting options selection and use.

Aliases: cli
```

## config-pull

Export and transfer config from one environment to another.

```
Examples:
 drush config-pull @prod @stage            Export config from @prod and transfer to @stage.
 drush config-pull @prod @self             Export config from @prod and transfer to the 'vcs' config directory of current site.
 --label=vcs

Arguments:
 source                                    A site-alias or the name of a subdirectory within /sites whose config you want to copy from.
 target                                    A site-alias or the name of a subdirectory within /sites whose config you want to replace.

Options:
 --label                                   A config directory label (i.e. a key in $config_directories array in settings.php). Defaults to
                                           'sync'
 --runner                                  Where to run the rsync command; defaults to the local site. Can also be "source" or "destination".
 --safe                                    Validate that there are no git uncommitted changes before proceeding

Topics:
 docs-aliases                              Site aliases overview on creating your own aliases for commonly used Drupal sites with examples from
                                           example.aliases.drushrc.php.
 docs-config-exporting                     Drupal configuration export instructions, including customizing configuration by environment.

Aliases: cpull
```

## config-set

Set config value directly.

```
Examples:
 drush config-set system.site page.front  Sets system.site:page.front to "node".
 node

Arguments:
 config-name                               The config object name, for example "system.site".
 key                                       The config key, for example "page.front".
 value                                     The value to assign to the config key. Use '-' to read from STDIN.

Options:
 --format=<yaml>                           Format to parse the object. Use "string" for string (default), and "yaml" for YAML.

Aliases: cset
```
