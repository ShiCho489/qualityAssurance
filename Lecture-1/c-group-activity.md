# Group Activities

## Activity 1

> Discussion about the exam 

## Activity 2: Introduction to unit testing

1. Extract `./src/unit-testing.zip`
2. open the terminal in your vs code and type `npm install`
3. open a split screen terminal with the window button on your terminal
4. next type `npm run test` in one side of your terminal and `npm run start` in the other side
5. Make the necessary changes in `index.js`  to pass all tests

## Activity 3: How run a single test

If some test is failing, it is recommended to only run that test while you are fixing the issue. You can run a single test with the [only](https://jestjs.io/docs/api#testonlyname-fn-timeout) method.

Another way of running a single test (or describe block) is to specify the name of the test to be run with the [-t](https://jestjs.io/docs/en/cli.html) flag:

```js
npm test -- -t 'when ...'
```
