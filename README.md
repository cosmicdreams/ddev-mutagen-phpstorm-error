# DDEV + Mutagen caused PhpStorm errors when running tests
## Steps to Reproduce
### 1. Use CMS Quickstart to build + install PHPUnit

```bash
mkdir my-drupal10-site
cd my-drupal10-site
ddev config --project-type=drupal10 --docroot=web --create-docroot
# If you want to run Functional / FunctionalJavascript Tests include:
ddev get ddev/ddev-selenium-standalone-chrome
ddev start
ddev composer create drupal/recommended-project
ddev composer require drupal/core-dev --dev --update-with-all-dependencies
ddev composer require drush/drush
ddev drush site:install --account-name=admin --account-pass=admin demo_umami -y
```
### 2. Run Functional Tests | these should work.
```bash
ddev exec phpunit -c /var/www/html/web/core/phpunit.xml.dist /var/www/html/web/core/modules/node/tests/src/Functional/NodeAdminTest.php
```
OR
```bash
ddev exec -d /var/www/html/web "../vendor/bin/phpunit -v -c ./core/phpunit.xml.dist ./core/modules/system/tests/src/FunctionalJavascript/FrameworkTest.php"
```

### 3. Run the same tests in PhpStorm
Navigate to core/modules/node/tests/src/Functional/NodeAdminTest.php and run the test by click on the test icon next to either a method that isn't setup or the class.
Try it again with /core/modules/system/tests/src/FunctionalJavascript/FrameworkTest.php
