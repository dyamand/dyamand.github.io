## Prototypes

DYAMAND uses the ```Prototype``` design pattern to be able to describe services in a type-safe manner. Specifically, state prototypes and service prototypes are used to easily create services with particular capabilities. These prototypes are used together with the ```Builder``` design pattern to create whatever states or services need to be created.

### State prototypes

State prototypes are blueprints for actual states in a service. Conceptually, there are three different types of states.

1. **Measurement states** States that have values related to one or more units of measurement, e.g. Temperature has values that are related to Kelvin, degrees Celsius or degrees Fahrenheit. A special unit is the lack of a unit, also called a scalar or a quantity of dimension one.
1. **Discrete states** States that have a finite collection of possible values, e.g. door open vs. closed.
1. **Dimensionless states** States that do not have a dimension, just a value, e.g. a message that was sent by a technology

State prototypes are both being used by general applications (generic states) as by technology-specific plugins (specific states). Translation plugins are responsible to translate specific states to generic states (TODO add link).

A state prototype is an instance of ```State```. _org.dyamand.description.api_ provides a number of abstract classes to help you in implementing new states.

Registering a state prototype can be done by getting a ```Description``` reference and registering it as in the example below.
```java
@Component(immediate = true)
public final class ExampleTypePlugin {
	private final Description description;

	@Activate
	public ExampleTypePlugin(@Reference Description description) {
		this.description = Objects.requireNonNull(description);
	}
	
	@Activate
	public void start() {
		this.description.newStatePrototype(ExampleGenericState.class, (AsyncResult<AsyncDisposable> newState) -> {
			if (newState.succeeded()) {
				// save the AsyncDisposable to remove later
			} else {
				// handle failure to add state prototype
			}
		});
	}
}
```

### Service prototypes

Apart from being able to register prototypes for a particular state, service prototypes allow for prototyping a complete service. A service prototype defines which states a service of that type can and should contain. The important methods for a service prototype are ```states()``` and ```mandatoryStates()```.

```java
public final class ExampleService implements Service<ExampleService> {
	...
	@Override
	public Class<ExampleService> type() {
		return ExampleService.class;
	}
	
	@Override
	public Collection<State<?>> states() {
		return Arrays.asList(exampleGenericStatePrototype, temperaturePrototype);
	}
	
	@Override
	public Collection<State<?>> mandatoryStates() {
		return Collections.singleton(temperaturePrototype);
	}
	...
}
```

Once such a service prototype is created, it can be registered similarly to how you register a state protoype. 

```java
@Component(immediate = true)
public final class ExampleTypePlugin {
	private final Description description;

	@Activate
	public ExampleTypePlugin(@Reference Description description) {
		this.description = Objects.requireNonNull(description);
	}
	
	@Activate
	public void start() {
		this.description.newServicePrototype(ExampleService.class, (AsyncResult<AsyncDisposable> newService) -> {
			if (newService.succeeded()) {
				// save the AsyncDisposable to remove later
			} else {
				// handle failure to add service prototype
			}
		});
	}
}
```

Once this has been done, SDP plugins can use these prototypes to describe the services they discover.
