# Persistence

**WARNING: THIS REPO IS DEPRECATED**

---

This repo contains the code used to persist events from kafka to Couchbase.

1. Ensure you have kafka running from the event-bus repo dockerfile first.
2. `docker-compose up`
3. Set up Couchbase: http://localhost:8091
  - Cluster name `couchbase.db`, user `connect`, pass `connect`
  - accept license terms
  - go to advanced options and in server name enter `couchbase.db` **THIS IS IMPORTANT**, otherwise connect won't be able to resolve Couchbase.
  - you may have to change some memory reservation sizes etc.
  - once in to the main GUI console, go to buckets and create bucket called `events`
4. Reboot connect `docker-compose restart connect`
5. Tail logs, check for any errors `docker-compose logs -f`


## Testing

1. Install `kt` (https://github.com/fgeller/kt) and produce an event:
    - `echo '{ "key": "HelloEvent1234123", "value": "MyWeirdEvent", "partition": 0 }' | kt produce -topic events -brokers kafka:9092 -timeout 1s`
    - (you may have to add `127.0.0.1 kafka` to your /etc/hosts file)

2. Check the Couchbase bucket documents to see if the event has been persisted.
