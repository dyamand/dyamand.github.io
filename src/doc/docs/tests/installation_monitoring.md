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
	installationChanged: Installation!
	# Subscription for removed installations
	installationRemoved: Installation!
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

!!! success "_c.S.0_ When the backend is started, there should be 0 detected installations"
!!! success "_c.S.1_ When a local installation is started, the amount of detected installations should be 1, that installation should be online"
!!! success "_c.S.2_ The online installation should contain at least 1 health check, 1 metric, 1 unique name and 1 machine generated friendly name"
!!! success "_c.S.3_ When the local installation sends an installation offline event, the amount of detected installations should still be 1 but the status should be offline and the health checks and metrics should be empty"
!!! success "_c.S.4_ As long as an installation is active and sending regular heartbeats (for example when running for 100 seconds), the status of the installation on the backend should still be online"
!!! success "_c.S.5_ When the local installations stops sending heartbeats and 2 heartbeats or more are missed (waiting at least 90 seconds), the amount of detected installations should still be 1 but the status should be lost communication"
!!! success "_c.S.6_ When the backend is restarted, the installation detected in the previous tests, should still be present and equal to the previous installation"
!!! success "_c.S.7_ When the backend is killed, it should revert to the latest snapshot it saved"

## Client

### Installation status

!!! success "_c.0.1_ At startup time, the HeartBeatSender should send a heartbeat"
!!! success "_c.0.2_ After n.5* heartbeat interval n+1 alive events should have been sent"
!!! success "_c.0.3_ When the installation is shutting down, a stopping event should be sent"
!!! success "_c.0.4_ An installation should only send a stopping event when it is actually stopping"
!!! success "_c.0.5_ When a new listener is registered, a heartbeat should be sent to that new listener"
!!! success "_c.0.6_ Every N heartbeats, there should be exactly 1 snapshot heartbeat (heartbeat that contains all information, no deltas)"

### Installation metrics

!!! success "_c.1.1_ Installation metrics should be sent alongside heart beats"
!!! warning "_c.1.2_ The installation information component should return the current metrics when asked"
!!! success "_c.1.3_ When sending updated metrics, all updated metrics should be included with their new value"
!!! success "_c.1.4_ When sending updated metrics, a metric that has no new value should not be included"
!!! success "_c.1.5_ When sending updated metrics, any new metrics should be included"

### Installation names

!!! success "_c.2.1_ The installation information component should generate as many names as there are non-local network interfaces"
!!! success "_c.2.2_ The installation information component should generate names that contain the MAC address of non-local network interfaces and it should start with dyamand"
!!! success "_c.2.3_ The installation information component should at least generate 1 friendly name"
!!! success "_c.2.4_ The generated friendly names should include the values for the environment values HOSTNAME (Linux) and COMPUTERNAME (Windows)"
!!! success "_c.2.5_ The generated friendly names should not contain the phrase "localhost" or "127.0.0.1""
!!! success "_c.2.6_ The generated friendly names should only contain a-z, A-Z, 0-9 and '-'"
!!! success "_c.2.7_ Each heartbeat should contain at least 1 friendly name"

### Installation health checks

!!! success "_c.7.0_ Any defined health check should be executed whenever the health of the installation is checked"
!!! success "_c.7.1_ A health check that is run successfully should have a result containing a corresponding status and clarifying message"
!!! success "_c.7.2.a_ A health check that throws an exception should be put in the failed state and the message should contain the corresponding error information"
!!! success "_c.7.2.b_ A health check that returns null should be put in the failed state and the message should be non-empty"
!!! success "_c.7.3_ A health check that stalls for longer than 2 seconds should be put in the failed state and the message should clarify this"
!!! success "_c.7.4_ When adding and removing N health checks, getting the installation health checks should return 0 results"
!!! success "_c.7.5_ When no heart beat has been sent yet, all health checks should be included in the next heart beat"
!!! success "_c.7.6_ If a health check has not changed since the last heart beat was sent, it should not be included in the next heart beat"
!!! success "_c.7.7_ Any health check that has changed (different state and/or different message) since the last heart beat, should be included in the next heart beat"
!!! success "_c.7.8_ If the blocking thread health check is executed without blocking threads, it should return a healthy result"
!!! success "_c.7.9_ If the blocking thread health check is executed while there are blocking threads, it should return an error"
!!! success "_c.7.10_ If the memory health check is executed while there are no memory usage thresholds, it should be healthy"
!!! success "_c.7.11_ If the memory health check is executed while all memory usage thresholds are set to 1, it should return an error"

## Backend

### Installation identification

!!! success "_c.3.1_ Requesting an installation by any subset of its known names should return that installation"
!!! success "_c.3.2_ For any installation with a certain collection of names, requesting an installation by any other name should not return that installation"
!!! success "_c.3.3_ Requesting an installation by a collection of names that only partially overlaps should throw an exception"
!!! success "_c.3.4_ Requesting an installation by a collection of names of which different names match different installations should throw an exception"
!!! success "_c.3.5_ When an event arrives which matches multiple installations, the corresponding installations should be merged into a single installation"
!!! success "_c.3.6_ When an event arrives which partially matches a single installation, the unmatched names should be added as names of that installation"
!!! success "_c.3.7_ When an event arrives with the friendly names property (INSTALLATION_ALIVE), the machine-generated friendly names of the installation should equal that of the event"
!!! success "_c.3.8_ When an event arrives without friendly names, the machine-generated friendly names of the installation should not change"
!!! success "_c.3.9_ When an event arrives with friendly names for an installation that already contains friendly names, the friendly names should be overwritten"
!!! success "_c.3.10_ When adding friendly names, all of them should be available through the retrieved installation object"
!!! success "_c.3.11_ When removing all friendly names, no friendly names should be available anymore"
!!! success "_c.3.12_ The user generated friendly names should still be available if a new event arrives"
!!! success "_c.3.13_ No matter how many events arrive for a particular installation, its ID can never change"
!!! success "_c.3.14_ No matter how many events arrive, the amount of different IDs should be exactly equal to the amount of detected installations"

### Status events

!!! success "_c.4.1_ If an event arrives at the installation monitor for an installation that is already known, the number of total installations should remain the same"
!!! success "_c.4.2_ If an event for an installation that is not yet in the system arrives at the installation monitor, the number of total installations should increase by 1"
!!! success "_c.4.3_ If an event that is not a stopping event arrives at the installation monitor and the corresponding installation is online, the number of online installations should remain the same and the corresponding installation's status should be online"
!!! success "_c.4.4_ If an event that is not a stopping event arrives at the installation monitor and the corresponding installation is not in the system, the number of online installations should increase by 1 and the corresponding installation's status should be online"
!!! success "_c.4.5_ If an event that is not a stopping event arrives at the installation monitor and the corresponding installation is offline, the number of online installations should increase by 1 and the corresponding installation's status should be online"
!!! success "_c.4.6_ If a stopping event arrives at the installation monitor and the corresponding installation is online, the number of online installations should decrease by 1 and the status of the corresponding installation should be offline"
!!! success "_c.4.7_ If a stopping event arrives at the installation monitor and the corresponding installation is offline, the number of online installations should remain the same and the status of the corresponding installation should be offline"
!!! success "_c.4.8_ If there is an event that is not a stopping event for an installation that is not online, the installation monitor should send an 'installation online' event"
!!! success "_c.4.9_ If there is an event for an installation that is not in the system, the installation monitor should send a 'new installation' event containing the installation information (currently only the unique names)"
!!! success "_c.4.10_ If there is a stopping event for an online installation, the installation monitor should send an 'installation offline' event containing the installation information (currently only the unique names)"
!!! success "_c.4.11_ If there is a stopping event for an installation that is not in the system, the installation monitor should send an 'installation offline' event containing the installation information (currently only the unique names)"
!!! success "_c.4.12_ If there is an event that is not a stopping event for an installation that is not in the system, the installation monitor should send an 'installation online' event containing the installation information (currently only the unique names)"
!!! success "_c.4.13_ If no new events are received for an installation that is currently online, the status should be changed to 'lost communication' and the installation monitor should send an 'installation lost communication' event containing the installation information (currently only the unique names)"
!!! success "_c.4.14_ If any number of installations are discovered and subsequently removed, there should be no remaining installations and there should have been an 'installation removed' event for each installation"
!!! success "_c.4.15_ If any number of installations are discovered and a listener for 'new installation' events is registered, it should received an event for all discovered installations"

### Installation info

!!! success "_c.5.1_ The lastContact (at the installation monitor) should be equal to the most recent moment an event for that installation got received"
!!! success "_c.5.2_ If an event arrives at the installation monitor, the lastContact field of the corresponding installation can't be earlier than the previous one"
!!! success "_c.5.3_ If an event arrives, the lastTimestamp of the corresponding installation should match the timestamp of that event"
!!! success "_c.5.4_ If the installation monitor receives an event which includes metrics or provides new information about known metrics, these metrics should be available in the installation monitor"
!!! success "_c.5.5_ If the installation monitor receives an event which includes metrics, older, unchanged metrics should still be available in the installation monitor"
!!! success "_c.5.6_ If the installation monitor receives an 'installation offline' event, the metrics of the corresponding installation should still be available"
!!! success "_c.5.7_ If the installation monitor receives an installation alive event with snapshot set to true, the health checks and metrics should be set to the information in the event"

### Installation merging

!!! success "_c.6.1_ A merged installation should be known by any and all names of the installations that were merged together"
!!! success "_c.6.2_ A merged installation's lastContact field should match the highest lastContact among merged installations or the event that triggered the merge"
!!! warning "_c.6.3_ A merged installation's metrics should match the value stored in the installation that has the most recent lastContact field from the group of installations containing that particular metric"

### Installation health checks

!!! success "_c.8.1_ If the installation monitor receives an event which includes health checks or provides new information about known health checks, these health checks should be available in the installation monitor"
!!! success "_c.8.2_ If the installation monitor receives an event which includes health checks, older, unchanged health checks should still be available in the installation monitor"
!!! success "_c.8.3_ If the installation monitor gets the same health check information multiple times, the duplicates should be ignored"
!!! success "_c.8.4_ If the installation monitor gets an 'installation offline' event, the health checks of the corresponding installation should still be available"

## Dashboard

### Overview page

!!! success "_c.9.1_ After getting all new installation events, the number of cards and table rows should be equal to the number of new installations + all installations with issues"
!!! success "_c.9.2_ All information of a specific installation match the card and row containing its ID"
!!! success "_c.9.3_ If n installations get an installation changed event, the amount of total installations should not change and all changes of that installation should be found on the specific installation card or in the specific installation row"
!!! success "_c.9.4_ If n installations get added and subsequently removed, no installations should be displayed"

### Details page

!!! success "_c.10.1_ Given a specific installation, the status and lastSeen should match"
!!! success "_c.10.2_ If there are n health checks, there are n health checks shown on the health check card"
!!! success "_c.10.3_ Given a specific health check id, all information related to the health check should match"
!!! success "_c.10.5_ Given a specific installation, all installation names should match"
!!! warning "_c.10.6_ Given a specific metric name, the value should match"
	!!! success "_a_ All information regarding memory resource should be displayed correctly"
	!!! success "_b_ All information regarding system CPU load should be displayed correctly"
	!!! success "_c_ All information regarding hardware uptime should be displayed correctly"
	!!! failure "_d_ All information regarding DYAMAND uptime should be displayed correctly"
!!! success "_c.10.7_ Given a specific installation, all supported technologies should match"
