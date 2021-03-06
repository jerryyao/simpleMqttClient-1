# What's this?

This is simple MQTT Client in Python.
You can publish and subscribe.
And this is consisted of MQTTManager and MQTTSubscriber class.

You can inherit MQTTSubscriber classs as your subscriber.
And you can hanle your registered topic in your subscriber class.

# How to use

## Preparation

```
$ pip install paho-mqtt
```

On Ubuntu environment, you need to use ```sudo pip install paho-mqtt``` instead.

## Help

```
./simpleMqttClient.py -h
Usage: simpleMqttClient.py [options]

Options:
  -h, --help            show this help message and exit
  -c CLIENTID, --clientId=CLIENTID
                        Specify client Id
  -s HOST, --host=HOST  Specify mqtt server
  -p PORT, --port=PORT  Specify mqtt port
  -u USERNAME, --username=USERNAME
                        Specify username
  -k PASSWORD, --password=PASSWORD
                        Specify password
  -t TOPIC, --topic=TOPIC
                        Specify subscribing topic
  -v PUBLISHVAL, --publishVal=PUBLISHVAL
                        Specify publishing value (use with --topic)
```

## How to subscribe

```
$ ./simpleMqttClient.py --host 192.168.10.1 -t "/hidenorly/#"
```

## How to publish

```
$ ./simpleMqttClient.py --host 192.168.10.1 -t "/hidenorly/yahho" -v "hoge" -c "hoge"
```


## Explanation of my classes

```
class MySubscriber(MQTTSubscriber):
	def onMessage(self, msg):
		print("MySubscriber:" + msg.topic + " " + str(msg.payload))

if __name__ == '__main__':
..snip..
	mqtt = MQTTManager(options.clientId, options.host, options.port, options.username, options.password, False)

	mqtt.connect()

	if options.publishVal:
		mqtt.publish(options.topic, options.publishVal)
	else:
		aSubscriber = MySubscriber(options.topic)
		mqtt.addSubscriber( aSubscriber.topic, aSubscriber )
		mqtt.enableSubscriber( aSubscriber.topic, True )
		mqtt.loop()

	mqtt.disconnect()
```