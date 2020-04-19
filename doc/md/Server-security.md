
## fail2ban
`fail2ban` is an intrusion prevention framework that reads server (Apache, SSH, etc.) and uses `iptables` profiles to block brute-force attempts:

- [Official website](http://www.fail2ban.org/wiki/index.php/Main_Page)
- [Source code](https://github.com/fail2ban/fail2ban)

### Read Shaarli logs to ban IPs
Example configuration:
- allow 3 login attempts per IP address
- after 3 failures, permanently ban the corresponding IP adddress

`/etc/fail2ban/jail.local`
```ini
[shaarli-auth]
enabled  = true
port     = https,http
filter   = shaarli-auth
logpath  = /var/www/path/to/shaarli/data/log.txt
maxretry = 3
bantime = -1
```

`/etc/fail2ban/filter.d/shaarli-auth.conf`
```ini
[INCLUDES]
before = common.conf
[Definition]
failregex = \s-\s<HOST>\s-\sLogin failed for user.*$
ignoreregex = 
```

## Robots - Restricting search engines and web crawler traffic

Creating a `robots.txt` with the following contents at the root of your Shaarli installation will prevent _honest_ web crawlers from indexing each and every link and Daily page from a Shaarli instance, thus getting rid of a certain amount of unsollicited network traffic.

```
User-agent: *
Disallow: /
```

See:

- http://www.robotstxt.org
- http://www.robotstxt.org/robotstxt.html
- http://www.robotstxt.org/meta.html
