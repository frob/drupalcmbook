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
hello
  term: definition
  example: here is a longer line
  another level
    - definition
    - here is a longer line
```
</dd>
<dl>

### Drupal cmi yml

### File structure

### Subcategories

## What does Drupal do with yaml files
