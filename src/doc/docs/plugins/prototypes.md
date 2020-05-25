## Prototypes

DYAMAND uses the ```Prototype``` design pattern to be able to describe services in a type-safe manner. Specifically, state prototypes and service prototypes are used to easily create services with particular capabilities. These prototypes are used together with the ```Builder``` design pattern to create whatever states or services need to be created.

### State prototypes

State prototypes are blueprints for actual states in a service. Conceptually, there are two different types of states, states that are generic and will be used by general applications and states that will be used by a particular technology. By convention, generic states should be registered by type plugins and technology-specific states should not.

A state prototype is an instance of ```State```. Below you can find an example of a generic state that has a ```Boolean``` value and supports two units, opposites of one another. The ```State``` is responsible to convert between different units.

```java
public final enum ExampleGenericStateUnit implements Unit<ExampleGenericState> {
	NORMAL("n"),
	REVERSE("r");
	private final String symbol;
	
	public ExampleGenericStateUnit (String symbol) {
		this.symbol = symbol;
	}
	
	@Override
	public String symbol() {
		return this.symbol;
	}
}

public final class ExampleGenericState implements State<ExampleGenericState, ExampleGenericStateUnit, Boolean> {
	private final boolean value;
	private final ExampleGenericState unit;
	
	private ExampleGenericState (boolean value, ExampleGenericStateUnit unit) {
		this.value = value;
		this.unit = unit;
	}
	
	@Override
	public Class<ExampleGenericState> type() {
		return ExampleGenericState.class;
	}
	
	@Override
	public Collection<Unit<ExampleGenericState>> supportedUnits() {
		return ExampleGenericStateUnit.values();
	}
	
	@Override
	public Boolean value(ExampleGenericStateUnit unitToConvertTo) {
		if (this.unit == unitToConvertTo) {
			return this.value;
		} else {
			return !this.value;
		}
	}
	
	@Override
	public StateBuilder<ExampleGenericState> create() {
		// return state builder to start building a state based on the provided prototype
	}
}
```

Registering this state prototype can be done by getting a ```Description``` reference and registering it as in the example below.
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
