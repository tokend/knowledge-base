# Transaction

Transaction is a joined sequence of commands (operations) to modify the state of TokenD's database. All 
actions in the TokenD network are represented by corresponding transactions.
For example, transactions are used to send payments, manage orders, change
account settings, etc. Transactions in TokenD's, blockchain-based database are similar to SQL commands and transactions in traditional databases.

## Transaction attributes

Each transaction has the following attributes:

* Source account
* Salt
* List of operations
* Time bounds (optional)
* Memo (optional)

### Source account

This is the account that initiated the transaction. The transaction 
must be signed by this account.

### Salt
Any 64-bit unsigned integer that ensures the uniqueness of each transaction. 
Usually, it is a random number.

### List of operations

Transactions contain an arbitrary list of [operations][3] inside of them. Generally, 
there is just one operation, however TokenD allows you to have up to 100 operations 
in one transaction. Operations are executed orderly as one [ACID][1] 
transaction, meaning either all operations are performed or none of them.
Thus, if any operation from the list fails, the whole transaction fails as well.

### Time bounds

The [UNIX][2] timestamps are determined by the ledger time. It contains `minTime` and 
`maxTime` attributes that means a lower and upper bound, respectively, of 
when this transaction will be valid. If a transaction is submitted too 
early or too late, it will fail to make it into the transaction set. 
`maxTime` equals `0` means that it’s not set.

> Note: there can be a situation when the local time of the client has 
been changed manually. Therefore, to form the time bounds, it is better to 
use the current TokenD server time received by a special request.

### Memo

The memo contains optional extra information. It is the responsibility of the 
client to interpret this value. Memos can be one of the following types:

* `MEMO_TEXT`: A string up to 28-bytes long.
* `MEMO_ID`: A 64-bit unsigned integer.
* `MEMO_HASH`: A 32-byte hash.
* `MEMO_RETURN`: A 32-byte hash intended to be interpreted as the hash of the 
transaction the sender is refunding.


[1]: https://en.wikipedia.org/wiki/ACID_(computer_science)
[2]: https://en.wikipedia.org/wiki/Unix_time
[3]: /tech/key_entities/operation.md
