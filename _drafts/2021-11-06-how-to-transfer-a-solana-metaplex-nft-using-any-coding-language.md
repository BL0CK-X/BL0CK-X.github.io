## How to transfer a Solana Metaplex NFT with any coding language

Need to programmatically transfer an NFT? The Blockchain API has a <a target="_blank" href="https://docs.theblockchainapi.com/#tag/Solana-Wallet/paths/~1v1~1solana~1wallet~1transfer/post">"Transfer SOL, a token, or an NFT to another address" API endpoint</a>. This will enable you to transfer an asset on the Solana blockchain reliably and easily in any coding language.

### Step 1: Get a free API key pair

First, get a free API key pair <a target="_blank" href="https://dashboard.theblockchainapi.com/api-keys?blog=subdomain-transfer-nft">here</a>.

### Step 2: Write the Transfer NFT request

If you're going to use Python, I highly suggest skipping to Step 3. The purpose of Step 2 is to show people using other languages how to write the raw API requests.

We will implement the transfer request using Python's requests library.

To demonstrate the transfer endpoint, we create a wallet, create an NFT, and then transfer that NFT.

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
CREATE_NFT_ENDPOINT = "https://api.theblockchainapi.com/v1/solana/nft"
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
home man chat enroll nice limit refuse base foam reduce gossip chief
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
CtE7ofpQqEvf5WZX2R3P2V2pSJ6r1fjpsgb3Aq1KNUUV
```

Let's now get an airdrop 0.015 SOL. This is just enough to mint an NFT and then transfer it.

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
zHgk845Bq1GAZDVoUbvWPfQtNe98anH2Ayk72wj2UAy1XmJbtWQHCsFzqHfMWWAWLNHjFLppxe8gooRYWUm4hjE
```

Let's write a `create_nft()` function to create an NFT so that we can transfer it. This will take about 60 seconds to mint.

```
def create_nft():
    response = requests.post(
        CREATE_NFT_ENDPOINT,
    params={
        'secret_recovery_phrase': secret_recovery_phrase,
        'network': 'devnet',
        'nft_name': "The Blockchain API",
        'nft_symbol': "BLOCKX",
        'nft_url':
            "https://pbs.twimg.com/profile_images/1441903262509142018/_8mjWhho_400x400.jpg"
        },
        headers=HEADERS
    )
    print(response.json())
    mint_address = response.json()['mint']
    print(f"NFT address: {mint_address}")
    return mint_address
```

Now call the function.

```
mint_address = create_nft()
```

The output:

```
NFT address: {
    'update_authority': 'CtE7ofpQqEvf5WZX2R3P2V2pSJ6r1fjpsgb3Aq1KNUUV',
    'mint': 'DnUVmi4ydP9XEnZhdkrWc25dFixSiCxWmBTjyRz3GpKL',
    'data': {
        'name': 'The Blockchain API',
        'symbol': 'BLOCKX',
        'uri': 'https://blockx-api-storage.s3.amazonaws.com/83055279955/data',     
        'seller_fee_basis_points': 0.0,
        'creators': [
            'CtE7ofpQqEvf5WZX2R3P2V2pSJ6r1fjpsgb3Aq1KNUUV'
        ],
        'verified': [1.0],
        'share': [100.0]
    },
    'primary_sale_happened': False,
    'is_mutable': True,
    'explorer_url': 'https://explorer.solana.com/address/DnUVmi4ydP9XEnZhdkrWc25dFixSiCxWmBTjyRz3GpKL?cluster=devnet',
    'mint_secret_recovery_phrase': 'supreme define elbow focus review gate hood enact ripple bind banner fence'
}
NFT address: DnUVmi4ydP9XEnZhdkrWc25dFixSiCxWmBTjyRz3GpKL
```

We can now transfer the NFT. Use your own public key address or the one provided.

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
        'token_address': mint_address
    },
    headers=HEADERS
)
transaction_signature = response.json()['transaction_signature']
print(transaction_signature)
```

Output:

```
61Wpy1aJLEeYh3ZPAiqmkMMdhRA3UKvs3WAhPyFhgCENRuHwMcAQT4cT5e2R3rcFFeRS8LZt1WHw6c9HoSU3nMXg
```

To confirm that the transaction went through, let's check it on <a target="_blank" href="https://explorer.solana.com/tx/61Wpy1aJLEeYh3ZPAiqmkMMdhRA3UKvs3WAhPyFhgCENRuHwMcAQT4cT5e2R3rcFFeRS8LZt1WHw6c9HoSU3nMXg?cluster=devnet">Solana via the explorer</a>.

If you scroll down to "Token Balances," you can see the "Change" in the amount of token (-1 from one address, +1 to another). The token address should match the mint_address provided.

The address column (left column) represents the associated token address, which is owned by the public key addresses you provided (one for your wallet, one for the receiving wallet). You can learn more about associated token <a target="_blank" href="https://blog.theblockchainapi.com/2021/11/05/how-to-get-an-associated-token-account-on-solana.html">accounts here</a>.

<img src="https://theblockchainapi-blog.s3.amazonaws.com/how-to-transfer-a-solana-metaplex-nft-using-any-coding-language/1_ISAfPGvsNrJzO83e4i42lQ.png"/>

If you have any questions, feature requests, or issues, don't hesitate to reach out via email (info [[at]] theblockchainapi.com) or <a target="_blank" href="https://dashboard.theblockchainapi.com/contact">another method</a>.

### [Optional] Step 3: Use the Blockchain API's Python wrapper to transfer an NFT

Instead, you can use the Blockchain API's Python wrapper to transfer an NFT. This can make development easier instead of having to rewrite the functions we already wrapped using the requests library. To get started, install our <a href="https://pypi.org/project/theblockchainapi/" target="_blank">PyPi package</a>.

```
pip install theblockchainapi
```

Import the necessary packages.

```
from theblockchainapi import TheBlockchainAPIResource, SolanaNetwork
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
Public Key: FsHEuc8XKWzuPMyDxfJRYAEsfmGKYmVuXK3A3J56pn2P
Secret Key: common electric hover affair abstract raccoon surprise brick advice various wall second
```

Get an airdrop of SOL on the devnet so that we can pay for the minting of a new NFT.

```
BLOCKCHAIN_API_RESOURCE.get_airdrop(public_key)
```

Now let's create an NFT.

```
def create_nft():
    response = BLOCKCHAIN_API_RESOURCE.create_nft(
        secret_recovery_phrase=secret_key,
        nft_name="The Blockchain API",
        nft_symbol="BLOCKX",
        nft_url="https://pbs.twimg.com/profile_images/1441903262509142018/_8mjWhho_400x400.jpg"
    )
    print(response)
    mint_address = response['mint']
    print(f"Mint address: {mint_address}")
    return mint_address
```

Now call the function. This will take about 60 seconds to mint.

```
mint_address = create_nft()
```

The output:

```
{
    'update_authority': 'FsHEuc8XKWzuPMyDxfJRYAEsfmGKYmVuXK3A3J56pn2P',
    'mint': '5K6Pr1cv48mxgDczwJBVSpMxybLsJrme1dtcdmkuK1VG',
    'data': {
        'name': 'The Blockchain API',
        'symbol': 'BLOCKX',
        'uri': 'https://blockx-api-storage.s3.amazonaws.com/68293087502/data',
        'seller_fee_basis_points': 0.0,
        'creators': [
            'FsHEuc8XKWzuPMyDxfJRYAEsfmGKYmVuXK3A3J56pn2P'
        ],    
        'verified': [1.0],
        'share': [100.0]
    },
    'primary_sale_happened': False,
    'is_mutable': True,
    'explorer_url': 'https://explorer.solana.com/address/5K6Pr1cv48mxgDczwJBVSpMxybLsJrme1dtcdmkuK1VG?cluster=devnet',
    'mint_secret_recovery_phrase': 'total later clock crane laptop pet chat humble runway rain music balcony'
}
Mint address: 5K6Pr1cv48mxgDczwJBVSpMxybLsJrme1dtcdmkuK1VG
```

We will now transfer the NFT. Use your own public key address or the one provided.

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
    token_address=mint_address,
    network=SolanaNetwork.DEVNET
)
print(transaction_signature)
```

The output should be:

```
7yNN4oC8aM8gpLQGo2PWTF4gSuDoB988EeBTAYotm5Df5XZEA45m4MUw1AA7zG6La53d2GrVDnVuLNfuvH7xRsV
```

You can view this transaction on the Solana blockchain <a target="_blank" href="https://explorer.solana.com/tx/7yNN4oC8aM8gpLQGo2PWTF4gSuDoB988EeBTAYotm5Df5XZEA45m4MUw1AA7zG6La53d2GrVDnVuLNfuvH7xRsV?cluster=devnet">here</a>.

If you scroll down to "Token Balances," you can see the "Change" in the amount of token (-1 from one address, +1 to another). The token address should match the mint_address provided.

The address column (left column) represents the associated token address, which is owned by the public key addresses you provided (one for your wallet, one for the receiving wallet). You can learn more about associated token <a target="_blank" href="https://blog.theblockchainapi.com/2021/11/05/how-to-get-an-associated-token-account-on-solana.html">accounts here</a>.

<img src="https://theblockchainapi-blog.s3.amazonaws.com/how-to-transfer-a-solana-metaplex-nft-using-any-coding-language/1_87spng1_GwRzRzDZBZzMdA.png"/>

If you have any questions, feature requests, or issues, don't hesitate to reach out via email (info [[at]] theblockchainapi.com) or <a target="_blank" href="https://dashboard.theblockchainapi.com/contact">another method</a>.
