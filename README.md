# CMS with shared configuration management.
A recipe to convert drupal CMS into an install profile with shared configuration management.

# Requirements
Composer 2
DDev

# Instructions
Download and install Drupal CMS with composer. (https://packagist.org/packages/drupal/cms)

```
composer create-project drupal/cms cms-shared-config-management
```

Change directory in to the new folder.

```
cd cms-shared-config-management
```

Tell composer about our custom repos.
```
composer config repositories.trackleft/cms_installer vcs "git@github.com:trackleft/cms_installer"
composer config repositories.trackleft/cms vcs "git@github.com:trackleft/cms"
```

Add Composer Lenient so we can install incompatible packages (This is only due to the lack of a release in two of our modules.)
```
composer require mglaman/composer-drupal-lenient
```
Configure Composer Lenient
```
composer config --merge --json extra.drupal-lenient.allowed-list '["drupal/config_distro", "drupal/config_distro_filter", "drupal/config_sync"]'
```

Add a patching mechanism since we need to patch config_distro and config_sync to work with Drupal 11.
```
composer require cweagans/composer-patches
```

Add the recipe.
```
composer require trackleft/cms_installer -W    
```

Spin up DDev
```
ddev config --project-type=drupal11 --docroot=web 
ddev start
ddev composer install
ddev launch
```

Install the recipe
```
ddev drush recipe /var/www/html/recipes/cms_installer
```
