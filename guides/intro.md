# Introduction

The goal of this guide is to to describe the key moments about writing client-side applications. It will include code examples written in Javascript \(so we suppose that reader is familiar with JS\).

All code snippets use the TokenD Javascript SDK that facilitates client integration into the TokenD platform. Complete reference for javascript SDK is available [here](https://tokend.gitlab.io/new-js-sdk).

In order to avoid code duplication, we suppose that this snippet is included into every example listed in subsequent guides:

```javascript
    import { TokenD, base } from 'tokend-sdk'

    const sdk = await TokenD.create('https://<tokend-backend-url>')

    const horizon = sdk.horizon // the middleware for sending requests to horizon server
    const api = sdk.api // the middleware for sending requests to api server
    const xdr = base.xdr
```

If you prefer not using the Javascript SDK, these guides may not be very helpful for you and we recommend you to take a look into our [API reference](https://tokend.gitlab.io/docs).

