
- [Prerequisites](#prerequisistes)
- [Apache](#apache)
- [Nginx](#nginx)
- [Proxies](#proxies)
- [See also](#see-also)

## Requirements

- A web server.
- Write access to the Shaarli installation directory.
- A PHP interpreter such as [Apache mod_php](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mariadb-php-lamp-stack-on-debian-10#step-3-%E2%80%94-installing-php) compatible with supported PHP versions:

Version | Status | Shaarli compatibility
:---:|:---:|:---:
7.2 | Supported | Yes
7.1 | Supported | Yes
7.0 | EOL: 2018-12-03 | Yes (up to Shaarli 0.10.x)
5.6 | EOL: 2018-12-31 | Yes (up to Shaarli 0.10.x)
5.5 | EOL: 2016-07-10 | Yes
5.4 | EOL: 2015-09-14 | Yes (up to Shaarli 0.8.x)
5.3 | EOL: 2014-08-14 | Yes (up to Shaarli 0.8.x)

- The following PHP extensions:

Extension | Required? | Usage
---|:---:|---
[`openssl`](http://php.net/manual/en/book.openssl.php) | All | OpenSSL, HTTPS
[`php-json`](http://php.net/manual/en/book.json.php) | required | configuration parsing
[`php-mbstring`](http://php.net/manual/en/book.mbstring.php) | CentOS, Fedora, RHEL, Windows, some hosting providers | multibyte (Unicode) string support
[`php-gd`](http://php.net/manual/en/book.image.php) | optional | required to use thumbnails
[`php-intl`](http://php.net/manual/en/book.intl.php) | optional | localized text sorting (e.g. `e->è->f`)
[`php-curl`](http://php.net/manual/en/book.curl.php) | optional | using cURL for fetching webpages and thumbnails in a more robust way
[`php-gettext`](http://php.net/manual/en/book.gettext.php) | optional | Use the translation system in gettext mode (faster)
--------------------------------------------------------------------------------

## Configuration

Here are links to useful guides on setting up a web server. You can find example webserver configurations for Shaarli further down this page.

- [How to install the Apache web server](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-debian-10)
- [How to install the Nginx web server](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-10)

### SSL/TLS (HTTPS)

We recommend setting up [HTTPS](https://en.wikipedia.org/wiki/HTTPS) on your webserver for secure communication between clients and the server.

For public-facing web servers this can be done using free SSL/TLS certificates from [Let's Encrypt](https://en.wikipedia.org/wiki/Let's_Encrypt), a non-profit certificate authority provididing free certificates.

 - [How to secure Apache with Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-debian-10)
 - [How to secure Nginx with Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-debian-10)
 - [How To Use Certbot Standalone Mode to Retrieve Let's Encrypt SSL Certificates](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-debian-10)

If you don't want to rely on a certificate authority, or the server can only be accessed from your own network, you can also generate self-signed certificates. Not that this will generate security warnings in web browsers/clients trying to access Shaarli:

- [How To Create a Self-Signed SSL Certificate for Apache](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-on-debian-10)
- [How To Create a Self-Signed SSL Certificate for Nginx](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-debian-10)

--------------------------------------------------------------------------------

### Examples

The following examples assume a Debian-based operating system is installed. On other distributions you may have to adapt details such as package installation procedures, configuration file locations, and webserver username/group.

#### Apache

```bash
# Install apache and modules
sudo apt install TODO

# Create a document root for the apache virtualhost
sudo mkdir -p /var/www/shaarli.mydomain.org/

# Edit the virtualhost configuration file with your favorite editor
sudo nano /etc/apache2/sites-available/shaarli.conf
```

```apache
# A Basic configuration example for the Apache web server with mod_php
<VirtualHost *:443>
    ServerName   shaarli.mydomain.org
    DocumentRoot /var/www/shaarli.mydomain.org/

    # Log level. Possible values include: debug, info, notice, warn, error, crit, alert, emerg.
    LogLevel  warn
    # Log file locations
    ErrorLog  /var/log/apache2/shaarli-error.log
    CustomLog /var/log/apache2/shaarli-access.log combined

    # SSL/TLS configuration (for Let's Encrypt certificates)
    SSLEngine             on
    SSLCertificateFile    /etc/letsencrypt/live/yourdomain.example.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/yourdomain.example.com/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf

    # SSL/TLS configuration (for self-signed certificates)
    #SSLEngine             on
    #SSLCertificateFile    /etc/ssl/certs/ssl-cert-snakeoil.pem
    #SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key

    # Optional, log PHP errors, useful for debugging
    #php_flag  log_errors on
    #php_flag  display_errors on
    #php_value error_reporting 2147483647
    #php_value error_log /var/log/apache2/shaarli-php-error.log

    <Directory /absolute/path/to/shaarli/>
        # Required for .htaccess support
        AllowOverride All
        Order allow,deny
        Allow from all

    </Directory>

</VirtualHost>
```

```bash
# Enable the virtualhost
sudo a2ensite shaarli

# mod_ssl must be enabled to use TLS/SSL certificates
# https://httpd.apache.org/docs/current/mod/mod_ssl.html
sudo a2enmod ssl

# mod_rewrite must be enabled to use the REST API
# https://httpd.apache.org/docs/current/mod/mod_rewrite.html
sudo a2enmod rewrite

# mod_version must be enabled if you use Apache 2.2 or lower
# https://httpd.apache.org/docs/current/mod/mod_version.html
# sudo a2enmod version

# restart the apache service
systemctl restart apache
```

Some [plugins](Plugins.md) may require additional configuration.


------------------------

<!--- TODO refactor everything below this point --->

#### Nginx

```bash
# install nginx


```

Nginx and PHP-FPM must be running using the same user and group, here we assume the user/group to be `www-data:www-data` but this may vary depending on your Linux distribution.

```nginx
# A basic configuration example for the Nginx web server, using the php-fpm PHP interpreter, and Nginx's FastCGI module.
# http://php-fpm.org
# https://en.wikipedia.org/wiki/FastCGI
```


```



# the nginx/php-fpm user/group must have read read permissions for Shaarli resources,
# and execute permissions for Shaarli resources AND their parent directories
```

```nginx
# /etc/nginx/nginx.conf

http {
    [...]
    # (optional) increase the maximum file upload size:
    # web browser bookmark exports can be large due to the presence of base64-encoded images and favicons/long subfolder names
    client_max_body_size 10m;
}
```

If you increased the maximum file upload size you will also need to update `/etc/php5/fpm/php.ini`

```ini
[...]
# (optional) increase the maximum file upload size:
post_max_size = 10M
[...]
# (optional) increase the maximum file upload size:
upload_max_filesize = 10M
```

### Minimal
_WARNING: Use for development only!_

```nginx
user www-data www-data;
# maximum number of worker processes
# https://nginx.org/en/docs/ngx_core_module.html#worker_processes
worker_processes  1;
events {
    # maximum number of simultaneous connections per worker
    # https://nginx.org/en/docs/ngx_core_module.html#worker_connections
    worker_connections  1024;
}

http {
    #
    include            mime.types;
    #
    default_type       application/octet-stream;
    #
    keepalive_timeout  20;

    #
    index index.html index.php;

    #
    server {
        listen       80;
        server_name  localhost;
        root         /home/john/web;

        # log file locations
        access_log  /var/log/nginx/access.log;
        error_log   /var/log/nginx/error.log;

        # 
        location /shaarli/ {
            try_files $uri /shaarli/index.php$is_args$args;
            access_log  /var/log/nginx/shaarli.access.log;
            error_log   /var/log/nginx/shaarli.error.log;
        }

        location ~ (index)\.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
            fastcgi_index  index.php;
            include        fastcgi.conf;
        }
    }
}
```

### Modular
The previous setup is sufficient for development purposes, but has several major caveats:

- every content that does not match the PHP rule will be sent to client browsers:
    - dotfiles - in our case, `.htaccess`
    - temporary files, e.g. Vim or Emacs files: `index.php~`
- asset / static resource caching is not optimized
- if serving several PHP sites, there will be a lot of duplication: `location /shaarli/`, `location /mysite/`, etc.

To solve this, we will split Nginx configuration in several parts, that will be included when needed:

```nginx
# /etc/nginx/deny.conf
location ~ /\. {
    # deny access to dotfiles
    access_log off;
    log_not_found off;
    deny all;
}

location ~ ~$ {
    # deny access to temp editor files, e.g. "script.php~"
    access_log off;
    log_not_found off;
    deny all;
}
```

```nginx
# /etc/nginx/php.conf
location ~ (index)\.php$ {
    # Slim - split URL path into (script_filename, path_info)
    try_files $uri =404;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;

    # filter and proxy PHP requests to PHP-FPM
    fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
    fastcgi_index  index.php;
    include        fastcgi.conf;
}

location ~ \.php$ {
    # deny access to all other PHP scripts
    deny all;
}
```

```nginx
# /etc/nginx/static_assets.conf
location ~* \.(?:ico|css|js|gif|jpe?g|png)$ {
    expires    max;
    add_header Pragma public;
    add_header Cache-Control "public, must-revalidate, proxy-revalidate";
}
```

```nginx
# /etc/nginx/nginx.conf
[...]

http {
    [...]

    root        /home/john/web;
    access_log  /var/log/nginx/access.log;
    error_log   /var/log/nginx/error.log;

    server {
        # virtual host for a first domain
        listen       80;
        server_name  my.first.domain.org;

        location /shaarli/ {
            # Slim - rewrite URLs
            try_files $uri /shaarli/index.php$is_args$args;

            access_log  /var/log/nginx/shaarli.access.log;
            error_log   /var/log/nginx/shaarli.error.log;
        }

        location = /shaarli/favicon.ico {
            # serve the Shaarli favicon from its custom location
            alias /var/www/shaarli/images/favicon.ico;
        }

        include deny.conf;
        include static_assets.conf;
        include php.conf;
    }

    server {
        # virtual host for a second domain
        listen       80;
        server_name  second.domain.com;

        location /minigal/ {
            access_log  /var/log/nginx/minigal.access.log;
            error_log   /var/log/nginx/minigal.error.log;
        }

        include deny.conf;
        include static_assets.conf;
        include php.conf;
    }
}
```

### Redirect HTTP to HTTPS
Assuming you have generated a (self-signed) key and certificate, and they are
located under `/home/john/ssl/localhost.{key,crt}`, it is pretty straightforward
to set an HTTP (:80) to HTTPS (:443) redirection to force SSL/TLS usage.

```nginx
# /etc/nginx/nginx.conf
[...]

http {
    [...]

    index index.html index.php;

    root        /home/john/web;
    access_log  /var/log/nginx/access.log;
    error_log   /var/log/nginx/error.log;

    server {
        listen       80;
        server_name  localhost;

        return 301 https://localhost$request_uri;
    }

    server {
        listen       443 ssl;
        server_name  localhost;

        ssl_certificate      /home/john/ssl/localhost.crt;
        ssl_certificate_key  /home/john/ssl/localhost.key;

        location /shaarli/ {
            # Slim - rewrite URLs
            try_files $uri /index.php$is_args$args;

            access_log  /var/log/nginx/shaarli.access.log;
            error_log   /var/log/nginx/shaarli.error.log;
        }

        location = /shaarli/favicon.ico {
            # serve the Shaarli favicon from its custom location
            alias /var/www/shaarli/images/favicon.ico;
        }

        include deny.conf;
        include static_assets.conf;
        include php.conf;
    }
}
```

## Proxies

If Shaarli is served behind a proxy (i.e. there is a proxy server between clients and the web server hosting Shaarli), please refer to the proxy server documentation for proper configuration. In particular, you have to ensure that the following server variables are properly set:

- `X-Forwarded-Proto`
- `X-Forwarded-Host`
- `X-Forwarded-For`

In you [Shaarli configuration](Shaarli-configuration) `data/config.json.php`, add the public IP of your proxy under `security.trusted_proxies`.

See also [proxy-related](https://github.com/shaarli/Shaarli/issues?utf8=%E2%9C%93&q=label%3Aproxy+) issues.

## Robots and crawlers

Shaarli disallows indexing and crawling of your local documentation pages by search engines, using `<meta name="robots">` HTML tags.
Your Shaarli instance and other pages you host may still be indexed by various robots on the public Internet.
You may want to setup a robots.txt file or other crawler control mechanism on your server.
See [[1]](https://en.wikipedia.org/wiki/Robots_exclusion_standard), [[2]](https://support.google.com/webmasters/answer/6062608?hl=en) and [[3]](https://developers.google.com/search/reference/robots_meta_tag)

## See also

 * [Server security](Server-security.md)

#### Webservers

- [Apache/PHP - error log per VirtualHost](http://stackoverflow.com/q/176) (StackOverflow)
- [Apache - PHP: php_value vs php_admin_value and the use of php_flag explained](https://ma.ttias.be/php-php_value-vs-php_admin_value-and-the-use-of-php_flag-explained/)
- [Server-side TLS (Apache)](https://wiki.mozilla.org/Security/Server_Side_TLS#Apache) (Mozilla)
- [Nginx Beginner's guide](http://nginx.org/en/docs/beginners_guide.html)
- [Nginx ngx_http_fastcgi_module](http://nginx.org/en/docs/http/ngx_http_fastcgi_module.html)
- [Nginx Pitfalls](http://wiki.nginx.org/Pitfalls)
- [Nginx PHP configuration examples](http://kbeezie.com/nginx-configuration-examples/) (Karl Blessing)
- [Server-side TLS (Nginx)](https://wiki.mozilla.org/Security/Server_Side_TLS#Nginx) (Mozilla)
- [How to Create Self-Signed SSL Certificates with OpenSSL](http://www.xenocafe.com/tutorials/linux/centos/openssl/self_signed_certificates/index.php)
- [How do I create my own Certificate Authority?](https://workaround.org/certificate-authority)

#### PHP

- [Travis configuration](https://github.com/shaarli/Shaarli/blob/master/.travis.yml)
- [PHP: Supported versions](http://php.net/supported-versions.php)
- [PHP: Unsupported versions](http://php.net/eol.php) _(EOL - End Of Life)_
- [PHP 7 Changelog](http://php.net/ChangeLog-7.php)
- [PHP 5 Changelog](http://php.net/ChangeLog-5.php)
- [PHP: Bugs](https://bugs.php.net/)
