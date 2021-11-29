## Derivation paths on Solana

To write to the Solana blockchain or interact with a protocol, you need a wallet.

A wallet consists of a mnemonic phrase (a sequence of twelve English words) and a public key address.

To derive the public key address from a mnemonic phrase, a derivation path is needed.

There are infinitely many derivation paths, and so you can generate an infinite number of public key addresses from a single mnemonic phrase.

Sollet and Phantom use the derivation path, “m/44'/501'/0'/0'” and Solflare uses “m/44'/501'/0'”. The Solana CLI uses a different one (not sure exactly what that is, but please let me know if you know).

You can increment the last digit to get a new address, such as “m/44'/501'/0'/1'” and “m/44'/501'/0'/2'”.

If you’d like to easily derive public key addresses, try out this endpoint <a target="_blank" href="https://docs.theblockchainapi.com/#operation/solanaDerivePublicKey">here</a> on The Blockchain API. You can provide the mnemonic phrase, optional passphrase, and derivation path and receive a public key in response. It makes it easy to derive keys from a mnemonic phrase.

If you have any questions on the API, email info [(at)] theblockchainapi.com or open <a target="_blank" href="https://github.com/BL0CK-X/the-blockchain-api/issues/new">a new GitHub issue on our GitHub repository</a>.