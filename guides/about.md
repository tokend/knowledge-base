# About

This guide sets it's goal to describe all key moment about writing client-side applications. We recommend you to use the
TokenD Javascript SDK and here we suppose this snippet is included in every example. 

```js
    import { TokenD } from 'tokend-sdk'

    const sdk = await TokenD.create('https://<tokend-backend-url>')

    const base = sdk.base // the module for crafting transactions
    const horizon = sdk.horizon // the middleware for sending requests to horizon server
    const api = sdk.api // the middleware for sending requests to api server
```
