
## Requirements

1. The Pub-Sub system should allow publishers to publish messages to specific topics.
2. Subscribers should be able to subscribe to topics of interest and receive messages published to those topics.
3. The system should support multiple publishers and subscribers.
4. Messages should be delivered to all subscribers of a topic in real-time.
5. The system should handle concurrent access and ensure thread safety.
6. The Pub-Sub system should be scalable and efficient in terms of message delivery.

## Classes, Interfaces and Enumerations

1. Publisher
2. Message
3. Topic
4. Subscriber
5. Thread Safety

1. The **Message** class represents a message that can be published and received by subscribers. It contains the message content.

```
public class Message {
	private final String content;

	public Message(String content) {
		this.content = content;
	}

	public String getContent() {
		return content;
	}
}
```


2. The **Topic** class represents a topic to which messages can be published. It maintains a set of subscribers and provides methods to add and remove subscribers, as well as publish messages to all subscribers.

```
public class Topic {
	private final String name;
	private final Set<Subscriber> subscribers = new CopyOnWriteArraySet<>();

	public Topic(String name) {
		this.name = name;
	}

	public String getName() {
		return name;
	}

	public void addSubscriber(Subscriber subscriber) {
		subscribers.add(subscriber);
	}

	public void removeSubscriber(Subscriber subscriber) {
		subscribers.remove(subscriber);
	}

	public void publish(String message) {
		for(Subscriber sub : subscribers) {
			sub.onMessage(message);
		}
	}
}
```


3. The **Subscriber** interface defines the contract for subscribers. It declares the onMessage method that is invoked when a subscriber receives a message.

```
interface Subscriber {
	void onMessage(String message);
}
```


4. The **PrintSubscriber** class is a concrete implementation of the Subscriber interface. It receives messages and prints them to the console

```
 public class PrintSubscriber implements Subscriber {
	private final String name;

	public PrintSubscriber(String name) {
		this.name = name;
	}

	@Override
	public void onMessage(String message) {
			System.out.println("Subscriber " + name + " received message: " + message.getContent());
	}
 }
```

5. The **Publisher** class represents a publisher that publishes messages to a specific topic.

```
public class Publisher {

	private final Set<Topic> topics;

	public Publisher() {
		this.topics = new HashSet<>();
	}

	public void registerTopic(Topic topic) {
		topics.add(topic);
	}

	public void publish(Topic topic, Message message) {
		if(!topics.contains(topic)) {
			System.out.println("The publisher can't publish to topic " + topic.getName());
			return;
		}
		topic.publish(message);
	}
}
```

6. The **PubSubSystem** class is the main class that manages topics, subscribers, and message publishing. It uses a ConcurrentHashMap to store topics and an ExecutorService to handle concurrent message publishing.

```
public class PubSubSystem {
	
	public static void main(String[] args) {

		Topic topic1 = new Topic("Topic1");
		Topic topic2 = new Topic("Topic2");

		Publisher publisher1 = new Publisher();
		Publisher publisher2 = new Publisher();

		Subscriber subscriber1 = new PrintSubscriber("Subscriber1");
		Subscriber subscriber2 = new PrintSubscriber("Subscriber2");
		Subscriber subscriber3 = new PrintSubscriber("Subscriber3");

		publisher1.registerTopic(topic1);
		publisher2.registerTopic(topic2);

		topic1.addSubscriber(subscriber1);
		topic1.addSubscriber(subscriber2);
		topic2.addSubscriber(subscriber2);
		topic2.addSubscriber(subscriber3);

		publisher1.publish(topic1, new Message("Message1 for Topic1"));
        publisher1.publish(topic1, new Message("Message2 for Topic1"));
        publisher2.publish(topic2, new Message("Message1 for Topic2"));

		topic1.removeSubscriber(subscriber2);

		publisher1.publish(topic1, new Message("Message3 for Topic1"));
        publisher2.publish(topic2, new Message("Message2 for Topic2"));
	}
}
```


