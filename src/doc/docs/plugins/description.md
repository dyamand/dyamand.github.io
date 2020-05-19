## Service description

Once a service has been discovered, it is time to describe its capabilities. Capabilities in DYAMAND are modeled by a concept called ```State```. A state describes a concept that the service provides data for or can control. A simple example of a state is a temperature, a device containing a temperature sensor can be modeled by a service containing a ```State``` of type Temperature. 

Indicating which states a particular service contains can be done by specifying the different states using ```State prototype```, by claiming that the service follows a certain ```Service protoype``` or by using a combination of the first two possibilities.

### Service prototypes

Services can optionally be created off of a service prototype. This means the created service will contain all mandatory states of the service prototype. Additional states can still be added if needed. The ```protocol``` variable in the snippet below is a ```DiscoveryProtocolProxy``` as described in [Discoverying a new service](discovery.md).

```java
ServiceBuilder<ExampleService> builder = this.protocol.newService(id, ExampleService.class);
// add extra states optionally
if (*some condition*) {
	builder.withState(ExampleState.class);
}
builder.build((AsyncResult<Service> addService) -> {
    if (addService.succeeded()) {
        // handle successfully adding the service
        // save the ServiceProxy from addService.result() for future reference
    } else {
        // handle failure to add service
    }
});
```

### State prototypes

If services are not created based on a service prototype, states can be added based on state prototypes. From a client perspective, the only thing that changes is that the ```newService``` method is invoked without a service prototype.

```java
this.protocol.newService(id).withState(ExampleState.class).build((AsyncResult<Service> addService) -> {
    if (addService.succeeded()) {
        // handle successfully adding the service
        // save the ServiceProxy from addService.result() for future reference
    } else {
        // handle failure to add service
    }
});
```
