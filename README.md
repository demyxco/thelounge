# thelounge
[![Build Status](https://img.shields.io/travis/demyxco/thelounge?style=flat)](https://travis-ci.org/demyxco/thelounge)
[![Docker Pulls](https://img.shields.io/docker/pulls/demyx/thelounge?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![Architecture](https://img.shields.io/badge/linux-amd64-important?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![Alpine](https://img.shields.io/badge/alpine-3.11.7-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![The Lounge](https://img.shields.io/badge/thelounge-v4.2.0-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![Buy Me A Coffee](https://img.shields.io/badge/buy_me_coffee-$5-informational?style=flat&color=blue)](https://www.buymeacoffee.com/VXqkQK5tb)
[![Become a Patron!](https://img.shields.io/badge/become%20a%20patron-$5-informational?style=flat&color=blue)](https://www.patreon.com/bePatron?u=23406156)
[![Become a Patron!](https://img.shields.io/badge/become%20a%20patron-$5-informational?style=flat&color=blue)](https://www.patreon.com/bePatron?u=23406156)

Non-root Docker image running Alpine Linux and The Lounge.

DEMYX | THELOUNGE
--- | ---
USER | demyx
ENTRYPOINT | ["thelounge", "start"]
WORKDIR | /var/opt/thelounge
PORT | 9000

## Updates & Support
[![Code Size](https://img.shields.io/github/languages/code-size/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Repository Size](https://img.shields.io/github/repo-size/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Watches](https://img.shields.io/github/watchers/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Stars](https://img.shields.io/github/stars/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Forks](https://img.shields.io/github/forks/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)

* Auto built weekly on Saturdays (America/Los_Angeles)
* Rolling release updates
* For support: [#demyx](https://webchat.freenode.net/?channel=#demyx)

## Environment Variables
These are the default environment variables.

```
- NODE_ENV=production
- THELOUNGE_HOME=/var/opt/thelounge
- PORT=9000
- TZ=America/Los_Angeles
```

## Usage
- Configured for remote VPS
- Ports 80 and 443 must be open when using Traefik
- TLS/SSL enabled by default

```
# Demyx
# https://demyx.sh
#
# Be sure to change all the domain.tld domains and credentials
#
version: "3.7"
services:
  traefik:
    image: traefik
    container_name: demyx_traefik
    restart: unless-stopped
    networks:
      - demyx
    ports:
      - 80:80
      - 443:443
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - demyx_traefik:/demyx
    environment:
      - TRAEFIK_API=true
      - TRAEFIK_PROVIDERS_DOCKER=true
      - TRAEFIK_PROVIDERS_DOCKER_EXPOSEDBYDEFAULT=false
      - TRAEFIK_ENTRYPOINTS_HTTP_ADDRESS=:80
      - TRAEFIK_ENTRYPOINTS_HTTPS_ADDRESS=:443
      - TRAEFIK_CERTIFICATESRESOLVERS_DEMYX_ACME_HTTPCHALLENGE=true
      - TRAEFIK_CERTIFICATESRESOLVERS_DEMYX_ACME_HTTPCHALLENGE_ENTRYPOINT=http
      - TRAEFIK_CERTIFICATESRESOLVERS_DEMYX_ACME_EMAIL=info@domain.tld
      - TRAEFIK_CERTIFICATESRESOLVERS_DEMYX_ACME_STORAGE=/demyx/acme.json
      - TRAEFIK_LOG=true
      - TRAEFIK_LOG_LEVEL=INFO
      - TRAEFIK_LOG_FILEPATH=/demyx/error.log
      - TRAEFIK_ACCESSLOG=true
      - TRAEFIK_ACCESSLOG_FILEPATH=/demyx/access.log
      - TZ=America/Los_Angeles
    labels:
      # traefik https://traefik.domain.tld
      - "traefik.enable=true"
      - "traefik.http.routers.traefik-http.rule=Host(`traefik.domain.tld`)"
      - "traefik.http.routers.traefik-http.service=api@internal"
      - "traefik.http.routers.traefik-http.entrypoints=http"
      - "traefik.http.routers.traefik-http.middlewares=traefik-redirect"
      - "traefik.http.routers.traefik-https.rule=Host(`traefik.domain.tld`)"
      - "traefik.http.routers.traefik-https.entrypoints=https"
      - "traefik.http.routers.traefik-https.service=api@internal"
      - "traefik.http.routers.traefik-https.tls.certresolver=demyx"
      - "traefik.http.routers.traefik-https.middlewares=traefik-auth"
      - "traefik.http.middlewares.traefik-auth.basicauth.users=demyx:$$apr1$$EqJj89Yw$$WLsBIjCILtBGjHppQ76YT1" # Password: demyx
      - "traefik.http.middlewares.traefik-redirect.redirectscheme.scheme=https"
  thelounge:
    container_name: demyx_thelounge
    image: demyx/thelounge
    restart: unless-stopped
    volumes:
      - thelounge:/var/opt/thelounge
    labels:
      # thelounge https://domain.tld
      - "traefik.enable=true"
      - "traefik.http.routers.thelounge-http.rule=Host(`domain.tld`)"
      - "traefik.http.routers.thelounge-http.entrypoints=http"
      - "traefik.http.routers.thelounge-http.middlewares=thelounge-redirect"
      - "traefik.http.routers.thelounge-https.rule=Host(`domain.tld`)"
      - "traefik.http.routers.thelounge-https.entrypoints=https"
      - "traefik.http.routers.thelounge-https.tls.certresolver=demyx"
      - "traefik.http.middlewares.thelounge-redirect.redirectscheme.scheme=https"
    networks:
      - demyx
volumes:
  demyx_traefik:
    name: demyx_traefik
  thelounge:
    name: thelounge
networks:
  demyx:
    name: demyx
```
