:::: details Click to view complete `docker-compose.yml` config including local agent

::: tip IMPORTANT

This configuration should work out of the box, but you must follow these steps when adding a new system in the web UI.

1. Update the `KEY` value with your public key, then run `docker compose up -d` again to restart the agent.

2. Use `host.docker.internal` as the **Host / IP**. Do not use `localhost` or `127.0.0.1`.

:::

::: code-group

```yaml [docker-compose.yml]
services:
  beszel:
    image: henrygd/beszel:latest
    container_name: beszel
    restart: unless-stopped
    extra_hosts:
      - host.docker.internal:host-gateway
    ports:
      - 8090:8090
    volumes:
      - ./beszel_data:/beszel_data

  beszel-agent:
    image: henrygd/beszel-agent:latest
    container_name: beszel-agent
    restart: unless-stopped
    network_mode: host
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    environment:
      PORT: 45876
      # Do not remove quotes around the key
      KEY: 'UPDATE WITH YOUR PUBLIC KEY (copy from "Add system" dialog)'
```

::::

:::: details Click to view instructions for running `docker compose` on Linux

::: tip NOTE
If you prefer to set up containers in a different way, please feel free to do so.
:::

1. Install [Docker](https://docs.docker.com/engine/install/) and [Docker Compose](https://docs.docker.com/compose/install/) if you haven't already.

2. Copy the `docker-compose.yml` content.

3. Create a directory somewhere to store the `docker-compose.yml` file.

```bash
mkdir beszel
cd beszel
```

4. Create a `docker-compose.yml` file, paste in the content, and save it.

::: code-group

```bash [nano]
nano docker-compose.yml
```

```bash [vim]
vim docker-compose.yml
```

```bash [emacs]
emacs docker-compose.yml
```

```bash [vscode]
code docker-compose.yml
```

:::

5. Start the service.

```bash
docker compose up -d
```

::::