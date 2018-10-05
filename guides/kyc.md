# KYC identification

KYC data format as well as flow details is contract between clients and possible integration service implementation. TokenD platform does not impose any limits and provides versatile infrastructure for your use case.

Below is given a generic flow of how one could implement KYC identification on the platform.

## Key points

KYC verification process starts with choosing right account type to apply for. Different types enables different functionality.

For compliance reasons it's not recommended to store raw KYC data in blockchain directly, that's where [blobs](https://tokend.gitlab.io/docs#blobs) might come in handy.
Blobs allow to store sensitive information securely, so system administrator or integration modules could access it using provided federation key.

User initiates KYC identification with submitting `UpdateKYCRequest`

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

Successful transaction will create reviewable request entity in the ledger for administrator of the system or integration module to review.

```
// TODO JS review request 
```

Approved request will update requestors account type as well as it's KYC data on the ledger.
