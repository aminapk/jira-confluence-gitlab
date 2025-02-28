# DevOps Platform Docker Compose Setup

## Overview

This repository provides a complete Docker Compose setup for running Atlassian Jira, Confluence, and GitLab CE with SSL termination via Nginx. The configuration is designed to be easy to deploy and customize for your organization's development workflow needs.

This setup includes:

- **Jira**: Project and issue tracking
- **Confluence**: Team collaboration and documentation
- **GitLab CE**: Source code management, CI/CD pipelines, container registry, and more
- **Nginx**: SSL termination and reverse proxy
- **PostgreSQL**: Database backend for all applications
- **Redis**: Caching and job queue for GitLab

## Prerequisites

- Docker and Docker Compose installed on your host system
- SSL certificates for your domains
- DNS configured for your domains to point to your server
- At least 8GB of RAM and 4 CPU cores recommended

## Quick Start

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/devops-platform-docker.git
   cd devops-platform-docker
   ```

2. Prepare your SSL certificates and place them in `/opt/ssl/`:
   - `jira.example.com.crt` and `jira.example.com.key`
   - `confluence.example.com.crt` and `confluence.example.com.key`
   - `gitlab.example.com.crt` and `gitlab.example.com.key`

3. Modify the domain names in `docker-compose.yml` and `nginx.conf` if needed (currently set to example.com)

4. Start the services:
   ```bash
   docker-compose up -d
   ```

5. Access your services:
   - Jira: https://jira.example.com
   - Confluence: https://confluence.example.com
   - GitLab: https://gitlab.example.com
   - GitLab SSH: ssh://git@gitlab.example.com:2222

## Configuration Details

### Domain Names

All services are configured to use subdomains of `example.com`. To use your own domain:

1. Replace all instances of `example.com` in both `docker-compose.yml` and `nginx.conf`
2. Update your SSL certificate paths accordingly

### Credentials

- **GitLab**:
  - Username: `root`
  - Password: `YourSecurePassword123` (configured in docker-compose.yml)

- **Jira and Confluence**:
  - Set up during the initial application startup

- **Databases**:
  - Jira: User: `jira`, Password: `jira_password`
  - Confluence: User: `confluence`, Password: `confluence_password`
  - GitLab: User: `gitlab`, Password: `gitlab_password`

### Ports

- 80: HTTP (redirects to HTTPS)
- 443: HTTPS
- 2222: GitLab SSH

### Persistence

All data is stored in Docker volumes:
- `jira-data`: Jira application data
- `jira-postgres-data`: Jira database data
- `confluence-data`: Confluence application data
- `confluence-postgres-data`: Confluence database data
- `gitlab-config`: GitLab configuration
- `gitlab-logs`: GitLab logs
- `gitlab-data`: GitLab application data
- `gitlab-postgres-data`: GitLab database data
- `gitlab-redis-data`: GitLab Redis data

## Security Considerations

1. **Change default passwords**: Update all default passwords in the configuration files before deploying to production.

2. **Regular backups**: Set up regular backup procedures for the Docker volumes.

3. **SSL certificates**: Use valid SSL certificates from a trusted CA for production use.

4. **Firewall rules**: Configure your firewall to only allow traffic on the required ports.

## Customization

### Resource Allocation

- Adjust the `JVM_MINIMUM_MEMORY` and `JVM_MAXIMUM_MEMORY` environment variables for Jira and Confluence based on your server's available resources.

### Email Configuration

- Add SMTP configuration to enable email notifications from the applications.

## Troubleshooting

### Logs

View logs for a specific service:
```bash
docker-compose logs -f [service-name]
```

### Container Access

Access a container's shell:
```bash
docker-compose exec [service-name] bash
```

## License

This Docker Compose setup is provided under the MIT License. The applications themselves (Jira, Confluence, and GitLab) are subject to their respective licenses.

## Acknowledgments

This configuration is inspired by the official Docker setups for Atlassian products and GitLab, adapted to work together seamlessly.
