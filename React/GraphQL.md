# GraphQL

[GraphQL](https://graphql.org/) is a query language for APIs.

## Installation

Install Apollo Client:
```bash
npm install @apollo/client
```

## Client

Client is a tool that **structures API requests** (queries, mutations) and **processes the responses**. It acts as a middle layer between the user and the API.

- `cache` - stores query results in memory for performance improvements, so that repeated requests for the same data can be served from the cache rather than making a new network request.
- `uri` - specifies the endpoint or URL of the GraphQL API server that Apollo Client will communicate with.

```ts
import { ApolloClient, InMemoryCache } from '@apollo/client';

const client = new ApolloClient({
  cache: new InMemoryCache(),
  uri: 'https://countries.trevorblades.com'
});
```

## Query

Query is used to **retrieve data** from the server.

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

## Mutation

Mutation is used to **alter data** on the server (creating, updating, or deleting records).

```ts
import { gql } from '@apollo/client';

const UPDATE_USER = gql`
  mutation UpdateUser($id: ID!, $name: String!) {
    updateUser(id: "1", name: "Alex") {
      id
      name
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
| len | matching specified length | `string` | `filter: { name: { len: 5 }}` |
| gt | greater than | `number`, date | `filter: { price: { gt: 100 }}` |
| gte | greater or equal | `number`, date | `filter: { date: { gte: "2017" }}` |
| lt | less than | `number`, date | `filter: { price: { lt: 50 }}` |
| lte | less or equal | `number`, date | `filter: { date: { lte: "2020-12-31" }}` |
| between | matching nodes between provided values | `number`, date | `filter: { price: { between: [49, 99] }}` |
| dteq | filter by date property equal to provided date value | date | `filter: { startDate: { dteq: "2021-01-01" }}` |
| size | filter nodes which have an array property of specified size | `array` | `filter: { tags: { size: 3 }}` |
| contains | find nodes with field containing the provided value | `string` | `filter: { description: { contains: "sample" }}` |
| containsAny | find nodes with field containing any of the provided values | `array` | `filter: { tags: { containsAny: ["urgent", "important"] }}` |
| containsNone | find nodes with field containing none of the provided values | `array` | `filter: { keywords: { contains: ["gridsome"] }}` |

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

## API call - data, error, loading

### useQuery

`useQuery` fetches data **immediately** when the component mounts.

Example:
```ts
const { data, loading, error } = useQuery(GET_COUNTRIES, { client });
```

Example with variables:
```ts
const { data, loading, error } = useQuery(GET_COUNTRIES, {
  client,
  variables: { name: "P"}
});
```

### useLazyQuery

`useLazyQuery` allows you to control when the query is executed, typically via a **manual trigger** (e.g., button click).

Example:
```ts
const [getCountry, { data, loading, error }] = useLazyQuery(GET_COUNTRIES, { client });

const handleClick = () => {
  getCountry();
};
```

Example with variables:
```ts
const [getCountry, { data, loading, error }] = useLazyQuery(GET_COUNTRIES, { client });

const handleClick = () => {
  if (searchedCountry) {
    getCountry({ variables: { name: searchedCountry } });
    }
  };
```

### data

### loading

### error

## Complete setup

```ts
import { useState } from 'react';
import { ApolloClient, InMemoryCache, gql, useLazyQuery } from '@apollo/client';

interface Country {
  name: string;
  capital: string;
}

const client = new ApolloClient({
  cache: new InMemoryCache(),
  uri: 'https://countries.trevorblades.com',
});

const GET_COUNTRIES = gql`
  query GetCountries($name: String!) {
    countries(filter: { name: { regex: $name } }) {
      name
      capital
    }
  }
`;

export const CountryFind = () => {
  const [searchedCountry, setSearchedCountry] = useState<string>('');

  const [getCountry, { data, loading, error }] = useLazyQuery(GET_COUNTRIES, { client });

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setSearchedCountry(e.target.value);
  };

  const handleClick = () => {
    if (searchedCountry) {
      getCountry({ variables: { name: searchedCountryText } });
    }
  };

  return (
    <>
      <label>
        Type country name:
        <br />
        <input type="text" placeholder="India..." onChange={handleChange} />
      </label>
      <button onClick={handleClick}>Search</button>
      {loading && <div>Loading...</div>}
      {error && <div>Error: {error.message}</div>}
      {data && data.countries.length > 0 ? (
        <div>
          <h3>Country Found:</h3>
          {data.countries.map((country: Country) => (
            <div key={country.code}>
              <h4>{country.name}</h4>
              <p>Capital: {country.capital}</p>
            </div>
          ))}
        </div>
      ) : (
        !loading && <div>No country found</div>
      )}
    </>
  );
};

```



