## Unified configuration of components

!!! success "k.0.0"
    If a component has published its configuration using the Metatype service and all configuration values are present in the bndrun, the component should be activated with that configuration
!!! success "k.0.1"
    If a component has published its configuration using the Metatype service and  all configuration values are present in the configuration directory, the component should be activated with that configuration
!!! success "k.0.2"
    If a component has published its configuration using the Metatype service and all configuration values are present split over the bndrun and the configuration directory, the component should be activated with that configuration
!!! success "k.0.3"
    If a component has published its configuration using the Metatype service and no config is present, the component should not be activated, no configuration with that PID should be present and the configuration health check should give a warning
!!! success "k.0.4"
    If a component has published its configuration using the Metatype service and not all required configuration values are present (some in bndrun, some in directory), the component should not be activated, no configuration with that PID should be present and the configuration health check should give an error
!!! success "k.0.5"
    If a component has not published its configuration using the Metatype service and configuration for that PID is present in the configuration directory, the configuration with that PID should be present
!!! success "k.0.6"
    If a component has published its configuration using the Metatype service and all required configuration values are present in the bndrun or the configuration directory, but the default configuration value is not present, the component should still be activated (using the default value for the parameter)
!!! success "k.0.7"
    If the bundle containing the component is stopped, its configuration should also be removed
!!! success "k.0.8"
    If a component has published its configuration using the Metatype service, all configuration values are present in bndrun and the component is started before configuration provider, the component should be activated with that configuration
!!! success "k.0.9
    If a component has published its configuration using the Metatype service, all configuration values are present, but the values are not convertible to the expected type, no configuration with the PID should be present and the component should not be activated and the configuration health check should be in error
!!! success "k.0.10"
    If a component has published its configuration using the Metatype service, all configuration values are present in the bndrun, but some values are overwritten in the configuration directory, the values that are specified in the configuration directory should reach the component
!!! success "k.0.11"
    If a component has published its configuration (only using default values) using the Metatype service and no configuration values are present in the bndrun or in the configuration directory, the component needs to be enabled

## Configuration directory (assuming the component's configuration has not been published using the Metatype service)

!!! success "k.1.0"
    If the environment variable for the configuration dir has been set and N files are put in that dir, the configuration should match the files
!!! success "DYAMAND-1700"
    If the environment variable for the configuration dir has not been set and N files are put in the directory 'config', the configuration should match the files
!!! success "k.1.1"
    If all files are removed, all configuration should have been removed as well
!!! success "k.1.2"
    If a file is removed, that piece of configuration should also have been removed
!!! success "k.1.3"
    If a file is added, the configuration should be adjusted
!!! success "k.1.4"
    If the contents of a file is modified, that configuration object should reflect the change
!!! success "k.1.4.E.1"
    If the contents of a file is not modified, but the file is touched, the configuration should not be updated
!!! success "k.1.5"
    If a multiline file is detected, each line should be considered a new entry in an array
!!! success "k.1.5.E.1"
    If a multiline is detected and the file is touched, the configuration should not be updated
!!! success "k.1.6"
    If configuration is put in a hidden file, the configuration should not be applied
!!! success "k.1.7"
    If configuration is put in a non-hidden file in a hidden directory, the configuration should not be applied
!!! success "k.1.8"
    If configuration is put in a hidden file that is linked to by a non-hidden symbolic link, the configuration should be applied even after changes in the hidden file

## Configuration for loggers

Logger configuration is quite complex ([Configuration Admin Integration](https://docs.osgi.org/specification/osgi.cmpn/7.0.0/service.log.html#d0e2548)). Therefore, a separate mechanism specifically for logger configuration is devised. Different instances of ```LoggerContext``` can be configured, within a ```LoggerContext``` the configuration contains a map from logger names to a ```LogLevel```. There is a special context, called the root ```LoggerContext```, the default one for the system.

Configuration can be provided using the PID **org.dyamand.logging** and the parameter name **config**. The value should be a String using the following pattern in ABNF.
```
config              = *(configLine CRLF) configLine
configLine          = configLeft equals logLevel
configLeft          = *1(contextName configSeparator) loggerName
configSeparator     = "#"
contextName         = packageName
loggerName          = packageName
packageName         = *(packagePart packageSeparator) packagePart ; Java package names
packagePart         = (ALPHA | "_") *(ALPHA | DIGIT | "_") ; package parts cannot start with a digit
packageSeparator    = "."
equals              = "="
logLevel            = AUDIT / ERROR / WARN / INFO / DEBUG / TRACE ; possible log levels
```

Examples:
**name.of.logger=ERROR** sets the log level of logger _name.of.logger_ to ERROR in the root context.
**name.of.context#name.of.logger=TRACE** sets the log level of logger _name.of.logger_ to TRACE in the logger context with name _name.of.context_.

!!! success "k.2.0"
    Providing the logger configuration in the bndrun (root and non-root context) should be applied to the ConfigurationAdmin
!!! success "k.2.1"
    Providing the logger configuration in the configuration directory (root and non-root context) should be applied to the ConfigurationAdmin
!!! success "k.2.2"
    When logger configuration is provided both in the bndrun and in the configuration directory but there are no conflicts, the applied configuration should be merged
!!! success "k.2.3"
    When logger configuration is provided both in the bndrun and in the configuration directory and there are conflicts, the configuration in the configuration directory should override the configuration in the bndrun

