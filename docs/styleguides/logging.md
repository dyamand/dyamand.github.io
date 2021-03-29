[![Suspended](https://img.shields.io/badge/status-mergeWithForTechnologyAndCoreDevelopers-red)](https://www.repostatus.org/#suspended)

## Using loggers
In DYAMAND plugins/components, the OSGi logger and loggerFactory interfaces are used.
A logger for your component can be obtained through reference of the LoggerFactory, by using the following syntax in the constructor of the class:
```java
@Component
public final class foo
  @Activate
  public foo(@Reference(service=LoggerFactory.class) final Logger logger){
    // ...
    logger.info("Foo started!")
  }
}
```
The obtained logger will use the component's PID in its log statements, therefore it is advised to let each component obtain its own logger.

The OGSi Logger interface provides 2 main ways to log messages:
1. Log message directly:<br>
  `logger.info("The statement will always be evaluated");`

2. Log message with lambda:<br>
  `logger.info((Logger l) -> l.info("This statement will only be evaluated if the log level is set at or below 'INFO'"));`

The second approach is generally recommended, as it is beneficial for performance on higher log levels.
For simple log statements, the first approach can still be used.

To add information to your log statements, use curly braces. Pairs of curly braces will be replaced, in order, by the additional parameters you hand to the log statement.
```java
logger.info("Use {} if you want to put information in your {}.", "curly braces", "log statements");
```

toString() is automatically called on objects you pass through to the logger.
```java
Map<String, String> someMap = new HashMap<>();
logger.info("Content of someMap: {}", someMap);
```

If you are handling exceptions, you can add stacktraces of throwables to your log statements by adding the throwable as the last parameter.
```java
try {
  // Throw something
} catch (Throwable t){
  logger.error("That did not go as expected", t);
}
```
You can combine this functionality with the use of curly braces.
```java
Object someObject;
try {
  // Throw something
} catch (Throwable t){
  logger.error("Failed to process {} correctly", someObject, t);
}
```
Your log statement should have the following parameters, in order:
* the message itself
* a number of objects or function calls equal to the number of curly brace pairs in your message
* (optional) a Throwable in case you want to append its stacktrace

### Changing the log level
If you are using the gogo shell, you can change the global log level with the command `logconfig [LEVEL]`.
To change the log level of a specific logger, mention the PID: `logconfig some.random.pid [LEVEL]`

The log level can also be set through the use of an environment variable. Set `DYAMAND_DEFAULT_LOG_LEVEL` to any supported log level to change the default configuration. Note that using gogo's logconfig command will override this setting.

## Supported log levels
| Level | Usage | Examples |
| ----- | ----- | -------- |
| `TRACE` | Display most minute details about operations and data going through the system. | <ul><li>Full content of data structures</li><li>Step-by-step data flow through function</li></ul>
| `DEBUG` | Provide detailed information about data and actions being taken, to make it easier to detect what is going wrong. | <ul><li>Important fields from used data structures (possible abbreviated or filtered)</li><li>Reporting of data flow through function</li></ul>
| `INFO`  | Give a coarse-grained indication of what the system is doing. | <ul><li>Start and stop of services</li><li>Important functions get called</li></ul>
| `WARN`  | Log information about actions that differ from the expected procedure. | <ul><li>Missing expected but non-critical data</li><li>Abnormal values indicating unusual behaviour</li><li>Semi-expected invalid values which map to valid values, e.g. integer sent as string<li>Iffy connections that have to be restarted in the middle of an operation</li></ul>
| `ERROR` | Log information about actions that could not be handled at all, or could only be partially completed | <ul><li>Missing critical data</li><li>Unexpected values</li><li>Missing required connection</li></ul>
| `AUDIT` | Audit statements will always be printed, regardless of configured log level. Use audit statements to track important user activities and to detect unauthorized access. See [the auditing subsection](#auditing) for more info. | <ul><li>User login attempt</li><li>Change of important data value</li></ul>

## Audit logging
Audit log statements are different from the other log levels in that their main purpose is not diagnosing, but tracking changes.
To allow easy processing of audit statements, they should be logged in the form of a JSON-object containing key-value pairs.

Recommended fields:

* `user`: JSON Object with nested key-values:
    * `username`: String representing user initiating action
    * `groups`: JSON Array with groups the user belonged to at moment of logging
       >In the case of a system generated action, username should be "system" and groups should be empty (or an appropriate list of groups if applicable)
* `verb`: String indicating action to be taken on resource. This should be a simple keyword, e.g. get/list/add/delete/...
* `objectRef`: 
    * `resource`: String indicating on what type of resource the action was performed. This is a high level indicator, and should not refer to specific instances.
    * `name`: name of the resource on which the action was taken
* `annotations`: JSON object containing extra information about the action, e.g.
    * `decision: allow|deny`: whether the action was allowed to go through or not
    * `reason: *`: details about why the action was (dis)allowed

> NOTE: while it is strongly recommended to provide as many of the recommended fields as possible, some components might be lacking information to completely fill out all fields. In such cases, multiple components should each logging their part of the audit statement. By following the audit log, one should still be able to obtain all required info.
> NOTE: Extra fields may be added as needed. While care should be taken to follow the recommended field layout as much as possible, any extra available information should be logged through additional fields. E.g. in some cases, the object might also belong to specific groups. Or an object might not possess a unique name, but consist of unique subelements.


### Implemented audit statements
#### User permissions
Permissions Manager (No initiator info)
```json
{
    "verb":"add",
    "objectRef":{
        "resource":"permission",
        "object":"system",
        "subject":"user:testuser1@dyamand.be",
        "permission":"CREATE_ORGANIZATION"
    },
    "annotations":{
        "decision":"allow",
        "result":"added"
    }
}
```
User Service (Has initatior info)
```json
{
    "user":{
        "username":"system",
        "groups":[           
        ]
    },
    "objectRef":{
        "resource":"permission",
        "object":"system",
        "subject":"user:testuser1@dyamand.be",
        "permission":"CREATE_ORGANIZATION"
    },
    "verb":"add",
    "annotations":{
    }
}
```

#### DYAMAND audit statement proposals
1. Detecting a new installation

The DYAMAND backend receives an installation event from a new installation. As the system is allowed to perform all actions, no decision annotation is added.

```json
{
  "user":
  {
    "username":"system",
    "groups":[]
  },
  {
    "objectRef":
    {
      "resource":"installation",
      "name":"COT-GW-001"
    },
  "verb":"add",
  "annotations":{}
}
```

2. Removing an installation from a project

User is assigned to project, therefore removing an installation from the project is allowed.
The installation itself is still present in the system and is potentially linked to other projects.
```json
{
  "user":
  {
    "username":"john-doe",
    "groups":["cot:internet-of-water","system:authenticated"]
  },
  {"objectRef":
  {
    "resource":"installationRef",
    "name":"COT-GW-001",
    "project":"cot:internet-of-water"
  },
  "verb":"delete",
  "annotations":{
    "authorization.dyamand/decision":"allow"
  }
}
```

3. Removing an installation from the system completely

A user attempts to remove an installation from the system, as this user is not a DYAMAND admin, the action is denied. The user is only able to configure project specific settings of the installation.
```json
{
  "user":
  {
    "username":"john-doe",
    "groups":["cot:internet-of-water","system:authenticated"]
  },
  {"objectRef":
  {
    "resource":"installation",
    "name":"COT-GW-001"
  },
  "verb":"delete",
  "annotations":{
    "authorization.dyamand/decision":"deny"
  }
}
```
