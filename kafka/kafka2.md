# Kafka Topics, Partitions and Offsets

## What are Kafka Topics?

A **Kafka topic** is a particular stream of data within your Kafka cluster. Topics are fundamental units for organizing data in Kafka.

### Topic Examples
- `logs`
- `purchases`
- `twitter_tweets`
- `trucks_gps`

### Key Characteristics

- **Database Analogy**: Topics are similar to tables in databases, but without constraints
- **No Data Verification**: You can send any data format to a Kafka topic
- **Identification**: Topics are identified by their unique names
- **Multiple Topics**: A Kafka cluster can have as many topics as needed

## Data Formats and Streams

### Supported Message Formats
- JSON
- Avro
- Text files
- Binary data
- Any custom format

### Data Streams
The sequence of messages in a topic is called a **data stream**, which is why Kafka is called a data streaming platform.

### Important Limitations
- **No Querying**: Unlike databases, you cannot query topics directly
- **Data Access**: Use Producers to add data, Consumers to read data

## Topic Partitions

### Partition Structure

Topics are divided into **partitions** for scalability and parallelism:

```
Topic: trucks_gps
├── Partition 0: [msg0, msg1, msg2, ..., msg9]
├── Partition 1: [msg0, msg1, msg2, ..., msg7]
└── Partition 2: [msg0, msg1, msg2, ..., msg5]
```

### Key Features

- A topic can have multiple partitions (e.g., 3, 10, 100, or more)
- Messages within each partition are **ordered**
- Each message gets an incrementing ID called an **offset**

## Offsets Explained

### What are Offsets?

An **offset** is a unique identifier for each message within a partition:

- Starts at 0 and increments by 1 for each new message
- Only meaningful within a specific partition
- Offsets are **not reused** even after message deletion
- Different partitions can have the same offset numbers for different messages

### Offset Example

```
Partition 0: offset 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
Partition 1: offset 0, 1, 2, 3, 4, 5, 6, 7
Partition 2: offset 0, 1, 2, 3, 4, 5
```

**Note**: Offset 3 in Partition 0 ≠ Offset 3 in Partition 1

## Real-World Example: Truck GPS Tracking

### Scenario Setup

Imagine a fleet management system where:
- Multiple trucks have GPS devices
- Each GPS reports position every 20 seconds
- Messages contain: `truck_id`, `latitude`, `longitude`

### Implementation

```
Producers: Truck GPS devices
Topic: trucks_gps (10 partitions)
Consumers: 
  ├── Location Dashboard (real-time tracking)
  └── Notification Service (delivery alerts)
```

### Benefits

- **Multiple Consumers**: Different services can read the same data stream
- **Real-time Processing**: Location updates processed as they arrive
- **Scalability**: 10 partitions allow parallel processing

## Important Rules and Characteristics

### 1. Immutability
- **Rule**: Once data is written to a partition, it **cannot be changed**
- **Implication**: No updates or deletes, only appends

### 2. Data Retention
- **Default**: Data kept for **1 week** (configurable)
- **Behavior**: After retention period, data automatically expires

### 3. Offset Uniqueness
- **Scope**: Offsets only have meaning within their specific partition
- **Reuse**: Offsets are never reused, even after data deletion
- **Increment**: Always increases sequentially (0, 1, 2, 3, ...)

### 4. Message Ordering

#### ✅ Guaranteed: Within Partition
```
Partition 0: [A] → [B] → [C] → [D]
Order: A, B, C, D (guaranteed)
```

#### ❌ Not Guaranteed: Across Partitions
```
Partition 0: [A] → [C] → [E]
Partition 1: [B] → [D] → [F]
Overall Order: Could be A,B,C,D,E,F OR A,C,B,D,E,F OR other combinations
```

### 5. Partition Assignment

**Without Key**: Messages assigned to **random** partitions
```java
// Random partition assignment
producer.send(new ProducerRecord<>("trucks_gps", message));
```

**With Key**: Messages with same key go to **same** partition
```java
// Same truck always goes to same partition
producer.send(new ProducerRecord<>("trucks_gps", truck_id, message));
```

## Partition Strategy Considerations

### Choosing Partition Count

Factors to consider:
- **Throughput Requirements**: More partitions = higher potential throughput
- **Consumer Parallelism**: Max consumers = number of partitions
- **Ordering Requirements**: Need ordering? Use fewer partitions or message keys
- **Resource Overhead**: Each partition consumes resources

### Common Patterns

1. **High Throughput**: 10-100+ partitions
2. **Strict Ordering**: 1 partition (or use message keys)
3. **Balanced Approach**: Start with 3-10 partitions

## Best Practices

### Topic Design
- Use meaningful topic names (`user_events`, `payment_transactions`)
- Plan partition strategy based on access patterns
- Consider data retention requirements

### Message Keys
- Use keys when ordering matters within logical groups
- Example: Use `user_id` as key for user-related events
- Same key = same partition = guaranteed ordering

### Monitoring
- Track partition utilization
- Monitor consumer lag per partition
- Watch for hot partitions (uneven load distribution)

*This tutorial covered the fundamental concepts of Kafka data organization. Master these concepts before moving to producer and consumer implementations.*
