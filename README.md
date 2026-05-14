# Home SIEM Lab

A comprehensive Security Information and Event Management (SIEM) lab setup for learning and experimentation with security monitoring, log aggregation, and threat detection.

## 📋 Overview

This project demonstrates a home-based SIEM infrastructure for security monitoring and log analysis. It covers log collection, centralization, analysis, and visualization of security events.

## 🎯 Objectives

- Set up a centralized log management system
- Collect logs from multiple sources
- Monitor and analyze security events in real-time
- Create custom alerts and dashboards
- Practice incident response and threat detection

## 🏗️ Architecture

```
┌─────────────────┐
│  Log Sources    │
│  - Servers      │
│  - Firewalls    │
│  - Endpoints    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Log Forwarders │
│  - Agents       │
│  - Collectors   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  SIEM Platform  │
│  - Aggregation  │
│  - Parsing      │
│  - Analysis     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Dashboards &   │
│  Visualization  │
└─────────────────┘
```

## 🚀 Getting Started

### Prerequisites

- Docker and Docker Compose
- Git
- Basic understanding of SIEM concepts
- 4GB+ RAM available for lab environment

### Quick Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/bharadwajagraharam-98/Home-SEIM-LAB.git
   cd Home-SEIM-LAB
   ```

2. **Review the documentation**
   - See `docs/` directory for detailed setup guides

3. **Deploy the lab**
   ```bash
   docker-compose up -d
   ```

4. **Access the dashboards**
 - Elasticsearch: http://localhost:5601


> This lab uses Elasticsearch with security disabled, so Kibana is configured to connect without a username/password.

## 📁 Project Structure

```
Home-SEIM-LAB/
├── README.md                  # Project overview
├── docker-compose.yml         # Docker Compose setup
├── .gitignore                 # Git ignore rules
├── docs/                      # Documentation
│   ├── SETUP.md              # Installation guide
│   └── ARCHITECTURE.md        # Architecture documentation
├── configs/                   # Configuration files
│   └── logstash.conf         # Logstash configuration
├── logs/                      # Log samples and test data
└── scripts/                   # Utility scripts
```

## 🔧 Components

- **Log Collection**: Agents and forwarders for data gathering
- **Log Storage**: Centralized log repository
- **Analysis Engine**: Rules and correlation
- **Visualization**: Dashboards and reports
- **Alerting**: Notification and response mechanisms

## 📚 Documentation

Detailed guides available in the `docs/` directory:
- Installation guide
- Configuration guide
- Usage examples
- Troubleshooting

## 🛠️ Tech Stack

- Container orchestration (Docker/Docker Compose)
- Elasticsearch (log storage and search)
- Logstash (log processing and forwarding)
- Kibana (visualization and dashboards)
- Open-source SIEM solutions

## 📝 License

This project is provided as-is for educational purposes.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## 📧 Support

For questions and support, please open an issue in the repository.

---

**Last Updated**: 2026-05-14
**Status**: Active Development
