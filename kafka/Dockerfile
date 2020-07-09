FROM confluentinc/cp-kafka:5.5.1

RUN apt-get update && apt-get install -y gettext

ARG KAFKA_SASL_USER
ARG KAFKA_SASL_PASS

COPY config/server.conf /tmp/kafka/secrets/server.temp
COPY config/client.conf /tmp/kafka/secrets/client.temp

RUN cat /tmp/kafka/secrets/server.temp \
	| envsubst '$KAFKA_SASL_USER' \
	| envsubst '$KAFKA_SASL_PASS' \
	| tee /tmp/kafka/server.txt

RUN cat /tmp/kafka/secrets/client.temp \
	| envsubst '$KAFKA_SASL_USER' \
	| envsubst '$KAFKA_SASL_PASS' \
	| tee /tmp/kafka/client.txt
