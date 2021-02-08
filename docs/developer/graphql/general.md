[![Suspended](https://img.shields.io/badge/status-mergeWithForApplicationDevelopers-red)](https://www.repostatus.org/#suspended)

External applications can get information about all DYAMAND concepts using the DYAMAND GraphQL API. This section will discuss the general mechanisms that are available for applications.

## Queries

GraphQL queries allows a client to request the exact information it is interested in. The different queries that are available are discussed in the following subsections. Queries are sent to the GraphQL endpoint of the DYAMAND backend and the response to the query is sent over the same connection.

## Mutations

GraphQL mutations allows a client to manipulate certain DYAMAND concepts. The following subsections will also document which concepts can be manipulated. Just as queries, mutations are sent to the GraphQL endpoint of the DYAMAND backend and its response is sent back over the same connection.

## Subscriptions

Subscriptions allow an external client to get notified of changes to DYAMAND concepts. In contrast to queries and mutations, the mechanism behind subscriptions is a bit more complex. The mechanism is explained in more detail in the [next subsection](../subscriptions). Specific subscriptions are also documented in the following subsections.
