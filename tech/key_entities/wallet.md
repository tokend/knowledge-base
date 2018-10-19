# Wallets

`Wallet` is one of the key entities in TokenD that is used to store 
encrypted user data and provide additional authentication means such as 2FA and email confirmations. Every 
account in TokenD is associated with the `wallet` that stores its keys encrypted
with email and password of the real user.

Wallet is identified by the wallet ID derived from email and password. Wallet ID
is SHA256 HMAC of a `wallet key`, which is used to decrypt account
keys. 

Wallet representation in JSON looks as follows:

```json
{
  "data": {
    "type": "wallet",
    "id": "4aadcd4eb44bb845d828c45dbd68d5d1196c3a182b08cd22f05c56fcf15b153c",
    "attributes": {
      "account_id": "GAIEBMXUPSGW2J5ELJFOY6PR5IWXXJNHIJSDKTDHK76HHRNYRL2QYU4O",
      "email": "booking@mailinator.com",
      "keychain_data": "eyJJViI6IjJESXZJbj...",
      "verified": true,
      "last_sent_at": "2018-04-20T14:29:35.816633Z"
    },
    "relationships": {}
  }
}
```

To read more details about wallet and how to work with them, see [API reference][1]. 
To find out some use cases, see our guides on [How to sign up][2] and 
[How to sign in][3]


[1]: https://tokend.gitlab.io/docs/?http#wallets
[2]: /tech/guides/sign_up.md
[3]: /tech/guides/sign_in.md
