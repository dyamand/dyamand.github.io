## Tests for plugins

Plugins can use these generic tests to test functionality they provide. To be able to use these tests, simply let your test class implement ```WithPluginTests```.

### Discovery

The test method _testDiscovery_ provides a way to test discovery implemented by a plugin. A ```Consumer<Discovery>``` that triggers the discovery of a single service has to be provided together with the type of service that should have been discovered and its ID.

### State changes

The test method _testStateChange_ provides a way to test whether or not a plugin successfully implements data retrieval. A ```Consumer<Discovery>``` that triggers discovery of a single service together with 1 changing state should be provided. Additionally, the type of state that should have been changed and optionally its value should be provided.

### Translators

## Tests for state prototypes

State prototypes have an important role in abstracting the functionality of devices towards applications, this section specifies which tests are relevant for state prototypes and how to take advantage of them.

All state prototypes can take advantage from the tests below. This can be done by extending ```AbstractStatePrototypeTest```.

!!! failure "_0.0.0_ When creating a state with a certain id and value from a state prototype, the created state should be of the same type as the state prototype, have the provided id and its value should be equal to the provided value"

Additionally, measurement states can taken advantage of the tests below. This can be done by extending ```AbstractMeasurementStatePrototypeTest```.

!!! failure "_0.1.0_ When a measurement state is created with a certain value and unit, creating a new state based on any other unit should result in the same value in the original unit"
