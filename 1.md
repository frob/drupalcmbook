# What came before CM

since version x drupal has had a base system for storing simple configuration. The primary interface to access this system was through two functions variable_get and variable_set. All data was stored in the variable table as text. Until version x of Drupal every module was in charge of serializing it's own data. Environmental overriding was handled by by hard coding the $conf array in the settings.php file.

## Problems with the variable table approach

This was a simple approach and it solved simple problems. However if a module required more complex configuration date it would either have to manage it's own db tables or have a complicated serialized data structures saved as text in the variable table.

A good example of when this can become a problem is when a module want to add features exporting. Look at the entity display mode module.

Another problem comes from if the data needs to change to accommodate an update. This could involve loading all the data and removing and rewriting it to the db.

There are other issues that come from this that are more functional and less technical. Such as, variable localization. A module could support localization by prefixing or many other means, but the problem still exists that there was nothing in the variable system to solve these problems.

## Why CMI is better

One big things that cm gives us that the old variable system didn't is that drupal now know about all configuration variables and how they are structured. That means the system is able to manage them. All configuration variables have language settings, a defined structure, and set defaults that are enforced by the system.

## Why is that better?

### Defined structure

A defined structure gives us a tighter integration. Tools like drush and devel can now tell the user all the variables that are defined in the system, not only the ones that are saved (and they do not have to resort to complicated grep regex ```grep -rnP "(?<=variable_get\(['|\"])[a-zA-Z_0-9]{1,}" .```).

### Configuration variables have language settings

Setting a site up to be Multilingual used to be a complicated hoge poge of different modules built to fill the gaps. No more will there be competing modules built to allow for true Multilingual sites. At least that's the intent.

### Defaults are now set in the system

No more passing the second parameter to variable_get to set the default if there was no variable set

# Enter Drupal 8 Configuration Management Initiative

With Drupal the project management took a turn in it’s fundamental strategy. Instead of plowing ahead en mass the decision was made to partition out the different pieces of rearchatechting into several focused initiative. There was an initiative for creating a plugin system for blocks, there was one for bringing REST services into core. And there was the CMI “Configuration Management Initiative.”

CMI was tasked with taking the variable system and making it good.
