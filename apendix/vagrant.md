* Installing Windows 8

For development I prefer to use Vagrant. I use a [configurable Vagrant box by Jeff Geerling Called Drupal-VM] (https://github.com/geerlingguy/drupal-vm). Installation is very simple and everything should work cross platform.

For detailed installation instructions read the project's README.md or the project's Doc page. Otherwise, for a TDLR, just follow along.

1. Download and install the latest versions of vagrant and virtualbox. (For faster provisioning download and install ansible too.)
1. Use git to clone the Drupal-VM Repository at https://github.com/geerlingguy/drupal-vm
1. Copy the example.config.yml to config.yml
1. Copy the example.drupal.make.yml to drupal.make.yml
1. If ansible is installed locally, use ansible galaxy to install the custom ansible roles.
1. Edit the config.yml and drupal.make.yml to fit whatever custom environment settings, including host name, machine name, and ip address.
1. Run vagrant up.

Vagrant should work with ansible to provision a Drupal 7 or 8 install. For the purposes of following the examples in this book the default Drupal 8 install should be fine. If so inclined there is also a Drupal Docker Image, however, I cannot verify how well it works.

> As of the time of this writing Vagrant and Virtual Box are not 100% compatible with Windows 10. Hopefully this notice will be removed before the book is published.
