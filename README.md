a. What is amqp?
AMQP (Advanced Message Queuing Protocol) is a communication protocol used for sending messages between applications through a message broker. Instead of applications communicating directly with each other, they send messages to the broker first, and the broker will distribute the messages to the correct receiver. This approach helps applications communicate more efficiently and asynchronously.

b. What does it mean? guest:guest@localhost:5672 , what is the first guest, and what is the second guest, and what is localhost:5672 is for?
The text guest:guest@localhost:5672 is part of the connection URL used to connect to the message broker.
- The first guest is the username.
- The second guest is the password.
- localhost means the broker is running on the same computer.
- 5672 is the default port commonly used by RabbitMQ for AMQP communication.
In this tutorial, the subscriber connects to RabbitMQ using this configuration to listen for incoming messages from the queue.

![alt text](queue-monitoring.png)
The screenshot above shows the RabbitMQ monitoring dashboard after running the publisher multiple times. The spikes on the chart indicate that the publisher was continuously sending event messages to the message broker. Those messages were then placed into queues before being processed by the subscriber. The total number of queues shown in RabbitMQ is 2 because RabbitMQ automatically created the main queue used for handling the user_created events and another related queue for message handling purposes. Since the subscriber processes messages one by one, the queue helps store incoming events temporarily so that no messages are lost while waiting to be consumed.

![alt text](<Screenshot 2026-05-12 at 10.12.54.png>) ![alt text](<Screenshot 2026-05-12 at 10.12.59.png>)
![alt text](<Screenshot 2026-05-12 at 10.12.47.png>)
The screenshot above shows that I ran three subscriber consoles at the same time. When the publisher was executed several times, the messages were not only processed by one subscriber. Instead, RabbitMQ distributed the messages between the active subscribers. This shows that multiple subscribers can work together to process messages from the same queue.

![alt text](three-subscribers-rabbitmq-chart.png)
The RabbitMQ chart also shows that the message queue spike decreased faster compared to when only one subscriber was running. This happens because the messages are processed by several subscribers at the same time, so the queue does not stay full for too long. From the code, one possible improvement is to avoid using an infinite empty loop {} because it keeps the program running without doing anything useful inside the loop. Another improvement is to handle errors more clearly when connecting to RabbitMQ or listening to the queue, instead of only using unwrap().
