---
title: "Kafka setup on Mac"
datePublished: Sun Apr 20 2025 17:29:42 GMT+0000 (Coordinated Universal Time)
cuid: cm9px9nai000009jshyhha60d
slug: kafka-setup-on-mac

---

✅ **Follow these steps to set up and run the demo properly on your Mac.**

**🔹 1️⃣ Install Homebrew (if not installed)**

First, make sure **Homebrew** is installed on your Mac. Run:

```javascript
/bin/bash -c "$(curl -fsSL <https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh>)"
```

✅ **To check if Homebrew is installed**, run:

```javascript
brew --version
```

**🔹 2️⃣ Install & Start Apache Kafka on macOS**

1️⃣ **Install Kafka using Homebrew**

```javascript
brew install kafka
```

2️⃣ **Start Zookeeper (Kafka requires Zookeeper)**

```javascript
brew services start zookeeper
```

3️⃣ **Start Kafka Broker**

```javascript
brew services start kafka
```

✅ **Check if Kafka is running:**

```javascript
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties &
kafka-server-start /usr/local/etc/kafka/server.properties &
```

*(You should see Kafka running without errors.)*

**🔹 3️⃣ Create a Kafka Topic (twitter-stream)**

```javascript
kafka-topics --create --topic twitter-stream --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```

✅ **Verify topic creation:**

```bash
kafka-topics --list --bootstrap-server localhost:9092
```

*(You should see twitter-stream in the list.)*

**🔹 4️⃣ Start Kafka Producer (Simulate Live Tweets)**

✅ **Send mock tweets into the Kafka topic:**

```javascript
kafka-console-producer --topic twitter-stream --bootstrap-server localhost:9092
```

✅ **Manually enter some messages to simulate tweets:**

```javascript
{"user": "Alice", "tweet": "I love Spark Streaming!"}
{"user": "Bob", "tweet": "Kafka is amazing for real-time data."}
```

*(These messages will be sent to the twitter-stream topic.)*