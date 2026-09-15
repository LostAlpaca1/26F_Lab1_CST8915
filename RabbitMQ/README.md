# RabbitMQ 

RabbitMQ is an open-source message broker that acts as an intermediary for messaging between different systems or services. In a microservices architecture, RabbitMQ allows different services to communicate with each other asynchronously and reliably through message queues. 

## Why RabbitMQ?
In microservice architectures, RabbitMQ is used to decouple services, meaning services do not directly communicate with each other. Instead, they send and receive messages through RabbitMQ. This allows each service to function independently, making the application more scalable and easier to maintain. RabbitMQ is built on the AMQP (Advanced Message Queuing Protocol), which allows for robust, reliable messaging.

## Core Concepts
To understand RabbitMQ, it's important to be familiar with a few core concepts:

- **Producer:** A producer is an application or service that sends messages to RabbitMQ. In our application, the Order Service acts as a producer when it sends an order to be processed.
- **Consumer:** A consumer is a service that receives and processes messages from RabbitMQ. For example, another service could act as a consumer by receiving order-related messages from RabbitMQ for further processing or shipping.
- **Queue:** A queue is where RabbitMQ stores messages before they are processed by consumers. Queues provide a way to buffer messages, so if one service sends a message faster than the other can process it, RabbitMQ will hold onto the message in the queue.

## Requirements

- Ubuntu 22.04 or 24.04 with sudo access (on your VM or local machine)
- Start inside the repository's `RabbitMQ` directory. The main guide already takes you there.

## Setup Instructions

1. Run the bundled installer matching your Ubuntu version (`cat /etc/os-release`):

   **Ubuntu 22.04:**
   ```bash
   sudo ./rabbitmq-quick-install-script-Ubuntu_22_04.sh
   ```

   **Ubuntu 24.04:**
   ```bash
   sudo ./rabbitmq-quick-install-script-Ubuntu_24_04.sh
   ```

   For other operating systems, use the [official installation guide](https://www.rabbitmq.com/docs/download).

2. Start RabbitMQ and check its status:

   ```bash
   sudo systemctl start rabbitmq-server
   sudo systemctl status rabbitmq-server --no-pager
   ```

   Expect `active (running)`. RabbitMQ runs in the background; it does not need a dedicated terminal. Its AMQP port is **5672**. The Order Service connects over localhost, so you do not need an inbound NSG rule for that port.

### Optional: Management UI

3. Enable the management plugin and restart RabbitMQ:

   ```bash
   sudo rabbitmq-plugins enable rabbitmq_management
   sudo systemctl restart rabbitmq-server
   ```

4. Create the account for remote management. The first command prompts for a password; choose your own and keep it for login:

   ```bash
   sudo rabbitmqctl add_user newuser
   sudo rabbitmqctl set_user_tags newuser administrator
   sudo rabbitmqctl set_permissions -p / newuser ".*" ".*" ".*"
   ```

5. Open `http://<VM-PUBLIC-IP>:15672` and log in as **newuser** with the password you chose. For a local installation, use `http://localhost:15672`.

   Public access requires the optional NSG rule for port 15672. The `guest` account works only through localhost; keep it for the Order Service's local AMQP connection. [RabbitMQ authentication documentation](https://www.rabbitmq.com/docs/access-control#loopback-users)

### Verify orders without the management UI

**RabbitMQ setup is now complete.** Return to the [main lab guide](../README.md) and continue setting up the rest of the application. This section is for verification later: return here only once all parts of the application are running.

Once the full application is running, place an order through the Store Front, then run:

```bash
sudo rabbitmqctl list_queues name durable messages
```

Expect `order_queue`, `durable=true`, and an increased message count. No order-processing consumer is included in this lab, so successful orders remain queued.
