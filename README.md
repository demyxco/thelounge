# thelounge
[![Build Status](https://img.shields.io/travis/demyxco/thelounge?style=flat)](https://travis-ci.org/demyxco/thelounge)
[![Docker Pulls](https://img.shields.io/docker/pulls/demyx/thelounge?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![Architecture](https://img.shields.io/badge/linux-amd64-important?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![Alpine](https://img.shields.io/badge/alpine-3.10.3-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![The Lounge](https://img.shields.io/badge/thelounge-v3.3.0-informational?style=flat&color=blue)](https://hub.docker.com/r/demyx/thelounge)
[![Buy Me A Coffee](https://img.shields.io/badge/buy_me_coffee-$5-informational?style=flat&color=blue)](https://www.buymeacoffee.com/VXqkQK5tb)

Non-root Docker image running Alpine Linux and The Lounge.

DEMYX | THELOUNGE
--- | ---
USER | demyx
ENTRYPOINT | ["thelounge", "start"]
PORT | 9000

## Updates & Support
[![Code Size](https://img.shields.io/github/languages/code-size/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Repository Size](https://img.shields.io/github/repo-size/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Watches](https://img.shields.io/github/watchers/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Stars](https://img.shields.io/github/stars/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)
[![Forks](https://img.shields.io/github/forks/demyxco/thelounge?style=flat&color=blue)](https://github.com/demyxco/thelounge)

* Auto built weekly on Sundays (America/Los_Angeles)
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
```
docker run -d \
--name=thelounge \
--network=demyx \
-v thelounge:/var/opt/thelounge \
-l "traefik.enable=true"
-l "traefik.http.routers.thelounge-http.rule=Host(\`domain.tld\`)"
-l "traefik.http.routers.thelounge-http.entrypoints=http"
demyx/thelounge
```
