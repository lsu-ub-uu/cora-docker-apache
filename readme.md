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

## Production Deployment

### Building for Production

```bash
docker compose -f docker-compose.prod.yml up -d --build
```

### Key Production Settings

The production configuration (`docker-compose.prod.yml`) includes:

- **Restart policy**: `unless-stopped` ensures containers restart after failures or host reboots
- **Health checks**: Apache config syntax validation runs every 30 seconds
- **Log rotation**: JSON file logging limited to 5 files of 10 MB each
- **Resource limits**: 512 MB memory / 1 CPU (adjustable per environment)
- **Read-only filesystem**: Container filesystem is read-only with writable tmpfs mounts for required paths
- **Security**: `no-new-privileges` prevents privilege escalation

### Environment-specific Overrides

Certificates and system-specific Shibboleth configurations (shibboleth2.xml, httpd.conf) are provided by downstream images (systeone, alvin, diva) and injected via Helm at deploy time.

### Performance Tuning

| Setting | Default | Notes |
|---------|---------|-------|
| `DeflateCompressionLevel` | 6 | Balanced CPU/compression ratio (1-9 scale) |
| Memory limit | 512 MB | Adjust based on traffic; monitor with `docker stats` |
| CPU limit | 1.0 | Adjust based on host capacity |
| Log max-size | 10 MB × 5 files | Increase for high-traffic environments |

## Container Security Checklist

- [x] Base image pinned to specific version (`httpd:2.4.62`)
- [x] No unnecessary packages in runtime image (removed `nano`)
- [x] Build artifacts cleaned (`rm -rf /var/lib/apt/lists/*`)
- [x] `no-new-privileges` security option enabled in production compose
- [x] Read-only root filesystem with explicit writable mounts
- [x] Security headers configured (X-Content-Type-Options, X-Frame-Options, Referrer-Policy)
- [x] Server tokens and signatures hidden (`ServerTokens Prod`, `ServerSignature Off`)
- [x] TRACE method disabled (`TraceEnable Off`)
- [x] No secrets or credentials in image (certificates injected via Helm)
- [x] Health check configured for automated recovery
- [x] Resource limits defined to prevent resource exhaustion
- [x] Log rotation configured to prevent disk exhaustion
- [x] Only port 80 exposed (TLS termination handled upstream)
