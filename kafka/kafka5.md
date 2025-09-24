# Kafka Consumer Groups

## Overview

Consumer Groups enable multiple consumers to work together to read from a Kafka topic in a coordinated, scalable way. This is essential for building high-throughput, fault-tolerant applications.

## What is a Consumer Group?

A **Consumer Group** is a collection of consumers that work together to consume messages from Kafka topics. Each consumer in the group reads from exclusive partitions, ensuring that each message is processed only once within the group.

## Basic Consumer Group Architecture

### Example: 5 Partitions, 3 Consumers

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Topic A (5 Partitions)                      │
├─────────────────────────────────────────────────────────────────────┤
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌─────┐ │
│  │Partition 0│  │Partition 1│  │Partition 2│  │Partition 3│  │Part │ │
│  │[0][1][2]  │  │[0][1][2]  │  │[0][1][2]  │  │[0][1][2]  │  │ 4   │ │
│  │[3][4][5]  │  │[3][4][5]  │  │[3][4][5]  │  │[3][4][5]  │  │[0]  │ │
│  └───────────┘  └───────────┘  └───────────┘  └───────────┘  └─────┘ │
└─────────────────────────────────────────────────────────────────────┘
         │              │              │              │           │
         │              │              └──────┐       │           │
         │              │                     │       │           │
         ▼              ▼                     ▼       ▼           ▼
┌─────────────────────────────────────────────────────────────────────┐
│              Consumer Group: "Application"                          │
├─────────────────────────────────────────────────────────────────────┤
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐           │
│   │ Consumer 1  │     │ Consumer 2  │     │ Consumer 3  │           │
│   │             │     │             │     │             │           │
│   │ Reads from: │     │ Reads from: │     │ Reads from: │           │
│   │ Part 0 & 1  │     │ Part 2 & 3  │     │ Part 4      │           │
│   └─────────────┘     └─────────────┘     └─────────────┘           │
└─────────────────────────────────────────────────────────────────────┘
```

### Key Characteristics:
- ✅ **Exclusive partition assignment**: Each partition is read by exactly one consumer
- ✅ **Load balancing**: Work is distributed across consumers
- ✅ **Scalability**: Add consumers to increase processing power
- ✅ **Fault tolerance**: If one consumer fails, others take over its partitions

## Consumer Group Scenarios

### Scenario 1: More Consumers Than Partitions

```
┌─────────────────────────────────────────────────────────────┐
│                   Topic A (3 Partitions)                   │
├─────────────────────────────────────────────────────────────┤
│  ┌───────────┐    ┌───────────┐    ┌───────────┐           │
│  │Partition 0│    │Partition 1│    │Partition 2│           │
│  │[0][1][2]  │    │[0][1][2]  │    │[0][1][2]  │           │
│  │[3][4][5]  │    │[3][4][5]  │    │[3][4][5]  │           │
│  └───────────┘    └───────────┘    └───────────┘           │
└─────────────────────────────────────────────────────────────┘
       │                 │                 │
       ▼                 ▼                 ▼
┌─────────────────────────────────────────────────────────────┐
│          Consumer Group: "Application" (4 Consumers)       │
├─────────────────────────────────────────────────────────────┤
│ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐   │
│ │Consumer 1 │ │Consumer 2 │ │Consumer 3 │ │Consumer 4 │   │
│ │           │ │           │ │           │ │           │   │
│ │Part 0     │ │Part 1     │ │Part 2     │ │ INACTIVE  │   │
│ │  ACTIVE   │ │  ACTIVE   │ │  ACTIVE   │ │ (Standby) │   │
│ └───────────┘ └───────────┘ └───────────┘ └───────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Result**: Consumer 4 remains inactive as a standby consumer
- ⚠️ **Inactive consumers don't help active ones**
- 📝 **Best practice**: Number of consumers ≤ Number of partitions for optimal resource usage

## Multiple Consumer Groups on Same Topic

### Real-World Example: Truck GPS Tracking System

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Topic: "truck-gps-events"                       │
│                         (3 Partitions)                             │
├─────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐       │
│  │   Partition 0   │ │   Partition 1   │ │   Partition 2   │       │
│  │ truck_001: GPS  │ │ truck_003: GPS  │ │ truck_005: GPS  │       │
│  │ truck_002: GPS  │ │ truck_004: GPS  │ │ truck_006: GPS  │       │
│  │ truck_001: GPS  │ │ truck_003: GPS  │ │ truck_005: GPS  │       │
│  └─────────────────┘ └─────────────────┘ └─────────────────┘       │
└─────────────────────────────────────────────────────────────────────┘
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
┌─────────────────────────────────────────────────────────────────────┐
│        Consumer Group: "location-service" (2 Consumers)            │
├─────────────────────────────────────────────────────────────────────┤
│     ┌─────────────────┐              ┌─────────────────┐             │
│     │   Consumer 1    │              │   Consumer 2    │             │
│     │                 │              │                 │             │
│     │ Reads Part 0&1  │              │ Reads Part 2    │             │
│     │                 │              │                 │             │
│     │ Updates truck   │              │ Updates truck   │             │
│     │ locations in DB │              │ locations in DB │             │
│     └─────────────────┘              └─────────────────┘             │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│      Consumer Group: "notification-service" (3 Consumers)          │
├─────────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐    ┌─────────────┐    ┌─────────────┐              │
│ │ Consumer 1  │    │ Consumer 2  │    │ Consumer 3  │              │
│ │             │    │             │    │             │              │
│ │ Part 0      │    │ Part 1      │    │ Part 2      │              │
│ │             │    │             │    │             │              │
│ │ ALL alerts  │    │ ALL alerts  │    │ ALL alerts  │              │
│ │ for trucks: │    │ for trucks: │    │ for trucks: │              │
│ │ 001,004,007 │    │ 002,005,008 │    │ 003,006,009 │              │
│ │ • Geo-fence │    │ • Geo-fence │    │ • Geo-fence │              │
│ │ • Speed     │    │ • Speed     │    │ • Speed     │              │
│ │ • Route     │    │ • Route     │    │ • Route     │              │
│ └─────────────┘    └─────────────┘    └─────────────┘              │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│         Consumer Group: "analytics-service" (1 Consumer)           │
├─────────────────────────────────────────────────────────────────────┤
│                    ┌─────────────────┐                             │
│                    │   Consumer 1    │                             │
│                    │                 │                             │
│                    │ Reads ALL       │                             │
│                    │ Partitions      │                             │
│                    │ (0, 1, 2)       │                             │
│                    │                 │                             │
│                    │ Generates       │                             │
│                    │ daily reports   │                             │
│                    └─────────────────┘                             │
└─────────────────────────────────────────────────────────────────────┘
```

### Why Multiple Consumer Groups?

Each service needs the **same data** for **different purposes**:

| Service | Purpose | Processing |
|---------|---------|------------|
| **Location Service** | Track truck positions | Update location database |
| **Notification Service** | Send alerts | Check rules, send notifications |
| **Analytics Service** | Generate reports | Aggregate data, create dashboards |

## Consumer Group Configuration

### Setting up Consumer Groups

```java
// Location Service Consumer
Properties props = new Properties();
props.put("group.id", "location-service");
props.put("bootstrap.servers", "localhost:9092");
// ... other properties

// Notification Service Consumer  
Properties props2 = new Properties();
props2.put("group.id", "notification-service");
props2.put("bootstrap.servers", "localhost:9092");
// ... other properties
```

### Consumer Group ID Rules
- ✅ **Unique per application/service**
- ✅ **Descriptive naming** (e.g., "location-service", "notification-service")
- ✅ **Consistent across consumer instances** in the same group

## Consumer Offsets Management

### What are Consumer Offsets?

Consumer offsets track **where each consumer group has read up to** in each partition.

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Kafka Topic Partition                       │
├─────────────────────────────────────────────────────────────────────┤
│  [0] [1] [2] [3] [4] [5] [6] [7] [8] [9] [10] ... [4258] [4259]    │
│   ▲                                                  ▲              │
│   │                                                  │              │
│   └── Consumer started reading here                  │              │
│                                                      │              │
│                   Current consumer position ────────┘              │
└─────────────────────────────────────────────────────────────────────┘

Offset committed: 4258
Next message to read: 4259
```

### Offset Storage

Offsets are stored in a special Kafka topic called `__consumer_offsets`:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    __consumer_offsets Topic                        │
│                        (Internal Kafka Topic)                      │
├─────────────────────────────────────────────────────────────────────┤
│  Group: "location-service"                                         │
│  Topic: "truck-gps-events"                                         │
│  Partition 0: offset 1234                                          │
│  Partition 1: offset 5678                                          │
│  Partition 2: offset 9012                                          │
│                                                                     │
│  Group: "notification-service"                                     │
│  Topic: "truck-gps-events"                                         │
│  Partition 0: offset 1100                                          │
│  Partition 1: offset 5500                                          │
│  Partition 2: offset 8800                                          │
└─────────────────────────────────────────────────────────────────────┘
```

### Consumer Restart Scenario

```
Step 1: Consumer Processing
┌─────────────────────────────────────────────────────────────────────┐
│  Partition: [4256] [4257] [4258] [4259] [4260] [4261] [4262]       │
│                              ▲                                     │
│                              │                                     │
│                    Consumer reads & processes                      │
│                    Commits offset: 4258                            │
└─────────────────────────────────────────────────────────────────────┘

Step 2: Consumer Crashes 💥

Step 3: Consumer Restarts
┌─────────────────────────────────────────────────────────────────────┐
│  Partition: [4256] [4257] [4258] [4259] [4260] [4261] [4262]       │
│                              │                                     │
│                              │    ▲                                │
│                              │    │                                │
│                   Last committed  │                                │
│                   offset: 4258    │                                │
│                              │    │                                │
│                              └────┘                                │
│                         Consumer resumes                           │
│                         from offset 4259                           │
└─────────────────────────────────────────────────────────────────────┘
```

## Delivery Semantics

### 1. At Least Once (Default)

**Process first, then commit offset**

```
Message Processing Flow:
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ Receive │ ──▶│ Process │ ──▶│ Success │ ──▶│ Commit  │
│ Message │    │ Message │    │   ✅    │    │ Offset  │
└─────────┘    └─────────┘    └─────────┘    └─────────┘

If processing fails:
┌─────────┐    ┌─────────┐    ┌─────────┐    
│ Receive │ ──▶│ Process │ ──▶│ Failure │    
│ Message │    │ Message │    │   ❌    │    
└─────────┘    └─────────┘    └─────────┘    
                                   │
                                   ▼
                              ┌─────────┐
                              │ Retry   │
                              │ Message │
                              └─────────┘
```

**Result**: Messages may be processed multiple times
**Requirement**: Processing must be **idempotent**

### 2. At Most Once

**Commit offset first, then process**

```
Message Processing Flow:
┌─────────┐    ┌─────────┐    ┌─────────┐    
│ Receive │ ──▶│ Commit  │ ──▶│ Process │    
│ Message │    │ Offset  │    │ Message │    
└─────────┘    └─────────┘    └─────────┘    

If processing fails:
┌─────────┐    ┌─────────┐    ┌─────────┐    
│ Receive │ ──▶│ Commit  │ ──▶│ Process │    
│ Message │    │ Offset  │    │   ❌    │    
└─────────┘    └─────────┘    └─────────┘    
                   ✅              │
                                   ▼
                              ┌─────────┐
                              │ Message │
                              │  LOST   │
                              └─────────┘
```

**Result**: Messages may be lost but never duplicated

### 3. Exactly Once

**Process message exactly once**

```
Kafka-to-Kafka (Transactional API):
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ Read    │ ──▶│ Process │ ──▶│ Write   │ ──▶│ Commit  │
│ Message │    │ Message │    │ Result  │    │Transaction│
└─────────┘    └─────────┘    └─────────┘    └─────────┘
     │                             │              │
     └─────────────────────────────┼──────────────┘
                                   ▼
                         ┌─────────────────┐
                         │ All or Nothing  │
                         │   (Atomic)      │
                         └─────────────────┘

Kafka-to-External-System (Idempotent Consumer):
┌─────────┐    ┌─────────┐    ┌─────────┐    
│ Read    │ ──▶│ Process │ ──▶│ Store   │    
│ Message │    │ w/ Key  │    │ Result  │    
└─────────┘    └─────────┘    └─────────┘    
                   │              │
                   ▼              ▼
            ┌─────────────────────────┐
            │ Duplicate Detection     │
            │ (Same key = ignore)     │
            └─────────────────────────┘
```

## Consumer Group vs. Partition Assignment - Key Clarification

### ❌ Common Misconception
**WRONG**: Different consumers handle different types of processing logic

```
Consumer 1 → Only geo-fencing alerts
Consumer 2 → Only speed alerts
Consumer 3 → Only route alerts
```

### ✅ Correct Understanding
**RIGHT**: Each consumer handles ALL processing for trucks in their assigned partition(s)

```
┌─────────────────────────────────────────────────────────────────────┐
│                   How Partitioning Really Works                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Partition 0: truck_001, truck_004, truck_007, truck_010...         │
│       │                                                             │
│       ▼                                                             │
│  Consumer 1: Handles ALL alerts for trucks 001, 004, 007, 010      │
│              • Geo-fencing alerts                                   │
│              • Speed violation alerts                               │
│              • Route deviation alerts                               │
│              • Any other alert type                                 │
│                                                                     │
│  Partition 1: truck_002, truck_005, truck_008, truck_011...         │
│       │                                                             │
│       ▼                                                             │
│  Consumer 2: Handles ALL alerts for trucks 002, 005, 008, 011      │
│              • Geo-fencing alerts                                   │
│              • Speed violation alerts                               │
│              • Route deviation alerts                               │
│              • Any other alert type                                 │
└─────────────────────────────────────────────────────────────────────┘
```

### Why This Matters
1. **Each truck's data** (with same truck_id key) **always goes to the same partition**
2. **Each consumer** processes **all message types** for **all trucks** in their partition(s)
3. **Consumer assignment** is by **partition**, not by **message content** or **processing logic**

## Alternative: Specialized Alert Processing

If you want different consumers to handle different alert types, you need **separate topics**:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Specialized Alert Architecture                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐ │
│  │ Topic:          │    │ Topic:          │    │ Topic:          │ │
│  │ "geo-alerts"    │    │ "speed-alerts"  │    │ "route-alerts"  │ │
│  │                 │    │                 │    │                 │ │
│  │ truck_001: geo  │    │ truck_001: speed│    │ truck_001: route│ │
│  │ truck_002: geo  │    │ truck_002: speed│    │ truck_002: route│ │
│  │ truck_003: geo  │    │ truck_003: speed│    │ truck_003: route│ │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘ │
│           │                       │                       │       │
│           ▼                       ▼                       ▼       │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐ │
│  │ Consumer Group: │    │ Consumer Group: │    │ Consumer Group: │ │
│  │ "geo-service"   │    │ "speed-service" │    │ "route-service" │ │
│  │                 │    │                 │    │                 │ │
│  │ Specialized in  │    │ Specialized in  │    │ Specialized in  │ │
│  │ geo-fencing     │    │ speed analysis  │    │ route planning  │ │
│  │ logic only      │    │ logic only      │    │ logic only      │ │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

### Scenario: Fleet Management System

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Truck Fleet                                │
├─────────────────────────────────────────────────────────────────────┤
│    🚚 Truck-001        🚛 Truck-002        🚚 Truck-003           │
│   (Route: A→B)        (Route: C→D)        (Route: E→F)            │
│                                                                     │
│   GPS: 40.7128,       GPS: 34.0522,       GPS: 41.8781,           │
│        -74.0060            -118.2437           -87.6298            │
│   Speed: 65mph        Speed: 45mph        Speed: 70mph             │
│   Status: On-time     Status: Delayed     Status: Speeding        │
└─────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│              Kafka Topic: "truck-gps-events"                       │
│                        Key: truck_id                               │
├─────────────────────────────────────────────────────────────────────┤
│  Partition 0        │  Partition 1        │  Partition 2          │
│  truck_001 →        │  truck_002 →        │  truck_003 →          │
│  truck_004 →        │  truck_005 →        │  truck_006 →          │
│  truck_007 →        │  truck_008 →        │  truck_009 →          │
└─────────────────────────────────────────────────────────────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          ▼                        ▼                        ▼

┌─────────────────────────────────────────────────────────────────────┐
│                    Location Service                                 │
│                Consumer Group: "location-service"                  │
├─────────────────────────────────────────────────────────────────────┤
│  📍 Updates truck positions in real-time                           │
│  📊 Calculates ETAs and route progress                             │
│  🗺️  Provides live tracking to customers                           │
│                                                                     │
│  Delivery Semantic: At Least Once                                  │
│  (Duplicate location updates are harmless)                         │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                  Notification Service                               │
│              Consumer Group: "notification-service"                │
├─────────────────────────────────────────────────────────────────────┤
│  🚨 Detects speeding violations                                     │
│  📢 Sends geofence breach alerts                                   │
│  ⏰ Notifies of delivery delays                                     │
│                                                                     │
│  Delivery Semantic: At Least Once                                  │
│  (Use deduplication to avoid duplicate alerts)                     │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                   Analytics Service                                 │
│               Consumer Group: "analytics-service"                  │
├─────────────────────────────────────────────────────────────────────┤
│  📈 Generates daily/weekly fleet reports                           │
│  ⛽ Calculates fuel efficiency metrics                             │
│  📊 Provides driver performance analytics                          │
│                                                                     │
│  Delivery Semantic: At Least Once                                  │
│  (Idempotent aggregations handle duplicates)                       │
└─────────────────────────────────────────────────────────────────────┘
```

## Truck Example: Complete Workflow

### ✅ Do's

- **Use meaningful group IDs** that reflect the service/application purpose
- **Keep consumer groups small** - ideally ≤ number of partitions
- **Monitor consumer lag** to ensure processing keeps up with production
- **Handle rebalancing gracefully** when consumers join/leave the group
- **Implement proper error handling** for failed message processing
- **Choose appropriate delivery semantics** based on your use case

### ❌ Don'ts

- **Don't create too many consumers** beyond the number of partitions
- **Don't change group IDs** frequently - this resets offset tracking
- **Don't ignore consumer lag** - it indicates processing bottlenecks
- **Don't assume messages won't be duplicated** with at-least-once processing
- **Don't commit offsets** before ensuring successful message processing

## Summary

- **Consumer Groups** enable scalable, fault-tolerant message processing
- **Partition assignment** is exclusive within a group, shared across groups  
- **Multiple consumer groups** can read from the same topic for different use cases
- **Consumer offsets** track processing progress and enable crash recovery
- **Delivery semantics** provide different guarantees: at-least-once, at-most-once, exactly-once
- **Real-world applications** like fleet management benefit from multiple specialized consumer groups
- **Proper configuration** and monitoring are essential for reliable operation

Understanding consumer groups is crucial for building scalable Kafka applications that can handle high-volume data streams reliably.
