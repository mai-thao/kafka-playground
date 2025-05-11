# kafka-playground

Kafka is a distributed event streaming platform. It lets us send, store, and process large volume of data in real time. I like to think of Kafka as a fast, durable, and highly scalable message queue.

Data is sent and recieved as key-value messages, and they are organized into topics. Those topics are split into partitions which are spread out across cluster of brokers. Those clusters are like a group of servers working together so it can handle large data volume in parallel and be fault tolerant in case one broker fails.

In this project, I created a simple Java app that has a producer (the component that sends a message) and a consumer (the component that reads and process a message). 
  * It uses Gradle to build, compile, and run this Java app
      * All the dependencies and its specific versions are managed by Gradle (see the `build.gradle` file) so you don't need to manually download and manage them. _Super nifty right?_
  * Docker is used to create the Kafka and Zookeeper containers that the app needs (aka setting up the message queue)
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
