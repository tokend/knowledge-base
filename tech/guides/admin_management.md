## Add/Remove admin
Imagine, TokenD's user data has grown, Alice's process of review user's requests would take a while. In order to reduce Alice's effort and time, she could share her resposibilities to assistant Bob.

## Add
Adding new admin process starts with Bob signing up in system as admin and passing Alice his public key. After that Bob will looking forward for Alice response by submitting `getSignerById` request to server

       async function getSignerById () {
          const bobPublicKey = 'GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636'
          const operation = base.ScopedServerCallBuilder.accounts().signer(masterAccountId, bobPublicKey)
    ​
          await horizon.transactions.submitOperations(operation)
        }

On Alice's side, she should approve an admin account creation request as adding to system new signer with Bob's **public key, name, identity, weight** and **signer type**

Identity - weights of admins with the same identities are not being summed up in case of multi-signature
Weight - weight determines how important admin’s sign is
Signer type - desired access policies

       async function addAdmin () {
          const signer = {
            pubKey = 'GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636' // Bob's public key
            weight: 2,
            signerType: 524288, // Withdraw requests reviewer signer type
            identity: 2,
            name: 'Bob'
        }
         const source = 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE', // Alice's account ID
         const operation = base.SetOptionsBuilder.setOptions(signer, source)
    ​
          await horizon.transactions.submitOperations(operation)
        }

## Remove

If Bob's help will be redundant, Alice can remove Bob by setting  **identity, weight** and **signer type** to **0**

    async function removeAdmin () {
          const signer = {
            pubKey = 'GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636' // Bob's public key
            weight: 0,
            signerType: 0,
            identity: 0,
        }
         const source = 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE', // Alice's account ID
         const operation = base.SetOptionsBuilder.setOptions(signer, source)
    ​
          await horizon.transactions.submitOperations(operation)
        }

## Key-server

Key-server is a module which stores client-side-encrypted private keys of users and admins. This prevents a malicious actor from getting access to the accounts of the system even having a full access to the storage.