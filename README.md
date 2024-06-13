# Dip Unleash

[Unleash](https://hub.docker.com/r/unleashorg/unleash-server) configuration for [Dip infra services](https://github.com/bibendi/dip).

## Usage

1. Add both the nginx service and the unleash service to the application's `dip.yml`.

```yaml
infra:
  nginx:
    git: https://github.com/bibendi/dip-nginx.git
  unleash:
    git: https://github.com/charism/dip-unleash.git
```

2. Add the unleash and nginx networks to the application's `docker-compose.yml`.

```yaml
networks:
  nginx-net:
    name: ${DIP_INFRA_NETWORK_NGINX}
    external: true
  unleash-net:
    name: ${DIP_INFRA_NETWORK_UNLEASH}
    external: true
```

Where `DIP_INFRA_NETWORK_UNLEASH` and `DIP_INFRA_NETWORK_NGINX` are special variables that are set by Dip.

3. Add env variables and unleash configuration as needed to a Docker Compose service. By default, `dip-unleash` will expose the unleash service UI at `http://unleash.localhost` via the nginx proxy. 

```yaml
services:
  environment:
    UNLEASH_KEY: default:development.unleash-insecure-api-token
    UNLEASH_URL: http://unleash.localhost/api/
  app:
    networks:
      - default
      - nginx-net
      - unleash-net
```

4. Start infra services.

```sh
dip infra up
```

5. Start the app

```sh
dip up
```

6. Ensure your app connects to the unleash server
