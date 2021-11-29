## How to airdrop SOL on the devnet to your wallet

Airdropping SOL to your wallet on the devnet consists of two steps — recovering your wallet and then running the airdrop command.

### Prerequesites

Install the <a href="https://docs.solana.com/cli/install-solana-cli-tools">Solana CLI</a>.

### Recover Your Public Key

If you already have a public key address, then skip this step. Otherwise, you can derive your public key as follows.

Open up a new terminal window to type a command.

Recover your keypair file. (In order to avoid overwriting any existing keypair file you have, make sure to write to a nonexistent file.)

```
solana-keygen recover --outfile temp-keypair.json
```

This will use the CLI’s default derivation path.

Enter the mnemonic phrase and passphrase when prompted. Ensure that the public key returned is what you were expecting. If so, enter “y.”

<em>If you don’t have a mnemonic phrase yet, generate one easily with the Blockchain API’s endpoint <a target="_blank" href="https://docs.theblockchainapi.com/#operation/solanaGenerateSecretRecoveryPhrase">here</a>. </em>

If not, you might have created the wallet with a separate interface (e.g., Phantom wallet), which uses a different derivation path. To see our tutorial on Solana derivation paths, see here.

You can now use this public key to get your airdrop.

### Get the Airdrop

Replace “8m24W8DvoJ9p1ANDcNbyZMQTgPV9Vrr8DtxApsE4oBTQ” below with your public key address.

```
solana airdrop 1 8m24W8DvoJ9p1ANDcNbyZMQTgPV9Vrr8DtxApsE4oBTQ
```

If Solana returns a 429 error (too many requests) or other such error, try connecting via a VPN to get a new IP address.

If you do get a 429, which you will inevitably run into if using in production or frequently at all, you can use our
<a href="https://docs.theblockchainapi.com/#operation/solanaGetAirdrop" target="_blank">airdrop endpoint</a> for reliably and predictably getting an airdrop!

Hope this helps! 

Please check out <a href="https://docs.theblockchainapi.com/" target="_blank">The Blockchain API’s documentation</a> for endpoints that can help you develop faster and more reliably. 

If you have any questions, feel free to email us at info [(at)] the blockchainapi.com or open up <a href="https://github.com/BL0CK-X/the-blockchain-api/issues/new" target="_blank">a new GitHub issue on our GitHub repository</a>.
