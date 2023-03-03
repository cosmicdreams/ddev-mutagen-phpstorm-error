# DDEV + Mutagen caused PhpStorm errors when running tests
## Steps to Reproduce
### 1. Use CMS Quickstart to build + install PHPUnit

Using Docker Desktop:
```bash
mkdir my-drupal10-site
cd my-drupal10-site
ddev config --project-type=drupal10 --docroot=web --create-docroot

# Enable Mutagen:  This part introduces the bug.
ddev config --mutagen-enabled=true

ddev start
ddev composer create drupal/recommended-project
ddev composer require drupal/core-dev --dev --update-with-all-dependencies
ddev composer require drush/drush
ddev drush site:install --account-name=admin --account-pass=admin -y
```
### 2. Run a basic Unit Test | this should work.
```bash
ddev exec phpunit -c /var/www/html/web/core/phpunit.xml.dist /var/www/html/web/core/modules/system/tests/src/Unit/Routing/AdminRouteSubscriberTest.php
```
OR like this:
```bash
ddev exec -d /var/www/html/web "../vendor/bin/phpunit -v -c ./core/phpunit.xml.dist ./core/modules/system/tests/src/Unit/Routing/AdminRouteSubscriberTest.php"
```

### 3. Run the same tests in PhpStorm
Navigate to core/modules/system/tests/src/Unit/Routing/AdminRouteSubscriberTest.php and run the test by click on the test icon next to either a method that isn't setup or the class.
![Run test in phpstorm.webp](doc-assets%2FRun%20test%20in%20phpstorm.webp)

The error you will receive will be: note I replaced my user directory with `<user-dir>`
```
Test framework quit unexpectedly

[docker-compose://[/Users/<user-dir>/Sites/ddev-mutagen/.ddev/.ddev-docker-compose-full.yaml]:web/]:php8.1 vendor/bin/phpunit --configuration /var/www/html/web/core/phpunit.xml.dist --filter Drupal\\Tests\\system\\Unit\\Routing\\AdminRouteSubscriberTest --test-suffix AdminRouteSubscriberTest.php /Users/<user-dir>/Sites/ddev-mutagen/web/core/modules/system/tests/src/Unit/Routing --teamcity
Testing started at 8:44 AM ...
PHPUnit 9.6.4 by Sebastian Bergmann and contributors.

Cannot open file "/Users/<user-dir>/Sites/ddev-mutagen/web/core/modules/system/tests/src/Unit/Routing".

Process finished with exit code 1
```


### 4. Disable Mutagen
```bash
ddev config --mutagen-enabled=false
ddev restart
```

### 5. Run the same tests in PhpStorm, again
You can run the test on the command line, it will continue to work.
```bash
ddev exec phpunit -c /var/www/html/web/core/phpunit.xml.dist /var/www/html/web/core/modules/system/tests/src/Unit/Routing/AdminRouteSubscriberTest.php
```
OR like this:
```bash
ddev exec -d /var/www/html/web "../vendor/bin/phpunit -v -c ./core/phpunit.xml.dist ./core/modules/system/tests/src/Unit/Routing/AdminRouteSubscriberTest.php"
```

Now, navigate to core/modules/system/tests/src/Unit/Routing/AdminRouteSubscriberTest.php and run the test by click on the test icon next to either a method that isn't setup or the class.

Running tests in PhpStorm should work now.
