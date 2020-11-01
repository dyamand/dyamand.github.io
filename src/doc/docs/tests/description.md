# Prototypes

DYAMAND uses prototypes to start describing services. There are state and service prototypes, a state prototype models a particular state that can be measured and/or controlled while service prototypes group different states for convenience.

## State prototypes

!!! success "_2.0.0_ After registering any state protoype, it should be available and an event should have been sent about that new state prototype"
!!! success "_2.0.1_ After registering and unregistering any state prototype, the state prototype should no longer be available and an event should have been sent about the removal of the state prototype"
!!! success "_2.0.2_ Registering a state prototype twice should result in an IllegalStateException (in the handler) and no events"
!!! success "_2.0.3_ Disposing a state prototype twice should result in an IllegalStateException (in the handler) and no events"

## Service prototypes

!!! success "_2.1.0_ After registering any service prototype, it should be available and an event should have been sent about that new service prototype"
!!! success "_2.1.1_ After registering and unregistering any service prototype, the service prototype should no longer be available and an event should have been sent about the removal of the service prototype"
!!! success "_2.1.2_ Registering a service prototype twice should result in an IllegalStateException (in the handler) and no events"
!!! success "_2.1.3_ Disposing a service prototype twice should result in an IllegalStateException (in the handler) and no events"

## Cleanup

!!! success "_2.2.0_ When a plugin registers multiple state prototypes and the plugin is stopped, no state prototypes can be available"
!!! success "_2.2.1_ When a plugin registers multiple service prototypes and the plugin is stopped, no service prototypes can be available"

# Service description

Services can be described at discovery time by using state and service prototypes as a blueprint for the newly discovered service.

## Describing a service using state prototypes

!!! success "_2.3.0_ When a service is described by using unnamed state prototypes, the resulting service should contain states for all state prototypes used and all states should contain unique names within the created service (the fully qualified type) and the states should be of the same type of the used prototypes"
!!! success "_2.3.1_ When a service is described by using named state prototypes, the resulting service should contain states for all state prototypes with the specified names"

## Describing a service using service prototypes

!!! success "_2.4.0_ When a service is described by a service prototype, the created service should be of the same type as the prototype and it should contain all non-optional states"
!!! success "_2.4.1_ When a service is described by a service prototype and is extended by using named state prototypes, both the mandatory states and the added states should be present"

# Serialization with prototypes

Serialization (and in particular deserialization) becomes hard once particular classes come into play. Serializing state prototypes and service prototypes and states or services based on those prototypes need additional testing to make sure deserialization is done properly. Deserialization depends on the context in which it is performed, if deserialization is done in a context where the prototype is not available (backend), deserialization still should create an object that is equal in content to the object that was serialized. However, if deserialization is done in a context where the prototype is available, the deserialized object should be at least of the same class as the object that was serialized. For prototypes, the deserialized object should even be the exact same object.

!!! success "_2.5.0_ Deserializing a known state prototype, should result in exactly the same object"
!!! success "_2.5.1_ Deserializing a state based on a known state prototype, should result in an object that is content equal AND of the same type as the state prototype"
!!! success "_2.5.2_ Deserializing a known service prototype, should result in exactly the same object"
!!! success "_2.5.3_ Deserializing a service based on a known service prototype, should result in an object that is content equal AND of the same type as the service prototype"

When the state prototype is unknown or the value class of the state cannot be serialized, the states should not be included in services and state changes while serializing.

!!! success "_2.5.4_ After deserializing a service, it should only contain serializable states"
!!! failure "_2.5.5_ After deserializing a state changes, it should only contain serializable states"

