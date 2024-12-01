0.8.0 will be released soon with better Podman support.

If anyone more familiar with Podman has any tips or suggestions for improvement, please let me know.

Note that currently this is an either-or situation. You can either use the Podman API or the Docker API, but not both at the same time. If you need both, let me know and I'll add it at some point.

### Start and enable the Podman API

This runs the REST API service as a regular user on any Linux machine with Podman installed:

```bash
systemctl --user enable podman.socket
systemctl --user start podman.socket
```

Restart the agent to allow it to connect to the Podman API.

### Permissions

You must run the agent as the same user that runs Podman.

```ini [beszel-agent.service]
# User=beszel
User=1000
```

```bash [podman run]
podman run -d --name=beszel-agent --network=host --user 1000 -v /run/user/1000/podman/podman.sock:/run/user/1000/podman/podman.sock -e KEY="..." henrygd/beszel-agent:latest
```

### Specifying a different socket path

The agent checks for the Podman socket at `/run/user/{uid}/podman/podman.sock`.

If you need to use a different path, specify it in the `DOCKER_HOST` environment variable: `DOCKER_HOST=unix:///path/to/podman.sock`.