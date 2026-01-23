FROM docker.io/library/debian:trixie@sha256:5cf544fad978371b3df255b61e209b373583cb88b733475c86e49faa15ac2104

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
  && apt-get -y --no-install-recommends install bind9 bind9utils dnsutils \
  && rm -rf /var/lib/apt/lists/*

COPY named.conf.local named.conf.options.template /etc/bind/
COPY db.example.com /var/lib/bind/
COPY nsupdate.txt /bind3/etc/bind/
COPY configure_bind.sh run_bind.sh /etc/bind/
RUN chmod +x /etc/bind/configure_bind.sh /etc/bind/run_bind.sh
RUN /etc/bind/configure_bind.sh

CMD ["/etc/bind/run_bind.sh"]
