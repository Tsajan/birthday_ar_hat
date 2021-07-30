FROM debian:buster-slim

ARG FREP_VERSION=1.3.11
ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2

COPY rootfs /

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y apt-transport-https apt-utils ca-certificates gnupg wget && \
	apt-dpkg-wrap apt-get install -y jq procps curl vim iputils-ping net-tools && \
  apt-dpkg-wrap apt-get install -y --allow-unauthenticated cron nginx-extras apache2-utils && \
	wget -qO - https://github.com/just-containers/s6-overlay/releases/download/v1.22.1.0/s6-overlay-amd64.tar.gz | tar xfz - -C / && \
  wget -q https://github.com/subchen/frep/releases/download/v$FREP_VERSION/frep-$FREP_VERSION-linux-amd64 -O /usr/bin/frep && \
	apt-cleanup && \
	rm -f /etc/nginx/conf.d/default.conf && \
	rm -rf /tmp/pkg /var/cache/apt && \
	chmod +x /usr/bin/frep

USER root

ADD https://raw.githubusercontent.com/certbot/certbot/v1.9.0/certbot-auto /usr/local/bin/

RUN \
  chmod a+x /usr/local/bin/certbot-auto && \
  certbot-auto --no-self-upgrade --noninteractive --install-only
  
RUN echo "set encoding=utf-8" >> /etc/vim/vimrc

EXPOSE 80 443

ENTRYPOINT ./init
