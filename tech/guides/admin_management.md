## Add/Remove admin
Let's suppose Charlie is the admin of the TokenD system. The project is growing and the process of review user's requests taking a while. In order to reduce Charlie's effort and time, he could share his resposibilities to assistant Dan.

## Add
Adding new admin process starts with Dan signing up in system as admin and passing Charlie his [public key](/technical-details/key-entities/accounts#account-id). After that Dan will looking forward for Charlie's response by submitting `getSignerById` request to server

       async function getSignerById () {
          const publicKey = 'GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636' // Dan's public key
          const operation = base.ScopedServerCallBuilder.accounts().signer(masterAccountId, publicKey)
    ​
          await horizon.transactions.submitOperations(operation)
        }

On Charlie's side, she should add to system new signer with Dan's **public key, name, identity, weight** and **signer type**

Identity - weights of admins with the same identities are not being summed up in case of multi-signature
Weight - weight determines how important admin’s sign is
Signer type - desired access policies, here's the list of [signer types](/technical-details/key-entities/signer#signer-types)

       async function addAdmin () {
          const signer = {
            pubKey = 'GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636' // Dan's public key
            weight: 2,
            signerType: 524288, // Withdraw requests reviewer signer type
            identity: 2,
            name: 'Dan'
        }
         const source = 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE', // Charlie's account ID
         const operation = base.SetOptionsBuilder.setOptions(signer, source)
    ​
          await horizon.transactions.submitOperations(operation)
        }

## Remove

If Dan's help will be redundant, Charlie can remove Dan by setting  **masterWeight** to **0**

    async function removeAdmin () {
         const operation = base.SetOptionsBuilder.setOptions({ masterWeight: 0 })
    ​
          await horizon.transactions.submitOperations(operation)
        }
