# Operation

[Transactions][1] are made up of a list of operations. Each operation is an 
individual command that mutates the ledger


## Operation thresholds

Each operation falls under a specific threshold category: `low`, `medium`, or 
`high`. Thresholds define the level of privilege an operation needs in order 
to succeed.

* Low Security:
    * Used to allow other signers to allow people to hold credit from 
    this account but not issue credit.
* Medium Security:
    * All else.
* High Security:
    * SetOptions.

[1]: /tech/key_entities/transaction.md
