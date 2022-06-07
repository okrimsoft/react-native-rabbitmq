## Installation

## RN 0.60 >

## Android

`npm install react-native-rabbit --save`
## IOS

```
npm install react-native-rabbit --save

cd ./ios

change in the Podfile line 1:
platform :ios, '9.0' to platform :ios, '10.0'

pod install
```

## Usage

### Consumer
```
import { Connection, Exchange, Queue } from 'react-native-rabbit';

const config = {
	host:'',
	port:5672,
	username:'user',
	password:'password',
	virtualhost:'vhost',
	ttl: 10000 // Message time to live,
	ssl: true // Enable ssl connection, make sure the port is 5671 or an other ssl port
}

const queue_name = '' ;
const exchange_name = '' ;
let connection = new Connection(config);

connection.on('error', (event) => {
    console.log(event);
});

connection.on('connected', (event) => {
    console.log(event);
	let queue = new Queue( connection, {
		name: queue_name,
		passive: false,
		durable: true,
		exclusive: false,
		consumer_arguments: {'x-priority': 1}
	}, {
	// queueDeclare args here like x-message-ttl
	});

	let exchange = new Exchange(connection, {
		name: exchange_name,
		type: 'direct',
		durable: true,
		autoDelete: false,
		internal: false
	});

	queue.bind(exchange, queue_name);

	// Receive one message when it arrives
	queue.on('message', (data) => {
        console.log(data);
        queue.basicAck(data.delivery_tag) ;
	});
    
    connection.on('error', (event) => {
        console.log(connection);
        console.log("you are not connected");
    });
});

connection.connect();
```

### Producer

Not yet tested and documented

## Credit

This library is a fork of [Tim Honders](https://github.com/kegaretail/react-native-rabbitmq) version .