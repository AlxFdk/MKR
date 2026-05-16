# МКР: Apache Kafka — request–reply (Гіпотеза Колатца)

## Опис
Проєкт реалізує міжконтейнерну взаємодію через Apache Kafka за шаблоном **request-reply**.
**Producer** генерує унікальний `correlation-id`, відправляє діапазон чисел (10-100) і чекає на відповідь.
**Consumer** отримує діапазон, обчислює середню кількість кроків для послідовностей Колатца, і повертає результат Producer'у з тим самим `correlation-id`.

## Інструкція з запуску

### 1. Створення мережі та запуск Kafka
```bash
docker network create kafka-net

docker run -d --name kafka --network kafka-net -p 9092:9092 -e KAFKA_NODE_ID=1 -e KAFKA_PROCESS_ROLES=broker,controller -e KAFKA_CONTROLLER_QUORUM_VOTERS=1@kafka:9093 -e KAFKA_CONTROLLER_LISTENER_NAMES=CONTROLLER -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:29092,CONTROLLER://0.0.0.0:9093,PLAINTEXT_HOST://0.0.0.0:9092 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092 -e KAFKA_LISTENER_SECURITY_PROTOCOL_MAP=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT -e KAFKA_INTER_BROKER_LISTENER_NAME=PLAINTEXT -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 -e KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR=1 -e KAFKA_TRANSACTION_STATE_LOG_MIN_ISR=1 -e KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS=0 -e KAFKA_AUTO_CREATE_TOPICS_ENABLE=true -e CLUSTER_ID=MkU3OEVBNTcwNTJENDM2Qk -v kafka-data:/var/lib/kafka/data confluentinc/cp-kafka:7.7.1