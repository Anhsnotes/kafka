# Kafka Docker Compose Setup

This repository contains a Docker Compose configuration for running Apache Kafka with Zookeeper and a web-based management UI.

## Services Included

- **Zookeeper**: Required for Kafka cluster coordination
- **Kafka**: Apache Kafka broker
- **Kafka UI**: Web-based interface for managing Kafka topics, consumers, and producers

## Quick Start

1. **Start all services:**
   ```bash
   docker-compose up -d
   ```

2. **Check service status:**
   ```bash
   docker-compose ps
   ```

3. **View logs:**
   ```bash
   # All services
   docker-compose logs -f
   
   # Specific service
   docker-compose logs -f kafka
   ```

## Access Points

- **Kafka Broker**: `localhost:9092`
- **Zookeeper**: `localhost:2181`
- **Kafka UI**: http://localhost:8080
- **Kafka JMX**: `localhost:9101`

## Configuration Details

### Kafka Configuration
- **Broker ID**: 1
- **Replication Factor**: 1 (single broker setup)
- **Auto-create topics**: Enabled
- **Delete topics**: Enabled
- **Listeners**: 
  - Internal: `kafka:29092` (for inter-container communication)
  - External: `localhost:9092` (for external clients)

### Zookeeper Configuration
- **Client Port**: 2181
- **Tick Time**: 2000ms
- **Data persistence**: Enabled via Docker volumes

## Usage Examples

### Using Kafka UI
1. Open http://localhost:8080 in your browser
2. Navigate to "Topics" to create and manage topics
3. Use "Produce" to send messages to topics
4. Use "Consume" to read messages from topics

### Using Kafka CLI (from host)
```bash
# Create a topic
docker exec -it kafka kafka-topics --create --topic test-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

# List topics
docker exec -it kafka kafka-topics --list --bootstrap-server localhost:9092

# Produce messages
docker exec -it kafka kafka-console-producer --topic test-topic --bootstrap-server localhost:9092

# Consume messages
docker exec -it kafka kafka-console-consumer --topic test-topic --bootstrap-server localhost:9092 --from-beginning
```

### Using Kafka from your application
```java
// Java example
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

## Management Commands

### Stop services
```bash
docker-compose down
```

### Stop and remove volumes (WARNING: This will delete all data)
```bash
docker-compose down -v
```

### Restart a specific service
```bash
docker-compose restart kafka
```

### Scale Kafka (for production, you'd want multiple brokers)
```bash
docker-compose up -d --scale kafka=3
```

## Data Persistence

Data is persisted using Docker volumes:
- `zookeeper-data`: Zookeeper data directory
- `zookeeper-logs`: Zookeeper logs
- `kafka-data`: Kafka data directory

## Troubleshooting

### Check if services are healthy
```bash
docker-compose ps
```

### View detailed logs
```bash
docker-compose logs kafka
docker-compose logs zookeeper
```

### Reset everything (WARNING: Deletes all data)
```bash
docker-compose down -v
docker-compose up -d
```

## Production Considerations

This setup is designed for development and testing. For production use, consider:

1. **Security**: Enable SSL/TLS and SASL authentication
2. **Monitoring**: Add Prometheus and Grafana
3. **Backup**: Implement proper backup strategies
4. **Scaling**: Use multiple Kafka brokers and Zookeeper instances
5. **Resource limits**: Set appropriate CPU and memory limits
6. **Network security**: Use proper network segmentation

## Version Information

- **Kafka**: 7.4.0 (Confluent Platform)
- **Zookeeper**: 7.4.0 (Confluent Platform)
- **Kafka UI**: Latest