# Kafka Producer Acknowledgments and Topic Durability

## Overview

Producer acknowledgments (acks) are a critical configuration that determines the level of durability and reliability for your Kafka messages. This setting controls how many brokers must confirm that they've received and stored the message before the producer considers the write operation successful.

## Producer Acknowledgment Settings

### acks=0 (No Acknowledgment)

**Description**: Producer sends data and immediately considers it successfully delivered without waiting for any confirmation.

```
Producer → Broker (Leader)
    ↓
"Message sent!" 
(No wait for confirmation)
```

**Characteristics**:
- **Fastest performance** - No network round-trip waiting
- **Highest risk** - Possible data loss
- **Use case**: High-throughput scenarios where occasional message loss is acceptable

**Risk Scenario**:
```
Producer sends message → Broker receives → Broker crashes before persisting
Result: Message lost, producer unaware
```

### acks=1 (Leader Acknowledgment)

**Description**: Producer waits for acknowledgment only from the partition leader broker.

```
Producer → Leader Broker → "ACK: Message received and persisted"
    ↓
Producer receives confirmation
```

**Characteristics**:
- **Moderate performance** - Single network round-trip
- **Limited data loss protection** - Data survives leader persistence
- **Risk**: If leader fails before replicating to followers

**Example Flow**:
```
Step 1: Producer → Leader (Broker 101) ← ACK
Step 2: Leader → Replica (Broker 102) [asynchronous]
Step 3: If Leader crashes before Step 2 completes → Data loss possible
```

### acks=all (All In-Sync Replicas)

**Description**: Producer waits for acknowledgment from the leader AND all in-sync replicas (ISRs).

```
Producer → Leader Broker → All ISR Replicas → "ACK: All confirmed"
    ↓
Producer receives confirmation after all ISRs acknowledge
```

**Characteristics**:
- **Highest durability** - No data loss under normal circumstances
- **Slower performance** - Multiple network round-trips
- **Best practice** for critical data

**Example Flow**:
```
Step 1: Producer → Leader (Broker 101)
Step 2: Leader → ISR Replica 1 (Broker 102) ← ACK
Step 3: Leader → ISR Replica 2 (Broker 103) ← ACK
Step 4: Leader → Producer ← "All ISRs confirmed"
```

## Visual Comparison

### Demo Scenario Setup
```
Topic: critical-data
Partitions: 1
Replication Factor: 3
Brokers: 101 (Leader), 102 (ISR), 103 (ISR)
```

### acks=0 Demo
```
Time: 0ms    Producer: "Sending message A"
Time: 1ms    Producer: "Message sent successfully!" ✓
Time: 2ms    Broker 101: Receives message A
Time: 3ms    Broker 101: CRASHES! ❌
Result: Message A lost, producer thinks it was delivered
```

### acks=1 Demo
```
Time: 0ms    Producer: "Sending message B"
Time: 1ms    Broker 101: Receives and persists message B
Time: 2ms    Broker 101: Sends ACK to producer
Time: 3ms    Producer: "Message sent successfully!" ✓
Time: 4ms    Broker 101: Starts replication to ISRs
Time: 5ms    Broker 101: CRASHES before replication completes ❌
Result: Message B lost, but producer knows it was initially received
```

### acks=all Demo
```
Time: 0ms    Producer: "Sending message C"
Time: 1ms    Broker 101: Receives and persists message C
Time: 2ms    Broker 101: Replicates to Broker 102 ← ACK received
Time: 3ms    Broker 101: Replicates to Broker 103 ← ACK received
Time: 4ms    Broker 101: Sends ACK to producer (all ISRs confirmed)
Time: 5ms    Producer: "Message sent successfully!" ✓
Time: 6ms    Broker 101: CRASHES ❌
Result: Message C safe on Brokers 102 and 103, no data loss
```

## Topic Durability

### Understanding Fault Tolerance

Topic durability is determined by the replication factor and directly relates to how many broker failures your cluster can survive.

### Durability Formula

**Rule**: With replication factor `N`, you can lose up to `N-1` brokers and still maintain data availability.

```
Replication Factor = N
Maximum Broker Failures = N - 1
```

### Examples

#### Example 1: Replication Factor 2
```
Initial State:
Broker 101: Partition 0 (Leader)
Broker 102: Partition 0 (Replica)

Failure Scenario:
Broker 102 FAILS ❌
Result: Broker 101 still serves partition 0 ✓
Maximum failures tolerated: 1
```

#### Example 2: Replication Factor 3
```
Initial State:
Broker 101: Partition 0 (Leader)
Broker 102: Partition 0 (ISR)
Broker 103: Partition 0 (ISR)

Failure Scenario 1:
Broker 102 FAILS ❌
Result: Brokers 101, 103 still available ✓

Failure Scenario 2:
Broker 102 FAILS ❌, then Broker 103 FAILS ❌
Result: Broker 101 still serves partition 0 ✓
Maximum failures tolerated: 2
```

## Real-World Configuration Examples

### High-Throughput Logging System
```yaml
# Configuration for non-critical logs
acks: 0
replication.factor: 2
# Prioritizes speed over durability
```

### Financial Transaction System
```yaml
# Configuration for critical financial data
acks: all
replication.factor: 3
min.insync.replicas: 2
# Maximum durability and consistency
```

### Analytics Data Pipeline
```yaml
# Configuration for analytics events
acks: 1
replication.factor: 3
# Balance between performance and reliability
```

## Best Practices

### Choosing the Right acks Setting

| Use Case | Recommended acks | Reasoning |
|----------|------------------|-----------|
| Critical financial data | `all` | Zero data loss requirement |
| User activity logs | `1` | Balance of speed and reliability |
| High-volume metrics | `0` | Speed over occasional loss |
| Audit trails | `all` | Regulatory compliance |

### Complementary Settings

When using `acks=all`, configure these related settings:

```properties
# Ensure at least 2 replicas must acknowledge
min.insync.replicas=2

# Prevent infinite retries
retries=3

# Set reasonable timeout
request.timeout.ms=30000
```

## Monitoring and Troubleshooting

### Key Metrics to Monitor

1. **Under-replicated partitions**: Indicates replication issues
2. **ISR shrink/expand rate**: Shows replica synchronization health
3. **Producer retry rate**: High retries may indicate acks configuration issues

### Common Issues

```bash
# Issue: Producer hangs with acks=all
# Cause: Not enough ISRs available
# Solution: Check min.insync.replicas setting

# Issue: High latency with acks=all
# Cause: Slow replica synchronization
# Solution: Optimize network or reduce batch sizes
```

## Summary

Producer acknowledgments and topic durability work together to provide configurable levels of data safety in Kafka:

- **acks=0**: Maximum speed, minimum safety
- **acks=1**: Balanced approach for most use cases  
- **acks=all**: Maximum safety for critical data

Choose your configuration based on your specific requirements for throughput, latency, and data durability. Remember that higher durability typically comes with performance trade-offs, so test thoroughly in your environment.
