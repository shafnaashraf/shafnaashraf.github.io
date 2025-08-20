# Kafka Consumers

## Overview

Kafka consumers are responsible for reading data from Kafka topics. Understanding how consumers work, their pull-based model, and deserialization process is essential for building robust Kafka applications.

## Consumer Pull Model

### How Consumers Request Data

Kafka uses a **pull model**, not a push model. This means:
- Consumers request data from Kafka brokers
- Brokers respond with available data
- Consumers control the rate of consumption

```
┌─────────────┐                    ┌─────────────────────────┐
│  Consumer   │────── Request ────▶│     Kafka Broker        │
│             │                    │                         │
│             │◄──── Response ─────│   ┌─────────────────┐   │
└─────────────┘                    │   │ Topic A         │   │
                                   │   │ ┌─────────────┐ │   │
                                   │   │ │ Partition 0 │ │   │
                                   │   │ │ Partition 1 │ │   │
                                   │   │ │ Partition 2 │ │   │
                                   │   │ └─────────────┘ │   │
                                   │   └─────────────────┘   │
                                   └─────────────────────────┘
```

## Consumer Reading Patterns

### Single Partition Consumer

```
┌─────────────┐    ┌─────────────────────────────────────────────┐
│ Consumer 1  │────▶│              Topic A                        │
└─────────────┘    │                                             │
                   │  ┌─────────────────────────────────────┐    │
                   │  │         Partition 0                 │    │
                   │  │  [0][1][2][3][4][5][6][7][8][9][10] │    │
                   │  │   ▲                               ▲ │    │
                   │  │   └── Read in order ──────────────┘ │    │
                   │  └─────────────────────────────────────┘    │
                   └─────────────────────────────────────────────┘
```

### Multiple Partition Consumer

```
┌─────────────┐    ┌─────────────────────────────────────────────┐
│ Consumer 2  │────▶│              Topic A                        │
└─────────────┘    │                                             │
      │            │  ┌─────────────────────────────────────┐    │
      │            │  │         Partition 1                 │    │
      │            │  │   [0][1][2][3][4][5][6][7][8]       │    │
      │            │  │    ▲                       ▲        │    │
      │            │  │    └── Read in order ──────┘        │    │
      │            │  └─────────────────────────────────────┘    │
      │            │                                             │
      └────────────┼─▶┌─────────────────────────────────────┐    │
                   │  │         Partition 2                 │    │
                   │  │   [0][1][2][3][4][5][6]             │    │
                   │  │    ▲                   ▲            │    │
                   │  │    └── Read in order ──┘            │    │
                   │  └─────────────────────────────────────┘    │
                   └─────────────────────────────────────────────┘
```

## Message Ordering in Consumers

### Within Partition Ordering (Guaranteed)

```
Partition 0:  [msg0] ──▶ [msg1] ──▶ [msg2] ──▶ [msg3]
             offset:0    offset:1    offset:2    offset:3
                 │          │          │          │
                 ▼          ▼          ▼          ▼
             Read in guaranteed order: 0 → 1 → 2 → 3
```

### Cross-Partition Ordering (NOT Guaranteed)

```
Partition 1:  [msgA] ──▶ [msgB] ──▶ [msgC]
             offset:0    offset:1    offset:2

Partition 2:  [msgX] ──▶ [msgY] ──▶ [msgZ]
             offset:0    offset:1    offset:2

Consumer reading from both partitions may read in this order:
Option 1: msgA → msgX → msgB → msgY → msgC → msgZ
Option 2: msgX → msgA → msgY → msgB → msgZ → msgC
Option 3: msgA → msgB → msgX → msgY → msgC → msgZ

Within each partition: A → B → C (guaranteed)
Within each partition: X → Y → Z (guaranteed)
Between partitions: No ordering guarantee
```

### Key Points:
- ✅ **Ordering guaranteed**: Within each partition (low to high offset)
- ❌ **Ordering NOT guaranteed**: Across different partitions
- 📍 **Reading direction**: Always from offset 0 towards higher offsets

## Message Deserialization

### The Deserialization Process

Since Kafka stores and transmits data as bytes, consumers must deserialize messages back into usable objects.

```
┌─────────────────────────────────────────────────────────────┐
│                    Kafka Message (Bytes)                    │
├─────────────────────────────────────────────────────────────┤
│  Key: [01111011] (binary)                                  │
│  Value: [01101000 01100101 01101100...] (binary)           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Consumer Application                     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────┐                    ┌─────────────────┐
│  Key Deserial.  │                    │ Value Deserial. │
│  (Integer)      │                    │  (String)       │
└─────────────────┘                    └─────────────────┘
         │                                       │
         ▼                                       ▼
┌─────────────────┐                    ┌─────────────────┐
│  Key Object     │                    │  Value Object   │
│  123 (Integer)  │                    │ "hello world"   │
└─────────────────┘                    └─────────────────┘
```

### Deserialization Example

```
┌─────────────────┐              ┌─────────────────┐
│ Binary Key      │              │ Binary Value    │
│ [01111011]      │              │ [01101000...]   │
└─────────────────┘              └─────────────────┘
         │                                │
         ▼                                ▼
┌─────────────────┐              ┌─────────────────┐
│ Integer         │              │ String          │
│ Deserializer    │              │ Deserializer    │
└─────────────────┘              └─────────────────┘
         │                                │
         ▼                                ▼
┌─────────────────┐              ┌─────────────────┐
│ Application     │              │ Application     │
│ Object: 123     │              │ Object:         │
│ (Integer)       │              │ "hello world"   │
└─────────────────┘              └─────────────────┘
```

## Common Deserializers

### Available Deserializer Types

| Data Type | Deserializer | Input Format | Output Object |
|-----------|-------------|--------------|---------------|
| String | StringDeserializer | UTF-8 bytes | String object |
| JSON | StringDeserializer | JSON bytes | String (parse separately) |
| Integer | IntegerDeserializer | 4 bytes | Integer object |
| Long | LongDeserializer | 8 bytes | Long object |
| Float | FloatDeserializer | 4 bytes | Float object |
| Double | DoubleDeserializer | 8 bytes | Double object |
| Avro | AvroDeserializer | Avro bytes | Generated object |
| Protobuf | ProtobufDeserializer | Protobuf bytes | Generated object |

### Serialization ↔ Deserialization Flow

```
Producer Side                           Consumer Side
┌─────────────┐                        ┌─────────────┐
│ Application │                        │ Application │
│ Objects     │                        │ Objects     │
└─────────────┘                        └─────────────┘
       │                                      ▲
       ▼                                      │
┌─────────────┐                        ┌─────────────┐
│ Serializer  │                        │Deserializer │
└─────────────┘                        └─────────────┘
       │                                      ▲
       ▼                                      │
┌─────────────┐    Kafka Storage       ┌─────────────┐
│    Bytes    │ ──────────────────────▶│    Bytes    │
└─────────────┘                        └─────────────┘
```

## Consumer Configuration Requirements

### Mandatory Deserializer Configuration

```java
// Consumer must specify both key and value deserializers
Properties props = new Properties();
props.put("key.deserializer", "org.apache.kafka.common.serialization.IntegerDeserializer");
props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

### Consumer Must Know Data Format in Advance

```
┌─────────────────────────────────────────────────────────────┐
│                    Topic Lifecycle                          │
│                                                             │
│  Producer writes:  Integer key + String value              │
│           │                                                 │
│           ▼                                                 │
│  Consumer expects: Integer key + String value              │
│                                                             │
│  ⚠️  NEVER change data types during topic lifetime!        │
└─────────────────────────────────────────────────────────────┘
```

## Data Type Consistency Rules

### ✅ Consistent Data Types (Good)

```
Timeline: Topic Creation ────────────────────────▶ Topic End of Life

Producer:    Integer + String    Integer + String    Integer + String
             ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
             │ key: 123    │    │ key: 456    │    │ key: 789    │
             │ val:"hello" │    │ val:"world" │    │ val:"test"  │
             └─────────────┘    └─────────────┘    └─────────────┘

Consumer:    Integer + String    Integer + String    Integer + String
             ✅ SUCCESS          ✅ SUCCESS          ✅ SUCCESS
```

### ❌ Inconsistent Data Types (Broken)

```
Timeline: Topic Creation ────────────────────────▶ Topic End of Life

Producer:    Integer + String    Float + Avro       Long + JSON
             ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
             │ key: 123    │    │ key: 3.14   │    │ key: 999L   │
             │ val:"hello" │    │ val: {avro} │    │ val: {json} │
             └─────────────┘    └─────────────┘    └─────────────┘

Consumer:    Integer + String    Integer + String   Integer + String
             ✅ SUCCESS          ❌ FAILURE!        ❌ FAILURE!
```

## Handling Data Type Changes

### Wrong Approach ❌
```
┌─────────────────┐
│ Change producer │
│ data types in   │
│ same topic      │
└─────────────────┘
         │
         ▼
┌─────────────────┐
│ Consumer breaks │
│ Can't deserialize│
│ new format      │
└─────────────────┘
```

### Right Approach ✅
```
┌─────────────────┐         ┌─────────────────┐
│ Old Topic       │         │ New Topic       │
│ Integer+String  │         │ Float+Avro      │
└─────────────────┘         └─────────────────┘
         │                           │
         ▼                           ▼
┌─────────────────┐         ┌─────────────────┐
│ Old Consumer    │         │ New Consumer    │
│ (continues      │         │ (programmed for │
│  working)       │         │  new format)    │
└─────────────────┘         └─────────────────┘
```

### Migration Strategy

1. **Create new topic** with desired data format
2. **Update producers** to write to new topic
3. **Create new consumers** with appropriate deserializers
4. **Migrate gradually** from old to new system
5. **Decommission old topic** when migration complete

## Consumer Fault Tolerance

### Broker Failure Recovery

```
┌─────────────┐    ❌ Failed    ┌─────────────────┐
│  Consumer   │ ────────────────│ Kafka Broker 1  │
│             │                 │ (Partition 0)   │
└─────────────┘                 └─────────────────┘
       │
       │ Auto-recovery
       ▼
┌─────────────────┐     ✅      ┌─────────────────┐
│ Consumer finds  │─────────────│ Kafka Broker 2  │
│ replica broker  │             │ (Partition 0    │
│ automatically   │             │  replica)       │
└─────────────────┘             └─────────────────┘
```

## Best Practices

### ✅ Do's
- Always specify appropriate deserializers for your data types
- Maintain consistent data formats throughout topic lifecycle
- Handle deserialization exceptions gracefully
- Monitor consumer lag and performance
- Use schema registry for complex data types (Avro, Protobuf)

### ❌ Don'ts
- Don't change data types in existing topics
- Don't assume cross-partition message ordering
- Don't ignore deserialization errors
- Don't use inappropriate deserializers for your data
- Don't forget that consumers control their reading pace

## Summary

- **Pull Model**: Consumers request data from brokers (not pushed)
- **Ordered Reading**: Guaranteed within partitions, not across partitions
- **Deserialization**: Converts bytes back to application objects
- **Type Consistency**: Must maintain same data types throughout topic lifecycle
- **Fault Tolerance**: Consumers automatically handle broker failures
- **Format Knowledge**: Consumers must know expected data format in advance
- **Topic Migration**: Create new topics for data type changes

Understanding these consumer concepts is crucial for building reliable Kafka applications that can properly read and process streaming data.
