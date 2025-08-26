# Cora Docker Apache

The goal of this project is to create **a common Docker image** of *Apache with Shibboleth* for Cora.  
This image acts as the foundation for system-specific Apache + Shibboleth images.

The base image (`cora-docker-apache`) provides a fully working Apache setup with Shibboleth.  
It is extended by three system-specific images:

- **systeone-docker-apache**  
- **alvin-docker-apache**  
- **diva-docker-apache**

## Common Shibboleth Configuration
All **common configuration files** for Shibboleth are included directly in this repository under: docker/files/shibboleth.

## Project-Specific Shibboleth Configurations
Each project (`systeone`, `alvin`, and `diva`) includes its own specific Shibboleth configuration files, extending the common base.

To main files are included here:
- **httpd.conf** : Specific apache configuration for the project
- **shibboleth2.xml** Specific shibboleth configuration for the project. All the members shibboleths endpoints are included here.

An specifict shibboleth2.xml and an apache routing configuration is intended to be 

## Certificates
Certificates are **not included in the Docker image**.  
They are injected dynamically during pod creation via **Helm**. See cora-deployment


