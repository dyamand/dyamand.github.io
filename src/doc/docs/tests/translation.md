## Translating state changes into state changes

!!! success "_4.0.0_ If a translator is registered for a particular state and a state change for that state is created, the translator should be invoked"
!!! success "_4.0.1_ If a translator is registered for a state and state change for another state is created, the translator should NOT be invoked"
!!! success "_4.0.2_ If a state change is created and afterwards a translator is registered for that state, the translator should be invoked"
!!! success "_4.0.3_ If a state change is created and afterwards a translator is registered for another state, the translator should NOT be invoked"
!!! failure "_4.0.4_ If a translator translates a state change into a new state change, that state should be added to the service and the value change should be present in the existing state changed event if the translator was registered before the state change happened"
!!! failure "_4.0.5_ If a translator translates a state change into a new state change, that state should be added to the service and the value change should be present in a new state changed event if the translator was registered after the state change happened"

TODO what if translator gets unregistered

## Translating state changes into new services

!!! failure "_4.1.0_ If a translator translates a state change into a new service, that service should be part of the same device as the original service, the original service should be set into the translated service and the translated service should be added to the original service, also a service online event should have been sent"
!!! failure "_4.1.1_ If a translator translates a state change into a new service and the original service goes offline, the translated service should also go offline (not being retrievable and service offline event)"

TODO what if translator gets unregistered
