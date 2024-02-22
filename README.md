# Docker-compose configuration
## Prerequisites

- Docker v20.10+
- Docker-compose 2.x.x+
- Running Ethereum JSON RPC client, preferably archive node:
```bash
docker run -p 8547:8547 -p 8548:8548 -v ./run.sh:/home/user/run.sh ghcr.io/fhenixprotocol/localfhenix:v0.1.0-beta5
# make a transaction, empty blockchain may not work
```

## Building Docker containers from source

```bash
docker-compose --profile blockscout up
```

**Note**: if you don't need to make backend customizations, you can run `docker-compose up` in order to launch from pre-build backend Docker image. This will be much faster.

This command uses `docker-compose.yml` by-default, which builds the backend of the explorer into the Docker image and runs 9 Docker containers:

- Postgres 15.x database, which will be available at port 7432 on the host machine.
- Redis database of the latest version.
- Blockscout backend with api at /api path.
- Nginx proxy to bind backend, frontend and microservices.
- Blockscout explorer at http://localhost.

and 4 containers for microservices (written in Rust):

- [Stats](https://github.com/blockscout/blockscout-rs/tree/main/stats) service with a separate Postgres 14 DB.
- [Sol2UML visualizer](https://github.com/blockscout/blockscout-rs/tree/main/visualizer) service.
- [Sig-provider](https://github.com/blockscout/blockscout-rs/tree/main/sig-provider) service.

**Note for Linux users**: Linux users need to run the local node on http://0.0.0.0/ rather than http://127.0.0.1/
