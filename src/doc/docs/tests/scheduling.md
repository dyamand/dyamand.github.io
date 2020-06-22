## Internal scheduling

!!! success "_a.0.0_ Submitting N tasks should result in N executions on the executor"
!!! success "_a.0.1_ Submitting tasks should be done asynchronous with respect to its execution, i.e. submitting a task should take no more than 1 second even though individual execution of that task takes 10 seconds"
!!! success "_a.0.2_ Submitting N tasks that execute successfully each with its own handler, should invoke each handler exactly once"
!!! success "_a.0.3_ Submitting N tasks that fail to execute each with its own handler, should invoke each handler exactly once with the correct error information"
!!! success "_a.0.4_ Submitting N tasks can never result in a concurrent execution on the task executor"
!!! success "_a.0.5_ When submitting N tasks each containing its start number as an argument, the number of execution should be the same as the start number"
!!! success "_a.0.6_ When submitting N tasks, each task should be executed on a worker thread"
!!! success "_a.0.7_ When submitting a task with a handler, that handler should be invoked with the correct result"
!!! success "_a.0.8_ When N tasks are submitted before the scheduler is started, they should be executed as soon as the scheduler starts"
!!! success "_a.0.9_ When submitting N tasks concurrently, they should all be executed"
!!! success "_a.0.10_ When a task scheduler is stopped, handlers of all pending tasks should be notified with an error"
!!! success "_a.0.11_ No matter how many times a scheduler has been started, it should succeed and the scheduler should be started"
!!! success "_a.0.12_ No matter how many times a scheduler has been stopped, it should succeed and the scheduler should not be started"

## External code execution

!!! success "_a.1.0_ When invoking external code, the supplier should be invoked and the return value should equal that of the supplier"
!!! success "_a.1.1_ When invoking external code that fails, the error should be available"
!!! success "_a.1.2_ When invoking external code that blocks, the caller should not be blocked"
!!! success "_a.1.3_ When invoking external code that blocks, the handler should be called after a certain timeout"

## Disposal

!!! success "_a.2.0_ When multiple external code executions are created and disposed, there should be no more resources (threads) as before"
!!! success "_a.2.1_ When an external code execution is created and disposed, executing something should result in an IllegalStateException"
!!! success "_a.2.2_ When code gets executed N times at the same time while also disposing the external code execution, all instances should either be successfully executed or have resulted in an IllegalStateException"
