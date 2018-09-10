# Headless WordPress via Docker Compose

[![Build status][build-status]][travis-ci]

This project is a minimalist boilerplate development starter kit for headless WordPress. It uses the official [Docker WordPress image][docker-wordpress] and [a (nearly) empty theme][no-theme] to effectively disable the WordPress frontend.

## Dependencies

* [Docker Compose][docker-compose]

## Setup

1. Clone this repo.

       git clone git@github.com:modelm/docker-headless-wordpress.git
       cd docker-headless-wordpress

2. Initialize containers.

       docker-compose up -d

3. Install WordPress, plugins, and theme. Optionally edit `bin/install-wp.sh` first to suit your needs.

       docker-compose run --rm wp-cli install-wp

## Services

### Apache

The `wordpress` container exposes Apache on host port `8080`:

[http://localhost:8080/wp-json/](http://localhost:8080/wp-json/)

[http://localhost:8080/wp-admin/](http://localhost:8080/wp-admin/)

The default credentials for the WordPress admin user are `wordpress`/`wordpress`.

### MySQL

The `mysql` container exposes MySQL on host port `8306`:

    mysql -uwordpress -pwordpress -h127.0.0.1 -P8306 wordpress

### WP-CLI

The `wp-cli` container supports WP-CLI commands as well as arbitrary shell code e.g.

    docker-compose run --rm wp-cli plugin list
    docker-compose run --rm wp-cli php -i

You may want to set up a shell alias for easy access:

    alias dwp='docker-compose run --rm wp-cli'

## Examples

Install & activate some plugins you might want in a typical headless setup:

    docker-compose run --rm wp-cli plugin install --activate \
        acf-to-wp-api \
        advanced-custom-fields \
        custom-post-type-ui \
        wp-rest-api-v2-menus \
        https://github.com/wp-graphql/wp-graphql/archive/master.zip

Uninstall & delete all plugins:

    docker-compose run --rm wp-cli plugin uninstall --deactivate --all --skip-packages

Uninstall & delete unused themes:

    docker-compose run --rm wp-cli theme delete \
        twentyfifteen \
        twentysixteen \
        twentyseventeen

[build-status]: https://travis-ci.org/modelm/docker-headless-wordpress.svg?branch=master
[travis-ci]: https://travis-ci.org/modelm/docker-headless-wordpress
[docker-compose]: https://docs.docker.com/compose/
[docker-wordpress]: https://hub.docker.com/_/wordpress/
[no-theme]: https://github.com/modelm/no-theme