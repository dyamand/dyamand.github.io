## GraphQL schema
```
extend type Query {
	# Query all devices
	devices: [Device!]!
	# Query device based on its id
	device(id: String!): Device
}

extend type Subscription {
	# Subscription for new devices
	newDevice(installation: String): Device!
	# Subscription for device changes
	deviceChanged(installation: String): Device!
	# Subscription when device is removed
	deviceRemoved(installation: String): Device!
	# Subscription for a particular device
	device(id: String!): Device!
}

# Device
type Device {
	# Id of the device
	id: ID!
	# Names of the device.
	names: Names!
	# When was the device last seen
	lastSeen: Timestamp!
	# status of the device
	status: Status!
	# Supported discovery protocols
	supportedDiscoveryProtocols: [DiscoveryProtocol!]!
	# Addresses the device has been discovered at
	discoveredAt: [String!]!
	# All states detected by the specific device
	states: [State!]!
}

# An installation can contain devices.
extend type Installation {
	# All discovered devices for this installation.
	devices: [Device!]!
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
	# States of a device
	states: [State!]!
}

# State of a service.
type State {
	# ID of a state
	id: ID!
	# Type of a state
	type: String!
	# Default unit of a state
	defaultUnit: String
	# Supported units
	supportedUnits: [String!]
	# Allowed values of a state
	allowedValues: [String!]
	# Value of a state
	value(unit: String): String
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
!!! success "_h.0.4_ When N devices have been discovered by installation A and installation A goes offline, all devices discovered by installation A should be offline"

### Detecting the same device at different installations

!!! success "_h.1.0_ When the exact same device is detected by multiple installations, that device should be retrievable by using the id of any installation and it should be online"
!!! success "_h.1.1_ When the exact same device is detected by multiple installations and a device offline event is received from some but not all installations, the device should still be online"
!!! success "_h.1.2_ When the exact same device is detected by multiple installations and a device offline event is received from all installations, the device should be offline"
!!! success "_h.1.3_ When a device is detected multiple times by the same installation, 1 monitored device should be available containing the information the last event contains"
!!! failure "_h.1.4_ When multiple installations detect the same device (not necessarily containing the same services, but having at least 1 service in common), the monitored device should contain the combined information of the devices discovered at each of the installations (addresses, discovery protocols, services)"

### Keeping information about devices across power cycle

!!! success "_h.2.0_ When any service discovered by a discovery protocol with a global identification scheme is discovered, the monitored device should have a unique name equal to the service id as specified by the discovery protocol and when the device comes back online after going offline, the same monitored device should be used for this installation device"
!!! success "_h.2.1_ When any service discovered by a discovery protocol with a per-protocol identification scheme is discovered, the monitored device should have a unique name equal to the service id prefixed by the protocol id and when the device comes back online after going offline, the same monitored device should be used for this installation device"
!!! success "_h.2.2_ When any service discovered by a discovery protocol with a per-installation identification scheme is discovered, the monitored device should have a machine generated name equal to the id of the service and when the device comes back online after going offline (on the same installation), the same monitored device should be used"
!!! success "_h.2.3_ When any service discovered by a discovery protocol with a per-installation identification scheme goes offline on one installation and comes online on another installation, another monitored device should be used"
!!! success "_h.2.4_ When any service discovered by a discovery protocol with a per-session identification scheme is discovered, the monitored device should have a machine generated name equal to the service id and when the service goes offline and comes back online on any installation, another monitored device should be used"
!!! success "_h.2.5_ When an installation puts online any number of random services, only offline devices should be present when it goes offline"
!!! success "_h.2.6_ When an installation puts online only per-session services, no devices should be present when it goes offline and a 'device removed' event per device should have been sent"

!!! failure "TODO: service merging (information can be different across installations, or even within the same installation, that can change over time)"

## Backend events

!!! success "_h.4.0_ When a new device is detected by the device monitor (by receiving any device event), it should send an event indication there is a new device"
!!! success "_h.4.1_ When the status of a previously detected device changed, the device monitor should send an event indicating the device changed"
!!! failure "_h.4.2_ Whenever the information on a previously detected device changes, an event should be sent indicating this change"
	!!! failure "_a_ Service has been detected by another installation as well"
	!!! failure "_b_ Child service have been added"
!!! success "_h.4.3_ If any number of devices are discovered and a listener for 'new device' events is registered, it should received an event for all discovered devices"
!!! success "_h.4.4_ When any service discovered by a discovery protocol with a per-session identification scheme is discovered and it goes offline subsequently, a 'device removed' events should be sent"

## Dashboard

### Overview page

!!! success "_h.5.0_ Getting all devices updates from the service should display all returned n devices correctly"
	!!! success "_a_ All supported states should be displayed correctly - duplicate state types should only be displayed once"

### Details page

!!! success "_h.6.1_ Given a specific device, the status and lastSeen should match"
!!! failure "_h.6.2_ If there are n health checks, there are n health checks shown on the health check card"
!!! failure "_h.6.3_ Given a specific health check id, all information related to the health check should match"
!!! success "_h.6.4_ Given a specific device, all device names should match"
!!! failure "_h.6.5_ Given a specific device, all installations which discovers that specific device should match"
!!! failure "_h.6.6_ Given a specific device service, all states of that specific device should match"
!!! failure "_h.6.7_ Given a specific device service, all attributes of that specific device should match"
!!! failure "_h.6.8_ Given a specific device, all addresses where the device got discovered should match"
!!! success "_h.6.9_ Given a specific device, all discovery protocols should match"
!!! success "_h.6.10_ Given a specific device, all states should match"

### State prototype service

!!! success "_h.7.1_ Adding a state prototype to a service and getting its icon based on its type should return the corresponding icon link"
!!! success "_h.7.2_ Adding n state prototypes and afterwards removing them all will return will return the default state prototype icon link"
!!! success "_h.7.3_ Querying for an unknown state prototype should return a default icon link"
