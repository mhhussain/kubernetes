docker pull centos

docker run -dit --name k-cent

yum install wget // for downloading kafka from website

yum install java // required for kafka

tar -xzf <kakfa.tar> // extract kafka tar

cd <kafka dir> // from above

// start single zookeeper
bin/zookeeper-server-start.sh config/zookeeper.properties

// start single broker on 9092
bin/kafka-server-start.sh config/server.properties

// create test topic
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test-topic

// produce messages: localhost:9092
// consume messages: localhost:9092

docker run -t --name kafkabox -p 9092:9092 moohh/kafkabox
