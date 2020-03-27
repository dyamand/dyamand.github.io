Eventing is used within DYAMAND to synchronize information across all components. Subscribers can subscribe to topics and filter the events they receive. Publishers can either just use the publish method of the EventChannel or be responsible for event at subscriber registration and deregistration as well. This is particularly useful for events that have been sent before the subscriber was registered. This means the EventChannel can remain stateless since all state can be found in the publishers. Subscribers in DYAMAND are called listeners to make the difference with interceptors clear.

## Matching

!!! success "_b.0.0_ An event with topic T matches T and all its elders"
!!! success "_b.0.1_ An event with topic T does not match any T' that is not T itself or any elder of T"

## Publishing events

!!! success "_b.1.0_ When publishing an event, all listeners which listen to any topic matching the event, should receive the event"
!!! success "_b.1.1_ When publishing an event, all listeners which listen to any topic NOT matching the event, should not receive the event"
!!! success "_b.1.2_ When publishing an event, all listeners with a predicate that evaluates to true, should receive the event"
!!! success "_b.1.3_ When publishing an event, all listeners with a predicate that evaluates to false, should not receive the event"
!!! success "_b.1.4_ When publishing an event, all listeners which listen to any topic matching the event and with a predicate that evaluates to true, should receive the event"
!!! success "_b.1.6_ When publishing an event, all listeners which listen to any topic NOT matching the event and with a predicate that evaluates to true, should not receive the event"
!!! success "_b.1.7_ When publishing an event, all listeners which listen to any topic NOT matching the event and with a predicate that evaluates to false, should not receive the event"
!!! success "_b.1.8_ When publishing an event, all listeners that have registered and unregistered to any topic with any predicate before the event was sent, should not receive the event"

## Historical events & events on removal

!!! success "_b.2.0_ A listener that is registered before a historical event publisher and a listener that is registered after, should receive the same events from that publisher"
!!! success "_b.2.1_ When a publisher is registered, it should be invoked regardless of the topics the listener is interested in"
!!! success "_b.2.2_ When a publisher is registered and unregistered, a registered listener should not receive any events"
!!! success "_b.2.3_ When a publisher fails to process a registered listener, that should not mean the listener is not registered"
!!! success "_b.2.4_ When a listener interceptor is registered for listener removal and it sends random events, the listener should receive them"

## Asynchronicity

!!! success "_b.3.0_ A publisher should not need to wait for a subscriber or its filter to complete"
!!! success "_b.3.1_ A subscriber should not need to wait for a previous subscriber or its filter to the same event"
!!! success "_b.3.2_ A publisher publishing historical events should not need to wait for a subscriber or its filter"
!!! success "_b.3.3_ When publishing an event, all listeners with a predicate that evaluates to false, should not receive the event"
!!! success "_b.3.4_ A subscriber should not need to wait until previous historical events are sent"
!!! success "_b.3.5_ A subscriber should not be invoked from different contexts"
!!! success "_b.3.6_ Multiple subscribers (registered on a non-Vert.x context) should all be invoked on a different Vert.x context"
!!! success "_b.3.7_ The filter should be invoked on the same context as the listener"
!!! success "_b.3.8_ Multiple listeners registered on the same Vert.x context should be invoked on the same context"

## Publication results

!!! success "_b.4.0_ When publishing an event to any number of listeners that register for all topics, the number of interested listeners should equal the total number of listeners"
!!! success "_b.4.1_ When publishing an event to any number of listeners that register for random topics, the number of interested listeners should equal the number of contacted listeners, the number of errors should be 0"
!!! success "_b.4.2_ When publishing an event to any number of listeners that register for all topics and throw an exception, the number of interested listeners should be equal to the number of listeners while the number of contacted listeners should be 0, the number of errors should be equal to the number of listeners"

## Event creation

!!! success "_b.5.0_ Building an event with a real non-validating topic, should generate an event with that topic"
!!! success "_b.5.1_ Building an event with a non-real non-validating topic, should result in an IllegalArgumentException"
!!! success "_b.5.2_ Building an event without specifying a timestamp should create an event with the current time"
!!! success "_b.5.3_ Building an event and specifying a timestamp, should create an event with that specific timestamp"
!!! success "_b.5.4_ Building an event without properties should result in Optional.empty() when getting any property"
!!! success "_b.5.5_ Building an event with any number of properties, should result in those properties being present and no other properties can be present"
!!! success "_b.5.6_ Building an event, the installation names should never be empty or null"
!!! success "_b.5.7_ Building an event with whatever input provided, should generate an event (non-null)"
!!! success "_b.5.8_ When creating an event, invoking publish should publish the created event"
!!! success "_b.5.9_ When creating an event, the created event should contain the names the installation information component provides"

## Listener disposal

!!! success "_b.6.0_ Creating and disposing N listeners, should not leave any non-disposed ExternalCodeExecution instances"
