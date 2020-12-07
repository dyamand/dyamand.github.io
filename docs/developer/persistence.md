DYAMAND clients running on gateways communicate with the DYAMAND backend by using events. The services deployed on the backend use these events to extract useful information and offer that information for further processing. These services hold their information in-memory to enable fast querying. This, however, poses a number of problems in terms of information persistence. This page discusses the way DYAMAND solves this problem. The Event Source component persists events it gets so that the application state of the different services can be rebuilt. Since the application state of the services cannot be rebuilt from the events alone and since we do not want to lose any important data, the services – for this purpose called Event Sinks – are offered a way to persist their application state so that it can be reinstated on reboot.

There are two important use cases, namely exporting the current application state and importing the existing application state.

## Exporting current application state

The event source is responsible to initiate an export routine. Export routines are managed per event sink. An export routine is triggered anytime the amount of persisted events reaches a certain threshold or whenever an event sink is going offline. Once the export routine has been started, the event source happily keeps on processing events. It is expected that event sinks persist their application state in an asynchronous manner and notify the event source once it is finished. Event source will not initiate a new export routine as long as the event sink is not finished.

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://www.lucidchart.com/documents/embeddedchart/07d6b152-2df8-4483-85fd-590751e4cf39" id="1~33jdtmSMSb"></iframe></div>

### Event Source

!!! success "_g.0.0.a_ Whenever the event source has processed N events, it should initiate an export routine on all registered event sinks"
!!! success "_g.0.0.b_ Whenever X seconds have passed, the event source should initiate an export routine on all registered event sinks"
!!! success "_g.0.1_ If another export routine is still running and another export is scheduled, no export routine should be started for that event sink until export is finished"

### Object Store

!!! success "_g.1.0_ Creating an object store with a name that already exists should fail"
!!! success "_g.1.1_ Exporting should not block execution of the current thread"

### Event Sink

Event sinks are expected to fetch the state on start and to export their state on stop. Letting the event source be responsible for triggering the export just does not work since the lifecycles of the event sink and the event source clash. Typically, the event sink will already have released all resources needed to export their state once the event source has the possibility to trigger another export for them.

!!! failure "_g.2.0_ On start, an event sink should fetch the current state from the object store"
!!! failure "_g.2.1_ On stop, an event sink should store its state in the object store"

## Importing existing application state

Once the event sinks have persisted their state, all information is encapsulated in the combination of persisted application state and events received by the event source since the last export routine. When an event sink comes online, the event sink should initiate an import routine. Once the import routine is done, the event sink should register with the event channel and the event source should replay all persisted events since the last export time for that event sink.

### Event Source

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://www.lucidchart.com/documents/embeddedchart/71bf049f-d53f-4993-b6a2-eba152303c75" id="nq43ZRYSh-F-"></iframe></div>

!!! success "_g.3.0_ Once the import routine finished, all events received by the event source since the last export should be published to the event sink (by using historical interception)"
!!! success "_g.3.1_ Only the cached events should be sent when an event sink registers a listener when the object store does not store any events"

### Object Store

!!! success "_g.4.0_ Exporting a collection of objects and importing it again should result in the same collection of objects, even when the object store is closed and created again"
!!! success "_g.4.1_ Exporting multiple collections of objects over multiple exports should result in the amount of objects exported last if snapshot is set to true"
!!! success "_g.4.2_ Exporting multiple collections of objects multiple times should result in the amount of objects equal to the sum of all exported collections if snapshot is set to false"
!!! success "_g.4.3_ When providing a timestamp function, fetching exported objects should return them in chronological order"
!!! success "_g.4.4_ When providing a timestamp function and fetching in a range, the returned and only the returned objects should be in range"

### Purging the object store

!!! success "_g.5.0_ When purging an object store without the retention time being set, it should fail"
!!! success "_g.5.1_ When an object store is not a snapshot object store, purging the object store should only keep objects with a timestamp after NOW - retention time"
!!! success "_g.5.2_ When an object store is a snapshot object store, purging should fail"

### Snapshots

!!! success "_g.6.0_ No matter how many snapshots have been made, restarting the object store should restore the latest one"
!!! success "_g.6.1_ If a snapshot is not recoverable, the object store should try to import the snapshot before"
