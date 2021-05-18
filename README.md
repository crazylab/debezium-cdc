Start Debezium with Kafka, Zookeeper and MySQL
```bash
$ docker container prune
$ docker compose up
```

Setup Debezium MySQL connector
```bash
# Connector rest api docs => https://docs.confluent.io/current/connect/references/restapi.html
$ KAFKA_CONNECT_HOST=http://localhost:8083/connectors/

# Check Existing Connectors
$ curl -XGET $KAFKA_CONNECT_HOST

# Add MySQL Connector
$ curl --fail -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" $KAFKA_CONNECT_HOST -d @register-connect-mysql.json
```
Reference: https://debezium.io/documentation/reference/tutorial.html

----
Test by doing some operations on mysql
- Start a MySQL command line client
```bash
$ docker exec -it mysql sh -c 'exec mysql -h"localhost" -P"3306" -uroot -p"$MYSQL_ROOT_PASSWORD"'
```

```mysql
use inventory;
show tables;

CREATE TABLE student (name varchar(100), roll_no integer);

INSERT INTO student VALUES ("Josh Doe", 10);
UPDATE student SET name="John Doe" WHERE roll_no=10;
DELETE student WHERE roll_no=10;
```
- Checking Kafka
```bash
# List all topics
$ docker exec -it kafka sh -c 'exec ./bin/kafka-topics.sh --bootstrap-server kafka:9092 --list'

# Consume messages from Kafka
$ TOPIC=local.inventory.student
$ docker exec -it kafka sh -c "exec ./bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 print.key=true --from-beginning --topic $TOPIC"
```
