# settings.php $conf to $settings

## What is settings.php and what does it do

The settings.php file is the file that Drupal loads to do the initial configuration. It is used to store pre-bootstrap configuration. This is where Drupal loads the settings for the DB connection. This ffile is usually set to read-only by the Drupal installation.

> The standard practice used to be to put conditional statments into the setting.php file to have some settings that are only enabled in a sites: dev, test, or production environments. This is no longer the recommended route. Now Drupal will load a settings.local.php file if the file is present and all environment spesific settings should be in that file.

## overriding settings.php before cmi

In the before time (pre [D8]), all configuration variables where stored as a key value pair in the variables table in the database. This allowed for the simple overriding of variables in the settings.php file using the $conf array. Anything set in $conf would override anything that was set in the database. This allowed us to use the settings.php file to set some defaults that the user couldn't change.

This is still the case with [D8]. The big difference is that settings in $conf are no longer meant to be user definable in the ui. This means that the only settings that should be in the setting.php file are the settings that are required for Drupal to boot.

### Multiple configuration storage locations

[D8] stores the site configuration in files. The location that Drupal stores these files is configurable. This allows developers to keep the working local copy of configuration seperate from the production copy and only pull in the configuration to be uploaded to production as needed.

### deployment issues

Discuss some issues that will come up with deployments, and have a note to read the chapeter on deployment strategies.

## overriding settings with $settings

## when to use $settings object
