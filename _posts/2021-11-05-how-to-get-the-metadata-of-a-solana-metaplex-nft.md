## How to get a Solana Metaplex NFT's Metadata

To get the metadata of a Solana Metaplex NFT, we will use The Blockchain API's "Get an NFT's metadata" endpoint.

The Blockchain API makes it easy to perform common functions on the blockchain by using a simple API, rather than figuring out the steep learning curve or unclear documentation.

### Step 1: Get the NFT's address

First, get the NFT's address. How you get this depends on where the NFT is coming from. If you created it, note down the address of the NFT returned. If it is an NFT listed on a marketplace, click "View on Solana" or a similar button that lets you see the NFT on the Solana explorer.
This will bring you to a link like this:

```
https://explorer.solana.com/address/2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn
```

Where `2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn` is the address you need.

### Step 2: Select the network

Determine whether the NFT is saved on the devnet or mainnet-beta. You will need to specify this in the API call.

### Step 3: Get an API key pair for free

Before making the API call, you must create an API key here. Save the key ID and the secret key.

### Step 4: Make the API request

Let's write this in Python, but it can be written in any language.

```
import requests

response = requests.get(
    "https://api.theblockchainapi.com/v1/solana/nft",
    params={
        'mint_address':
        '2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn',
        'network': 'mainnet-beta'
    },
    headers={
        'APISecretKey': 'YOUR SECRET KEY',
        'APIKeyId': 'YOUR KEY ID'
    }
)

print(response.json())
```
The output will look something like:
```
{
    'data': {
        'creators': [
            'HuAiZg55P557gdjr5jkq79YdMe9sbdHuPj5UN21XuSyK',
            '5FzddvKbxE54KEg2WoeNiGWJYAUBabKVFg2MVCSacWiJ'
        ],
        'name': 'Vox Punks Club #3628',
        'seller_fee_basis_points': 500.0,
        'share': [0.0, 100.0],
        'symbol': '',
        'uri':
        'https://arweave.net/45txDYtu_UcLnF-q8WQcFMPANBInL84LfCAazfwvhbc',
        'verified': [1.0, 1.0]
    },
    'explorer_url': 'https://explorer.solana.com/address/2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn',
    'is_mutable': True,
    'mint': '2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn',   
    'primary_sale_happened': True,
    'update_authority': '5FzddvKbxE54KEg2WoeNiGWJYAUBabKVFg2MVCSacWiJ'
}
```

Any issues? Contact us. info [[at]] theblockchainapi.com.

### [Optional] Step 5: Write it with the Blockchain API Python Wrapper

If you want to write this with the Blockchain API's Python wrapper.

```pip install theblockchainapi```

Create the resource.

```
from theblockchainapi import TheBlockchainAPIResource, SolanaNetwork

BLOCKCHAIN_API_RESOURCE = TheBlockchainAPIResource(
    api_key_id=MY_API_KEY_ID,
    api_secret_key=MY_API_SECRET_KEY
)
```

Write the request.

```
nft_address = '2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn'

nft_metadata = BLOCKCHAIN_API_RESOURCE.get_nft_metadata(
    mint_address=nft_address,
    network=SolanaNetwork.MAINNET_BETA
)

print(nft_metadata)
```

The output will look something like this:

```
{
    'data': {
        'creators': [
            'HuAiZg55P557gdjr5jkq79YdMe9sbdHuPj5UN21XuSyK',
            '5FzddvKbxE54KEg2WoeNiGWJYAUBabKVFg2MVCSacWiJ'
        ],
        'name': 'Vox Punks Club #3628',
        'seller_fee_basis_points': 500.0,
        'share': [0.0, 100.0],
        'symbol': '',
        'uri':
        'https://arweave.net/45txDYtu_UcLnF-q8WQcFMPANBInL84LfCAazfwvhbc',
        'verified': [1.0, 1.0]
    },
    'explorer_url': 'https://explorer.solana.com/address/2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn',
    'is_mutable': True,
    'mint': '2pQPtnFm2mgXZrVWyNdcf5Qf2TWBGkTAeKZJhPjsc7Jn',   
    'primary_sale_happened': True,
    'update_authority': '5FzddvKbxE54KEg2WoeNiGWJYAUBabKVFg2MVCSacWiJ'
}
```