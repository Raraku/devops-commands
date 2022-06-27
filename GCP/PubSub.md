# Commands related to GCP PubSub

- Create a topic in pubsub  
  `gcloud pubsub topics create <topic-name>`

- Create subscription to topic  
  `gcloud pubsub subscriptions create --topic <topic> <subscription-name>`

- List subscriptions  
  `gcloud pubsub topics list-subscriptions <subscription-name>`

- publish message to topic  
  `gcloud pubsub topics publish <topic-name> --message "Hello"`

- Pull a message from topic, the `auto-ack` acknoledges the message.  
  `gcloud pubsub subscriptions pull <subscription-name> --auto-ack`
