## How do I ... Drush

### Find all config settings?

Drush can present a nice list of config object that can be filtered by prefix.

```bash
drush config-list
```

The ```drush config-list``` command will list all config object names. In order to filter this list down we need to pass another argument. For example ```drush config-list system``` will pring a list of all system level configuration object names.


Once we have the name of the configuration object, we may edit it with the ```drush config-get``` command.
```bash
drush config-get
```

### Set a config setting?

Running ```drush config-edit``` will present you with a list of options of config files to edit.

@TODO find out what happens when no files are exported.

```bash
drush config-edit
```

### Export a single config setting?

As far as I have found there is no easy way to export a single config setting with drush. The best way that I have found is to run.

```bash
drush config-export --skip-modules=devel
```

@TODO find out what format this list needs to be in. Also, find out if there is a better way.

This can be handy to set development modules that have settings that you would not want to export --such as devel's query logger.

### Export a collection of config settings?

If you want all your config to be exported then running ```config-export``` with no options is what you want.

```bash
drush config-export
```

or the alias

```bash
drush cex
```

This command with no other options will write your config settings to your ```/sync``` config folder. Another argument can be passed to be an arbitrary label. Running  ```drush cex foo``` for example will export all the settings to the foo directory. This is useful if you want to work in a different active config directory but keep the default around. More on this in the [workflows chapter].


### Import config settings?

```bash
drush config-import
```

or the alias

```bash
drush cim
```

This command with no other options will import your config settings from your ```/sync``` config folder.

### Syncing config between environments

Drush also has commands to help with syncing configuration between development environments and production.

```bash
drush config-pull @prod @stage
```

Much like drush's ```drush sql-sync``` commands, ```drush config-pull``` takes two arguments (drupal environments) and pulls the configuration from one environment to another. The environments are defined as [drush aliases](link to drush aliases).
