## How to get an associated token account on Solana

### Background

Each wallet can correspond to ownership over many types of tokens, but in Solana each token is actually held by an <a href="https://spl.solana.com/associated-token-account" target="_blank">associated token account</a> (ATA). An ATA is an account specific for a token owned by the wallet. When you transfer an SPL token, such as Serum, or transfer an NFT, you're transferring from an ATA you own to another person's ATA for that specific token.

### How To

To derive the endpoint, we will use <a href="https://theblockchainapi.com/" target="_blank">the Blockchain API</a>. Specifically, we will use the <a target="_blank" href="https://docs.theblockchainapi.com/#tag/Solana-Wallet/paths/~1v1~1solana~1wallet~1associated_token_account/post">"Derive an associated token account address" endpoint</a>.
With this endpoint, you can derive an associated token address given a wallet and a token address.

### Step 1: Get a free API key pair

First get a free API key pair <a target="_blank" href="https://dashboard.theblockchainapi.com/api-keys?blog=subdomain-get-ata">here</a>.

### Step 2: Write and send the API request

We will use Python's request library to format the request.

Let's find the associated for Serum token.

Note that associated token accounts are specific for a given combination of seed phrase, derivation path, and passphrase. You can read more about this here.

```
import requests

SERUM_TOKEN_ADDRESS = "SRMuApVNdxXokk5GT7XD5cUUgXMBCoAz2LHeuAoKWRt"
TEST_PHRASE = "fire owner display success half rescue pledge oval foam gossip window once"
DERIVATION_PATH = "m/44/501/0/0"

response = requests.post(
    SOLANA_WALLET_ATA_ENDPOINT,
    params={
        "secret_recovery_phrase": TEST_PHRASE,
        "derivation_path": DERIVATION_PATH,
        "token_address": SERUM_TOKEN_ADDRESS,
        "network": "mainnet-beta"
    },
    headers={
        "APISecretKey": "YOUR API SECRET KEY",
        "APIKeyId": "YOUR KEY ID"
    }
)
```

The output will look something like this:

```
{
    'associated_token_address': 'CYbSDBihm9k2M45G8sfEkWta2ifCf1WgbPsiVcbjSNN9'
}
```

Any issues or feature requests? <a target="_blank" href="https://dashboard.theblockchainapi.com/contact">Contact us</a> or email us at info [[at]] theblockchainapi.com.

### [Optional] Step 3: Use the Blockchain API's Python wrapper instead

You can also use our <a target="_blank" href="https://pypi.org/project/theblockchainapi/">Python PyPi package</a>.

``` pip install theblockchainapi ```

Now set up the resource using your key pair.

```
from theblockchainapi import TheBlockchainAPIResource, SolanaNetwork

BLOCKCHAIN_API_RESOURCE = TheBlockchainAPIResource(
    api_key_id='MY_API_KEY_ID',
    api_secret_key='MY_API_SECRET_KEY'
)
```

Now, write the API call.

```
SERUM_TOKEN_ADDRESS = "SRMuApVNdxXokk5GT7XD5cUUgXMBCoAz2LHeuAoKWRt"

TEST_PHRASE = "fire owner display success half rescue pledge oval foam gossip window once"
DERIVATION_PATH = "m/44/501/0/0"

associated_token_address = BLOCKCHAIN_API_RESOURCE.derive_associated_token_account_address(
    token_address=SERUM_TOKEN_ADDRESS,
    secret_recovery_phrase=TEST_PHRASE,
    derivation_path=DERIVATION_PATH,
    network=SolanaNetwork.MAINNET_BETA
)

print(associated_token_address)
```
The output will look something like this:

```
CYbSDBihm9k2M45G8sfEkWta2ifCf1WgbPsiVcbjSNN9
```