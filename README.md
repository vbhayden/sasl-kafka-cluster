ADL Kafka Cluster 
==============

This folder contains the scripts to set up a 3/3 SASL-authenticated Kafka cluster with Docker. Nothing in this project is proprietary in the least and we use the Confluent Docker images for both Zookeeper and Kafka.  The only modifications made are for administrative quality of life.

## What's in the box
Everything necessary to stand up the Kafka cluster, including:
- `docker-compose.yml`: The Compose file we use to stand up:
  - 3 Kafka Brokers
  - 3 Zookeeper nodes
- **Configuration files:**
  - `env.env`: Change the `KAFKA_HOST` to an accessible IP / DNS running this.
  - `tla-topics.txt`: Text file with each TLA topic.  
- **Scripts to maintain the cluster:**
  - `rebuild.sh`: Starts or restarts the cluster
  - `export-topics.sh`: Creates the Kafka topics (this is necessary as we disable auto-topics)
  - `list-topics.sh`: Lists all topics available in the cluster

## Setup
We've tried to remove all pain from setting up a Kafka cluster with SASL authentication, but there is still a little more than just running `rebuild.sh` and logging out.

### TL;DR Initial Setup
1. `git clone https://github.com/vbhayden/sasl-kafka-cluster`
1. `cd sasl-kafka-cluster`
1. `sudo ./install-reqs.sh`
1. `sudo ./rebuild.sh`
1. `sudo docker logs -f kafka_1`, then wait until the text stops
1. `sudo ./export-topics.sh`

### TL;DR Restarting
The containers are set up to restart regardless of what killed them, so this should only be necessary when making changes to the cluster.
1. `sudo ./rebuild.sh`

## Topics
Each topic on the cluster exists for a reason, but we also have topics for testing purposes.  By default, there are 3 topics you can use to test out messages and demo producers / consumers: `test-1`, `test-2`, and `test-3`.  

### `learner-xapi`
All statements sent to the LRS will be pushed into this topic by our LRS proxy Kafka producers.  

### `system-xapi`
Historically used to emit logs sent as xAPI statements.

### `resolve-pending`
Topic reserved for statements that were deemed MOM relevant but require additional resolution.

### `resolved-xapi`
Transactional statements.  These statements have been either resolved or confirmed relevant to the current TLA enclave.

### `authority-xapi`
Authoritative statements.  These statements have influenced a competency decision -- usually assertions.