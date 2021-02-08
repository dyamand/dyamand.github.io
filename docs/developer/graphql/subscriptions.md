[![Suspended](https://img.shields.io/badge/status-mergeWithForApplicationDevelopers-red)](https://www.repostatus.org/#suspended)

# GraphQL subscriptions

Subscriptions allow you to get changing information as soon as it is available. This makes for an important tool to create a responsive application. To keep this mechanism scalable, subscriptions work a bit different than queries and mutations. Apart from the DYAMAND GraphQL endpoint, you will need to use the DYAMAND Subscriptions endpoint to make use of subscriptions.

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://lucid.app/documents/embeddedchart/2c5b2b2c-40e1-4f73-a99b-9a357be790c3" id="0vUM40o3QvFm"></iframe></div>

## Setting up server-sent events

The first step in subscribing to changes in DYAMAND concepts is to acquire a subscription ID using the DYAMAND Subscriptions endpoint. The subscripion ID you receive, can be used to setup a connection over which the subscription updates will be sent using server-sent events. Closing the SSE connection will unsubscribe from all subscriptions that were created.

## Subscribing to particular updates

Once this SSE connection has been setup, the normal DYAMAND GraphQL endpoint can be used to subscribe to events your application is interested in. The subscription ID received in the previous step should be offered whenever a subscription is created. As soon as the subscription has been created, updates will be sent over the SSE connection created in the previous step.
