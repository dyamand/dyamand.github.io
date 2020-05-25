# Prototypes

DYAMAND uses prototypes to start describing services. There are state and service prototypes, a state prototype models a particular state that can be measured and/or controlled while service prototypes group different states for convenience.

## State prototypes

!!! failure "_2.0.0_ After registering any state protoype, it should be available and an event should have been sent about that new state prototype"
!!! failure "_2.0.1_ After registering and unregistering any state prototype, the state prototype should no longer be available and an event should have been sent about the removal of the state prototype"
!!! failure "_2.0.2_ Registering a state prototype twice should result in an IllegalStateException (in the handler)"

## Service prototypes

!!! failure "_2.1.0_ After registering any service prototype, it should be available and an event should have been sent about that new service prototype"
!!! failure "_2.1.1_ After registering and unregistering any service prototype, the service prototype should no longer be available and an event should have been sent about the removal of the service prototype"
!!! failure "_2.1.2_ Registering a service prototype twice should result in an IllegalStateException (in the handler)"

## Cleanup

!!! failure "_2.2.0_ When a plugin registers multiple state prototypes and the plugin is stopped, no state prototypes can be available"
!!! failure "_2.2.1_ When a plugin registers multiple service prototypes and the plugin is stopped, no service prototypes can be available"

# Service description

Services can be described at discovery time by using state and service prototypes as a blueprint for the newly discovered service.

## Describing a service using state prototypes

!!! failure "_2.3.0_ When a service is described by using state prototypes, the resulting service should contain states for all state prototypes used and all states should contain unique names within the created service"
!!! failure "_2.3.1_ When a service is described by using named state prototypes, the resulting service should contain states for all state prototypes with the specified names"
!!! failure "_2.3.2_ When a service is described by using named state prototypes and there are overlapping names for the states, an IllegalStateException should be thrown"

## Describing a service using service prototypes

!!! failure "_2.4.0_ When a service is described by a service prototype, the service should contain all mandatory states"
!!! failure "_2.4.1_ When a service is described by a service prototype and is extended by using state prototypes, both the mandatory states and the added states should be present"

