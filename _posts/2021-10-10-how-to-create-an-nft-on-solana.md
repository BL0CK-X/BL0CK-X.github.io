## How to create an NFT on Solana

Creating an NFT on Solana can be difficult because the infrastructure for doing so is so new. Here, I’ll explain how to do it with <a target="_blank" href="https://theblockchainapi.com/">The Blockchain API</a>, which is an API that simplifies many functions you may wish to perform on Solana. If you have a feature request, consider creating <a target="_blank" href="https://github.com/BL0CK-X/the-blockchain-api/issues/new">a request</a> on our <a href="https://github.com/BL0CK-X/the-blockchain-api" target="_blank">GitHub repository</a>.

In this tutorial, we will use Python, but we won’t use any special Python libraries, so you can use any language.

First, get an API key pair. Go to <a target="_blank" href="https://dashboard.theblockchainapi.com/">dashboard.theblockchainapi.com</a>, sign in with Google, click “API Keys,” then click “Create New Key.” Copy down your key ID and your secret key.

---

### Prerequisites

1. To create an NFT, we need a wallet. This consists of a mnemonic phrase and a public key.
2. To get a mnemonic phrase, see <a target="_blank" href="https://docs.theblockchainapi.com/#tag/Solana-Wallet/paths/~1v1~1solana~1wallet~1secret_recovery_phrase/post">this endpoint here</a>.
3. Once we have a mnemonic phrase, we will use the CLI derivation path and the default passphrase. (For a quick tutorial on how derivation paths work on Solana, <a target="_blank" href="https://medium.com/@josh.wolff.7/derivation-paths-on-solana-fb08d3dd09f1">see here</a>.)
4. To create an NFT, you also need a balance in your account. To get a fee estimate for minting the NFT, <a href="https://docs.theblockchainapi.com/#tag/Solana-NFT/paths/~1v1~1solana~1nft~1mint~1fee/get" target="_blank">see this endpoint</a>. If the fee is greater than your balance, then consider using devnet for now to test. Once you are ready, you can use the mainnet and transfer real SOL to your wallet.
5. With the devnet, we can get simulated SOL for free. To airdrop some SOL to your account to enable testing, first see <a href="https://medium.com/@josh.wolff.7/how-to-airdrop-sol-on-the-devnet-to-your-wallet-5f607c363201">this tutorial</a> on airdropping SOL to your devnet account.
6. Now that you have a wallet and enough to pay the fee, check over the documentation on <a href="https://docs.theblockchainapi.com/#tag/Solana-NFT/paths/~1v1~1solana~1nft/post" target="_blank">how to create an NFT using the API</a>.
7. Note that we only need to provide our mnemonic phrase, but there are other things we can provide, such as a URI for the NFT’s data and the name of the NFT. Collect this data.

### Mint the NFT

We will use the Blockchain API’s <a target="_blank" href="https://docs.theblockchainapi.com/#tag/Solana-NFT/paths/~1v1~1solana~1nft/post">endpoint for creating an NFT</a>.

Once we have the prerequisites set up, minting the NFT consists of a single API call.

```
import requests
API_KEY_ID = ""
API_SECRET_KEY = ""

HEADERS = {
    "APIKeyID": API_KEY_ID,
    "APISecretKey": API_SECRET_KEY
}
MNEMONIC_PHRASE = ""
PARAMS = {
    "secret_recovery_phrase": MNEMONIC_PHRASE,
    "derivation_path": "",
    "network": "devnet",
    "nft_name": "Rare Rare Punk Pig #001",
    "nft_symbol": "PUNKPIG",
    "nft_url": "https://pbs.twimg.com/profile_images/724046973276278784/8fvrG3zo_400x400.jpg",
    "nft_upload_method": "LINK"
}
response = requests.post(
    url="https://api.theblockchainapi.com/v1/solana/nft",
    params=PARAMS,
    headers=HEADERS
)
print(response.json())
```

The API call takes about 60 seconds to complete, after which you will get back the NFT mint info!

You can check it out on the <a target="_blank" href="https://explorer.solana.com/address/AH9PQLt4ksW4FhoCSGzjcwPjzpnhUbhZEwFrdDYyMjA?cluster=devnet">Solana explorer</a> by going to explorer/solana.com/address/{mint_address}?cluster=devnet, where in our case the mint address is AH9PQLt4ksW4FhoCSGzjcwPjzpnhUbhZEwFrdDYyMjA. Make sure to select the devnet network when viewing it on the block explorer.

If you have any questions, please email us at info [[at]] theblockchainapi.com or <a target="_blank" href="https://github.com/BL0CK-X/the-blockchain-api/issues/new">open a GitHub issue</a>.

[comment]: <> (#### Some T-SQL Code)

[comment]: <> (```tsql)

[comment]: <> (SELECT This, [Is], A, Code, Block -- Using SSMS style syntax highlighting)

[comment]: <> (    , REVERSE&#40;'abc'&#41;)

[comment]: <> (FROM dbo.SomeTable s)

[comment]: <> (    CROSS JOIN dbo.OtherTable o;)

[comment]: <> (```)
