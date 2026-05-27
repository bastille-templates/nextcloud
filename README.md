# Nextcloud template for BastilleBSD

Template for [BastilleBSD](https://bastillebsd.org/) to run a
[Nextcloud](https://nextcloud.com) service inside of a
[FreeBSD](https://www.freebsd.org/) jail.

This template will install the official server package from the Nextcloud
website. Additionally a PostgreSQL database will be used.
Change the version by passing the `NC_VERSION` argument.

The following arguments specify externally mounted directories for data
storage:

- `APP_DIR` (external webroot for Nextcloud)
- `DATA_DIR` (external data directory for Nextcloud)
- `DB_DIR` (external directory for database server)

If you do not set them then please look up the defaults in the
[Bastillefile](Bastillefile) to be sure that the directories exist.

**Please note that you might want to set `DB_USER` and `DB_PASSWORD`
arguments for the creation of the Nextcloud DB user.**

If you put the jail beyond a nginx reverse proxy using a sub-path (like
`/my-cloud` for example) then you have to add some entries to the
`config.php` of Nextcloud:

```
...
'overwritehost' => 'external.hostname.domain',
'overwritewebroot' => '/your-sub-path',
...
```

Additionally you will need a rewrite in your proxy settings for nginx:

```
...
rewrite ^/your-sub-path(.*)$ $1 break;
...
```

## Caveat emptor

This template is intended to give you a base installation with all required
dependencies. It is not providing a fully configured instance after the run.
Therefore you need to configure the parts yourself.

## License

This program is distributed under 3-Clause BSD license. See the file
[LICENSE](LICENSE) for details.

## Usage

```
bastille create nextcloud 14.4-RELEASE YourIP-Bastille
bastille bootstrap https://github.com/bastille-templates/nextcloud
bastille template nextcloud bastille-templates/nextcloud \
  --arg APP_DIR=/mnt/nextcloud --arg DATA_DIR=/mnt/data \
  --arg DB_DIR=/opt/postgresql/data \
  --arg DB_NAME=nextcloud --arg DB_USER=nextcloud \
  --arg DB_PASSWORD=secret
```
