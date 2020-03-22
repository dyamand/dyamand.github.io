## GraphQL schema
```
extend type Query {
	# Query all services
	services: [Service!]!
}

# Service which got discovered by an installation.
type Service {
	# Status of service.
	status: Status!
	# When was the service last seen
	lastSeen: Timestamp!
	# ID of the service.
	id: ID!
	# is this service a topLevel service
	topLevel: Boolean!
	# Discovery protocol that discovered this service
	discoveredBy: DiscoveryProtocol!
	# addresses this service is discovered at
	discoveredAt: [String!]!
	# Child services
	services: [Service!]!
}

# An installation is any instance of DYAMAND that is running somewhere.
extend type Installation {
	# All discovered services for this installation.
	services: [Service!]!
}

# Discovery protocol
type DiscoveryProtocol {
	# id of the protocol
	id: ID!
	# symbolic name of the plugin that registered the protocol
	pluginName: String!
	# version of the plugin that registered the protocol
	pluginVersion: String!
}
```

## System tests

!!! failure "_h.S.0_ When the service monitor is started, there should be 0 detected devices"
!!! failure "_h.S.1_ When a local installation that put 1 service online is started, the amount of detected services should be 1, that service should be online and it should be retrievable by using the installation's id"
!!! failure "_h.S.2_ When the local installation sends a service offline event for the discovered service, the amount of detected services should still be 1 but the status should be offline"
!!! failure "_h.S.3_ When the backend is restarted, the service detected in the previous tests, should still be present and equal to the previous service"
!!! failure "_h.S.4_ When the backend received events for another installation and N services, those N services should be available and retrievable by the new installation id and not with the old installation id and vice versa"

## Client events

!!! success "_h.0.0_ When the service monitor is started, there should be 0 detected services"
!!! success "_h.0.1_ When service online/updated events for N services are received by the service monitor, all of them should be retrievable through the service monitor and their status should be online"
!!! failure "_h.0.2_ When service offline events for N services are received by the service monitor, all of them should be retrievable through the service monitor and their status should be offline"
!!! failure "_h.0.3_ When service offline events are received for services that were previously known, the status for all the services it attains to should be offline"
!!! failure "_h.0.4_ When N service events from installation A and M service events from installation B are received, all N services should be retrievable by using installation A's id and all M services should be retrievable by using installation B's id"
!!! failure "_h.0.5_ When the same service is detected by multiple installations, that service should be retrievable by using the id of any installation and it should be online"
!!! failure "_h.0.6_ When the same service is detected by multiple installations and a service offline event is received from some but not all installations, the service should still be online"
!!! failure "_h.0.6_ When the same service is detected by multiple installations and a service offline event is received from all installations, the service should be offline"

TODO: service merging (information can be different across installations, or even within the same installation, that can change over time)

## Backend events

!!! failure "_h.1.0_ When a new service is detected by the service monitor (by receiving any service event), it should send an event indication there is a new service"
!!! failure "_h.1.1_ When the status of a previously detected service changed (by receiving any service event that changes the status), the service monitor should send an event indicating the status of the service changed"
!!! failure "_h.1.2_ Whenever the information on a previously detected service changes, an event should be sent indicating this change"
	!!! failure "_a_ Service has been detected by another installation as well"
	!!! failure "_b_ Child service have been added"

## Dashboard

### Overview page

!!! success "_h.2.1_ If the backend has n non-toplevel services, all n services should be displayed correctly"

### Details page

!!! failure "_h.3.1_ Given a specific service, the status and lastSeen should match"
!!! failure "_h.3.2_ If there are n health checks, there are n health checks shown on the health check card"
!!! failure "_h.3.3_ Given a specific health check id, all information related to the health check should match"
!!! failure "_h.3.4_ Given a specific service, all service names should match"
!!! failure "_h.3.5_ Given a specific service, all installations which discovers that specific service should match"
!!! failure "_h.3.6_ Given a specific service, all states of that specific service should match"
!!! failure "_h.3.7_ Given a specific service, all attributes of that specific service should match"
!!! failure "_h.3.8_ Given a specific service, all addresses where the service got discovered should match"
!!! failure "_h.3.9_ Given a specific service, the discovery protocol should match"
