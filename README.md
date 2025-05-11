# kafka-playground

Kafka is a distributed event streaming platform. It lets us send, store, and process large volume of data in real time. I like to think of Kafka as a fast, durable, and highly scalable message queue.

Data is sent and recieved as key-value messages, and they are organized into topics. Those topics are split into partitions which are spread out across cluster of brokers. Those clusters are like a group of servers working together so it can handle large data volume in parallel and be fault tolerant in case one broker fails.

In this project, I created a simple **Java** app that has a producer (the component that sends a message) and a consumer (the component that reads and process a message). 
  * It uses **Gradle** to build, compile, and run this Java app
      * All the dependencies and its specific versions are managed by Gradle (see the `build.gradle` file) so you don't need to manually download and manage them. _Super nifty right?_
  * **Docker** is used to create the Kafka and Zookeeper containers that the app needs (aka setting up the message queue). _I have a separate [Docker repo](https://github.com/mai-thao/docker-playground) to learn more!_
      * The Kafka container stores the topic and data
      * The Zookeeper handles the cluster coordination, topic/partition metadata, configurations, etc. But, in my example, we're using the [confluentinc Docker image](https://hub.docker.com/r/confluentinc/cp-kafka) which defaults to just one single-broker

### How to run the app
1) Start up the Docker containers with the command: `docker-compose up -d`
2) Open two different terminals and execute both these commands:
    * On the first terminal, execute the command to start the Kafka consumer app: `./gradlew runConsumer`
       * You should see the printed message: `Waiting for messages...` 
    * On the second terminal, execute the commmand 3 times to run the Producer app so it sends 3 messages to the topic: `./gradlew runProducer`
3) Give it a few seconds and you should see there are 3 key-value messages consumed from the consumer app:
```
Received: key = key1, value = Hello from Java!
Received: key = key1, value = Hello from Java!
Received: key = key1, value = Hello from Java!
```
4) Stop the consumer app with Ctrl + C
5) Stop the Docker containers with the command: `docker compose down`

### Notes

As you can see, Kafka is super helpful because it acts like a middle-man that stores large volume of messages between two different systems. It loosely decouples those two systems so they don't talk to each directly and can send and process large volume of data at different speeds. 

For example, if the producer is too fast, it doesn't affect the consumer and likewise, if the consumer is too slow, it doesn't affect the producer's speed either! The messages can live in the topics for days or weeks and the consumer can catch up whenever they're ready.

_Kafka is a huge platform! Take your time to learn and explore it using reputable sources like: https://developer.confluent.io/what-is-apache-kafka/ or https://kafka.apache.org/intro_
