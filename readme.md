Cora Docker Apache

The goal of this project is to create a Cora common image of an Apache with Shibboleth.

In order to have a fully working apache with an specific shibboleth installation it needs to be completed running systeone-docker-apache, alvin-docker-apache and diva-docker-apache

All common configurations files for shibboleth can be found in the folder docker/files/shibboleth.

No certificates are included in this phase.

Specific configurations files for shibboleth are inclueded in systeone-docker-apache, alvin-docker-apache and diva-docker-apache.

Certificates are injected during pod creation in helm. See cora-deployment

TO BE CONTINUED...