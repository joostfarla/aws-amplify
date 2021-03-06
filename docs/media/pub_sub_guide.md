---
---
# PubSub

AWS Amplify PubSub module provides connectivity with cloud-based message-oriented middleware. You can use PubSub to pass messages between your app instances and your app's backend for creating real-time interactive experiences.

PubSub is available with **AWS IoT** and **Generic MQTT Over WebSocket Provider**. 

With AWS IoT, AWS Amplify PubSub module automatically signs your HTTP requests when sending your messages.
{: .callout .callout--info}

## Installation and Configuration

Import PubSub module and related service provider plugin to your app:
```js
import { PubSub } from 'aws-amplify';
import { MqttOverWSProvider } from "aws-amplify/lib/PubSub/Providers";
```

To configure your service provider with a service endpoint, add following code:
```js
// Apply plugin with configuration
Amplify.addPluggable(new MqttOverWSProvider({
    aws_pubsub_endpoint: 'wss://iot.eclipse.org:443/mqtt',
}));
```

You can integrate any MQTT Over WebSocket provider with your app. Click [here](https://docs.aws.amazon.com/iot/latest/developerguide/protocols.html#mqtt-ws) to learn more about about MQTT Over WebSocket.
{: .callout .callout--info}

## Working with the API

### Subscribe to a topic

In order to start receiving messages from your provider, you need to subscribe to a topic as follows;
```js
PubSub.subscribe('myTopic').subscribe({
    next: data => console.log('Message received', data),
    error: error => console.error(error),
    close: () => console.log('Done'),
});
```

Following events will be triggered with `subscribe()`

Event | Description 
`next` | Triggered every time a message is successfully received for the topic
`error` | Triggered when subscription attempt fails 
`close` | Triggered when you unsubscribe from the topic

### Subscribe to multiple topics

To subscribe for multiple topics, just pass a String array including the topic names:
```js
PubSub.subscribe(['myTopic1','myTopic1']).subscribe({
    // ...
});
```

### Publish to a topic

To send a message to a topic, use `publish()` method with your topic name and the message:
```js
await PubSub.publish('myTopic1', { msg: 'Hello to all subscribers!' });
```

You can also publish a message to multiple topics:
```js
await PubSub.publish(['myTopic1','myTopic2'], { msg: 'Hello to all subscribers!' });
```

### Unsubscribe from a topic

To stop receiving messages from a topic, you can use `unsubscribe()` method:
```js
const sub1 = PubSub.subscribe('myTopicA').subscribe({
    next: data => console.log('Message received', data),
    error: error => console.error(error),
    close: () => console.log('Done'),
});

sub1.unsubscribe();
// You will no longer get messages for 'myTopicA'
```

### API Reference

For the complete API documentation for PubSub module, visit our [API Reference]({%if jekyll.environment == 'production'%}{{site.amplify.baseurl}}{%endif%}/api/classes/pubsub.html)
{: .callout .callout--info}
