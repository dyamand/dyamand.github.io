## Unified configuration of components

!!! success "_k.0.0_ If a component has published its configuration using the Metatype service and all configuration values are present in the bndrun, the component should be activated with that configuration"
!!! success "_k.0.1_ If a component has published its configuration using the Metatype service and  all configuration values are present in the configuration directory, the component should be activated with that configuration"
!!! success "_k.0.2_ If a component has published its configuration using the Metatype service and all configuration values are present split over the bndrun and the configuration directory, the component should be activated with that configuration"
!!! success "_k.0.3_ If a component has published its configuration using the Metatype service and no config is present, the component should not be activated, no configuration with that PID should be present and the configuration health check should give a warning"
!!! success "_k.0.4_ If a component has published its configuration using the Metatype service and not all required configuration values are present (some in bndrun, some in directory), the component should not be activated, no configuration with that PID should be present and the configuration health check should give an error"
!!! success "_k.0.5_ If a component has not published its configuration using the Metatype service and configuration for that PID is present in the configuration directory, the configuration with that PID should be present"
!!! success "_k.0.6_ If a component has published its configuration using the Metatype service and all required configuration values are present in the bndrun or the configuration directory, but the default configuration value is not present, the component should still be activated (using the default value for the parameter)"
!!! success "_k.0.7_ If the bundle containing the component is stopped, its configuration should also be removed"
!!! success "_k.0.8_ If a component has published its configuration using the Metatype service, all configuration values are present in bndrun and the component is started before configuration provider, the component should be activated with that configuration"
!!! success "_k.0.9_ If a component has published its configuration using the Metatype service, all configuration values are present, but the values are not convertible to the expected type, no configuration with the PID should be present and the component should not be activated and the configuration health check should be in error"
!!! success "_k.0.10_ If a component has published its configuration using the Metatype service, all configuration values are present in the bndrun, but some values are overwritten in the configuration directory, the values that are specified in the configuration directory should reach the component"

## Configuration directory (assuming the component's configuration has not been published using the Metatype service)

!!! success "_k.1.0_ If the environment variable for the configuration dir has been set and N files are put in that dir, the configuration should match the files"
!!! success "_DYAMAND-1700_ If the environment variable for the configuration dir has not been set and N files are put in the directory 'config', the configuration should match the files"
!!! success "_k.1.1_ If all files are removed, all configuration should have been removed as well"
!!! success "_k.1.2_ If a file is removed, that piece of configuration should also have been removed"
!!! success "_k.1.3_ If a file is added, the configuration should be adjusted"
!!! success "_k.1.4_ If the contents of a file is modified, that configuration object should reflect the change"
!!! success "_k.1.5_ If a multiline file is detected, each line should be considered a new entry in an array"

