# Welcome to DYAMAND

DYAMAND is a software component that enables developers to integrate connected devices into their application. DYAMAND uses a plugin architecture to support different technologies, types of devices and interfaces.

## Concepts

Interoperability between different technologies requires a common model to be used. DYAMAND uses a generic domain model that is flexible enough to model every technology.

<div style="width: 100%;"><iframe allowfullscreen frameborder="0" style="width:100%; height:480px" src="https://www.lucidchart.com/documents/embeddedchart/f652ccea-df06-47df-b378-80fda3f07ef7" id="DDEYNB8aI8QT"></iframe></div>

The world according to DYAMAND is a collection of devices modeled by ```org.dyamand.model.Device```. These devices can contain any number of services (modeled by ```org.dyamand.model.Service```).

DYAMAND identifies four types of plugins depending on what they implement. 

- SDP plugins
- Type plugins
- Translation plugins
- Application plugins

SDP plugins are responsible for the interaction with physical devices. They are responsible to discover, get state changes and control services. Whenever an SDP plugin discovers a new service, it is automatically added to a top-level service according to whichever address the new service was found on. 

Type plugins extend the type system of DYAMAND to model any application domain imaginable.

Translation plugins are plugins that translate services discovered by SDP plugins into services that use generic types (independent of any device technology).

Application plugins are those plugins that use the functionality of any ```Service``` discovered by DYAMAND.

## Writing an application

Writing an application on top of DYAMAND consists of different steps. First of all, as an application, you are typically interested in the [lifecycle of services](./applications/services/).

## Extending DYAMAND

DYAMAND can be extended in various ways. First and foremost, Service Discovery Protocol (SDP) plugins extend DYAMAND by adding support for a particular protocol. This is necessary since DYAMAND in and of itself is not able to communicate with any device. The very first thing an SDP plugin should do is [discover new services](./plugins/discovery/).



