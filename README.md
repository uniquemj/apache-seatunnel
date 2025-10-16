# Apache SeaTunnel Setup with Docker

This guide explains how to run Apache SeaTunnel using Docker with a sample configuration.

## Prerequisites

- Docker installed on your machine
- `docker-compose` installed

## Steps

### 1. Pull the SeaTunnel Docker Image

```bash
docker pull apache/seatunnel:2.3.12
````

### 2. Navigate to Your Configuration Folder

Go to the folder where your SeaTunnel configuration files (`.yaml`) and folder (`config`) are present:

```bash
cd /path/to/your/config/folder
```

### 3. Start Docker Compose

Run the following command to start SeaTunnel using Docker Compose:

```bash
docker-compose -f seatunnel-docker.yaml up -d
```

### 4. Run SeaTunnel Job

Run your SeaTunnel job using the configuration file:

```bash
docker run --rm -it \
  --network=learn-seatunnel_custom_network \
  -v /<path-to-your-config-file>/:/config \
  apache/seatunnel:2.3.12 \
  ./bin/seatunnel.sh -m local -c /config/mongo-batch.conf
```

Use this for `startup.mode ='Timestamp'`:
```bash
  docker run --rm -it --network=learn-seatunnel_custom_network -e STARTUP_TIMESTAMP=$(date +%s%3N) -v /<path-to-your-config-file>/:/config apache/seatunnel:2.3.12 ./bin/seatunnel.sh -m local -c /config/mongo_cdc-stream.conf
```

Example:
```bash
docker run --rm -it \
  --network=learn-seatunnel_custom_network \
  -v /mnt/e/learn/learn-seatunnel/config/:/config \
  apache/seatunnel:2.3.12 \
  ./bin/seatunnel.sh -m local -c /config/mongo-batch.conf
```

**Explanation of flags:**

* `--rm`: Remove the container after it exits
* `-it`: Interactive mode with terminal
* `--network`: Use the custom Docker network for SeaTunnel
* `-v`: Mount local configuration folder into the container
* `-m local`: Run in local mode
* `-c /config/mongo-batch.conf`: Path to your job configuration file inside the container



