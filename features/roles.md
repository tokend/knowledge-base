# Roles

In order to reduce the risk of fraud and collusion, different types of actions must be performed by different (sometimes independent) entities. Therefore, there is a set of roles (in terms of interaction with the system) that TokenD supports:

* *Administrator* makes decisions on configuring the system and setting business rules, performs verifications of the user and grants rights to perform certain actions.
* *User* can perform set of actions defined by the administrator.

## Administrator

Administrator is the main entity which defines rules of the system (if trading of certain asset pair is allowed, what is flow of deposit or withdrawal of the tokens) and grants permissions to users (are users allowed to create their own tokens, are users allowed to interact with token). To begin system operations it's enough to have only one *Super Administrator*, but as number of users and value of tokenized assets grows it is crucial to split rights among several administrators to ensure best user experience anh highest level of security.
On technical level administrator is signer of special account type *Master* and it's possible to precisely define which actions admin can perform. Usually TokenD based solutions has following administrator types:

* Super Administrator (Owner of the system) - can do all actions.
* KYC administrator – responsible for the KYC requests review/acceptance/rejection.
* Fees administrator – responsible for the management of fees imposed on an asset, account type or a specific account.
* Limits administrator – responsible for the management of the daily, weekly, monthly, annual limits imposed on an account type or a specific account.
* Trade administrator – responsible for the asset pairs management.
* Crowdfunding administrator – responsible for the crowdfunding campaigns requests review/acceptance/rejection.
* Security administrator – responsible for ban/unban of existing users.
* Pre-issuance administrator – responsible for the pre-issuance management.
* Issuance administrator – responsible for the issuance management.
