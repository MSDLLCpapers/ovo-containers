# OVO Containers

This repository contains Docker container definitions for OVO pipeline modules.

## Building Docker Containers

To build all containers, run:
```bash
docker compose build
```

To build a specific container, use:
```bash
docker compose build container-name

# for example, to build ovo-rfdiffusion:
docker compose build rfdiffusion
```
