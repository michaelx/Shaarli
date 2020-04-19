# Download and installation

## Prebuilt release ZIP

To install Shaarli, simply place the files from the release archive in a directory under your webserver's Document Root (or directly at the document root).

In most cases, you should download the latest Shaarli release from the [releases](https://github.com/shaarli/Shaarli/releases) page. Download our **shaarli-vX.X.X-full** archive to include dependencies.

```bash
$ wget https://github.com/shaarli/Shaarli/releases/download/v0.10.4/shaarli-v0.10.4-full.zip
$ unzip shaarli-v0.10.4-full.zip
$ mv Shaarli /var/www/shaarli.mydomain.org
```

Also, please make sure your server is properly [configured](Server-configuration.md).


## Build from git sources

Multiple releases branches are available:

- `latest`: the latest [release](https://github.com/shaarli/Shaarli/releases)/tag
- `master`: development branch


 * Clone the branch/tag of your choice using `git`, OR download and extract git ZIP files ([eg. for the master branch](https://github.com/shaarli/Shaarli/archive/master.zip))
 * Install [Composer](Unit-tests.md#install_composer) to manage third-party [PHP dependencies](3rd-party-libraries.md#composer).
 * Install [yarn](https://yarnpkg.com/lang/en/docs/install/) to build the frontend dependencies.
 * Install [python3-virtualenv](https://pypi.python.org/pypi/virtualenv) to build the local HTML documentation.

```bash
$ mkdir -p /path/to/shaarli && cd /path/to/shaarli/
$ git clone -b latest https://github.com/shaarli/Shaarli.git .
$ composer install --no-dev --prefer-dist
$ make build_frontend
$ make translate
$ make htmldoc
```

## Using Docker

[See the documentation](docker/shaarli-images.md)



## Finish Installation

Once Shaarli is downloaded and files have been placed at the correct location, open it this location your favorite browser.

![install screenshot](images/install-shaarli.png)

Setup your Shaarli installation, and it's ready to use!

## Updating Shaarli

See [Upgrade and Migration](Upgrade-and-migration)
