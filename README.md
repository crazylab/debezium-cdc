Start Debezium with Kafka, Zookeeper and MySQL
```bash
$ docker compose up --no-deps --build
```

Setup Debezium MySQL connector
```bash
# Connector rest api docs => https://docs.confluent.io/current/connect/references/restapi.html
KAFKA_CONNECT_HOST=http://localhost:8083/connectors/

# Check Existing Connectors
curl -XGET $KAFKA_CONNECT_HOST

# Add MySQL Connector
$ curl --fail -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" $KAFKA_CONNECT_HOST -d @register-connect-mysql.json
```

Start a MySQL command line client
```bash
$ docker exec -it mysql sh -c 'exec mysql -h"localhost" -P"3306" -uroot -p"$MYSQL_ROOT_PASSWORD"'
```

Checking Kafka
```bash
# List all topics
docker exec -it kafka sh -c 'exec ./bin/kafka-topics.sh --bootstrap-server kafka:9092 --list'

# Consume messages from Kafka
TOPIC=local.inventory.student
docker exec -it kafka sh -c "exec ./bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --from-beginning --topic $TOPIC"
```

Reference: https://debezium.io/documentation/reference/tutorial.html


