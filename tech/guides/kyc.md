# KYC identification

KYC data format as well as the flow details is a contract between clients and possible integration service implementation. TokenD platform does not impose any limits and provides a versatile infrastructure for your use case.

Below is given a generic flow of how one could implement KYC identification on the platform.

## Key points

KYC verification process starts with choosing right account type to apply for. Different types enable different functionality.

For the compliance reasons, it's not recommended to store raw KYC data on the blockchain directly, that's where [blobs](https://tokend.gitlab.io/docs#blobs) might come in handy.
Blobs allow storing sensitive information securely so that system administrator or integration modules could access it using the provided federation key.

User initiates KYC identification by submitting `UpdateKYCRequest`

```
// TODO make it JS
{
    UpdateKycRequestData: {
        AccountToUpdateKyc:  GD5A75HQRF3W4ZO2E7OKZJH7PFPVIZ2XUANJEWAMJCGVGKVLCBUZ3QDN
	    AccountTypeToSet:    2
        KycLevelToSet:       0
        KycData:             57UOND563OVFKI6BDZU7DBPDZFJJORIGFPEMHZQ44IQLWO4RYUOQ
    }
}
```

A successful transaction will create a reviewable request entity in the ledger for administrator of the system or integration module to review.

```
// TODO JS review request 
```

Approved request will update requestors account type as well as its KYC data on the ledger.
