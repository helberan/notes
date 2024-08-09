# Basic commands

Create a new project using [Vite](https://vitejs.dev/guide/) and a TypeScript template
```
npm create vite@latest . -- --template react-ts
```
Run the app
```
npm run dev
```
Build the app
```
npm run build
```

# React Router

## Installation and import

Install dependencies to your project.
```
npm install react-router
npm install react-router-dom
```
Import BrowserRouter at the top of your components structure.
```ts
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
```

## Implementation example

```ts
<Router>
   <header>
      <nav>
         <Link to="/">Home</Link>
         <Link to="/contact">Contact</Link>
      </nav>
   </header>
   <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/contact" element={<Contact />} />
      <Route path="*" element={<PageNotFound />} />
   </Routes>
</Router>
```

## useNavigate

`useNavigate` is a hook, which redirects the user to a different url just like `Link` component.
Compared to `Link`, `useNavigate` is used in a ts/js logic and not in JSX.

```ts
import { useNavigate } from "react-router-dom";

function MyComponent() {
  const navigate = useNavigate();

  function handleClick() {
    // Navigate to a specific route
    navigate('/contact');
  }

  return (
    <div>
      <button onClick={handleClick}>Go to Contact Page</button>
    </div>
  );
}
```

# Redux
Redux is a state management library for JavaScript applications, that helps you manage the state of your app in a more predictable and organized way.
Redux simplifies state management by centralizing your application's state in a single place called a store.
The store holds all the state for your app, allowing different parts of your app to access and update the state consistently.

## Installation and basic setup

Install Redux Toolkit and React-Redux as dependencies:
```
npm install @reduxjs/toolkit react-redux
```

I'll be using this folder structure:
```scss
src/
  store/
    index.ts       // Store configuration
    numberSlice.ts // Slice for a number state
```

Create store `index.ts`:
```ts
import { configureStore } from '@reduxjs/toolkit';

const store = configureStore({
  reducer: {
    //this is where we'll add parts of the store
  },
});

export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;

export default store;
```

Integrate the store into your app using `Provider` and your created `store`:
```ts
import ReactDOM from 'react-dom/client';
import App from './App.tsx';
import { Provider } from 'react-redux';
import store from './store/index.ts';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

## Creating slices

For example, let's create a slice to store a number:
```ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

const initialState: number = 0;

const numberSlice = createSlice({
  name: 'numberSlice',
  initialState,
  reducers: {
    setNumberState(state, action: PayloadAction<number>) {
      return action.payload;
    },
  },
});

export const { setNumberState } = numberSlice.actions;
export default numberSlice.reducer;
```

Add the numberSlice reducer to the store:
```ts
import { configureStore } from '@reduxjs/toolkit';
import numberReducer from './numberSlice';

const store = configureStore({
  reducer: {
    number: numberReducer,
  },
});

export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;

export default store;
```

## Accessing stored data

Use `useSelector` to access the state:
```ts
import { useSelector } from 'react-redux';
import { RootState } from './store/index';

export const Example = () => {
  const number = useSelector((state: RootState) => state.number);

  return (
    <div>
      <p>Number from store: {number}</p>
    </div>
  );
};
```

## Updating stored data

Use `useDispatch` to update the state:
```ts
import { useDispatch } from 'react-redux';
import { AppDispatch } from './store/index';
import { setNumberState } from './store/numberSlice';

export const Example = () => {
  const dispatch: AppDispatch = useDispatch();

  const handleNumberChange = () => {
    dispatch(setNumberState(1));
  };

  return (
    <div>
      <p>Click to change the value of the number in store:</p>
      <button onClick={handleNumberChange}>Change</button>
    </div>
  );
};
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


# EXCEL data import, reading, export

More info at [SheetJS](https://docs.sheetjs.com/docs/getting-started/installation/frameworks/).

## Installation and import

Install dependency to your project.
```
npm install --save https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz
```
Import XLSX where you want to use it.
```ts
import * as XLSX from 'xlsx';
```

## IMPORT

I have created a button that lets you import a .xlsx document, once the document is selected, the function `handleImport` is called.

```ts
const handleImport = (e: React.ChangeEvent<HTMLInputElement>) => {
    //checks whether the user selected a file or not and if he selected multiple files, it selects the first one
    const file = e.target.files?.[0];

    if (file) {
      //a new FileReader object that is used to read the loaded file
      const reader = new FileReader();

      //here we read and extract the first sheet (through sheetName), sheetData and numberOfRows from the imported file
      reader.onload = (event: ProgressEvent<FileReader>) => {
        const workbook = XLSX.read(event.target?.result as string, { type: 'binary' });
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];
        const sheetData: Data[] = XLSX.utils.sheet_to_json(sheet);
        const numberOfRows = sheet['!ref'] ? XLSX.utils.decode_range(sheet['!ref']).e.r + 1 : 0;

        //store the sheetData to be used in the app
        setImportedData(sheetData);
      };

      //this function triggers the file read that we defined in the reader.onload
      reader.readAsBinaryString(file);
    }
  };

```

 
## EXPORT

I have created a button that lets you export data to a .xlsx document, once the button is clicked a `handleExport` function is called.

```ts
const handleExport = () => {
    //here we create a new, empty Excel workbook object
    let wb = XLSX.utils.book_new();

    //here we convert our stored JSON data into a format that represents an Excel worksheet
    let ws = XLSX.utils.json_to_sheet(data);

    //this line adds the worksheet (ws) to the workbook (wb) with the name 'sheet1'
    XLSX.utils.book_append_sheet(wb, ws, 'sheet1');

    //this writes the workbook (wb) to a file named 'export.xlsx' and triggers a download
    XLSX.writeFile(wb, 'export.xlsx');
  };
```




