# <center>Team 16</center>

## <center>Architecture, styles and tactics</center>

### <center>Conceptual design</center>

This system is a distributed system with a presentation layer that communicates with its subsystems via a broker and a gateway. The Gateway in question is used to act as both a type of validator and a filter for the data in the messages sent. Each subsystems are independent and have no coupling to one another which is the intention for the system to achieve easy scalability through modularization with the subsystem modules.

### Architectural styles

- The system will be event driven, using the publish/subscribe model. This means that in short, there will be event producers publishing messages to a topic to which one or mupltiple consumers are subscribed to. These consumers are independent of each other and are only subscribed the necessary topics. A broker is implemented to act as a middle man between the producers and consumers. The broker keeps track of which subsystem is subscribed to what topic. This style is well-suited for a distributed system where multiple subsystems has to process the same event. It also allows for real-time processing and better performance. Horizontal expansion of this system is also easily achived using this style.


### <center>Conceptual design mapping</center>

Our system consists of a frontend and backend subsystems, in which messages to and from the subsystems is sent over MQTT
using a publish/subscribe to transfer the data, and gets filtered through a gateway. 
Once the required filtering and processing has occurred, the subsystem that requested data to be process will
handle the data. 
