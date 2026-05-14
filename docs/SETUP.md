# Home SIEM Lab - Setup Guide

This guide provides step-by-step instructions for setting up the Home SIEM Lab.

## Prerequisites

- **Docker**: Version 20.10+ ([Install Docker](https://docs.docker.com/engine/install/))
- **Docker Compose**: Version 1.29+ ([Install Docker Compose](https://docs.docker.com/compose/install/))
- **Hardware Requirements**:
  - Minimum 4GB RAM
  - 20GB free disk space
  - Dual-core processor or better
- **Network**: Stable internet connection

## Installation Steps

### Step 1: Clone the Repository

```bash
git clone https://github.com/bharadwajagraharam-98/Home-SEIM-LAB.git
cd Home-SEIM-LAB
```

### Step 2: Create Required Directories

```bash
mkdir -p configs logs data
```

### Step 3: Verify Docker Installation

```bash
docker --version
docker-compose --version
```

### Step 4: Start the SIEM Stack

```bash
docker-compose up -d
```

This command will:
- Pull the necessary Docker images
- Create and start all containers
- Set up the network and volumes

### Step 5: Verify Services are Running

```bash
docker-compose ps
```

All services should show `Up` status.

### Step 6: Wait for Services to Initialize

Allow 30-60 seconds for all services to fully initialize.

```bash
# Check Elasticsearch health
curl http://localhost:9200/_cluster/health

# Check Kibana status
curl http://localhost:5601/api/status
```

## Accessing the Lab

### Kibana Dashboard
- **URL**: http://localhost:5601
- **Default User**: elastic
- **Default Password**: changeme

### Elasticsearch API
- **URL**: http://localhost:9200
- **API Endpoint**: `/_cluster/health`

## Configuration

### Logstash Configuration

Edit `configs/logstash.conf` to configure log inputs and filters:

```conf
input {
  # Input sources here
}

filter {
  # Processing rules here
}

output {
  # Output destinations here
}
```

### Docker Compose Customization

Modify `docker-compose.yml` to:
- Change port mappings
- Adjust memory allocation
- Add additional services

## Common Commands

### Start the Lab
```bash
docker-compose up -d
```

### Stop the Lab
```bash
docker-compose down
```

### View Container Logs
```bash
docker-compose logs -f [service-name]
```

### Rebuild Containers
```bash
docker-compose up -d --build
```

### Remove All Data (Hard Reset)
```bash
docker-compose down -v
```

## Troubleshooting

### Port Already in Use

If you get a "port already in use" error:
```bash
# Find process using the port
lsof -i :5601

# Kill the process
kill -9 [PID]
```

### Elasticsearch Not Starting

```bash
# Check logs
docker-compose logs elasticsearch

# Increase virtual memory
sudo sysctl -w vm.max_map_count=262144
```

### Memory Issues

Edit `docker-compose.yml` and adjust:
```yaml
environment:
  - "ES_JAVA_OPTS=-Xms256m -Xmx256m"
```

## Next Steps

1. Review the [Architecture Guide](ARCHITECTURE.md)
2. Configure log sources
3. Set up Logstash filters
4. Create Kibana dashboards
5. Configure alerts and notifications

## Support

For issues or questions:
1. Check the logs: `docker-compose logs`
2. Review the troubleshooting section above
3. Open an issue on GitHub
