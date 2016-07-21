FROM ubuntu:latest
MAINTAINER Pascal M. <pascal@garance.fr>

# build arguments
ARG SOURCE=http://apache.trisect.eu/spamassassin/source/Mail-SpamAssassin-3.4.1.tar.gz
ARG ARCHIVE_DIR=Mail-SpamAssassin-3.4.1
ARG CONTACT=noreply@example
ARG SPAMD_HOME=/var/lib/spamassassin

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get install -y wget libhtml-parser-perl libnet-dns-perl \
    libnet-dns-async-perl libnetaddr-ip-perl libio-socket-ssl-perl \
    libmail-dkim-perl libmail-spf-perl libdigest-sha-perl curl \
    libgeo-ip-perl razor libnet-cidr-perl libnet-cidr-lite-perl \
    liblwp-mediatypes-perl libtest-lwp-useragent-perl \
    libio-socket-ip-perl libnet-patricia-perl libdbi-perl \
    liburi-fetch-perl libencode-detect-perl make gcc \
    pyzor

RUN wget $SOURCE -O - | tar -zxC /tmp && \
    ( cd /tmp/$ARCHIVE_DIR ; \
      perl Makefile.PL CONTACT_ADDRESS=$CONTACT && \
      make && \
      make install )

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN useradd -s /sbin/nologin -c "Spamassassin" -d $SPAMD_HOME -m spamd

# start with up-to-date rules
RUN /usr/local/bin/sa-update -vvv

# create BayesDB directory
RUN mkdir /var/spamassassin && chown spamd:spamd /var/spamassassin

# copy default configuration to image
COPY local.cf /etc/mail/spamassassin/

# Pyzor
#RUN su -s /bin/sh - spamd -c "pyzor --homedir /var/lib/spamassassin discover"
RUN su -s /bin/sh - spamd -c "pyzor discover"

VOLUME ["$SPAMD_HOME"]
VOLUME ["/etc/mail/spamassassin"]

EXPOSE 783

ENTRYPOINT ["/usr/local/bin/spamd"]
CMD [ "--help" ]

