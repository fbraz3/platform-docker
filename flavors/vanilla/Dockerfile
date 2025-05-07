ARG PHP_VERSION=8.2
ARG BUILD_ARCH="amd64"
ARG BUILD_FLAVOR=""
FROM fbraz3/php-fpm:$PHP_VERSION$BUILD_FLAVOR
USER root

ARG PHP_VERSION
ARG BUILD_ARCH
ENV DEBIAN_FRONTEND=noninteractive
ENV ENTRYPOINT_COMMAND="/usr/bin/tail -f /var/log/nginx/*.log"

COPY ../../assets/scripts/docker-entrypoint.sh /entrypoints/2-nginx-entrypoint.sh
RUN chmod +x /entrypoints/2-nginx-entrypoint.sh

#ensure php repo is up-to-date
RUN for i in {1..10}; do \
      add-apt-repository -y ppa:ondrej/php && break || sleep 10; \
    done

RUN /usr/bin/unattended-upgrades -v;

RUN apt-get install -y nginx;

COPY ../../assets/monit/nginx /etc/monit/conf-enabled/
COPY ../../assets/nginx/default /etc/nginx/sites-enabled/default

# Fix Permissions
RUN chown -R www-data. /var/log/nginx; \
    chown -R www-data. /var/lib/nginx; \
    chown -R www-data. /etc/nginx/sites-enabled/; \
    chown -R www-data. /etc/nginx/sites-available/; \
    touch /run/nginx.pid; \
    chown www-data. /run/nginx.pid;

WORKDIR /app
USER www-data
ENTRYPOINT ["/docker-entrypoint.sh"]
