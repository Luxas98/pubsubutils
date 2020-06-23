# Small wrapper around GCP PubSub

I created this small wrapper as I noticed lot of code repetition with publisher/subscribers and path consistency.

## Usage

ENV variable:

```bash
export PUB_SUB_CREDENTIALS=/path/to/gcp/service/account/credentials.json
export MAX_MESSAGES=100
```

### Subscribers

```python
import os
from pubsubutils.pubsub import subscribe, callback_info

topic_name = os.environ.get("TOPIC_NAME", "my-topic")
subscription_name = os.environ.get("SUB_NAME", f"{topic_name}-sub")

subscribe(topic_name, subscription_name, callback)

def callback(message):
    data, metadata = callback_info(message)

    # Process your message
    message.ack()
```

### Publishers

```python
import os
from pubsubutils.pubsub import publisher, publish

topic_name = os.environ.get("TOPIC_NAME", "my-topic")
my_publisher, my_topic_path = publisher()

publish(
    my_publisher,
    my_topic_path,
    data=b'Message to send!',
    meta={"metadata_field": "YAY!"},
)
```