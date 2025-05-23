
### 1. How much data your publisher program will send to the message broker in one run? 

    fn main() {
        let mut p = CrosstownBus::new_queue_publisher("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "1".to_owned(), user_name: "2306256324-Amir".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "2".to_owned(), user_name: "2306256324-Budi".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "3".to_owned(), user_name: "2306256324-Cica".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "4".to_owned(), user_name: "2306256324-Dira".to_owned() });
        _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "5".to_owned(), user_name: "2306256324-Emir".to_owned() });
    }

according to the main.rs snippet above, our publisher program will send five distict event to the message broker

### 2. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?

"amqp" is the same protocol that is used by the subscriber program, to connect with the message broker

first "guest" is the username used

the second guest is the password for username "guest"

"localhost" refer to where our message broker is run

and "5672" is the port that our message broker listen to for amqp protocol

the fact that both subscriber and publisher uses the same url means that both subscriber and publisher uses the same instances of RabbitMQ message broker

the RabbitMQ message broker instance in this case works as somekind of event station where the publisher send event and subscriber receive event and process it 

### RabbitMQ image

![](RabbitMQ.png)

### Cargo Run 1

![](CargoRun1.png)

What happened is: first i cargo run on the subscriber directory, the subscriber program establish connection to RabbitMQ message broker through port 5672 with username guest and password guest.

Then i cargo run on the publisher directory, the publisher program then connect to the RabbitMQ message broker through port 5672 with username and password as the subscriber to connect with the same instance of the message broker. 

The publisher program then send 5 message to the message broker through port 5672, and then the message is received by subscriber which is processed and printed on the screen

### RabbitMQ Spike

![](RabbitMQSpike.png)

The spike shown on message rates directly affected by the rates of message that that the RabbitMQ message broker received from the publisher program, in this case because i run the program repeateadly the spike continue to went up until i stop cargo running the publisher program, on which the stream of message stops and the rates went to 0 again