ARG PHP_VERSION=8.2
ARG BUILD_ARCH="amd64"
ARG BUILD_FLAVOR=""
FROM fbraz3/php-fpm:$PHP_VERSION$BUILD_FLAVOR
USER root

ARG PHP_VERSION
ARG BUILD_ARCH
ENV DEBIAN_FRONTEND=noninteractive
ENV ENTRYPOINT_COMMAND="/usr/bin/tail -f /var/log/nginx/*.log"

# TODO list
# Rearrange directory structure                | OK
# filter entrypoint to get only nginx stuff    | OK
# copy entrypoint to /entrypoints folder       | OK
# refactor dockerfile to use php-fpm image     | OK
# compile and test loccaly                     | OK
# create dockerhub repo php-nginx              | OK
# delcare platform image as php-nginx          | OK
# add code of conduct and contributing.md      | OK
# Create gh actions for build and push         | OK
# Define tests for each flavor                 | OK
# Implement tests for each flavor              | OK
# refactor README.md                           | OK
# rename github project to php-nginx           | OK
# update remote repo name in local git         | OK
# enable cron build                            |
# merge to main branch                         |
# Create deepwiki page                         |
# Update readme with deepwiki link             |
# Update dockerhub image description           |

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
