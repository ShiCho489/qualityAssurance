# Group Activity

## Activity 1

- Extract `part4-4.zip`
- run `npm install` and `npm test`
- Explain the following snippet:
```js
beforeEach(async () => {
  await Note.deleteMany({})
  let noteObject = new Note(helper.initialNotes[0])
  await noteObject.save()
  noteObject = new Note(helper.initialNotes[1])
  await noteObject.save()
})
```
- Explain the following snippet:
```js
afterAll(() => {
  mongoose.connection.close()
})
```

- Explain the following snippet:
```js
const api = supertest(app)
//...

test('notes are returned as json', async () => {
  await api
    .get('/api/notes')
    .expect(200)
    .expect('Content-Type', /application\/json/)
})
```

## Activity 2
Discuss: 
- Difference between TDD and BDD.
- Difference between it and test. You can check the [Jest](https://jestjs.io/docs/api#testname-fn-timeout) docs.
- How to run single tests?
- What is integration testing?

## Activity 3: Run single tests

The npm test command executes all of the tests for the application. When we are writing tests, it is usually wise to only execute one or two tests. Jest offers a few different ways of accomplishing this, one of which is the only method. If tests are written across many files, this method is not great.

A better option is to specify the tests that need to be run as parameters of the npm test command.

The following command only runs the tests found in the `tests/note_api.test`.js file:

```js
npm test -- tests/note_api.test.js
```

The `-t` option can be used for running tests with a specific name:

```js
npm test -- -t "a specific note is within the returned notes"
```

The provided parameter can refer to the name of the test or the describe block. The parameter can also contain just a part of the name. The following command will run all of the tests that contain notes in their name:

```js
npm test -- -t 'notes'
```

> When running a single test, the mongoose connection might stay open if no tests using the connection are run. The problem might be due to the fact that supertest primes the connection, but Jest does not run the `afterAll` portion of the code. 

