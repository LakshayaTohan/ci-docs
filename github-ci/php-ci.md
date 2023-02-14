# Configuration

Create a file in your project root with the following path: `.github\workflows\php-ci.yml`

Copy the below configuration and paste it in `php-ci.yml`

```yaml
name: PHP-CI

on:
  pull_request:
    branches: [ "main" ]

jobs:
  Laravel-Lint:

    runs-on: ubuntu-latest

    steps:
    - uses: shivammathur/setup-php@15c43e89cdef867065b0213be354c2841860869e
      with:
        php-version: '8.0'
    - uses: actions/checkout@v3
    - name: Copy .env
      run: php -r "file_exists('.env') || copy('.env.example', '.env');"
    - name: Install Dependencies
      run: composer install -q --no-ansi --no-interaction --no-scripts --no-progress --prefer-dist
    - name: Generate key
      run: php artisan key:generate
    - name: Directory Permissions
      run: chmod -R 777 storage bootstrap/cache
    - name: Run PHP_CodeSniffer (Code linting)
      run: ./vendor/bin/phpcs --standard=./.phpcs.xml
    - name: Run Phan (Static code analysis)
      run: ./vendor/bin/phan --config-file ./.phan/config.php
```
