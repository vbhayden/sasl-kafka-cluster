TLA Kafka Cluster 
==============
This repo is a clone of the Kafka cluster used to route xAPI within the ADL's TLA reference implementation.  

It contains the container definitions and scripts for standing up up a 3/3 SASL-authenticated Kafka cluster with Docker. Images are slightly modified versions of the Confluent Docker images for both Zookeeper and Kafka, with the only adjustments being to relay Compose variables to the Kafka/Zookeeper config files.  Admittedly, this makes the image build time a bit long as I have to install `gettext`, so a cleaner / faster solution would be welcomed.

## What's in the box
Everything necessary to stand up the Kafka cluster, including:
- `docker-compose.yml`: The Compose file we use to stand up:
  - 3 Kafka Brokers
  - 3 Zookeeper nodes
- **Configuration files:**
  - `.env`: Change the `KAFKA_HOST` to an accessible IP / domain name running this.
  - `tla-topics.txt`: Text file with each TLA topic.  
- **Scripts to maintain the cluster:**
  - `tla-topics.txt`: Text file with the Kafka topics.
  - `export-topics.sh`: Creates the Kafka topics (this is necessary as we disable auto-topics).
  - `list-topics.sh`: Lists all topics available in the cluster.

## Setup
We've tried to remove all pain from setting up a Kafka cluster with SASL authentication, but Kafka can still be a bit clunky to get running.

### TL;DR Initial Setup
1. `git clone https://github.com/vbhayden/sasl-kafka-cluster`
1. `cd sasl-kafka-cluster`
1. Modify `.env` and set `KAFKA_HOST` to the resolvable network path for your cluster - this can be a domain name or an IP.
1. `sudo ./install-reqs.sh`
1. `sudo docker-compose up -d --build`
1. `sudo docker logs -f kafka_1`, then wait until the text stops
1. `sudo ./export-topics.sh`

## Topics
Each topic on the cluster exists for a reason, but we also have topics for testing purposes.  By default, there are 3 topics you can use to test out messages and demo producers / consumers: `test-1`, `test-2`, and `test-3`.  

### `learner-xapi`
All statements sent to the LRS will be pushed into this topic by our LRS proxy Kafka producers.  

### `system-xapi`
Historically used to emit logs sent as xAPI statements, but not currently used.

### `resolve-pending`
Topic reserved for statements that were deemed MOM relevant but require additional resolution.

### `resolved-xapi`
Transactional statements.  These statements have been either resolved or confirmed relevant to the current TLA enclave.

### `authority-xapi`
Authoritative statements.  These statements have influenced a competency decision -- usually assertions.