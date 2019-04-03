Source: [Advanced GraphQL (optional)](https://www.howtographql.com/advanced/0-clients/)

### Clients
- There are two major GraphQL clients available: [Apollo Client](https://github.com/apollographql/apollo-client) (a community-driven effort to build a powerful and flexible GraphQL client for all major development platforms) and [Relay](https://facebook.github.io/relay/) (Facebook's homegrown GraphQL client that heavily optimizes for performance and is only available on the web)
- GraphQL allows to fetch and update data in a declarative manner
- In React, GraphQL clients use HOC to fetch data under the hood and make it available in the props of other components
- When the build environment has access to the schema, it can essentially parse all the GraphQL code that's located in the project and compare it against the information from the schema
- GraphQL allows to have UI code and data requirements side-by-side, thus the mental overhead of thinking how data ends up in the right parts of the UI is eliminated

### Server
- GraphQL enables the server developer to focus on describing the data available rather than implementing and optimizing specific endpoints
- Resolvers can be defined on a field granularity

### More GraphQL Concepts
- A fragment is a collection of fields on a specific type
- Each field in type definitions can take zero or more arguments
- Each argument needs to have a name and a type
- GraphQL allows the usage of aliases (i.e. specifying names for the query results)
- Scalar types represent concrete units of data: `String`, `Int`, `Float`, `Boolean`, and `ID`
- Object types have fields that express the properties of that type and are composable
- An common example for a custom scalar would be a `Date` type where the implementation needs to define how that type is validated, serialized, and deserialized

### Tooling and Ecosystem
- GraphQL allows clients to ask a server for information about its schema. This is known as introspection

### Security
- The first strategy and the simplest one is using a timeout to defend against large queries
- A GraphQL server is able to reject or accept a request based on its depth
- A good estimate of how expensive a query is is the server time it needs to complete. We can use this heuristic to throttle queries. With a good knowledge of your system, you can come up with a maximum server time a client can use over a certain time frame.

### Common Questions
- GraphQL is database agnostic and can be used with any kind of database or even no database at all
- To implement authorization, it is recommended to delegate any data access logic to the business logic layer and not handle it directly in the GraphQL implementation
