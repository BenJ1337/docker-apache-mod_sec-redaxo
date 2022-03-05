FROM ubuntu:20.04 as tmplt
RUN set -eu; \
    apt update && \
    apt install -y openssl && \
    rm -rf /var/lib/apt/lists/*;
RUN mkdir -p /usr/share/TLS
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /usr/share/TLS/server.key \
    -out /usr/share/TLS/server.crt \
    -subj "/CN=dev.local"

ARG DOCKER_MOD_SEC_IMAGE

FROM owasp/modsecurity:${DOCKER_MOD_SEC_IMAGE} as mod_sec_prod
#RUN sed -i 's/\${SERVER_NAME}/dev.local/g' /etc/nginx/conf.d/default.conf; 
RUN mkdir -p /etc/nginx/TLS/

COPY --from=tmplt /usr/share/TLS/server.key /etc/nginx/TLS/server.key
COPY --from=tmplt /usr/share/TLS/server.crt /etc/nginx/TLS/server.crt