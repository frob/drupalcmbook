# Writing a module that uses $conf object

## Anatomy of a configuration object

### Yaml Conventions

> For more detail read Chapter3:Yaml

### Location

There are two default Configuration file locations, in the module that they belong to and in the sites config directory.

A core module, such as Contact, will store its default configuration settings in: ``` core/modules/contact/config/install/contact.settings.yml ```

When the module has installed Drupal will copy the configruation to the database and to ``` sites/default/files/config_9RwHAqNH0ZhLwUsLUin_iKsp8IMZTSxwGefeBKH-XyBKmBnKzRZIDaOHQyuGrdABUFq3gy_riQ/staging ``` (assuming this is default and not a site spesific multisite configuration).

> Please note that the 9RwHAqNH0ZhLwUsLUin_iKsp8IMZTSxwGefeBKH-XyBKmBnKzRZIDaOHQyuGrdABUFq3gy_riQ is a unique hash for installed site. This is directory is stored as a hash to avoid conflicts.

> @todo: include note about active configuration settings override.

#### Namespaces

Not all yaml files that are in the the module directory are traditional Configuration objects. Other things are stored in yaml files and not all of them are actual config yaml files. [D8] has a very spesific file directory structure due to its incorporation of [autoload spec].

Another thing to remember is the naming of the config yaml files. As in most things in Drupal, the way things are named matters and the purpose for the file can usually be dirived from the name of the file. For example the file ``` node.type.article.yml ``` is a file that defines a node content-type called article. Can you guess what the yaml file ``` contact.form.personal.yml ``` is for? It describes a the personal contact form. The naming convention is to use the namespace of the module that the config belongs to followed by more detail about the config entity.

These namespaces are important when writing modules because they dictate the names used to get and set the config entities. More on that later in the chapter. Check the appendix for a list of all namespaces that are shipping with core.

> The namespace here is completely arbratary. It can be named whatever the developer wants. However, it is important to follow the drupal naming conventions when developing with drupal, more info on that at https://drupal.org/standards

### Using $conf in code

## Simple $conf object

As far as simple examples for Configuration objects. It doesn't get much simpler than the contact module. This is the default configuration settings for the Contact module.

```yaml
default_form: feedback
flood:
  limit: 5
  interval: 3600
user_default_enabled: true
```

## Inspect some core conf objects

### Content types

Here is the standard installation Article content type configuration entity.

```yaml
uuid: 4bcca9d6-2b27-468a-a53f-63d09e1846cd
langcode: en
status: true
dependencies: {  }
name: Article
type: article
description: 'Use <em>articles</em> for time-sensitive content like news, press releases or blog posts.'
help: ''
new_revision: false
preview_mode: 1
display_submitted: true
```

The first thing to notice is that there is no field information. The field information is stored as *fields* for a field instance and *field storage* for the global field storage.

### Vocabularies

### User

### Generic Entity and Bundle
