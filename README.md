develop on digital ocean

see [frizinak/provision.sh](https://github.com/frizinak/provision.sh) for more info

wont work out of the box yet, will try to get to it soon.

if you don't mind tinkering you could change `provdir` to the dir where you
cloned frizinak/provision.sh.

# in short:

wraps provision.sh to create a droplet and provision it

creates and manages a systemd-inotify-rsync user service to upload files to the droplet

(seems to be the fastest way to sync files out of all methods i've tried, except for nfs which is way to inconvenient, you know ports and stuff. Seemingly faster or identical performance to my local vagrant+nfs setup ymmv)

a few drupal8 convenience cmds, but should be application agnostic

# usage:

put a .drp file in your repo root (bash syntax fwiw)

```
sitekey=          # required
db=               # required
droplet_size=     # required
# db_ignore=      # optional list of tables to ignore when mysqldumping
# host=           # optional defaults to $sitekey.
# files=''        # optional list of paths that are downloaded with 'drp files'.
# image=          # optional image to create the droplet from, defaults to 'php-7-dev-ubuntu-16'.
# provisioners=   # optional comma separated set of provisioners. defaults to 'php7:seven'.
# docroot=        # optional docroot name, depends on '$provisioners'.
# php=            # optional php binary that will be used for php related commands (drush).
# drupal_version= # optional drupal version, defaults to 8.
# ignore=         # optional set of rsync --excludes.
# packages=       # optional set of apt packages to install after provisioning.
# make=           # optional command to run when 'drp make' is called.
# make_admin=     # optional command to run when 'drp make_admin' is called.
```

e.g.:

```
sitekey=my-d7-site
db=d7db
droplet_size=512mb
php=php5.6
drupal_version=7
files='sites/default/files sites/all/modules/features'
image=php-5-dev-ubuntu-16    # this is a custom snapshot specific to my account
                             # only to speed things up, perfectly viable to use
                             # a do stock image: e.g.: ubuntu-16-04-x64
provisioners=php5:five
docroot=five/web
db_ignore='cache
cache_form'                  # The table definitions will be exported but data will be skipped
```

run

- `drp init` create and provision without apt-get update
- `drp init -u` apt-get update
- `drp init -uu` apt-get update && apt-get upgrade

you will be prompted for the 'admin user' which atm is `asdf` (as configured in provision.sh/src/provision/config.sh)

you will encounter no bugs or inconveniences as this script is impeccable...

`drp dump` and `drp import-db` to respectively dump and import databases to/from {repo-root}/drp/dumps/dbdump-YYYY-MM-DD--HH-II.sql.gz

ok whatever just do `drp -h`

;)
