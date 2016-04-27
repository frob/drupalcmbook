# What is CM

As I said in the history chapter, CMI stands for Configuration Management Initiative. CMI was the project within the [D8] project to make configuration sane and standard for developers.

## Configuration Entities

One of the primary results of CMI was the ability to create configuration entities. [D7] saw the creation of the Entity system for content, D8 expanded on that consept for configuration. There are several key differences between configuration and content entities.

### Configuration Entities are stored in files

Unlike conent entities, configuration entities are stored in files. They are cached in the db for ease of access, but they are stored long term in files. These files are normally stored in [cm storage location].

> Notice that I said "These fiels are normally stored." These configuration files can be stored nearly anywhere. This allows for some very intesting configuration management and development strategies. More on this later on.

This gives the developer the ability to store this configuration in the site's repository. This doens't give developers a place that is in code to edit configuration. Configuration should be edited in the Drupal UI or through a command line tool. What is in code isn't nessesarly what is in the db. After an update in code (normally through a source code repository update or rsync) then the configuration needs to be synced or imported into the Drupal site.

### Not meant for end users

It should go without saying, but, configuration entities are not a place to store content. Content Entities are used to store content, end users should be writing content. Therefore, end users will in most cases not be making configuration entities.

## Format

As said before these files are stored in [cm storage location] in a format called yaml (pronounced "yaamel"). In its own standards page's words yaml is a "is a straight-forward machine parable data serialization format designed for human readability and interaction with scripting languages."

### Yaml

In practice yaml files are a whitespace strict, declaritive, human-readable format of markup that looks like json and python had a baby. If none of that made sense then don't fret and read on. It will all become clear.

In yaml, spaces are important.

#### Basics

<dl>
<dt>Associative Array/Dictionary</dt>
<dd>
```yaml
- hello: world
  term: definition
  example: here is a longer line
```
</dd>

<dt>Array/List</dt>
<dd>
```yaml
hello:
  - definition
  - here is a longer line
  - one more, just for fun.
```
</dd>

<dt>"Nesting things Array"</dt>
<dd>
```yaml
hello:
  term: definition
  example: here is a longer line
  another level
    - definition
    - here is a longer line
```
</dd>
<dl>

### Drupal Configuration Management yaml

Lets take a look at a simple configuration management file and how it maps to a configuration management object.

_system.theme.yml_

```Yaml
admin: seven
default: bartik
```

This is about as simple as configuration gets. There is no nesting or arrays inside the file and getting or setting the theme for a [D8] site is equally simple. Here is some code that will get the name of the default theme.

```php
$config = \Drupal::config('system.theme');
$config->get('default');
```

As you can see [D8]'s configuration system give developers a straight-forward way to get and set configuration. But what about getting and parsing a more complicated configuration object.

> If you do not know what a node is in drupal, then this next part can be confusing. It is basically a data type in the Drupal CMS. For more information you will need to read another book.

_node.type.article.yml_
@TODO get the correct default article configuration yaml
```yaml
uuid: 85d6c3d5-5d53-45c8-9101-1cf87aa0eeee
langcode: en
status: true
dependencies:
  module:
    - menu_ui
third_party_settings:
  menu_ui:
    available_menus:
      - main
    parent: 'main:'
name: Article
type: article
description: 'Not currently used, <em>articles</em> would be for time-sensitive content like news, press releases or blog posts.'
help: ''
new_revision: false
preview_mode: 1
display_submitted: true
```

>It is importent to remember that [D8] thas many yaml files and not all of them are considered configuration management. If you are familiar with info files from [D7] and early, they have all been switched over to yaml files. Also, many info hooks have been replaced, at least partially, with yaml files. Routes used to be implementations of hook_menu they are now yaml files.

To get the human name we can call the ```get('name')``` method on it, to get the type we can call the ```get('type')```
@TODO find out if there are magic methods for config objects.

```php
$config = \Drupal::config('node.type.article');
$config->get('name');
```

See easy, right? OH, you want to see me get the *Article* node type's dependencies? Well that is easy too, but not quite as straight-forward --this is because like anything in programming there is more than one way to do something when using an api.

@TODO make sure all this works
_The easy way_

```php
$config = \Drupal::config('node.type.article.dependencies');
$config->get('modules');
```

### File structure & Subcategories

A module can define its default values for configuration inside ```config/install``` and ```config/schema``` in the module's directory. The *install* directory holds configuration management files that will be imported upon installation of the module. The *schema* directory holds configuration object definitions. This is used when a modules might not want to import configuration when it is installed. If you take a look at that node type configuration file for the _Article_ content type definition, you will see a uuid. If we look at the schema file ```core/modules/node/config/schema/node.schema.yml``` for node types:

```yaml
node.settings:
  type: config_object
  label: 'Node settings'
  mapping:
    use_admin_theme:
      type: boolean
      label: 'Use admin theme when editing or creating content'

node.type.*:
  type: config_entity
  label: 'Content type'
  mapping:
    name:
      type: label
      label: 'Name'
    type:
      type: string
      label: 'Machine-readable name'
    description:
      type: text
      label: 'Description'
    help:
      type: text
      label: 'Explanation or submission guidelines'
    new_revision:
      type: boolean
      label: 'Whether a new revision should be created by default'
    preview_mode:
      type: integer
      label: 'Preview before submitting'
    display_submitted:
      type: boolean
      label: 'Display setting for author and date Submitted by post information'
```

> There is more to a node than a node type. I have condensed this configuration schema to only include the node type schema and the node settings schema.

If we look at the keys in the config entity for the _Article_ content type and compare it to the keys in the mapping we will see that they are all present in the schema.

- name: Article
- type: article
- description: 'Not currently used, <em>articles</em> would be for time-sensitive content like news, press releases or blog posts.'
- help: ''
- new_revision: false
- preview_mode: 1
- display_submitted: true

There are a few fields in the config entity that are not in the schema: _uuid_,  _langcode_, _status_, _dependencies_, and _third_party_settings_. These are config entity properties, they are used mostly internally to manage the mechanical work of importing, exporting, or managing the configuration entities.

@TODO find out about these: status, dependencies and third_party_settings.

Active configuration is stored in a flat file structure in the ```sites/default/files/config_HASH/sync``` directory. Subcategories are organized into files by the name of the file and the hierarchy of the yaml in the file. For example; configuration files that have the pattern ```system.*.yml``` are a part of the system namespace. If we look at these files:

- ```system.image.yml```
- ```system.logging.yml```
- ```system.mail.yml```
- ```system.maintenance.yml```

We will see this inside them:

```yaml
toolkit: gd
_core:
  default_config_hash: durWHaKeBaq4d9Wpi4RqwADj1OufDepcnJuhVLmKN24
```

```yaml
error_level: hide
_core:
  default_config_hash: u3-njszl92FaxjrCMiq0yDcjAfcdx72w1zT1O9dx6aA
```

```yaml
interface:
  default: php_mail
_core:
  default_config_hash: rYgt7uhPafP2ngaN_ZUPFuyI4KdE0zU868zLNSlzKoE
```

```yaml
message: '@site is currently under maintenance. We should be back shortly. Thank you for your patience.'
langcode: en
_core:
  default_config_hash: Z5MXifrF77GEAgx0GQ6iWT8wStjFuY8BD9OruofWTJ8
```

Looking at these simple configuration files it should be easy to see a pattern. They all only have one or two things in them. It might seem like a better idea to have one system configuration file with the contents:

```yaml
image:
  toolkit: gd
  _core:
    default_config_hash: durWHaKeBaq4d9Wpi4RqwADj1OufDepcnJuhVLmKN24
logging:
  error_level: hide
  _core:
    default_config_hash: u3-njszl92FaxjrCMiq0yDcjAfcdx72w1zT1O9dx6aA
mail:
  interface:
    default: php_mail
  _core:
    default_config_hash: rYgt7uhPafP2ngaN_ZUPFuyI4KdE0zU868zLNSlzKoE
maintenance:
  message: '@site is currently under maintenance. We should be back shortly. Thank you for your patience.'
  langcode: en
  _core:
    default_config_hash: Z5MXifrF77GEAgx0GQ6iWT8wStjFuY8BD9OruofWTJ8
```

This is nice right? All our configuration is in one place. We can just edit this file and import the changes. There is a problem with that idea however, if we do this then any little change to completely unrelated systems would cause this entire file to need to be synced. Also, all changes are meant to be made in the ui. In other words, the system is designed in a way that we do not make changes by hand in files. Instead we use the Drupal UI or a command line tool to edit the configuration.

## What does Drupal do with yaml files

It is important to remember that in D8, the configuration objects, by default, live in the database. So why then have the file storage mechanism at all? Early in development D8 was going to use file based active configuration by default, this was true all the way up until just after the file format was finalized. File access is slow when dealing with large scale sites, and yaml parsing is even slower still. Remember that there is a reason D7 and below stores everything in the DB and is able to remain stable at scale. Databases exist for a reason and they allow for fast resource efficient of random access data.

So why all the files? The major goal of the Configuration Management Initiative was to create a mechanism that could allow for configuration to be stored in a source version control system. This is so developers can more easily work on unrelated parts of the site and merge all those pieces back together. In D7 and D6 days this was mostly done with the Features module, but that wasn't ideal. Also, Features was designed to do a different task. It caused lots of waisted hours in the development cycle of a Drupal site to manage.

By putting everything into source control and allowing different developers to have different versions of the configuration loaded at any point allows for far more flexible and customizable workflows.
