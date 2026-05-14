# Home SIEM Lab - Architecture Documentation

## System Overview

The Home SIEM Lab follows a classic ELK stack architecture (Elasticsearch, Logstash, Kibana) with containerized components.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     Log Sources                              │
│  ┌─────────────┬─────────────┬─────────────┬──────────────┐ │
│  │   Servers   │  Firewalls  │  Endpoints  │  Applications│ │
│  └──────┬──────┴──────┬──────┴──────┬──────┴────────┬─────┘ │
└─────────┼─────────────┼─────────────┼────────────────┼───────┘
          │             │             │                │
          └─────────────┴─────────────┴────────────────┘
                        │
                        ▼
          ┌─────────────────────────┐
          │  Log Forwarders/Agents  │
          │  - Filebeat             │
          │  - Metricbeat           │
          │  - Syslog Agents        │
          └────────────┬────────────┘
                       │
              ┌────��───▼────────┐
              │   UDP/TCP 5000  │
              └────────┬────────┘
                       │
          ┌────────────▼────────────┐
          │  LOGSTASH (Processing)  │
          │  ┌────────────────────┐ │
          │  │ Input Processing   │ │
          │  │ - Parse logs       │ │
          │  │ - Extract fields   │ │
          │  │ - Normalize data   │ │
          │  └────────────────────┘ │
          │  ┌────────────────────┐ │
          │  │ Filter Processing  │ │
          │  │ - Grok patterns    │ │
          │  │ - Conditional      │ │
          │  │ - Enrichment       │ │
          │  └────────────────────┘ │
          │  ┌────────────────────┐ │
          │  │ Output Routing     │ │
          │  │ - Elasticsearch    │ │
          │  │ - Files            │ │
          │  │ - Alerts           │ │
          │  └────────────────────┘ │
          └────────────┬────────────┘
                       │
              ┌────────▼────────┐
              │   Port 9200     │
              └────────┬────────┘
                       │
        ┌──────────────▼───────────────┐
        │  ELASTICSEARCH (Storage)     │
        │  ┌───────────────────────┐  │
        │  │ Index Management      │  │
        │  │ - Sharding            │  │
        │  │ - Replication         │  │
        │  │ - Lifecycle Policy    │  │
        │  └───────────────────────┘  │
        │  ┌───────────────────────┐  │
        │  │ Full-Text Search      │  │
        │  │ - Inverted Index      │  │
        │  │ - Query DSL           │  │
        │  │ - Aggregations        │  │
        │  └───────────────────────┘  │
        │  ┌───────────────────────┐  │
        │  │ Data Persistence      │  │
        │  │ - Persistent volumes  │  │
        │  │ - Recovery            │  │
        │  └───────────────────────┘  │
        └──────────────┬───────────────┘
                       │
              ┌────────▼────────┐
              │   Port 5601     │
              └────────┬────────┘
                       │
          ┌────────────▼────────────┐
          │  KIBANA (Visualization) │
          │  ┌────────────────────┐ │
          │  │ Dashboards         │ │
          │  │ - Real-time charts │ │
          │  │ - Custom widgets   │ │
          │  │ - KPIs             │ │
          │  └────────────────────┘ │
          │  ┌────────────────────┐ │
          │  │ Discover           │ │
          │  │ - Log exploration  │ │
          │  │ - Field analysis   │ │
          │  └────────────────────┘ │
          │  ┌────────────────────┐ │
          │  │ Alerts & Rules     │ │
          │  │ - Alert triggers   │ │
          │  │ - Notifications    │ │
          │  └────────────────────┘ │
          └─────────────────────────┘
                       │
                       ▼
              ┌──────────────┐
              │  End Users   │
              └──────────────┘
```

## Components

### 1. Log Sources

These are systems generating logs:
- **Servers**: Linux/Windows servers (syslog, eventlogs)
- **Network Devices**: Firewalls, routers, switches
- **Applications**: Custom applications, databases
- **Endpoints**: Workstations, mobile devices

### 2. Log Forwarders

Agents that collect and send logs:
- **Filebeat**: Lightweight file log shipper
- **Metricbeat**: Metrics collection agent
- **Syslog**: Standard syslog protocol
- **Custom Agents**: Application-specific collectors

**Transport**: UDP/TCP on port 5000

### 3. Logstash (Processing Layer)

Central processing engine:

**Input**:
- Receives logs from forwarders
- Listens on port 5000
- Supports multiple protocols

**Filter**:
- Parses unstructured data
- Extracts fields using Grok patterns
- Enriches events with context
- Applies conditional logic
- Normalizes data formats

**Output**:
- Sends to Elasticsearch
- Can route to multiple destinations
- Applies buffering and retry logic

### 4. Elasticsearch (Data Store)

Scalable search and analytics engine:

**Storage**:
- Distributed document store
- Full-text search indexing
- Time-series data optimization

**Features**:
- Horizontal scalability through sharding
- High availability through replication
- Index lifecycle management
- Automatic data retention policies

**Port**: 9200 (REST API)

### 5. Kibana (Visualization)

Data visualization and exploration:

**Capabilities**:
- Real-time dashboards
- Ad-hoc log exploration
- Custom visualizations
- Alert creation and management
- User authentication

**Port**: 5601

## Data Flow

1. **Collection**: Agents collect logs from source systems
2. **Transmission**: Logs sent to Logstash via network (UDP/TCP)
3. **Processing**: Logstash parses and enriches logs
4. **Indexing**: Processed data indexed in Elasticsearch
5. **Visualization**: Kibana queries and displays data
6. **Alerting**: Rules trigger on specific conditions

## Network Architecture

```yaml
Network: siem-network (Bridge)

Services:
  elasticsearch: 9200
  logstash: 5000, 9600
  kibana: 5601

Connections:
  logstash -> elasticsearch (internal)
  kibana -> elasticsearch (internal)
```

## Storage

- **Elasticsearch Data**: Docker volume (persistent)
- **Logstash Config**: Mounted from host
- **Logs Directory**: Mounted from host (read-only)

## Scalability Considerations

### Horizontal Scaling
- Add multiple Elasticsearch nodes
- Configure Logstash pipelines
- Implement load balancing

### Vertical Scaling
- Increase container memory
- Optimize JVM settings
- Increase storage capacity

## Security Considerations

1. **Network**:
   - Isolate containers in private network
   - Use firewall rules
   - Implement VPN access

2. **Authentication**:
   - Enable X-Pack security
   - Implement role-based access control
   - Use API keys for service accounts

3. **Data Protection**:
   - Enable encryption at rest
   - Use TLS for transit
   - Implement data retention policies

4. **Auditing**:
   - Enable audit logging
   - Monitor access logs
   - Track configuration changes

## Performance Optimization

1. **Elasticsearch**:
   - Tune JVM heap size
   - Optimize index settings
   - Implement shard allocation awareness

2. **Logstash**:
   - Configure filter performance
   - Implement batching
   - Optimize grok patterns

3. **Kibana**:
   - Cache frequently used visualizations
   - Optimize dashboard queries
   - Implement index patterns efficiently

## Disaster Recovery

1. **Backups**:
   - Implement snapshot policies
   - Store backups off-site
   - Test recovery procedures

2. **Redundancy**:
   - Multi-node Elasticsearch clusters
   - Load balancing for failover
   - Replication across zones

## Maintenance

1. **Updates**:
   - Plan update windows
   - Test in development
   - Document changes

2. **Monitoring**:
   - Monitor resource usage
   - Track log ingestion rates
   - Monitor storage growth

3. **Optimization**:
   - Regular index maintenance
   - Archive old logs
   - Optimize queries
