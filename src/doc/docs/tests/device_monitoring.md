## GraphQL schema
```
extend type Query {
	# Query all devices
	devices: [Device!]!
	# Query device based on its id
	device(id: String!): Device
}

# Device
type Device {
	# id of the device
	id: ID!
	# Names of the device.
	names: Names!
	# When was the device last seen
	lastSeen: Timestamp!
	# status of the device
	status: Status!
	# supported discovery protocols
	supportedDiscoveryProtocols: [DiscoveryProtocol!]!
	# addresses the device has been discovered at
	discoveredAt: [String!]!
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
}

# Discovery protocol
type DiscoveryProtocol {
	# id of the protocol
	id: ID!
	# symbolic name of the plugin that registered the protocol
	pluginName: String!
	# version of the plugin that registered the protocol
	pluginVersion: Version!
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
!!! success "_h.1.3_ When a device is detected multiple times by the same installation, 1 monitored device should be available containing the information the last event contains"
!!! failure "_h.1.4_ When multiple installations detect the same device (not necessarily containing the same services, but having at least 1 service in common), the monitored device should contain the combined information of the devices discovered at each of the installations (addresses, discovery protocols, services)"

### Keeping information about devices across power cycle

!!! success "_h.2.0_ When any service discovered by a discovery protocol with a global identification scheme is discovered, the monitored device should have a unique name equal to the service id as specified by the discovery protocol and when the device comes back online after going offline, the same monitored device should be used for this installation device"
!!! success "_h.2.1_ When any service discovered by a discovery protocol with a per-protocol identification scheme is discovered, the monitored device should have a unique name equal to the service id prefixed by the protocol id and when the device comes back online after going offline, the same monitored device should be used for this installation device"
!!! failure "_h.2.2_ When any service discovered by a discovery protocol with a per-installation identification scheme is discovered, the monitored device should have a machine generated name equal to the id of the service and when the device comes back online after going offline (on the same installation), the same monitored device should be used"
!!! failure "_h.2.3_ When any service discovered by a discovery protocol with a per-installation identification scheme goes offline on one installation and comes online on another installation, another monitored device should be used"
!!! success "_h.2.4_ When any service discovered by a discovery protocol with a per-session identification scheme is discovered, the monitored device should have a machine generated name equal to the service id and when the service goes offline and comes back online on any installation, another monitored device should be used"

!!! failure "TODO: service merging (information can be different across installations, or even within the same installation, that can change over time)"

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

!!! success "_h.6.1_ Given a specific device, the status and lastSeen should match"
!!! failure "_h.6.2_ If there are n health checks, there are n health checks shown on the health check card"
!!! failure "_h.6.3_ Given a specific health check id, all information related to the health check should match"
!!! success "_h.6.4_ Given a specific device, all device names should match"
!!! failure "_h.6.5_ Given a specific device, all installations which discovers that specific device should match"
!!! failure "_h.6.6_ Given a specific device service, all states of that specific device should match"
!!! failure "_h.6.7_ Given a specific device service, all attributes of that specific device should match"
!!! failure "_h.6.8_ Given a specific device, all addresses where the device got discovered should match"
!!! success "_h.6.9_ Given a specific device, all discovery protocols should match"
