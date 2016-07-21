# dockerfiles
Various dockerfiles

## SpamAssassin

### Building image

`docker build --rm -t spamassassin .`

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
