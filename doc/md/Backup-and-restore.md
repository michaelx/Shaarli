## Backup and restore

All data and configuration is kept in the `data` directory. Backup this directory: 

```bash
rsync -avzP my.server.com:/var/www/shaarli.mydomain.org/data /home/me/backups/shaarli-data-$(date +%Y-%m-%d_%H%M)
```

It is strongly recommended to do periodic, automatic backups to a sperate machine. You can automate the command above using a cron job or full-fetured bacvkup solutions such as [rsnapshot](https://rsnapshot.org/)

To restore a backup, simply put back the `data/` directory in place, owerwriting any existing files.