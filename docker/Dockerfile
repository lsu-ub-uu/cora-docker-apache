ARG APACHE_VERSION=2
FROM httpd:${APACHE_VERSION} AS build

RUN apt-get update && \
    apt-get install -y libapache2-mod-shib libapache2-mod-qos libapache2-mod-evasive &&\
	rm -rf /var/lib/apt/lists/*
	

FROM httpd:${APACHE_VERSION}
COPY --from=build /usr/lib/apache2/modules/mod_shib.so /usr/local/apache2/modules/
COPY --from=build /usr/lib/apache2/modules/mod_evasive.so /usr/local/apache2/modules/
COPY --from=build /usr/lib/apache2/modules/mod_qos.so /usr/local/apache2/modules/

RUN apt-get update && \
    apt-get install -y supervisor lynx shibboleth-sp-utils &&\
	rm -rf /var/lib/apt/lists/*

COPY files/shibboleth/* /etc/shibboleth/ 

COPY files/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
RUN mkdir -p /var/run/shibboleth
RUN chown 100:102 /var/run/shibboleth

EXPOSE 80