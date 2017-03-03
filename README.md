# [TypeScript FSA](https://github.com/aikoven/typescript-fsa) utilities for redux-saga [![npm version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url]
 
## Installation

```
npm install --save typescript-fsa-redux-saga
```

## API

### `bindAsyncAction(actionCreators: AsyncActionCreators): HigherOrderSaga`

Creates higher-order-saga that wraps target saga with async actions.
Resulting saga dispatches `started` action once started and `done`/`failed`
upon finish.

**Example:**

```ts
// actions.ts
import actionCreatorFactory from 'typescript-fsa';

const actionCreator = actionCreatorFactory();

// specify parameters and result shapes as generic type arguments
export const doSomething = 
  actionCreator.async<{foo: string},   // parameter type
                      {bar: number}    // result type
                     >('DO_SOMETHING');
                      
// saga.ts
import {SagaIterator} from 'redux-saga';
import {call} from 'redux-saga/effects';
import {doSomething} from './actions';
                      
const doSomethingWorker = bindAsyncAction(doSomething)(
  function* (params): SagaIterator {
    // `params` type is `{foo: string}`
    const bar = yield call(fetchSomething, params.foo);
    return {bar};
  },       
);        
              
function* mySaga(): SagaIterator {
  yield call(doSomethingWorker, {foo: 'lol'});
}              
```

[npm-image]: https://badge.fury.io/js/typescript-fsa-redux-saga.svg
[npm-url]: https://badge.fury.io/js/typescript-fsa-redux-saga
[travis-image]: https://travis-ci.org/aikoven/typescript-fsa-redux-saga.svg?branch=master
[travis-url]: https://travis-ci.org/aikoven/typescript-fsa-redux-saga
