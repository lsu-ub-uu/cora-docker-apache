ARG APACHE_VERSION=2
FROM httpd:2 AS build

RUN apt-get update && \
    apt-get install -y \
	libapache2-mod-shib \
	&&	rm -rf /var/lib/apt/lists/*
	
FROM httpd:2
COPY --from=build /usr/lib/apache2/modules/mod_shib.so /usr/local/apache2/modules/

RUN apt-get update && \
    apt-get install -y supervisor lynx shibboleth-sp-utils &&\
	rm -rf /var/lib/apt/lists/*

COPY files/shibboleth/* /etc/shibboleth/ 

COPY files/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
RUN mkdir -p /var/run/shibboleth
RUN chown 100:102 /var/run/shibboleth

EXPOSE 80