# apache-kafka
Messaging system to read log files from web server

Setup 3 node cluster with replication factor 3 for fault-tolerance

Install Apache Kafka:
  tar -zxvf kafka_2.11-0.10.0.1.tgz

Zookeeper(with default proeprties):
  START zookeeper: bin/zookeeper-server-start.sh config/zookeeper.properties
  
Broker/kafka server(multi-broker cluster 3-node kafka cluster):
  config/server.properties
  cp config/server.properties config/server-1.properties
  cp config/server.properties config/server-2.properties
  
  config/server.properties
    broker.id=0
    listeners=PLAINTEXT://:9092
    log.dir=/tmp/kafka-logs
  config/server-1.properties
    broker.id=1
    listeners=PLAINTEXT://:9093
    log.dir=/tmp/kafka-logs-1
  config/server-1.properties
    broker.id=2
    listeners=PLAINTEXT://:9094
    log.dir=/tmp/kafka-logs-2
    
  START kafka servers:
    bin/kafka-server-start.sh config/server.properties
    bin/kafka-server-start.sh config/server-1.properties
    bin/kafka-server-start.sh config/server-2.properties
  
Topics:
  => Create topics
    bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic event-topic-1
    bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 2 --topic event-topic-2
  => Check topic status 
    bin/kafka-topics.sh --describe -zookeeper localhost:2181 --topic event-topic-1
    bin/kafka-topics.sh --describe -zookeeper localhost:2181 --topic event-topic-2
    
Producer:
  => Create messages
    bin/kafka-conole-producer.sh --broker-list loclahost:9092 --topic event-topic-1 --zookeeper localhost:2181

Consumer:
  => Consumes messages
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic --zookeeper event-topic-1
    


