# MobX

MobX is a simple, scalable and battle tested **state management** JS library.

- **state** - objects, arrays, primitives, references
- **actions** - functions that alter the state
- **derivations** - any value that can be computed automatically from the state - usually a **getter function**
- **reactions** - functions that don't produce a value; they run **automatically** to perform some side effect - e.g.  making API calls, logging and debugging, updating UI etc.

## Installation and basic setup

Install MobX:
```bash
npm install mobx mobx-react-lite
```

I'll be using this folder structure:
```scss
src/
  store/
    counterStore.ts       // Store configuration
```

Create store - I'll be creating a simple counter - `counterStore.ts`:
```ts
import { makeObservable, observable, action } from 'mobx';

class CounterStore {
  count: number = 0;

  constructor () {
    makeObservable(this, {
      count: observable,
      increment: action,
      decrement: action,
    });
  }

  increment() {
    this.count += 1;
  }

  decrement() {
    this.count -= 1;
  }
}

const counterStore = new CounterStore();
export default counterStore;
```

## Usage - accessing and updating stored data

Use `observer` to watch the state changes:
```ts
import { observer } from 'mobx-react-lite';
import counterStore from './store/counterStore';

export const Counter = observer(() => {

  return (
    <>
      <h2>{counterStore.count}</h2>
      <button onClick={() => counterStore.increment()}>+</button>
      <button onClick={() => counterStore.decrement()}>-</button>
    </>
  );
});
```

## Derivations

Derivations produce any value that can be computed automatically from the state - usually a **getter function**:
```ts
import {  makeObservable, observable, action } from "mobx";

class CounterStore {
    count: number = 0;

    constructor() {
        makeObservable(this, {
            count: observable,
        });
    };

    ...

    //DERIVATION
    get isEven() {
        return this.count % 2 === 0;
    };
};

const counterStore = new CounterStore();
export default counterStore;
```

Using derivation:
```ts
import { observer } from 'mobx-react-lite';
import counterStore from './store/counterStore';

export const isEven = observer(() => {

  return (
    <>
      <h2>{counterStore.isEven}</h2>
    </>
  );
});


## Reactions

Functions that don't produce a value; they run **automatically** to perform some side effect - e.g.  making API calls, logging and debugging, updating UI etc.

`autorun`
  - **Key Feature**: runs immediately when created and every time any observed data changes
  - **Use Case**: ideal for performing side effects when multiple observables change, without needing to specify exactly which observables

`reaction`
  - **Key Feature**: tracks a specific observable expression and reacts when it changes. Unlike autorun, it does not run immediately
  - **Use Case**: useful when you want to control what to track and run a side effect only when a specific observable changes

```ts
import { makeObservable, observable, action, reaction, autorun } from 'mobx';

class CounterStore {
  count: number = 0;

  constructor() {
    makeObservable(this, {
      count: observable,
      increment: action,
      decrement: action, 
    });

    autorun(() => {
      if (!this.isEven) {
        console.log('reacted - is odd');
      }
    });

    reaction(
      () => this.count,
      () => {
        if (this.count === 5) {
          console.log('reacted - count is 5');
        }
      }
    );
  }

  ...
}

const counterStore = new CounterStore();
export default counterStore;
```
