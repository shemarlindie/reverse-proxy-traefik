# Reverse Proxy using [Traefik](https://github.com/traefik/traefik)
Traefik template for hosting multiple containers on the same port via reverse proxy.

## Overview
- Uses the Docker version of Traefik (in `./reverse-proxy`)
- Includes two sample apps running a basic httpd server for testing (in `./site1`, `./site2`)
- Configured for secure dashboard and apps with auto-generated SSL certs
- Cert config is accessible via volume in `./reverse-proxy/letsencrypt/acme.json` when the container has been run

## Setup

#### Install `htpasswd`
https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-apache-on-ubuntu-18-04-quickstart

#### Generate dashboard users
> htpasswd reverse-proxy/.users admin

Follow the prompts to generate the admin user. Repeat for any additional users required.

#### Configure `reverse-proxy/docker-compose.yml`
- Cert generation email (line  15)
- Dashboard domain (line 23)

#### Debug Config
While debugging uncomment the following in `reverse-proxy/docker-compose.yml`:
- Line 9 `# - "--log.level=DEBUG"`: to show more verbose log messages
- Line 21 `# - "--certificatesresolvers.myresolver.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory"`: to use letsencrypt staging server for test certificates

They are uncommented by default. Remove or comment them again when you're ready to deploy.

#### Configure each app's `docker-compose.yml`
- Domain (`site1/docker-compose.yml` line 8)

Repeat for each additional app.

**NOTE:** You can modify each app's `Dockerfile` and `docker-compose.yml` as long as they expose the port correctly and the Traefik labels are maintained and configured.

## Run

#### Start Traefik container
> cd ./reverse-proxy
> docker-compose up -d --build

> docker-compose logs -f

#### Start apps
> cd ./site1
> docker-compose up -d --build

> cd ./site1
> docker-compose up -d --build
