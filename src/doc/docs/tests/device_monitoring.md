## GraphQL schema
```
extend type Query {
	# Query all devices
	devices: [Device!]!
}

# Device
type Device {
	# id of the device
	id: ID!
	# supported discovery protocols
	supportedDiscoveryProtocol: [DiscoveryProtocol!]!
	# addresses the device has been discovered at
	discoveredAt: [String!]!
	# Child services
	services: [Service!]!
	# status of the device
	status: Status!
	# When was the device last seen
	lastSeen: Timestamp!
}

# Service which got discovered by an installation.
type Service {
	# Status of service.
	status: Status!
	# When was the service last seen
	lastSeen: Timestamp!
	# ID of the service.
	id: ID!
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

!!! failure "_h.S.0_ When the device monitor is started, there should be 0 detected devices"
!!! failure "_h.S.1_ When a local installation that put 1 service online is started, the amount of detected devices should be 1, that device should be online, it should contain 1 service and it should be retrievable by using the installation's id"
!!! failure "_h.S.2_ When the local installation sends a device offline event for the discovered device, the amount of detected devices should still be 1 but the status should be offline"
!!! failure "_h.S.3_ When the backend is restarted, the device detected in the previous tests, should still be present and equal to the previous device"
!!! failure "_h.S.4_ When the backend received events for another installation and N devices, those N devices should be available and retrievable by the new installation id and not with the old installation id and vice versa"

## Client events

### Basic cases

!!! success "_h.0.0_ When the device monitor is started, there should be 0 detected devices"
!!! success "_h.0.1_ When device online/updated events for N devices are received by the device monitor, all of them should be retrievable through the device monitor and their status should be online"
!!! success "_h.0.2_ When device offline events for N devices are received by the device monitor, all of them should be retrievable through the service monitor and their status should be offline"
!!! success "_h.0.3_ When N device events from installation A and M device events from installation B are received, all N devices should be retrievable by using installation A's id (and not by installation B's) and all M devices should be retrievable by using installation B's id (and not by installation A's)"

### Detecting the same device at different installations

!!! success "_h.1.0_ When the exact same device is detected by multiple installations, that device should be retrievable by using the id of any installation and it should be online"
!!! success "_h.1.1_ When the exact same device is detected by multiple installations and a device offline event is received from some but not all installations, the device should still be online"
!!! success "_h.1.2_ When the exact same device is detected by multiple installations and a device offline event is received from all installations, the device should be offline"
!!! failure "_h.1.3_ When multiple installations detect the same device (not necessarily containing the same services, but having at least 1 service in common), the monitored device should contain the combined information of the devices discovered at each of the installations (addresses, discovery protocols, services)"

TODO: service merging (information can be different across installations, or even within the same installation, that can change over time)

## Backend events

!!! failure "_h.4.0_ When a new device is detected by the device monitor (by receiving any device event), it should send an event indication there is a new device"
!!! failure "_h.4.1_ When the status of a previously detected device changed, the device monitor should send an event indicating the device changed"
!!! failure "_h.4.2_ Whenever the information on a previously detected device changes, an event should be sent indicating this change"
	!!! failure "_a_ Service has been detected by another installation as well"
	!!! failure "_b_ Child service have been added"

## Dashboard

### Overview page

!!! success "_h.5.1_ If the backend has N devices, all N devices should be displayed correctly"

### Details page

!!! failure "_h.6.1_ Given a specific service, the status and lastSeen should match"
!!! failure "_h.6.2_ If there are n health checks, there are n health checks shown on the health check card"
!!! failure "_h.6.3_ Given a specific health check id, all information related to the health check should match"
!!! failure "_h.6.4_ Given a specific service, all service names should match"
!!! failure "_h.6.5_ Given a specific service, all installations which discovers that specific service should match"
!!! failure "_h.6.6_ Given a specific service, all states of that specific service should match"
!!! failure "_h.6.7_ Given a specific service, all attributes of that specific service should match"
!!! failure "_h.6.8_ Given a specific service, all addresses where the service got discovered should match"
!!! failure "_h.6.9_ Given a specific service, the discovery protocol should match"
