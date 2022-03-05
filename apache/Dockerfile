ARG DOCKER_APACHE_VERSION

FROM httpd:${DOCKER_APACHE_VERSION} as tmplt
RUN set -eux; \
    apt update && \
    apt install -y openssl && \
    rm -rf /var/lib/apt/lists/*;
RUN mkdir -p /usr/share/TLS
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /usr/share/TLS/server.key \
    -out /usr/share/TLS/server.crt \
    -subj "/CN=dev.local"

FROM httpd:${DOCKER_APACHE_VERSION} as base
COPY vhosts.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf
COPY httpd.conf /usr/local/apache2/conf/httpd.conf
RUN mkdir -p /usr/local/apache2/TLS/
RUN mkdir -p /var/www/html/; \
    chown www-data:www-data -R /var/www/

FROM base as httpd-dev
COPY --from=tmplt /usr/share/TLS/server.key /usr/local/apache2/TLS/server.key
COPY --from=tmplt /usr/share/TLS/server.crt /usr/local/apache2/TLS/server.crt
COPY dev.conf /usr/local/apache2/conf/extra/dev.conf
RUN echo "Include conf/extra/dev.conf" >> /usr/local/apache2/conf/httpd.conf;

FROM base as httpd-prod