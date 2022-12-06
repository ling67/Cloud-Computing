# Kafka + Spark Streaming + PySpark

## Introduction

Real-time data ingesting is a common problem in real-time analytics, because in a platform such as e-commerce, active users in a given time and the number of events created by each active user are many. Hence, recommendations (i.e., predictions) for each event or groups of events are expected to be near real-time.

Apache Kafka addresses the first two problems stated above. It is a distributed streaming platform, which helps to build real-time streaming data pipelines.

In this project, we want use kafka and Spark Streaming to work together.

![image](https://user-images.githubusercontent.com/93315926/205775026-2681cd4d-f970-4e4a-9158-4e58b56162e1.png)

## Steps
1. Discuss the steps to perform to setup Apache Spark in a Linux environment.
2. Starting Kafka (for more details, please refer to this article).
3. Creating a PySpark app for consume and process the events and write back to Kafka.
4. Steps to produce and consume events using Kafka-Python.

## Environment install

### Create GCP vitural machine 
Reference week 4 homework 2.

1.Create, Configure and Launch a Google Cloud Dataproc cluster.

<img width="250" alt="image" src="https://user-images.githubusercontent.com/93315926/205791107-c8a67256-fd53-4142-8289-d13a86090e2b.png">

<img width="600" alt="image" src="https://user-images.githubusercontent.com/93315926/205791268-451d8fef-ff6a-4489-a24a-bcd3f25c3a28.png">

<img width="250" alt="image" src="https://user-images.githubusercontent.com/93315926/205791594-2e26c438-ce23-4315-922b-c399688f0b28.png">

2.Connecting to the Master Node using Secure Shell (ssh) 

<img width="600" alt="image" src="https://user-images.githubusercontent.com/93315926/205791836-2e503731-1218-4061-b946-f4e80cf241a8.png">

### Setup Kafka

Step 1. Downlaod kafka 

which is available at https://kafka.apache.org/downloads

```
$ wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
$ tar -xvf kafka_2.12-3.3.1.tgz
```
<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205801575-d1fd1817-006e-4957-92d9-f3d6a2b0a9f2.png">

## Setup Spark
```
$ pip3 install msgpack
$ pip3 install kafka-python
if pip3 command not found,
$ sudo apt install python3-pip
$ wget https://repo1.maven.org/maven2/org/apache/spark/spark-streaming-kafka-0-8-assembly_2.11/2.3.2/spark-streaming-kafka-0-8-assembly_2.11-2.3.2.jar  （感觉不用）
```

## Check kafka is working, use input_event topic as example

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205804044-ba9d9d3a-0fa9-434b-8833-c20940af18cd.png">

Step 1. Start Kafka Zookeeper (Keep this terminal open: terminal 1)
```
$ cd kafka_2.12-3.3.1/
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```
<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205801936-68f18e1d-9815-4935-bad0-299b2d9cdce3.png">

Step 2. Start Kafka broker(Keep this terminal open: terminal 1)
```
$ cd kafka_2.12-3.3.1/
$ bin/kafka-server-start.sh config/server.properties
```
<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205802349-56ed3163-1629-41a5-9054-9d17c157f37f.png">

Step 3: Create two Kafka Topics (input_event and output_event)
```
$ cd kafka_2.12-3.3.1/
$ bin/kafka-topics.sh --create --topic input_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
$ bin/kafka-topics.sh --create --topic output_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
```

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205804044-ba9d9d3a-0fa9-434b-8833-c20940af18cd.png">


Step 4.Create consumer.py, then run it (terminal 3)
```
$ vi consumer.py
$ python3 consumer.py
```

```consumber.py code
from kafka import KafkaConsumer

consumer = KafkaConsumer('input_event', bootstrap_servers=['localhost:9092'])
for msg in consumer:
    print(msg.value)
```

Step 5.Create producer.py, then run it(terminal 4)
```
$ vi producer.py
$ python3 producer.py
```

```producer.py code
from kafka import KafkaProducer

producer = KafkaProducer(bootstrap_servers='localhost:9092', value_serializer=str.encode, key_serializer=str.encode)
event_stream_key = 'product_list'
event_stream_value = 'product1 product2 product3 product1'
producer.send('input_event', key = event_stream_key, value = event_stream_value)
```

Step 6.Result

图

## implement

Now, we have two topic: input_event, output_event

Runing sequence:

1. producer.py
2. consumer.py
3. spark_processor.py

In terminal 1(Start Kafka Zookeeper):
```
$ cd kafka_2.12-3.3.1/
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

In terminal 2(Start Kafka broker):
```
$ cd kafka_2.12-3.3.1/
$ bin/kafka-server-start.sh config/server.properties
```

In terminal 3, create consumer.py, then run it:
```
$ vi consumer.py
$ python3 consumer.py

from kafka import KafkaConsumer

consumer = KafkaConsumer('output_event', bootstrap_servers=['localhost:9092'])
for msg in consumer:
    print(msg.value)
```

In terminal 4, create producer.py, then run it:
```
$ vi producer.py
$ python3 producer.py

from kafka import KafkaProducer

producer = KafkaProducer(bootstrap_servers='localhost:9092', value_serializer=str.encode, key_serializer=str.encode)
event_stream_key = 'product_list'
event_stream_value = 'product1 product2 product3 product1'
producer.send('input_event', key = event_stream_key, value = event_stream_value)
```

In terminal 5
```
$ start-master.sh
```

In terminal 6, running spark_processor.py

spark_processor.py will process events and write back to Kafka:
1.consume the message from input_event topic 
2.process the data 
3.push the message to output_event topic.

Reference: code in 上面

```
$ ./spark/bin/spark-submit 
--jars myrun/spark-streaming-kafka-0-10_2.12-3.3.1.jar 
--packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.1
--master spark://34.70.211.224:7077    
--deploy-mode client pyspark_script/spark_processor.py
```

## Result



