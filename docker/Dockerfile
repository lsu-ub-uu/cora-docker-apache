ARG APACHE_VERSION=2.4.62
FROM httpd:${APACHE_VERSION} AS build

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
	libapache2-mod-shib \
	&& rm -rf /var/lib/apt/lists/*

FROM httpd:${APACHE_VERSION}

LABEL org.opencontainers.image.source="https://github.com/lsu-ub-uu/cora-docker-apache" \
      org.opencontainers.image.description="Cora Docker Apache with Shibboleth" \
      org.opencontainers.image.licenses="GPL-3.0"

COPY --from=build /usr/lib/apache2/modules/mod_shib.so /usr/local/apache2/modules/

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
	supervisor \
	shibboleth-sp-utils \
	&& rm -rf /var/lib/apt/lists/*

COPY files/shibboleth/ /etc/shibboleth/

COPY files/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
RUN mkdir -p /var/run/shibboleth /var/log/supervisor && \
    chown 100:102 /var/run/shibboleth

COPY files/apache/coraConfig.conf /usr/local/apache2/conf/extra/coraConfig.conf
RUN echo "Include conf/extra/coraConfig.conf" >> /usr/local/apache2/conf/httpd.conf

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD httpd -t || exit 1

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]