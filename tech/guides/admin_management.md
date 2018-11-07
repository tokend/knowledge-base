## Add/Remove admin
Let's suppose Charles is the admin of the TokenD system. The project is growing and the process of review user's requests taking a while. In order to reduce Charles' effort and time, he could share his resposibilities to assistant Delta.

## Add
Adding new admin process starts with Delta signing up in system as admin and passing Charles his public key. After that Delta will looking forward for Charles response by submitting `getSignerById` request to server

       async function getSignerById () {
          const publicKey = 'GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636' // Delta's public key
          const operation = base.ScopedServerCallBuilder.accounts().signer(masterAccountId, publicKey)
    ​
          await horizon.transactions.submitOperations(operation)
        }

On Charles's side, she should add to system new signer with Delta's **public key, name, identity, weight** and **signer type**

Identity - weights of admins with the same identities are not being summed up in case of multi-signature
Weight - weight determines how important admin’s sign is
Signer type - desired access policies

       async function addAdmin () {
          const signer = {
            pubKey = 'GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636' // Delta's public key
            weight: 2,
            signerType: 524288, // Withdraw requests reviewer signer type
            identity: 2,
            name: 'Delta'
        }
         const source = 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE', // Charles's account ID
         const operation = base.SetOptionsBuilder.setOptions(signer, source)
    ​
          await horizon.transactions.submitOperations(operation)
        }

## Remove

If Delta's help will be redundant, Charles can remove Delta by setting  **masterWeight** to **0**

    async function removeAdmin () {
         const operation = base.SetOptionsBuilder.setOptions({ masterWeight: 0 })
    ​
          await horizon.transactions.submitOperations(operation)
        }
