# Compiling

Both the hub and agent are written in Go, so you can easily build them yourself, or cross-compile for different platforms. Please [install Go](https://go.dev/doc/install) first if you haven't already.

## Using Makefile

Run `make` in `/beszel`. This creates a `build` directory containing the binaries.

### Agent & Hub

```bash
# Builds both the agent and hub
make
# Builds the agent only
make build-agent
# Builds the hub only
make build-hub
```

You can also build for different platforms:

```bash
make OS=freebsd ARCH=arm64
```

See a list of valid options by running `go tool dist list`.

## Manual compilation

### Prepare dependencies

```bash
cd beszel && go mod tidy
```

### Agent

Go to `beszel/cmd/agent` and run the following command to create a binary in the current directory:

```bash
CGO_ENABLED=0 go build -ldflags "-w -s" .
```

### Hub

The hub embeds the web UI in the binary, so you must build the website first. I use [Bun](https://bun.sh/), but you may use Node.js if you prefer:

```bash
cd beszel/site
bun install
bun run build
```

Then in `beszel/cmd/hub`:

```bash
CGO_ENABLED=0 go build -ldflags "-w -s" .
```

### Cross-compiling

You can cross-compile for different platforms using the `GOOS` and `GOARCH` environment variables.

For example, to build for FreeBSD ARM64:

```bash
GOOS=freebsd GOARCH=arm64 CGO_ENABLED=0 go build -ldflags "-w -s" .
```

See a list of valid options by running `go tool dist list`.