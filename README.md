# LV AF PubSub
A PubSub solution for LabVIEW Actors.

This is a demo as a proof-of-concept. Better dont use it in production code.
 
# Description
Ever got tiered of the quirks of keeping references to your actors enqueuers up to date and know which reference can accept which messages?
I was... so I implemented some kind of PubSub-like helper that allows to add actors to topics and send messages to topics. 
These topics may simple be strings, but -more interstingly- may also be interfaces. So you can simply send a message related to an interface some of your actors implement to the channel associated to this interface. As the sender, you can do nothing wrong. You simply have to make sure, that your actors subscribe to the interfaces they implement. 
Even better: It doesn't matter if the sender or the reciever comes up first. And subscriptions can be made and rejected anytime in runtime.
You can even retain messages, eleminating race-conditions during startup of actors. And (not implemented here but tested), you can create a channel that pushes serialized messages over MQTT or something else. Or you can create load-balancing channels... Or a /dev/null channel if you like... Or you can subscribe a channel to all other channels, that logs your messages... 

# Usage
Subscribe your actor anytime, anywhere to a topic either given by a string id or by a class / interface:

![grafik](https://user-images.githubusercontent.com/4790227/227496376-629ff2bd-2431-4c37-bc54-3625e60f934e.png)


Send a message to the topic channel anywhere, anytime:

![grafik](https://user-images.githubusercontent.com/4790227/227496886-d689a02a-f8c7-4d6d-8837-18e377b41c3c.png)


Unsubscribe the actor anytime or when it stops. If no subscribers are left, the channel will close.

![grafik](https://user-images.githubusercontent.com/4790227/227503878-78718da6-0ae8-44ea-a677-aa2894e0af6b.png)

# Details

Every channel has its own message queue and runs asynchronous. It keeps track of the subscribed actors (identified by their enqueuer). When a message is dequeued, copies of it are send to every subscribed enqueuer.
The management of the channels and their assoicited topics is done using a single VI, which is wrapped by some helpers. This way, no references need to be passed, everything is stored in the non-reentrant registry, which is associated to the application instance.

# Dependencies

Just the vanilla Actor Framework.
