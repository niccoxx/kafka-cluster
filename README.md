# Local kafka cluster
## With this yaml file you can get up a kafka cluster in your local environment. My test is only for an api rest env :)
- podman compose up -d
- podman-compose down
- http://localhost:38082/ (api rest endpoint)
- enjoy

# Example
## GET Topic 
curl -v http://localhost:38082/topics

## Produce
curl -X POST \
     -H "Content-Type: application/vnd.kafka.json.v2+json" \
     -H "Accept: application/vnd.kafka.v2+json" \
     --data '{"records":[{"key":"jsmith","value":"alarm clock"},{"key":"htanaka","value":"batteries"},{"key":"awalther","value":"bookshelves"}]}' \
     "http://localhost:38082/topics/test_topic"

## Create consumer
curl -X POST \
     -H "Content-Type: application/vnd.kafka.v2+json" \
     --data '{"name": "ci1", "format": "json", "auto.offset.reset": "earliest"}' \
     http://localhost:38082/consumers/cg1

## Subscribe topic
curl -X POST \
     -H "Content-Type: application/vnd.kafka.v2+json" \
     --data '{"topics":["test_topic"]}' \
     http://localhost:38082/consumers/cg1/instances/ci1/subscription 

## Consume event
 curl -X GET \
     -H "Accept: application/vnd.kafka.json.v2+json" \
     http://localhost:38082/consumers/cg1/instances/ci1/records

sleep 10

curl -X GET \
     -H "Accept: application/vnd.kafka.json.v2+json" \
     http://localhost:38082/consumers/cg1/instances/ci1/records | jq .
