## State changes

!!! success "_3.0.0_ When a new value for a known state is changed, a STATE_CHANGED event should be sent with the right service and state change inside"
!!! success "_3.0.1_ When a new value for an unknown state is changed, the state for which the value changed should have been added and a SERVICE_UPDATED event should have been sent together with the STATE_CHANGED event"
!!! success "_3.0.2_ When the value of multiple states are changed, they all should be included in the STATE_CHANGED event"

