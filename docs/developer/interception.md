Interception is the act of changing the functionality of a certain component without changing that component. The decision to perform some action is left to interceptors who get limited access to the inner workings of a specific component. Interceptors can register for certain interception points (points in the workflow of a component in which flexibility is needed). A context object will be provided to the interceptor that will influence the way the components proceeds.

## Interception points

!!! success "_f.0.0_ When an interceptor is registered for a particular interception point, that interceptor should be invoked with the provided context object when that interception point is reached"
!!! success "_f.0.1_ When an interceptor is registered for an interception point, that interceptor should NOT be invoked when a different interception point is reached"
!!! success "_f.0.2_ When multiple interceptors are registered for the same interception point, all interceptors should be invoked with the same input object (object wrapped by context object)"
!!! success "_f.0.3_ When an interceptor is registered for a particular interception point with a filter, that interceptor should only be invoked when the filter returns true"
!!! success "_f.0.4_ If multiple interceptors are registered and unregistered, they should not be invoked anymore when an interception point is reached"

## Historical interception

!!! success "_f.1.0_ When no framework is registered, the interceptor should not be invoked on its registration"
!!! success "_f.1.1_ An interceptor that is registered before the interception happens should receive the same context object as an interceptor that is registered after the interception happens (or none if filtered), if a framework that does historical interception is registered"
!!! success "_f.1.2_ When a framework fails to process an interceptor's registration, that interceptor should be invoked when future interception happen"
!!! success "_f.1.3_ When a framework is registered and unregistered, that framework should no longer be invoked when an interceptor is registered"
!!! success "_f.1.4_ When a framework is registered, all already registered interceptors interested in one of the framework's interception point should be invoked for all the context objects the framework manages"
!!! success "_f.1.5_ When multiple frameworks are registered for the same interception point, all frameworks should be invoked, even if some of them fail"
!!! success "_f.1.6_ A framework should not be invoked if an interceptor for another interception point is registered"

## Asynchronicity

!!! success "_f.2.0_ Interception should not block the calling thread"
!!! success "_f.2.1_ An interceptor should not need to wait for another interceptor's filter or interception to complete"
!!! success "_f.2.2_ An interceptor should not need to wait for a previous interception to complete"
!!! success "_f.2.3_ An interceptor should not be invoked from different Vert.x contexts"
!!! success "_f.2.4_ Multiple interceptors registered from a non-Vert.x context should be invoked on different Vert.x contexts"
!!! success "_f.2.5_ The filter for an interceptor should be called on the same Vert.x context as the interceptor is called on"
!!! success "_f.2.6_ Multiple interceptors registered from a Vert.x context should be invoked on that Vert.x context"
!!! success "_f.2.7_ A context object should never be invoked concurrently"
!!! success "_f.2.8_ A framework's intercept function and result handler should always be invoked on the same context, irrespective of the number of registered interceptors or when they are registered"
!!! success "_f.2.9_ If multiple frameworks are registered, they should all be invoked from a different context"

## Interception results

!!! success "_f.3.0_ When interception for a certain interception point gets triggered, the number of interested interceptors should equal the number of interceptors registered for that interception point and the original context object should be returned with all actions performed"
!!! success "_f.3.1_ Whatever interception point you are intercepting for, the number of contacted interceptors should equal the number of interceptors that do not fail and the number of errors should be the same as the number of failing interceptors"
