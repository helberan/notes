# GraphQL

[GraphQL](https://graphql.org/) is a query language for APIs.

## Installation

Install Apollo Client:
```bash
npm install @apollo/client
```

## Create client

- cache - stores API call responses - meaning that when we make the same API call it 

```ts
import { ApolloClient, InMemoryCache } from '@apollo/client';

const client = new ApolloClient({
  cache: new InMemoryCache(),
  uri: 'https://countries.trevorblades.com'
});
```

## Create query

```ts
import { gql } from '@apollo/client';

const GET_COUTNRIES = gql`
  query GetCountries {
    country(code: "CZ") {
      name
      capital
    }
  }
`;
```

### query variables

All variables start with **$** symbol, followed by the **argument type**. They can be **optional or required**.
Required variable definitions carry an **!** next to the type.

- `($code: String!)` defines a variable with name code, of type String, that is required.
- `($code: String!, $name: String)` defines **two variables** of type String, one required, one optional.
- `($code: String = "CZ")` defines a **default value**.

```ts
query GetCountries($code: String!) {
  country(code: $code) {
    name
    capital
  }
}
`;
```

### filtering


## Create query

```ts
import { gql } from '@apollo/client';

const GET_COUTNRIES = gql`
  query GetCountries($name: String!) {
    countries(filter: { name: { regex: $name } }) {
      name
      code
    }
  }
`;
```

useQuery fetches data immediately when the component mounts.
useLazyQuery allows you to control when the query is executed, typically via a manual trigger (e.g., button click).
