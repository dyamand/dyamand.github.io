## State changes

!!! success "_3.0.0_ When a new value for a known state is changed, a STATE_CHANGED event should be sent with the right service and state change inside"
!!! success "_3.0.1_ When a new value for an unknown state is changed, the state for which the value changed should have been added and a SERVICE_UPDATED event should have been sent together with the STATE_CHANGED event"
!!! success "_3.0.2_ When the value of multiple states are changed, they all should be included in the STATE_CHANGED event"
!!! success "_3.0.3_ When a timestamp is specified while creating state changes, the timestamp of the send STATE_CHANGED event should be equal to that timestamp"
!!! failure "_3.0.4_ When 1 state changes with a timestamp earlier then the already present state, no STATE_CHANGED event should be sent"
!!! failure "_3.0.5_ When multiple states change, only the states with a timestamp later than the previous timestamp they changed should be included in the STATE_CHANGED event"

### Default units

!!! success "_3.1.0_ When a state change (for a measurement state) is created without a unit, the state change should use the default unit of the state"
!!! success "_3.1.1_ When a state change (for a measurement state) is created without a unit and the default unit has been overridden while adding the state to the service, the state change should use the overridden unit"
!!! success "_3.1.2_ When a state change (for a measurement state) is created with a unit, the state change should use the unit that was used to create the state change. If afterwards a state change is created without a unit, the state change should use the default unit of the state"
!!! success "_3.1.3_ When a state change (for a measurement state) is created with a unit and the default unit has been overridden while adding the state to the service, the state change should use the unit that was used to create the state change. If afterwards a state change is created without a unit, the state change should use the overridden unit of the state"

