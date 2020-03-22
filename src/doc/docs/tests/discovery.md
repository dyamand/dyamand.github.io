## Protocol registration

!!! success "_1.0.0_ After registering multiple discovery protocols, all discovery protocols should be retrievable"
!!! success "_1.0.1_ After registering multiple discovery protocols, an event should have been sent for each new discovery protocol"
!!! success "_1.0.2_ After registering and unregistering multiple discovery protocols, no discovery protocol should be retrievable"
!!! success "_1.0.3_ After registering and unregistering multiple discovery protocols, an event should have been sent for each discovery not being supported anymore"
!!! success "_1.0.4_ After registering the same protocol multiple times within the same plugin, there should only be one protocol"
!!! success "_1.0.5_ After registering the same protocol multiple times within the same plugin, only 1 event should have been sent"
!!! success "_1.0.6_ After registering the same protocol N times from different plugins, there should be N protocols"
!!! success "_1.0.7_ After registering the same protocol N times from different plugins, N events should have been sent"

## Service coming online

!!! success "_1.1.0_ If no services have been discovered, no device should be retrievable"
!!! success "_1.1.1_ After multiple services with unique identifiers have been discovered, they should all be retrievable through the discovery protocol & the device repository"
!!! success "_1.1.2_ After multiple services with unique identifiers have been discovered without an address, there should be an event for all of them"
!!! success "_1.1.3_ When a service with the same ID gets discovered multiple times, the service should only be present once and only 1 event should have been sent"
!!! success "_1.1.4_ When a service with the same ID is discovered by multiple discovery protocols, 1 service should be available through every protocol and an event should have been sent for all of the discovered services"
!!! success "_1.1.5_ If N services have been discovered at potentially different addresses (but with at least one address in common), there should only be N services and 1 device containing all addresses. Additionally, N online events should have been sent among for the services and 1 online event should have been sent for the device, additionally N-1 device updated events should have been sent about the only device (because services were added to it)"
!!! success "_1.1.6_ If N services have been discovered, all at different addresses but with unique identifiers, there should be N devices each containing 1 service, N service online events and N device online events should have been sent"

TODO Adding service with ID that already exists but different content --> merge?

TODO Merging services previously part of different service

## Service going offline

!!! success "_1.2.0_ After multiple services with unique identifiers have been discovered without an address and put offline, an event for each of the services going offline should be sent and an event for each of the devices that have been created should also have been sent"
!!! success "_1.2.1_ If the same service is put offline different times, only 1 offline event for the service and device going offline should have been sent"
!!! success "_1.2.2_ If N services have been discovered, all at potentially different addresses (but with at least one address in common), and put offline afterwards, there should be N events about services going offline and 1 for the device and 2*(N-1) about the device being updated"
!!! success "_1.2.3_ If N services have been discovered, all at different addresses but with unique identifiers, and put offline afterwards, there should be an event for every discovered service and an event for every device"

## Cleanup

!!! success "_1.3.0_ After registering multiple services each discovered by potentially different discovery protocols and unregistering all discovery protocols, there should not be any remaining devices"

# Integration tests

!!! success "_1.I.0_ When a service is put online, it should be available for retrieval by the device and an event should have been sent"
!!! success "_1.I.1_ When a service is put offline, it should no longer be available for retrieval and an event should have been sent"
!!! success "_1.I.2_ When a plugin that previously registered a discovery protocol with which a service has been discovered, is stopped, there should be no discovery protocol or service left"
