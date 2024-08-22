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