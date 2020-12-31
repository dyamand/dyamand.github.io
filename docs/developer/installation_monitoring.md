## GraphQL schema

```
extend type Query {
	# Query for all installations.
	installations: [Installation!]!
	# Query installation based on its ID
	installation(id: String!): Installation
}

extend type Subscription {
	# Subscription for new installations
	newInstallation: Installation!
	# Subscription for changes in installations
	installationChanged(id: String): Installation!
	# Subscription for removed installations
	installationRemoved(id: String): Installation!
	# Subscription for a particular installation
	installation(id: String!): Installation!
}

extend type Mutation {
	# Add a friendly name to an installation
	identifyInstallationAs(
		# id of the installation
		id: String!,
		# friendly name to add
		friendlyName: String!): Installation!
	# Remove a friendly name from an installation, will be changed to ID when ID is added
	noLongerIdentifyInstallationAs(
		# id of the installation
		id: String!,
		# friendly name to remove
		friendlyName: String!): Installation!
	# Remove an installation, returns true if installations was actually removed, false if installation did not exist
	removeInstallation(
		# id of the installation
		id: String!
	): Boolean!
}

# An installation is any instance of DYAMAND that is running somewhere.
type Installation {
	# Unique identifier of the installation
	id: String!
	# Names of the installation.
	names: Names!
	# When was the installation last seen.
	lastSeen: Timestamp!
	# Current status of the installation.
	status: Status!
	# Health of the installation.
	health: Health!
	# Current metrics of the installations. A metric is any parameter that can be measured.
	metrics: [Metric!]!
	# Currently supported discovery protocols
	supportedDiscoveryProtocols: [DiscoveryProtocol!]!
}

# Metric that is being measured by the installation.
type Metric {
	# Name of the metric.
	name: String!
	# Stringified value of the metric.
	value: String!
}
```

## System tests

!!! success "c.S.0"
    When the backend is started, there should be 0 detected installations
!!! success "c.S.1"
    When a local installation is started, the amount of detected installations should be 1 and that installation should be online
!!! success "c.S.2"
    When the local installation sends an installation offline event, the amount of detected installations should still be 1 but the status should be offline
!!! success "c.S.3"
    As long as an installation is active and sending regular heartbeats (for example when running for 100 seconds), the status of the installation on the backend should still be online
!!! success "c.S.4"
    The online installation should contain at least 1 health check, 1 metric, 1 unique name and 1 machine generated friendly name
!!! success "c.S.5"
    When the local installations stops sending heartbeats and 2 heartbeats or more are missed (waiting at least 90 seconds), the amount of detected installations should still be 1 but the status should be lost communication, and the installation should still contain health checks and metrics
!!! success "c.S.6"
    When the local installation starts sending heartbeat again, the installation should reappear as online


TODO: Re-implement these tests in a separate test (toxiproxy does not appear to like it when the containers it is proxying for are restarted)
!!! failure "c.S.6"
    When the backend is restarted, the installation detected in the previous tests, should still be present and equal to the previous installation
!!! failure "c.S.7"
    When the backend is killed, it should revert to the latest snapshot it saved

## Client

### Installation status

!!! success "c.0.1"
    At startup time, the HeartBeatSender should send a heartbeat
!!! success "c.0.2"
    After n.5* heartbeat interval n+1 alive events should have been sent
!!! success "c.0.3"
    When the installation is shutting down, a stopping event should be sent
!!! success "c.0.4"
    An installation should only send a stopping event when it is actually stopping
!!! success "c.0.5"
    When a new listener is registered, a snapshot heartbeat should be sent to that new listener
!!! success "c.0.6"
    Every N heartbeats, there should be exactly 1 snapshot heartbeat (heartbeat that contains all information, no deltas)

### Installation metrics

!!! success "c.1.1"
    Installation metrics should be sent alongside heart beats
!!! warning "c.1.2"
    The installation information component should return the current metrics when asked
!!! success "c.1.3"
    When sending updated metrics, all updated metrics should be included with their new value
!!! success "c.1.4"
    When sending updated metrics, a metric that has no new value should not be included
!!! success "c.1.5"
    When sending updated metrics, any new metrics should be included

### Installation names

!!! success "c.2.1"
    The installation information component should generate as many names as there are non-local network interfaces
!!! success "c.2.2"
    The installation information component should generate names that contain the MAC address of non-local network interfaces and it should start with dyamand
!!! success "c.2.3"
    The installation information component should at least generate 1 friendly name
!!! success "c.2.4"
    The generated friendly names should include the values for the environment values HOSTNAME (Linux) and COMPUTERNAME (Windows)
!!! success "c.2.5"
    The generated friendly names should not contain the phrase "localhost" or "127.0.0.1"
!!! success "c.2.6"
    The generated friendly names should only contain a-z, A-Z, 0-9 and '-'
!!! success "c.2.7"
    Each heartbeat should contain at least 1 friendly name

### Installation health checks

!!! success "c.7.0"
    Any defined health check should be executed whenever the health of the installation is checked
!!! success "c.7.1"
    A health check that is run successfully should have a result containing a corresponding status and clarifying message
!!! success "c.7.2.a"
    A health check that throws an exception should be put in the failed state and the message should contain the corresponding error information
!!! success "c.7.2.b"
    A health check that returns null should be put in the failed state and the message should be non-empty
!!! success "c.7.3"
    A health check that stalls for longer than 2 seconds should be put in the failed state and the message should clarify this
!!! success "c.7.4"
    When adding and removing N health checks, getting the installation health checks should return 0 results
!!! success "c.7.5"
    When no heart beat has been sent yet, all health checks should be included in the next heart beat
!!! success "c.7.6"
    If a health check has not changed since the last heart beat was sent, it should not be included in the next heart beat
!!! success "c.7.7"
    Any health check that has changed (different state and/or different message) since the last heart beat, should be included in the next heart beat
!!! success "c.7.8"
    If the blocking thread health check is executed without blocking threads, it should return a healthy result
!!! success "c.7.9"
    If the blocking thread health check is executed while there are blocking threads, it should return an error
!!! success "c.7.10"
    If the memory health check is executed while there are no memory usage thresholds, it should be healthy
!!! success "c.7.11"
    If the memory health check is executed while all memory usage thresholds are set to 1, it should return an error

## Backend

### Installation identification

!!! success "c.3.1"
    Requesting an installation by any subset of its known names should return that installation
!!! success "c.3.2"
    For any installation with a certain collection of names, requesting an installation by any other name should not return that installation
!!! success "c.3.3"
    Requesting an installation by a collection of names that only partially overlaps should throw an exception
!!! success "c.3.4"
    Requesting an installation by a collection of names of which different names match different installations should throw an exception
!!! success "c.3.5"
    When an event arrives which matches multiple installations, the corresponding installations should be merged into a single installation
!!! success "c.3.6"
    When an event arrives which partially matches a single installation, the unmatched names should be added as names of that installation
!!! success "c.3.7"
    When an event arrives with the friendly names property (INSTALLATION"
   ALIVE), the machine-generated friendly names of the installation should equal that of the event
!!! success "c.3.8"
    When an event arrives without friendly names, the machine-generated friendly names of the installation should not change
!!! success "c.3.9"
    When an event arrives with friendly names for an installation that already contains friendly names, the friendly names should be overwritten
!!! success "c.3.10"
    When adding friendly names, all of them should be available through the retrieved installation object
!!! success "c.3.11"
    When removing all friendly names, no friendly names should be available anymore
!!! success "c.3.12"
    The user generated friendly names should still be available if a new event arrives
!!! success "c.3.13"
    No matter how many events arrive for a particular installation, its ID can never change
!!! success "c.3.14"
    No matter how many events arrive, the amount of different IDs should be exactly equal to the amount of detected installations

### Status events

!!! success "c.4.1"
    If an event arrives at the installation monitor for an installation that is already known, the number of total installations should remain the same
!!! success "c.4.2"
    If an event for an installation that is not yet in the system arrives at the installation monitor, the number of total installations should increase by 1
!!! success "c.4.3"
    If an event that is not a stopping event arrives at the installation monitor and the corresponding installation is online, the number of online installations should remain the same and the corresponding installation's status should be online
!!! success "c.4.4"
    If an event that is not a stopping event arrives at the installation monitor and the corresponding installation is not in the system, the number of online installations should increase by 1 and the corresponding installation's status should be online
!!! success "c.4.5"
    If an event that is not a stopping event arrives at the installation monitor and the corresponding installation is offline, the number of online installations should increase by 1 and the corresponding installation's status should be online
!!! success "c.4.6"
    If a stopping event arrives at the installation monitor and the corresponding installation is online, the number of online installations should decrease by 1 and the status of the corresponding installation should be offline
!!! success "c.4.7"
    If a stopping event arrives at the installation monitor and the corresponding installation is offline, the number of online installations should remain the same and the status of the corresponding installation should be offline
!!! success "c.4.8"
    If there is an event that is not a stopping event for an installation that is not online, the installation monitor should send an 'installation online' event
!!! success "c.4.9"
    If there is an event for an installation that is not in the system, the installation monitor should send a 'new installation' event containing the installation information (currently only the unique names)
!!! success "c.4.10"
    If there is a stopping event for an online installation, the installation monitor should send an 'installation offline' event containing the installation information (currently only the unique names)
!!! success "c.4.11"
    If there is a stopping event for an installation that is not in the system, the installation monitor should send an 'installation offline' event containing the installation information (currently only the unique names)
!!! success "c.4.12"
    If there is an event that is not a stopping event for an installation that is not in the system, the installation monitor should send an 'installation online' event containing the installation information (currently only the unique names)
!!! success "c.4.13"
    If no new events are received for an installation that is currently online, the status should be changed to 'lost communication' and the installation monitor should send an 'installation lost communication' event containing the installation information (currently only the unique names)
!!! success "c.4.14"
    If any number of installations are discovered and subsequently removed, there should be no remaining installations and there should have been an 'installation removed' event for each installation
!!! success "c.4.15"
    If any number of installations are discovered and a listener for 'new installation' events is registered, it should received an event for all discovered installations
!!! success "c.4.16"
    When an event arrives from a known installation, an 'installation updated' event should be sent

### Installation info

!!! success "c.5.1"
    The lastContact (at the installation monitor) should be equal to the most recent moment an event for that installation got received
!!! success "c.5.2"
    If an event arrives at the installation monitor, the lastContact field of the corresponding installation can't be earlier than the previous one
!!! success "c.5.3"
    If an event arrives, the lastTimestamp of the corresponding installation should match the timestamp of that event
!!! success "c.5.4"
    If the installation monitor receives an event which includes metrics or provides new information about known metrics, these metrics should be available in the installation monitor
!!! success "c.5.5"
    If the installation monitor receives an event which includes metrics, older, unchanged metrics should still be available in the installation monitor
!!! success "c.5.6"
    If the installation monitor receives an 'installation offline' event, the metrics of the corresponding installation should still be available
!!! success "c.5.7"
    If the installation monitor receives an installation alive event with snapshot set to true, the health checks and metrics should be set to the information in the event

### Installation merging

!!! success "c.6.1"
    A merged installation should be known by any and all names of the installations that were merged together
!!! success "c.6.2"
    A merged installation's lastContact field should match the highest lastContact among merged installations or the event that triggered the merge
!!! warning "c.6.3"
    A merged installation's metrics should match the value stored in the installation that has the most recent lastContact field from the group of installations containing that particular metric

### Installation health checks

!!! success "c.8.1"
    If the installation monitor receives an event which includes health checks or provides new information about known health checks, these health checks should be available in the installation monitor
!!! success "c.8.2"
    If the installation monitor receives an event which includes health checks, older, unchanged health checks should still be available in the installation monitor
!!! success "c.8.3"
    If the installation monitor gets the same health check information multiple times, the duplicates should be ignored
!!! success "c.8.4"
    If the installation monitor gets an 'installation offline' event, the health checks of the corresponding installation should still be available

## Dashboard

### Overview page

!!! success "c.9.1"
    Getting all installations updates from the service should display all returned n installations correctly
!!! success "c.9.2"
    If n installations get removed by the user and the removal was successful, no installations should be displayed anymore
!!! success "c.9.3"
    If n installations get removed by the user and the removal failed, n installations should still be displayed

### Details page

!!! success "c.10.1"
    Getting an update for an installation should display the installation correctly
!!! success "c.10.2"
    If n user friendly names got added successfully by the user, the added user friendly names should be displayed correctly
!!! success "c.10.3"
    If n user friendly names got added by the user but adding the names failed, the added user friendly names should not be displayed
!!! success "c.10.4"
    If n user friendly names got added by the user and subsequently get removed again without any issues, these names should not be displayed anymore
!!! success "c.10.5"
    If n user friendly names got added by the user and subsequently get removed again but a failure occurred for removing the names, all added names should be displayed
