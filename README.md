# dockerfiles
Various dockerfiles

## SpamAssassin

This Dockerfile creates a container image with an installation of SpamAssassin
from sources.

### Building image

`docker build --rm -t spamassassin .`

You can customize the build process using [ARG variables](https://docs.docker.com/engine/reference/commandline/build/#/set-build-time-variables-build-arg), for example :
```
docker build --build-arg CONTACT=me@example.net SPAMD_HOME=/home/spamd --rm -t spamassassin .
```

The container has two volumes:
- `/var/lib/spamassassin` (defined by `$SPAMD_HOME variable) for Bayes tokens database and various files
- `/etc/mail/spamassassin` for the configuration files

### Run

Docker compose example :
```
spamassassin:
  image: spamassassin
  restart: "unless-stopped"
  hostname: sa
  domainname: EXAMPLE.NET
  ports:
    - "783:783"
  command: "-4 -A <CLIENT-IP> -u spamd -l -i --max-children=10 --min-children=5 -x --syslog=stderr"
```
