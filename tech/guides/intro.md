# Introduction

This guide sets it's goal to describe all key moment about writing client-side applications. It will include
code examples written is Javascript so we suppose the person who is reading this guides is familiar with JS.

All code snippets are using TokenD Javascript SDK, that facilitates client integration into TokenD 
platform. Complete reference for javascript SDK is available [here][1].

To avoid code duplication, we will suppose this snippet is included into every example listed in guides: 

```js
    import { TokenD, base } from 'tokend-sdk'

    const sdk = await TokenD.create('https://<tokend-backend-url>')

    const horizon = sdk.horizon // the middleware for sending requests to horizon server
    const api = sdk.api // the middleware for sending requests to api server
    const xdr = base.xdr
```

If you prefer not using the Javascript SDK, this guides may not be very helpful for and we can recommend 
you to take a look into our [API reference][2].

[1]: https://tokend.gitlab.io/new-js-sdk
[2]: https://tokend.gitlab.io/docs
