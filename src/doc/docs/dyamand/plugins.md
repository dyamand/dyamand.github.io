# Extending DYAMAND using plugins

DYAMAND uses a plugin architecture to support different technologies, types of devices and interfaces. Four types of plugins can be identified, depending on what extra functionality they add to DYAMAND. 

- SDP plugins
- Translation plugins
- Application plugins

## Type plugins

DYAMAND only knows about the generic model described [here](../model). New concepts are defined by so-called type plugins. These plugins allow any application domain to be modeled without having to change anything to the core DYAMAND framework. More information on how to add new types can be found [here](../../plugins/prototypes).

## Service Discovery Protocol plugins

SDP plugins are responsible for the interaction with physical devices. They are responsible to discover, get state changes from and control services. Whenever an SDP plugin discovers a new service, it is automatically added to a top-level service according to whichever address the new service was found on. More information on how to write an SDP plugin can be found [here](../../plugins/discovery).

## Translation plugins

Translation plugins are plugins that translate services discovered by SDP plugins into services or states that use generic types (independent of any device technology).

## Application plugins

Application plugins are those plugins that use the functionality of any ```Service``` discovered by DYAMAND.
