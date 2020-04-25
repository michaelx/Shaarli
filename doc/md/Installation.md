# Installation

## From release ZIP

To install Shaarli, simply place the files from the latest [release .zip file](https://github.com/shaarli/Shaarli/releases) archive under your webserver's Document Root (directly at the document root, or in a subdirectory). Download our **shaarli-vX.X.X-full** archive to include dependencies.

```bash
$ wget https://github.com/shaarli/Shaarli/releases/download/v0.10.4/shaarli-v0.10.4-full.zip
$ unzip shaarli-v0.10.4-full.zip
$ mv Shaarli /var/www/shaarli.mydomain.org
```

Also, please make sure your server is properly [configured](Server-configuration.md).


## From git sources

Multiple releases branches are available:

- `latest`: the latest [release](https://github.com/shaarli/Shaarli/releases)/tag
- `master`: development branch

How to proceed:

- Clone the branch/tag of your choice using `git`, OR download and extract git ZIP files ([eg. for the master branch](https://github.com/shaarli/Shaarli/archive/master.zip))
- Install [Composer](Unit-tests.md#install_composer) to manage third-party [PHP dependencies](3rd-party-libraries.md#composer).
- Install [yarn](https://yarnpkg.com/lang/en/docs/install/) to build the frontend dependencies.
- Install [python3-virtualenv](https://pypi.python.org/pypi/virtualenv) to build the local HTML documentation.

```bash
$ mkdir -p /path/to/shaarli && cd /path/to/shaarli/
$ git clone -b latest https://github.com/shaarli/Shaarli.git .
$ composer install --no-dev --prefer-dist
$ make build_frontend
$ make translate
$ make htmldoc

# then copy the resulting shaarli directory top your webserver's document root
$ mv /path/to/shaarli /var/www/shaarli.mydomain.org
```

Also, please make sure your server is properly [configured](Server-configuration.md).


## Using Docker

[See the documentation](Docker.md)


## Finish Installation

Once Shaarli is downloaded and files have been placed at the correct location, open it this location your favorite browser.

![install screenshot](images/install-shaarli.png)

Setup your Shaarli installation, and it's ready to use!


## Updating Shaarli

See [Upgrade and Migration](Upgrade-and-migration)
