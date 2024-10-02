# GraphQL

[GraphQL](https://graphql.org/) is a query language for APIs.

## Installation

Install Apollo Client:
```bash
npm install @apollo/client
```

## Setup

### Create client

- cache - stores API call responses - meaning that when we make the same API call it 

```ts
import { ApolloClient, InMemoryCache } from '@apollo/client';

const client = new ApolloClient({
  cache: new InMemoryCache(),
  uri: 'https://countries.trevorblades.com'
});
```

### Create query

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


## Query variables, argument types, filtering

### Query variables

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

### Argument types

| Type | Description |
| ------------- | ------------- |
| String | `string` |
| Int  | `number` |
| Float  | `number` (float or number) |
| Boolean  | `number` (true or false) |
| ID  | unique identifier, often used to refetch an object or as the key for a cache |
| [String]  | array of `string` values |


### Filtering fields

| Operator | Description | Type | Example |
| ------------- | ------------- | ------------- | ------------- |
| eq | strict equality | `string`, `boolean`, `number` | `filter: { name: { eq: "India" }}` |
| regex | matching provided regular expression | `string` | `filter: { name: { regex: "Ind" }}` |
| ne | to equal | `string`, `boolean`, `number` | `filter: { name: { ne: "India" }}` |
| in | matching any of the provided values | `string`, `boolean`, `number` | `filter: { code: { in: ["CZ", "US"] }}` |
| nin | not matching any of the provided values | `string`, `boolean`, `number` | `filter: { code: { nin: ["CZ", "US"] }}` |
| len | matching specified length | `string` | `filter: ` |
| gt | greater than | `number`, date | `filter: ` |
| gte | greater or equal | `number`, date | `filter: { date: { gte: "2017" }}` |
| lt | less than | `number`, date | `filter: ` |
| lte | less or equal | `number`, date | `filter: ` |
| between | matching nodes between provided values | `number`, date | `filter: { price: { between: [49, 99] }}` |
| dteq | filter by date property equal to provided date value | date | `filter: ` |
| size | filter nodes which have an array property of specified size | date | `filter: ` |
| contains | find nodes with field containing the provided value | date | `filter: ` |
| containsAny | find nodes with field containing any of the provided values | date | `filter: ` |
| containsNone | find nodes with field containing none of the provided values | date | `filter: { keywords: { contains: ["gridsome"] }}` |

Example:
```ts
query GetCountries($code: String!) {
  countries(filter: { code: { in: ["CZ", "US"] } }) {
    name
    capital
  }
}
`;
```

useQuery fetches data immediately when the component mounts.
useLazyQuery allows you to control when the query is executed, typically via a manual trigger (e.g., button click).
