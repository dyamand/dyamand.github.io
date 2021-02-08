[![Suspended](https://img.shields.io/badge/status-mergeWithForTechnologyDevelopers-red)](https://www.repostatus.org/#suspended)

## Service discovery

In the beginning, there was nothing. Discovery is the process of detecting new services in the environment and notifying applications of this fact.

To be able to support this, there are two situations to take into account. First of all, when applications start, they want to be notified of the current situation. Second, if the application has already been started, it wants to be notified of any changes in the environment. This can be services coming online or leaving the environment.

Protocols might or might not support searching the environment for services they support. Protocols also might or might not explicitly support discovering new services in the environment.

### Abstraction

DYAMAND offers an abstraction towards applications that enables unified discovery of services regardless of the mechanism used by a particular protocol. To be able to do this, DYAMAND offers a way for plugins to indicate to DYAMAND when a new service has been detected. Similarly, it offers a way to indicate that a service is no longer active. This information allows DYAMAND to notify interested applications.

### Protocol registration

The very first thing to do as a plugin that can discover services, is registering the protocol the plugin is responsible for. This can be done using the ```org.dyamand.discovery.Discovery``` service.

```java
@Component(immediate = true)
public final class NewProtocolPlugin {
	private final Discovery discovery;

	@Activate
	public NewProtocolPlugin(@Reference Discovery discovery) {
		this.discovery = Objects.requireNonNull(discovery);
	}
}
```

Once you have acquired a reference to the ```Discovery``` service, you can build a ```org.dyamand.discovery.DiscoveryProtocolProxy``` like below. Be sure to save the ```DiscoveryProtocolProxy``` instance as it will be used to indicate to DYAMAND that you discovered a new service. The second argument of the _withProtocol_ method is an ```IdentificationScheme```. It is important to provide the correct identification scheme for a protocol since the generation of IDs is based on the identification scheme provided when building the ```DiscoveryProtocolProxy```. There are four different identification schemes:

- ```IdentificationScheme.GLOBAL``` indicates that the identifier provided by the protocol uniquely identifies the discovered service globally
- ```IdentificationScheme.PROTOCOL``` indicates that the identifier provided by the protocol uniquely identifies the discovered service within the ecosystem of the protocol
- ```IdentificationScheme.INSTALLATION``` indicates that the identifier provided by the protocol is only unique on the specific installation that discovered the service, i.e. a service that is discovered on a different installation by the same protocol and with the same identifier does not (necessarily) refer to the same service
- ```IdentificationScheme.SESSION``` indicates that the identifier provided by the protocol is only unique for the current session, i.e. a service that is discovered later by the same protocol with the same identifier on the same installation does not (necessarily) refer to the same service

```java
@Activate
public void start() {
	this.discovery.withProtocol("newProtocol", IdentificationScheme.PROTOCOL).build().onSuccess((DiscoveryProtocolProxy createdProtocol) -> {
		this.protocol = createdProtocol;
		// start discovery process
	});
}
```

### Service discovery

Whenever a new service has been detected, the plugin should notify this using the ```DiscoveryProtocolProxy``` instance.

```java
// Just discovered a new service with identifier id at address address
this.protocol.newService(id).atAddress(address)
	.build()
	.onSuccess((final ServiceProxy<BasicService> proxy)->{
        // handle successfully adding the service
        // save the ServiceProxy from addService.result() for future reference
    })
    .onFailure((final Throwable t)->{
        // handle failure to add service
    });
```

As soon as the service is no longer online, the plugin should call ```dispose``` on the correct ```ServiceProxy``` instance. The correct ```ServiceProxy``` instance can either be saved when adding the service or it can be retrieved using the ```DiscoveryProtocolProxy#service(String)``` method.

```java
// service with identifier id disappeared
this.protocol.service(id).onSuccess(AsyncDisposable::dispose);
```

