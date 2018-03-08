class: center, middle

# A JavaScript GraphQL Stack Built With Apollo

[![Apollo GraphQL](img/apollo.png)](https://apollographql.com)

Daniel Maslowski |
![Twitter](img/twitter.png) [@orangecms](https://twitter.com/orangecms) |
![GitHub](img/github.png) [orangecms](https://github.com/orangecms)

---

# ToC

1. Introduction to GraphQL
2. RPC Architectures
3. GraphiQL Frontend
4. Tooling and Testing
5. Logging and Monitoring

---

class: center, middle

## Introduction to GraphQL

---

### GraphQL

![GrapbQL](img/graphql.png)

--

* a query language for APIs
* views queries as graphs 🤷
* provides predictable results
* has a built-in type system
* [spec](http://facebook.github.io/graphql/October2016/) is a working draft

---

### Example Query

--

```graphql
query {
  coalMines {
    name
    location {
      address
      zipCode
      city
    }
  }
}
```

--

* fields: a coal mine may have a lot of data, e.g.:
  - name
  - location
  - website
  - shutdown date
* relation: every location can hold a reference to a geo API like OSM

---

### Example Response

```json
{
  "data": {
    "coalMines": [
      {
        "name": "Zeche Carl",
        "location": {
          "address": "Wilhelm-Nieswandt-Allee 100",
          "zipCode": "45326",
          "city": "Essen"
        }
      },
      {
        "name": "Zeche Helene",
        "location": {
          "address": "Twentmannstraße 125",
          "zipCode": "45326",
          "city": "Essen"
        }
      },
      {
        "name": "Zeche Zollverein",
        "location": {
          "address": "Gelsenkirchener Str. 181",
          "zipCode": "45309",
          "city": "Essen"
        }
      }
    ]
  }
}
```

---

class: center, middle

## RPC Architectures

---

### Handling Remote Procedure Calls

--


* Input Validation

--


* Authentication

--


* Authorization

--


* Operation

--


* Output Validation

---

### Input and Output Validation

--


* should happen on every layer (API, application logic, DB, ...)

--


* provided through GraphQL's type system

---

### Authentication


--


* makes the app stateful

--


* requires session middleware

--


* adds a security context to requests

--


* existing library: [Passport](http://www.passportjs.org/docs/)

--

#### Integration example
```javascript
function handleRequest(ctx, next) {
  const { user } = ctx.session;
  const context = { user };
  return graphqlKoa({
    schema,
    context,
  })(ctx, next);
}

```

---

### GraphQL Operations

#### [In Current Spec](http://facebook.github.io/graphql/October2016/#sec-Executing-Operations)

* Request-Response
  - `Query` (request that only fetches data)
  - `Mutation` (optional, request that may manipulate data)

--

#### [Subscription Proposal](https://github.com/facebook/graphql/blob/master/rfcs/Subscriptions.md)

* Publish-Subscribe
  - `Subscription` (client registers for receiving data continuously)

---

### Backend Requirements

* HTTP server, e.g., koa 🎲
* Websocket support for subscriptions
* body parser middleware
* session middleware
* authentication middleware
* router middleware

---

class: center, middle

## GraphiQL Frontend

---

### GraphiQL

--


* query editor with auto-completion and syntax highlighting
* schema exploration via introspection queries


--

* can be used as a React component
* can be served as a full app with a one-liner

--

```javascript
router.get('/graphiql', graphiqlKoa({ endpointURL: '/graphql' }));
```

--

![GraphiQL screenshot](img/graphiql.png)

--

#### Demo Time! :)

---

class: center, middle

## Tooling and Testing

---

### GraphQL Schemas

* type definitions
  - graphql-tag (allows syntax highlighting)
  - precompiled (best performance)
  - export strings (with the two above, documentation directives get lost)
  - programmatic (hand-crafted, yay)
* resolvers
  - requests to other services
  - transformations

---

### Unit Testing

* suitable for transformations
* run fast, offline, without extra dependencies
* provide fixtures for responses from other services

---

### Integration & Service Testing

* run an instance of the GraphQL API service
* stub out external dependencies (used by resolvers)
* provide fixtures for responses to the requests to other services
* assert that a query receives a response or an error as intended

---

### Error Types

--

* operational errors
  - expected
  - need to be exposed
  - can accumulate: GraphQL returns a list of errors
  - may require individual action from the client

--


* programmatic errors
  - unexpected
  - should remain internal

---

### Error Occurrences

--


* web server

--


* resolvers
* subscriptions

--


* proposal:
  * try/catch around transformations
  * generate UUID
  * log UUID and error with stack trace
  * throw new error with UUID as reference

---

class: center, middle

## Logging and Monitoring

---

### Logging

* log incoming requests for auditability: who is doing what?
  - do *not* log sensitive information
* `makeExecutableSchema` takes a `logger` as an optional argument
  - logger has to implement a function `log`

--

#### Simple logging library

```js
import getenv from 'getenv';
import bunyan from 'bunyan';
import { ConsoleLogger } from '@cdm-logger/server';

const isProduction = getenv('NODE_ENV', 'development') === 'production';

export default (name) => {
  const logger = (isProduction
    ? bunyan.createLogger({ name })
    : ConsoleLogger.create(name)
  );
  // Apollo requires logger.log for error logging support
  logger.log = logger.error;
  return logger;
}
```

---

### Monitoring With [Apollo Engine](https://www.apollographql.com/engine)

* UI with dashboard is a [hosted service](http://engine.apollographql.com/)
* [Engine Proxy](https://www.apollographql.com/docs/engine/#engine-proxy) comes as a binary written in Go
* can be run as
  - middleware that spawns a child process
  - a container in a service landscape
* performance tracing and error tracking
  - requires tracing extension from GrahQL API
  - recommends compression middleware
  - extensions can be filtered out for the client with `formatResponse`
* response caching and persisted queries
* schema usage analysis

---

class: center, middle
# Thanks, that's it! 😸
