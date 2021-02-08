[![Suspended](https://img.shields.io/badge/status-mergeWithWhatIsDYAMAND-red)](https://www.repostatus.org/#suspended)

Interoperability between different technologies requires a common model to be used. DYAMAND uses a generic domain model that is flexible enough to model every technology.

<div style="width: 100%;"><iframe allowfullscreen frameborder="0" style="width:100%; height:480px" src="https://www.lucidchart.com/documents/embeddedchart/f652ccea-df06-47df-b378-80fda3f07ef7" id="DDEYNB8aI8QT"></iframe></div>

The world according to DYAMAND is a collection of devices modeled by ```org.dyamand.model.Device```. These devices can contain any number of services (modeled by ```org.dyamand.model.service.Service```).

Each service in its turn consists of any number of states that represent the capabilities of the service. To model these states, the [objects defined by LwM2M](http://openmobilealliance.org/wp/OMNA/LwM2M/LwM2MRegistry.html) are used as a basis.

To see how this generic model can be extended for specific technologies or use cases, take a look at [the documentation on plugins](../plugins).
