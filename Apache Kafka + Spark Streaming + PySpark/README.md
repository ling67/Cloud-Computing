# Kafka + Spark Streaming + PySpark

## 1. Introduction

Real-time data ingesting is a common problem in real-time analytics, because in a platform such as e-commerce, active users in a given time and the number of events created by each active user are many. Hence, recommendations (i.e., predictions) for each event or groups of events are expected to be near real-time.

Apache Kafka addresses the first two problems stated above. It is a distributed streaming platform, which helps to build real-time streaming data pipelines.

In this project, we want use kafka and Spark Streaming to work together.

![image](https://user-images.githubusercontent.com/93315926/205775026-2681cd4d-f970-4e4a-9158-4e58b56162e1.png)

## 2. Steps
1. Discuss the steps to perform to setup Apache Spark in a Linux environment.
2. Starting Kafka (for more details, please refer to this article).
3. Creating a PySpark app for consume and process the events and write back to Kafka.
4. Steps to produce and consume events using Kafka-Python.

## 3. Environment install

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

### Setup Spark
```
$ pip3 install msgpack
$ pip3 install kafka-python
```

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205808538-c65e2bbf-f463-426a-8511-7ce3bf78cf16.png">

if pip3 command not found:(if it works, you don't need to do it again.)
```
$ sudo apt install python3-pip
$ wget https://repo1.maven.org/maven2/org/apache/spark/spark-streaming-kafka-0-8-assembly_2.11/2.3.2/spark-streaming-kafka-0-8-assembly_2.11-2.3.2.jar  （感觉不用）
```

## 4. Check kafka is working, use input_event topic as example

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205804044-ba9d9d3a-0fa9-434b-8833-c20940af18cd.png">

Step 1. Start Kafka Zookeeper (Keep this terminal open: terminal 1)
```
$ cd kafka_2.12-3.3.1/
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```
<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205801936-68f18e1d-9815-4935-bad0-299b2d9cdce3.png">

Step 2. Start Kafka broker(Keep this terminal open: terminal 2)
```
$ cd kafka_2.12-3.3.1/
$ bin/kafka-server-start.sh config/server.properties
```
<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205802349-56ed3163-1629-41a5-9054-9d17c157f37f.png">

Step 3: Create one Kafka Topics (input_event)
```
$ cd kafka_2.12-3.3.1/
$ bin/kafka-topics.sh --create --topic input_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
```

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/205804044-ba9d9d3a-0fa9-434b-8833-c20940af18cd.png">

Step 4: Create consumer.py, then run it (terminal 3)
```
$ vi consumer.py
$ python3 consumer.py
```

```consumber.py code
from kafka import KafkaConsumer

consumer = KafkaConsumer('input_event', bootstrap_servers=['localhost:9092'])
for msg in consumer:
    print(msg)
```

Step 5: Create producer.py, then run it(terminal 4)
```
$ vi producer.py
$ python3 producer.py
```

```producer.py code
from kafka import KafkaProducer
producer = KafkaProducer(bootstrap_servers='localhost:9092')
producer.send('input_event', b'(1, Main Menu), (2, Phone), (3, Smart Phone), (4, iPhone)')
producer.close()
```

Step 6: Result

<img width="800" alt="image" src="https://user-images.githubusercontent.com/93315926/205826199-d0236087-b38f-40c0-a9b1-818bed79d2d5.png">

## 5. Implement Kafka + Spark Streaming + PySpark

Step 1: In terminal 1(Start Kafka Zookeeper):
```
$ cd kafka_2.12-3.3.1/
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

Step 2: In terminal 2(Start Kafka broker):
```
$ cd kafka_2.12-3.3.1/
$ bin/kafka-server-start.sh config/server.properties
```

Step 3: Create two Kafka Topics (input_event and output_event)
```
$ cd kafka_2.12-3.3.1/
$ bin/kafka-topics.sh --create --topic input_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
$ bin/kafka-topics.sh --create --topic output_event --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
```

Now, we have two topic: input_event, output_event

Step 4: In terminal 3, create consumer.py, then run it:
```
$ vi consumer.py
$ python3 consumer.py

from kafka import KafkaConsumer

consumer = KafkaConsumer('output_event', bootstrap_servers=['localhost:9092'])
for msg in consumer:
    print(msg)
```

Step 5: In terminal 4, create producer.py, then run it:
```
$ vi producer.py
$ python3 producer.py

from kafka import KafkaProducer

producer = KafkaProducer(bootstrap_servers='localhost:9092', value_serializer=str.encode, key_serializer=str.encode)
event_stream_key = 'product_list'
event_stream_value = 'product1 product2 product3 product1'
producer.send('input_event', key = event_stream_key, value = event_stream_value)
```

In terminal 5, running spark_processor.py

spark_processor.py will process events and write back to Kafka:
1.consume the message from input_event topic 
2.process the data 
3.push the message to output_event topic.

Reference: code in 上面

```
$ spark-submit --packages org.apache.spark:spark-streaming-kafka-0-10_2.12:3.1.3,org.apache.spark:spark-sql-kafka-0-10_2.12:3.1.3 --deploy-mode client spark_processor.py
```

Error: It is not working. Spark-streaming-kafka-0-10 doesn't support python, spark-streaming-kafka-0-8 supports python, but spark version need roll back to 2.* version. Please refer to this link for details.

## Analyse

Now, we have two method:
1. install 2.* version spark to run the program. It is not working
2. Change the spark_processor.py code to receiving data from kafka. https://spark.apache.org/docs/latest/structured-streaming-kafka-integration.html

Then I try the second method, it still have some problem, I can't receive the data from kafka.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/93315926/206615779-467f263e-bd22-4f7d-86f1-0fb741a2d701.png">

<img width="800" alt="image" src="https://user-images.githubusercontent.com/93315926/206615600-d7742a82-8fe0-447e-90d9-80293e45a54c.png">

<img width="800" alt="image" src="https://user-images.githubusercontent.com/93315926/206615819-1bfea664-52a6-45b7-933b-f010265c8ef5.png">

## 


