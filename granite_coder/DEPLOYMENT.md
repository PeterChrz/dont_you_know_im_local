# Granite Coder Deployment Guide

This setup uses Podman Quadlet for efficient, systemd-managed deployment of Ollama with the granite-code:3b model.

## Quick Start

### 1. Build the Container Image

```bash
podman build -t granite-coder:latest .
```

This builds the image with ollama pre-installed (only needed once).

### 2. Install the Quadlet File

Copy the Quadlet configuration to systemd:

```bash
# For user service (rootless)
mkdir -p ~/.config/containers/systemd/
cp granite-coder.container ~/.config/containers/systemd/

# For system service (rootful)
sudo mkdir -p /etc/containers/systemd/
sudo cp granite-coder.container /etc/containers/systemd/
```

### 3. Reload systemd and Start the Service

```bash
# Reload systemd to pick up the new Quadlet file
systemctl --user daemon-reload

# Enable and start the service
systemctl --user enable granite-coder.service
systemctl --user start granite-coder.service
```

**Note:** Remove `--user` if you installed as a system service.

### 4. Pull the Granite Code Model

After the service starts, pull the model via HTTP API:

```bash
curl http://localhost:11434/api/pull -d '{"name":"granite-code:3b"}'
```

This may take several minutes (the model is ~2GB).

## Managing the Service

### Check Status
```bash
systemctl --user status granite-coder.service
```

### View Logs
```bash
journalctl --user -u granite-coder.service -f
```

### Restart Service
```bash
systemctl --user restart granite-coder.service
```

### Stop Service
```bash
systemctl --user stop granite-coder.service
```

### Disable Auto-Start
```bash
systemctl --user disable granite-coder.service
```

## Using Ollama

### Test the Service
```bash
curl http://localhost:11434/api/generate -d '{
  "model": "granite-code:3b",
  "prompt": "Write a hello world function in Python",
  "stream": false
}'
```

### List Available Models
```bash
curl http://localhost:11434/api/tags
```

### Interactive Chat (if ollama CLI is installed on host)
```bash
podman exec -it granite-coder ollama run granite-code:3b
```

## Architecture

- **Containerfile**: Builds image with ollama pre-installed (Fedora base from quay.io)
- **granite-coder.container**: Quadlet file that creates a systemd service
- **Volume**: `ollama-data` persists models and configuration
- **Port**: 11434 exposed for HTTP API access

## Benefits

- **Fast restarts**: Ollama is pre-installed, container starts in seconds
- **Systemd integration**: Auto-start on boot, automatic restarts on failure
- **Persistent storage**: Models stored in named volume, survive container restarts
- **Efficient**: Ollama runs as PID 1, minimal overhead
- **Portable**: Container can be moved to any system with Podman

## Troubleshooting

### Service won't start
```bash
journalctl --user -u granite-coder.service -n 50
```

### Rebuild image after changes
```bash
podman build -t granite-coder:latest .
systemctl --user restart granite-coder.service
```

### Remove everything and start fresh
```bash
systemctl --user stop granite-coder.service
systemctl --user disable granite-coder.service
podman volume rm ollama-data
podman rmi granite-coder:latest
```
