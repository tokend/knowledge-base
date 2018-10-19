# XDR

XDR, also known as External Data Representation, is applied in the TokenD-based platform. 
The ledger, transactions, results, history, and even the messages transferred between the nodes that run the 
TokenD-core are encoded using XDR.

XDR is specified in [RFC 4506][1] and is similar to such tools as Protocol Buffers or Thrift. XDR provides for multiple important features:

* It is very compact so can be transmitted quickly and stored with minimal disk space.
* Data encoded in XDR is reliably and predictably stored. Fields are always in the same order, 
which makes signing and verifying XDR messages cryptographically simple.
* XDR definitions include rich descriptions of data types and structures, which is not possible 
in simpler formats such as JSON, TOML, or YAML.
* Since XDR is a binary format and not as widely known as simpler formats like JSON, all TokenD's
SDKs include tools for parsing XDR and will do it automatically when retrieving data.

In addition, the TokenD API server generally exposes the most important parts of the XDR data in JSON, so 
they are easier to parse if you are not using an SDK. The XDR data is still included (encoded as a base64 string) 
in the JSON in case you need a direct access to it.

In most cases, when building a client-side part of an application, you will only need the values of XDR enums
that are exposed by javascript SDK's.

## Examples

XDR representation of the [Payment][3] operation:

```
struct PaymentOpV2
{
    BalanceID sourceBalanceID;

    union switch (PaymentDestinationType type) {
        case ACCOUNT:
            AccountID accountID;
        case BALANCE:
            BalanceID balanceID;
    } destination;

    uint64 amount;

    PaymentFeeDataV2 feeData;

    longstring subject;
    longstring reference;

    // reserved for future use
    union switch (LedgerVersion v)
    {
        case EMPTY_VERSION:
            void;
        }
    ext;
};
```

XDR representation of the [Transaction][4]:

```
struct Transaction
{
    AccountID sourceAccount; //Source account

    Salt salt; //Salt

    TimeBounds timeBounds; //Time Bounds

    Memo memo; //Memo

    Operation operations<100>; //List of operations. A transaction can have up to 100 operations

    // reserved for future use
    union switch (LedgerVersion v)
    {
    case EMPTY_VERSION:
        void;
    }
    ext;
};
```

XDR representation of the FeeType:

```
enum FeeType
{
    PAYMENT_FEE = 0,
    OFFER_FEE = 1,
    WITHDRAWAL_FEE = 2,
    ISSUANCE_FEE = 3
};
```

XDR representation of the [AccountType][5]:

```
enum AccountType
{
  OPERATIONAL = 1,       // Operational account for system
  GENERAL = 2,           // General account that can perform payments, setoptions, be source account for tx, etc
  COMMISSION = 3,        // Commission account
  MASTER = 4,            // Master account
  NOT_VERIFIED = 5,
  SYNDICATE = 6,         // Can create asset
  EXCHANGE = 7
};
```

[1]: https://tools.ietf.org/html/rfc4506.html
[3]: /tech/operations/payment.md
[4]: /tech/key_entities/transaction.md
[5]: /tech/key_entities/accounts.md#account-type
