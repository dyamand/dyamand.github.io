## Gateway

### Event Forwarder

!!! success "_d.0.1_ The Event Forwarder forwards any valid event it receives to the backend"
!!! success "_d.0.2_ When N events are published "at the same time", they should all arrive on the backend"


## Backend

### Event Receiver

TODO adjust taking into account new architecture

!!! success "_d.1.1_ Any valid event received from the Gateway should be processed by the Event Receiver and published on the Event Channel"

### Remote event channel

!!! success "_d.2.0_ When subscribing to a topic N times, publishing an event on that topic should notify all N subscribers once"
!!! success "_d.2.1_ When subscribing to a topic, publishing N different events should result in the subscriber getting all of them in order"
!!! success "_d.2.2_ When subscribing to N topics, publishing an event on another topic should not notify any subscriber"
!!! success "_d.2.3_ When subscribing to a topic including a wildcard (*), publishing on N different topics that matches with the subscribed topic, the subscriber should be notified N times"
!!! success "_d.2.4_ When subscribing and unsubscribing to a topic, publishing an event on that topic should not notify the subscriber"

### Key value store

!!! success "_d.3.0_ When adding the same value N times to a random key, that key should contain 1 element and it should equal the value that has been added, additionally, only the first time adding the value can return true, the other times it should have returned false"
!!! success "_d.3.1_ When adding N different values, adding the values should always return true and the key should contain N values, equal to all added values"
!!! success "_d.3.2_ When adding and removing N different values, the set should be empty"
