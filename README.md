# creepypasta-backend

It's a backend.

### Run locally

Install [PostgreSQL](https://www.postgresql.org/download/) and configure settings:

- db name: `mycreepypastadb`
- user: `mycreepypastauser`
- password: `mycreepypastapassword`

after that run commands:

```bash
go get github.com/creepypasta-club/creepypasta-backend
cd $GOPATH/src/github.com/creepypasta-club/creepypasta-backend
go build
./creepypasta-backend
```

### Run in docker

```bash
docker network create --driver bridge creepypasta-network

docker run --rm \
    -v `pwd`/postgres:/var/lib/postgresql/data \
    --network creepypasta-network \
    --name creepypasta-postgres \
    -e POSTGRES_DB=mycreepypastadb \
    -e POSTGRES_USER=mycreepypastauser \
    -e POSTGRES_PASSWORD=mycreepypastapassword \
    -d postgres:10.5-alpine
    
docker build -t creepypasta .

docker run --rm \
    -e GIN_MODE=release \
    -e CREEPYPASTA_POSTGRES_HOST=`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' creepypasta-postgres` \
    -p 9000:9000 \
    --network creepypasta-network \
    --name creepypasta \
    -d creepypasta:latest .
```
