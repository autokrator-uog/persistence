# Persistence
Before a revamped implementation of querying was added to the event bus proper, this repository made use of [Kafka Connect](https://www.confluent.io/product/connectors/) to persist events sent to Kafka by the event bus into Couchbase for later querying.

**Deprecated:** This repository is no longer in use, please review the `project-and-dissertation` project for currently in-use repositories.
# Persistence

## How to use
Follow the below steps to set up the persistence.

  1. Ensure you have Kafka running from the core repository.
  2. Run `docker-compose up`
  3. Set up Couchbase: `http://localhost:8091`
    - Use the following details:
      - Cluster Name: `couchbase.db`
      - User: `connect`
      - Pass: `connect`
    - Accept license terms.
    - Go to advanced options and for server name enter `couchbase.db`.
      - You may have to change some memory reservation sizes etc.
    - Once logged in to the main GUI console, go to buckets and create bucket called `events`.
  4. Reboot connect `docker-compose restart connect`
  5. Tail logs, check for any errors `docker-compose logs -f`

## How to test

  1. Install [kt](https://github.com/fgeller/kt) and produce an event:
      - `echo '{ "key": "HelloEvent1234123", "value": "MyWeirdEvent", "partition": 0 }' | kt produce -topic events -brokers kafka:9092 -timeout 1s`
      - You may have to add `127.0.0.1 kafka` to your /etc/hosts file.
  2. Check the Couchbase bucket documents to see if the event has been persisted.
