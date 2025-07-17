# Messaging System Using Kafka

A hands-on Java project demonstrating basic Apache Kafka producer and consumer patterns.

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Project Structure](#project-structure)
4. [Getting Started](#getting-started)
   - [Clone the repository](#clone-the-repository)
   - [Start Kafka broker](#start-kafka-broker)
   - [Build the project](#build-the-project)
5. [Running Examples](#running-examples)
   - [Main Demo](#main-demo)
   - [ProducerDefault](#producerdefault)
   - [ProducerWithCallback](#producerwithcallback)
   - [ProducerWithKeys](#producerwithkeys)
   - [ConsumerDefault](#consumerdefault)
   - [ConsumerWithShutdown](#consumerwithshutdown)
   - [ConsumerCooperative](#consumercooperative)
6. [Configuration](#configuration)
7. [Contributing](#contributing)
8. [License](#license)

---

## Overview

`messaging-with-kafka` is a multi-module Gradle project providing standalone Java examples that illustrate key Apache Kafka patterns:

- Simple producer and consumer setup
- Asynchronous sends with callbacks
- Using message keys for partitioning
- Graceful shutdown of consumers
- Cooperative rebalancing strategy

All examples use the official [org.apache.kafka](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients)[:kafka-clients](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients) library along with SLF4J for logging.

## Prerequisites

- **Java 17** or newer
- **Git** (to clone the repository)
- **Apache Kafka** broker running on `localhost:9092`
- (Optional) **Gradle**—the project includes the Gradle Wrapper (`gradlew`)

## Project Structure

```text
kafka-beginners-course/
├── build.gradle.kts           # Root build file (JUnit setup)
├── settings.gradle.kts        # Includes module "kafka-basics"
├── gradlew / gradlew.bat      # Gradle Wrapper scripts
├── src/
│   └── main/java/io/conduktor/demos/Main.java  # Simple demo application
└── kafka-basics/              # Module with Kafka producer/consumer examples
    ├── build.gradle.kts       # Kafka and SLF4J dependencies
    └── src/main/java/io/conduktor/demos/kafka/
        ├── ProducerDefault.java
        ├── ProducerWithCallback.java
        ├── ProducerWithKeys.java
        ├── ConsumerDefault.java
        ├── ConsumerWithShutdown.java
        └── ConsumerCooperative.java
```

> **Note:** IDE metadata (e.g., `.idea/`, `.DS_Store`) and compiled output are excluded from version control via `.gitignore`.

## Getting Started

### Clone the repository

```bash
git clone https://github.com/your-username/kafka-beginners-course.git
cd kafka-beginners-course
```

### Start Kafka broker

Ensure Kafka is running locally. Example using Docker:

```bash
# Start Zookeeper
docker run -d --name zookeeper -p 2181:2181 zookeeper:3.8

# Start Kafka broker
docker run -d --name kafka \
  -p 9092:9092 \
  --env KAFKA_ZOOKEEPER_CONNECT=host.docker.internal:2181 \
  --env KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 \
  bitnami/kafka:latest
```

Or download Kafka directly from [kafka.apache.org](https://kafka.apache.org/) and run:

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties
```

### Build the project

Using the Gradle Wrapper:

```bash
./gradlew clean build
```

This compiles both the root application and the `kafka-basics` module.

## Running Examples

All examples are standard Java `main()` classes. You can run them from your IDE or via command line.

### Main Demo

A simple loop printing a welcome message and counter:

```bash
java -cp build/classes/java/main io.conduktor.demos.Main
```

### ProducerDefault

Sends a single "hello world" message to topic `demo_java`:

```bash
java -cp kafka-basics/build/classes/java/main io.conduktor.demos.kafka.ProducerDefault
```

### ProducerWithCallback

Sends messages asynchronously and logs metadata on completion:

```bash
java -cp kafka-basics/build/classes/java/main io.conduktor.demos.kafka.ProducerWithCallback
```

### ProducerWithKeys

Demonstrates sending messages with keys to control partition assignment:

```bash
java -cp kafka-basics/build/classes/java/main io.conduktor.demos.kafka.ProducerWithKeys
```

### ConsumerDefault

A basic consumer that polls and prints records from `demo_java`:

```bash
java -cp kafka-basics/build/classes/java/main io.conduktor.demos.kafka.ConsumerDefault
```

### ConsumerWithShutdown

Adds a JVM shutdown hook for graceful consumer shutdown:

```bash
java -cp kafka-basics/build/classes/java/main io.conduktor.demos.kafka.ConsumerWithShutdown
```

### ConsumerCooperative

Configures the cooperative sticky assignor for lower-rebalance impact:

```bash
java -cp kafka-basics/build/classes/java/main io.conduktor.demos.kafka.ConsumerCooperative
```

## Configuration

All examples default to:

```properties
bootstrap.servers=127.0.0.1:9092
key.serializer=org.apache.kafka.common.serialization.StringSerializer
value.serializer=org.apache.kafka.common.serialization.StringSerializer
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

To change the broker or topic names, edit the `Properties` block in the source files.

## Contributing

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/MyExample`
3. Commit your changes: `git commit -m "Add MyExample demo"`
4. Push to your branch: `git push origin feature/MyExample`
5. Open a Pull Request

Please follow the existing code style and include Javadoc or comments for new examples.

## License

This project is released under the MIT License. See the [LICENSE](LICENSE) file for details.

