# Intercept HTTP request

```
npm i -D undici
```

```js
import { describe, test, before, after, beforeEach } from 'node:test'
import assert from 'node:assert'
import { Auth } from '../index.js'
import { Agent, MockAgent,  setGlobalDispatcher } from 'undici'

let mockAgent = new MockAgent()
let mockPool

// Initialize interceptor
before(() => {
  mockAgent = new MockAgent()
  mockAgent.disableNetConnect();
  setGlobalDispatcher(mockAgent);

})

after(async () => {
  await mockAgent.close();
  setGlobalDispatcher(new Agent());
});


beforeEach(() => {
  const fixtureContent = readFileSync(resolve(__DIRNAME_AUTHENTICATION, 'tests/fixtures/oidc.json')).toString()
  mockPool = mockAgent.get('https://host-to-intercept.com')
  mockPool
    .intercept({
      path: '/api',
      method: 'GET'
    })
    .reply(200, {
      "message": "I'm a fake message"
    })
})

describe('Auth', () => {
    test('Default Authorization url', async () => {
      const auth = new Auth()
      const result = await auth.getMessage()
      const expected = 'I'm a fake message'

      assert.equal(result.message, expected)
    })
})
```
