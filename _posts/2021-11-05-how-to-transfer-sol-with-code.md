## How to transfer SOL with code

Need to programmatically transfer SOL? <a target="_blank" href="https://theblockchainapi.com">The Blockchain API</a> has a <a target="_blank" href="https://docs.theblockchainapi.com/#tag/Solana-Wallet/paths/~1v1~1solana~1wallet~1transfer/post">"Transfer SOL, a token, or an NFT to another address" API endpoint</a>. This will enable you to transfer an asset on the Solana blockchain reliably and easily in any coding language.

### Step 1: Get a free API key pair

First, get a free API key pair <a target="_blank" href="https://dashboard.theblockchainapi.com/api-keys?blog=subdomain-transfer-sol">here</a>.

### Step 2: Write the Transfer SOL request

If you're going to use Python, I highly suggest going to Step 3. The purpose of Step 2 is to show people using other languages how to write the raw API requests.

We will implement the transfer request using Python's requests library.

To demonstrate the transfer endpoint, we create a wallet, airdrop some SOL to it, and then transfer that SOL.

```
import requests
```

Fill in your API key pair.

```
MY_API_KEY_ID = "YOUR API KEY ID"
MY_API_SECRET_KEY = "YOUR API SECRET KEY"
HEADERS = {
    'APIKeyID': MY_API_KEY_ID,
    'APISecretKey': MY_API_SECRET_KEY
}
```

First, let's define some of the API endpoints we'll use.

```
SECRET_PHRASE_ENDPOINT = "https://api.theblockchainapi.com/v1/solana/wallet/secret_recovery_phrase"
PUBLIC_KEY_ENDPOINT = "https://api.theblockchainapi.com/v1/solana/wallet/public_key"
BALANCE_ENDPOINT = "https://api.theblockchainapi.com/v1/solana/wallet/balance"
AIRDROP_ENDPOINT = "https://api.theblockchainapi.com/v1/solana/wallet/airdrop"
TRANSFER_ENDPOINT = "https://api.theblockchainapi.com/v1/solana/wallet/transfer"
```

Let's create a test wallet instead of using your real wallet. To do so, we first create a secret key.

```
response = requests.post(
    SECRET_PHRASE_ENDPOINT,
    headers=HEADERS
)
secret_recovery_phrase = response.json()['secret_recovery_phrase']
print(secret_recovery_phrase)
```

The output should be:
```
escape glow bracket drop clip divert sentence minor actual mansion kangaroo entire
```

Now, let's generate a public key from this phrase. Note that a public key is made up of a secret phrase (generated above), a derivation path, and a passphrase. We will use the API's default derivation path and passphrase. So, we won't provide the values for these in our request. You can read more about derivation paths on Solana <a target="_blank" href="https://blog.theblockchainapi.com/2021/10/10/derivation-paths-on-solana.html">here</a>.

```
response = requests.post(
    PUBLIC_KEY_ENDPOINT,
    params={
        'secret_recovery_phrase': secret_recovery_phrase
    },
    headers=HEADERS
)
public_key = response.json()['public_key']
print(public_key)
```

The output:

```
J4xbgpJ35dRo9otThGxDwh4yDVaUGL8B4f7LZUDJ1qSH
```

Let's now get an airdrop 0.015 SOL. We will transfer this later.

```
response = requests.post(
    AIRDROP_ENDPOINT,
    params={
        'recipient_address': public_key
    },
    headers=HEADERS
)
transaction_signature = response.json()['transaction_signature']
print(transaction_signature)
```

The output will look something like this, representing a transaction signature for the transaction of the airdrop on the devnet.

```
5hLcZA4kZhc83iz34tSKvatLMz6JySzmL72J25muS5iT68SUxPZt98acYepY38bTx4pKdRXPnaLGeE4zZvYFgu5q
```

Let's write a `get_balance()` function to check the balance after the airdrop. We'll use this twice.

```
def get_balance():
    response = requests.get(
        BALANCE_ENDPOINT,
        params={
            'public_key': public_key,
            'network': 'devnet',
            'unit': 'sol'
        },
        headers=HEADERS
    )
    print(f"Balance: {response.json()['balance']}")

# Get the balance after the airdrop
get_balance()
```

The output in SOL:

```
Balance: 0.015
```

First, let's calculate the amount to transfer. Note that the transaction fee is 0.000005 SOL.

```
airdrop_amount = 0.015
transfer_fee = 0.000005
amount_to_send = airdrop_amount - transfer_fee
print(amount_to_send)
```
Output:

```
0.014995
```

Second, determine who you want to transfer to.

```
# Transfer to this address
transfer_to = "31LKs39pjT5oj6fWjC3F76dHWf9489asCthmgj8wu7pj"
```

Using this, we can call the transfer endpoint.

```
response = requests.post(
    TRANSFER_ENDPOINT,
    params={
        'secret_recovery_phrase': secret_recovery_phrase,
        'recipient_address': transfer_to,
        'amount': str(amount_to_send)
    },
    headers=HEADERS
)
transaction_signature = response.json()['transaction_signature']
print(transaction_signature)
```

Output:

```
5q8eiad36twBcekvHo2BGoUPbjeh1UbhiRjSPtfcB3eghXdenTTTuqF5PBfuJM9Auq17uqNwZj9KEpnHVRKeFQqt
```

To confirm that the transaction went through, let's check our balance again.

```
get_balance()
```

Output:

```
Balance: 0.0
```

Great! Nothing like a 0 balance.

If you have any questions, feature requests, or issues, don't hesitate to reach out via email (info [[at]] theblockchainapi.com) or <a target="_blank" href="https://dashboard.theblockchainapi.com/contact">another method</a>.

### [Optional] Step 3: Use the Blockchain API's Python wrapper to transfer SOL

Instead, you can use the Blockchain API's Python wrapper to transfer SOL. This can make development easier instead of having to rewrite the functions we already wrapped using the requests library. To get started, install our <a target="_blank" href="https://pypi.org/project/theblockchainapi/">PyPi package</a>.

```
pip install theblockchainapi
```

Initialize the resource with your API key pair.

```
MY_API_KEY_ID = "YOUR API KEY ID"
MY_API_SECRET_KEY = "YOUR API SECRET KEY"
BLOCKCHAIN_API_RESOURCE = TheBlockchainAPIResource(
    api_key_id=MY_API_KEY_ID,
    api_secret_key=MY_API_SECRET_KEY
)
```

Generate a secret key and public key for testing purposes.

```
# Create a wallet
secret_key = BLOCKCHAIN_API_RESOURCE.generate_secret_key()
public_key = BLOCKCHAIN_API_RESOURCE.derive_public_key(
    secret_recovery_phrase=secret_key
)
print(f"Public Key: {public_key}")
print(f"Secret Key: {secret_key}")
```

The output will look something like this:

```
Public Key: 8vTgbAEVnh2RAP85njYmcSUAjYCBqbkZ638VsgJYbCve
Secret Key: tattoo step foot essence amused marble slim access fiction lamp dawn talent
```

Get an airdrop of SOL on the devnet so that we can transfer it.

```
BLOCKCHAIN_API_RESOURCE.get_airdrop(public_key)
```

Wait for 20 or so seconds so that the transfer can be fully confirmed on the Solana blockchain. Then you can transfer the SOL.

```
import time
time.sleep(25)
```

Now check your balance!

```
def get_balance():
    balance_result = BLOCKCHAIN_API_RESOURCE.get_balance(
        public_key,
        unit=SolanaCurrencyUnit.SOL,
        network=SolanaNetwork.DEVNET
    )
    print(f"Balance: {balance_result['balance']}")

# Get the balance after the airdrop
get_balance()
```

The output should be:

```
Balance: 0.015
```

We will now transfer about 0.015 SOL from our address to another.

First, let's calculate the amount to transfer. Note that the transaction fee is 0.000005 SOL.

```
# This is the amount we received in SOL from the Airdrop
airdrop_amount = 0.015
transfer_fee = 0.000005
# We cannot send `airdrop_amount` because we need to pay for the transaction, and so we wouldn't have enough to
# pay for the transaction because our balance would be `0` after sending `airdrop_amount` yet we still need to cover
# the transaction.
amount_to_send = str(airdrop_amount - transfer_fee)
print(amount_to_send)
```

Output:

```
0.014995
```

Determine the public key address to which you want to send the SOL.

```
# Transfer to this address
transfer_to = "31LKs39pjT5oj6fWjC3F76dHWf9489asCthmgj8wu7pj"
```

Fill in the transfer function and print out the signature.

```
# Call the transfer endpoint
transaction_signature = BLOCKCHAIN_API_RESOURCE.transfer(
    secret_recovery_phrase=secret_key,
    recipient_address=transfer_to,
    amount=amount_to_send,
    network=SolanaNetwork.DEVNET
)
print(transaction_signature)
```

The output should be:

```
5pWrB5SKoSAAfFqVFvvhyoUnxJGdY9gBQtuEbevVRgEHWybKGjBb7HfJXJVMM8xSijJ56EdoSohDGtmxzjuQpW46
```

You can view this transaction on the Solana blockchain here: https://explorer.solana.com/tx/5pWrB5SKoSAAfFqVFvvhyoUnxJGdY9gBQtuEbevVRgEHWybKGjBb7HfJXJVMM8xSijJ56EdoSohDGtmxzjuQpW46?cluster=devnet

Now check your balance. You should have 0 SOL.

```
get_balance()
```

The output:

```
Balance: 0.0
```

Great! Nothing like a 0 balance.

If you have any questions, feature requests, or issues, don't hesitate to reach out via email (info [[at]] theblockchainapi.com) or <a target="_blank" href="https://dashboard.theblockchainapi.com/contact">another method</a>.
