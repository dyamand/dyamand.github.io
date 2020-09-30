## Tests for plugins

Plugins can use these generic tests to test functionality they provide. To be able to use these tests, simply let your test class implement _WithPluginTests_.

### Discovery

The test method _testDiscovery_ provides a way to test discovery implemented by a plugin. A _Consumer<Discovery\>_ that triggers the discovery of a single service has to be provided together with the type of service that should have been discovered and its ID.

### State changes

The test method _testStateChange_ provides a way to test whether or not a plugin successfully implements data retrieval. A _Consumer<Discovery\>_ that triggers discovery of a single service together with 1 changing state should be provided. Additionally, the type of state that should have been changed and optionally its value should be provided.

### Translators
