# Server configuration

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

Some [plugins](Plugins.md) may require additional configuration.


### SSL/TLS (HTTPS)

We recommend setting up [HTTPS](https://en.wikipedia.org/wiki/HTTPS) on your webserver for secure communication between clients and the server.

For public-facing web servers this can be done using free SSL/TLS certificates from [Let's Encrypt](https://en.wikipedia.org/wiki/Let's_Encrypt), a non-profit certificate authority provididing free certificates.

 - [How to secure Apache with Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-debian-10)
 - [How to secure Nginx with Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-debian-10)
 - [How To Use Certbot Standalone Mode to Retrieve Let's Encrypt SSL Certificates](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-debian-10).

In short:

```bash
sudo apt install certbot
# stop your webserver to allow certbot standalone to bind to port 80 (only needed on initial generation)
sudo systemctl stop apache2
sudo systemctl stop nginx
# generate initial certificates
# Let's Encrypt ACME servers must be able to access your webserver! (DNS records must be correctly pointing to  it, firewall/NAT needs to be open)
sudo certbot certonly --standalone --noninteractive --agree-tos --email "admin@shaarli.mydomain.org" -d shaarli.mydomain.org
# this will generate a private key and certificate at /etc/letsencrypt/live/shaarli.mydomain.org/{privkey,fullchain}.pem
# restart the web server
sudo systemctl start apache2
sudo systemctl start nginx
```

If you don't want to rely on a certificate authority, or the server can only be accessed from your own network, you can also generate self-signed certificates. Not that this will generate security warnings in web browsers/clients trying to access Shaarli:

- [How To Create a Self-Signed SSL Certificate for Apache](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-on-debian-10)
- [How To Create a Self-Signed SSL Certificate for Nginx](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-debian-10)

--------------------------------------------------------------------------------

### Examples

The following examples assume a Debian-based operating system is installed. On other distributions you may have to adapt details such as package installation procedures, configuration file locations, and webserver username/group.

#### Apache

Guide on setting up the Apache web server: [How to install the Apache web server](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-debian-10)


```bash
# Install apache and modules
sudo apt update
sudo apt install apache2 libapache2-mod-php php-json php-mbstring php-gd php-intl php-curl php-gettext

# Create a document root for the apache virtualhost
# Edit the virtualhost configuration file with your favorite editor
sudo mkdir -p /var/www/shaarli.mydomain.org/
sudo chown -R www-data:root /var/www/shaarli.mydomain.org
sudo chmod -R u=rX /var/www/shaarli.mydomain.org
sudo nano /etc/apache2/sites-available/shaarli.mydomain.org.conf
```

```apache
# configuration example for the Apache web server with mod_php
<VirtualHost *:80>
    ServerName shaarli.mydomain.org
    DocumentRoot /var/www/shaarli.mydomain.org/

    # Log level. Possible values include: debug, info, notice, warn, error, crit, alert, emerg.
    LogLevel  warn
    # Log file locations
    ErrorLog /var/log/apache2/error.log
    CustomLog /var/log/apache2/access.log combined

    # Automatic redirect to HTTPS, except for Let's Encrypt ACME challenge
    RewriteEngine on
    RewriteRule ^.well-known/acme-challenge/ - [L]
    RewriteCond %{HTTP_HOST} =shaarli.mydomain.org
    RewriteRule  ^ https://shaarli.mydomain.org%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>

<VirtualHost *:443>
    ServerName   shaarli.mydomain.org
    DocumentRoot /var/www/shaarli.mydomain.org/

    # Log level. Possible values include: debug, info, notice, warn, error, crit, alert, emerg.
    LogLevel  warn
    # Log file locations
    ErrorLog  /var/log/apache2/error.log
    CustomLog /var/log/apache2/access.log combined

    # SSL/TLS configuration (for Let's Encrypt certificates)
    SSLEngine             on
    SSLCertificateFile    /etc/letsencrypt/live/shaarli.mydomain.org/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/shaarli.mydomain.org/privkey.pem
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

    <Directory /var/www/shaarli.mydomain.org/>
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

# mod_version must only be enabled if you use Apache 2.2 or lower
# https://httpd.apache.org/docs/current/mod/mod_version.html
# sudo a2enmod version

# restart the apache service
systemctl restart apache
```

------------------------


#### Nginx

Guide on setting up the Nginx web server: [How to install the Nginx web server](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-10)

You will also need to install the [PHP-FPM](http://php-fpm.org) interpreter as detailed [here](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mariadb-php-lemp-stack-on-debian-10#step-3-%E2%80%94-installing-php-for-processing). Nginx and PHP-FPM must be running using the same user and group, here we assume the user/group to be `www-data:www-data` but this may vary depending on your Linux distribution.


```bash
# install nginx
sudo apt update
sudo apt install nginx php-fpm

# Create a document root for the apache virtualhost
# Edit the virtualhost configuration file with your favorite editor
sudo mkdir -p /var/www/shaarli.mydomain.org/
sudo chown -R www-data:root /var/www/shaarli.mydomain.org
sudo chmod -R u=rX /var/www/shaarli.mydomain.org
sudo nano /etc/nginx/sites-available/shaarli.mydomain.org
```

```nginx
server {
    listen       80;
    server_name  shaarli.mydomain.org;

    # redirect all plain HTTP requests to HTTPS
    return 301 https://shaarli.mydomain.org$request_uri;
}

server {
    listen       443 ssl;
    server_name  shaarli.mydomain.org;
    root         /var/www/shaarli.mydomain.org;

    # log file locations
    # combined log format prepends the virtualhost/domain name to log entries
    access_log  /var/log/nginx/access.log combined;
    error_log   /var/log/nginx/error.log;

    # paths to private key and certificates for SSL/TLS
    ssl_certificate      /etc/ssl/shaarli.mydomain.org.crt;
    ssl_certificate_key  /etc/ssl/private/shaarli.mydomain.org.key;


    # increase the maximum file upload size if needed: by default nginx limits file upload to 1MB (413 Entity Too Large error)
    # client_max_body_size 1m;

    # relative path to shaarli from the root of the webserver
    location / {
        index index.php;
        try_files $uri /index.php$is_args$args;
    }

    location ~ (index)\.php$ {
        try_files $uri =404;
        # Slim - split URL path into (script_filename, path_info)
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

    location = /favicon.ico {
        # serve the Shaarli favicon from its custom location
        alias /var/www/shaarli/images/favicon.ico;
    }

    # allow client-side caching of static files
    location ~* \.(?:ico|css|js|gif|jpe?g|png)$ {
        expires    max;
        add_header Pragma public;
        add_header Cache-Control "public, must-revalidate, proxy-revalidate";
    }

}
```

```bash
# enable the configuration/virtualhost
sudo ln -s /etc/nginx/sites-available/shaarli.mydomain.org /etc/nginx/sites-enabled/shaarli.mydomain.org
# reload nginx configuration
sudo systemctl reload nginx



## Proxies

If Shaarli is served behind a proxy (i.e. there is a proxy server between clients and the web server hosting Shaarli), please refer to the proxy server documentation for proper configuration. In particular, you have to ensure that the following server variables are properly set:

- [`X-Forwarded-Proto`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-Proto)
- [`X-Forwarded-Host`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-Host)
- [`X-Forwarded-For`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For)

In your [Shaarli configuration](Shaarli-configuration) `data/config.json.php`, add the public IP of your proxy under `security.trusted_proxies`.

See also [proxy-related](https://github.com/shaarli/Shaarli/issues?utf8=%E2%9C%93&q=label%3Aproxy+) issues.



## Allow import of large browser bookmarks export

Web browser bookmark exports can be large due to the presence of base64-encoded images and favicons/long subfolder names. Edit the PHP configuration file

- Apache: `/etc/php/<PHP_VERSION>/apache2/php.ini`
- Nginx + PHP-FPM: `/etc/php/<PHP_VERSION>/fpm/php.ini` (in addition to `client_max_body_size` in the [Nginx configuration](#nginx))

```ini
[...]
# (optional) increase the maximum file upload size:
post_max_size = 10M
[...]
# (optional) increase the maximum file upload size:
upload_max_filesize = 10M
```

To verify PHP settings currently set on the server
- create a `phpinfo.php` in your webserver's document root (eg. `/var/www/shaarli.mydomain.org/phpinfo.php`):

```php
<?php phpinfo(); ?>
```
- give read-only access to this file to the webserver user (eg. `sudo chown www-data:root /var/www/shaarli.mydomain.org/phpinfo.php && sudo chmod 0400 /var/www/shaarli.mydomain.org/phpinfo.php`)
- Access the file from a web browser (eg. `https://shaarli.mydomain.org/phpinfo.php`) and look at the _Loaded Configuration File_ and _Scan this dir for additional .ini files_ entries

It is recommended to remove the `phpinfo.php` when no logner needed as it publicly discloses details about your webserver configuration.


## Robots and crawlers

Shaarli disallows indexing and crawling of your local documentation pages by search engines, using `<meta name="robots">` HTML tags.
Your Shaarli instance and other pages you host may still be indexed by various robots on the public Internet, that do not respect this header.

Another way is to create a `robots.txt` file at the root of your webserver:

```
User-agent: *
Disallow: /
```

- [Robots exclusion standard](https://en.wikipedia.org/wiki/Robots_exclusion_standard)
- [Introduction to robots.txt](https://support.google.com/webmasters/answer/6062608?hl=en)
- [Robots meta tag, data-nosnippet, and X-Robots-Tag specifications](https://developers.google.com/search/reference/robots_meta_tag)
- [About robots.txt](http://www.robotstxt.org)
- [About the robots META tag](https://www.robotstxt.org/meta.html)


## Fail2ban

[`fail2ban`](http://www.fail2ban.org/wiki/index.php/Main_Page) is an intrusion prevention framework that reads server (Apache, SSH, etc.) and uses `iptables` profiles to block brute-force attempts. You need to create a filter to detect shaarli login failures in logs, and a jail configuation to configure the behavior when failed login attempts are detected:

```ini
# /etc/fail2ban/filter.d/shaarli-auth.conf
[INCLUDES]
before = common.conf
[Definition]
failregex = \s-\s<HOST>\s-\sLogin failed for user.*$
ignoreregex = 
```

```ini
# /etc/fail2ban/jail.local
[shaarli-auth]
enabled  = true
port     = https,http
filter   = shaarli-auth
logpath  = /var/www/shaarli.mydomain.org/data/log.txt
# allow 3 login attempts per IP address
# (over a period specified by findtime = in /etc/fail2ban/jail.conf)
maxretry = 3
# permanently ban the IP address after reaching the limit
bantime = -1
```


#### References

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
- [Travis configuration](https://github.com/shaarli/Shaarli/blob/master/.travis.yml)
- [PHP: Supported versions](http://php.net/supported-versions.php)
- [PHP: Unsupported versions](http://php.net/eol.php) _(EOL - End Of Life)_
- [PHP 7 Changelog](http://php.net/ChangeLog-7.php)
- [PHP 5 Changelog](http://php.net/ChangeLog-5.php)
- [PHP: Bugs](https://bugs.php.net/)
