FROM debian:stretch

MAINTAINER Florent DESPIERRES <orions07@gmail.com>

# Copy all scripts
COPY ./image/scripts/*.sh /tmp/
COPY ./image/7.1/*.sh /tmp/
RUN chmod +x /tmp/*.sh

# Run all scripts
RUN sh /tmp/env.sh && \
    sh /tmp/common-tools.sh && \
    apt-get  install --no-install-recommends -qy gnupg && \
    sh /tmp/apt.sh && \
    sh /tmp/js-tools.sh && \
    sh /tmp/composer.sh && \
    sh /tmp/clean.sh \

# Config
COPY ./image/config/php.ini /etc/php/7.1/cli/conf.d/50-setting.ini
COPY ./image/config/pool.conf /etc/php/7.1/fpm/pool.d/www.conf
COPY ./image/config/nginx.conf /etc/nginx/nginx.conf
COPY ./image/7.1/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY ./image/config/ssl/ /etc/ssl/nginx/
COPY ./image/config/.bashrc /root/.bashrc

# Run supervisor
EXPOSE 80 443

COPY ./image/config/motd.head /etc/motd.head
COPY ./image/config/phpdocker-release /etc/phpdocker-release
COPY ./image/config/50-phpdocker /etc/update-motd.d/50-phpdocker
RUN rm -f /etc/motd && chmod +x /etc/update-motd.d/* && ln -s /etc/update-motd.d/50-phpdocker /etc/motd

WORKDIR /app

CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
