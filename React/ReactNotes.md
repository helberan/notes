# Basic commands

Create a new project using [Vite](https://vitejs.dev/guide/) and a TypeScript template
```
npm create vite@latest . -- --template react-ts
```
Install dependency package
```
npm install
```
Run the app
```
npm run dev
```
Build the app
```
npm run build
```

# Security - .env

Some sensitive data, such as API keys should not be accessible and commited to git.
More details at: [dotenv](https://www.npmjs.com/package/dotenv)

## Installation

```
npm install dotenv
```

## Setup

Create a `.env` file in the root of your project.
If you are using React, make sure to start each of your keys name with `REACT_APP`:
```
REACT_APP_API_KEY=your-api-key
```

Create a ts file `env.d.ts` in the root of your project where you'll define interfaces:
(I couldn't access my api keys without this step.)
```ts
interface ImportMetaEnv {
  readonly REACT_APP_API_KEY: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

Access your api key in your component:
```ts
const APIKey: string | undefined = process.env.REACT_APP_API_KEY;
```

## .gitignore

Make sure you don't commit your `.env` file to git - add this line to your `.gitignore` file:
```
# api keys
.env
```