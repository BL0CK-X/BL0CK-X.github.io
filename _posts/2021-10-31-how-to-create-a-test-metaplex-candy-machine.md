### How to create a test Metaplex candy machine

We will use <a href="https://theblockchainapi.com/" target="_blank">the Blockchain API</a> for this tutorial. Specifically, we will use the "Create a test Metaplex candy machine" endpoint, available <a target="_blank" href="https://docs.theblockchainapi.com/#tag/Solana-NFT/paths/~1v1~1solana~1nft~1candy_machine/post">here</a>.

We setup an API endpoint to easily create a test Metaplex candy machine with 2 NFTs available for minting.

The purpose is to enable you to test your code that interacts with real candy machines.

To get started, first get an API key pair for free <a target="_blank" href="https://dashboard.theblockchainapi.com/api-keys">here</a>.

Then, install the Python wrapper.

``pip install theblockhchainapi``

We will follow this example here, which creates a new wallet, gets an airdrop on the devnet, and then creates a test candy machine.

(Creating a test NFT candy machine will cost about 0.045 SOL, but this technically free because we will do this on the ‘devnet’. This code is easily portable to mainnet-beta, as you will see.)

To complete the code below, fill in your API key pairs. Follow the comments in the code below to see what each endpoint does.

```
from theblockchainapi import TheBlockchainAPIResource, SolanaNetwork, SolanaCurrencyUnit 
# Get an API key pair for free here: 
# https://dashboard.theblockchainapi.com/api-keys
MY_API_KEY_ID = None
MY_API_SECRET_KEY = None
BLOCKCHAIN_API_RESOURCE = TheBlockchainAPIResource(    
    api_key_id=MY_API_KEY_ID,    
    api_secret_key=MY_API_SECRET_KEY
)  
def example():    
    try:       
        assert MY_API_KEY_ID is not None        
        assert MY_API_SECRET_KEY is not None    
    except AssertionError:        
        raise Exception("Fill in your key ID pair!")     
    # Create a new wallet    
    network = SolanaNetwork.DEVNET    
    secret_recovery_phrase = \ 
        BLOCKCHAIN_API_RESOURCE.generate_secret_key()    
    derivation_path = str()    
    pass_phrase = str()    
    print(secret_recovery_phrase)    
    public_key = BLOCKCHAIN_API_RESOURCE.derive_public_key(        
        secret_recovery_phrase=secret_recovery_phrase,        
        derivation_path=derivation_path,        
        passphrase=pass_phrase    
    )    
    print(public_key)     
    for _ in range(3):        
        # Get an airdrop of 0.045 (0.015 * 3) 
        # to be able to pay for the creation of the candy machine        
    
        BLOCKCHAIN_API_RESOURCE.get_airdrop(
            recipient_address=public_key
        )
     
    print(
        BLOCKCHAIN_API_RESOURCE.get_balance(
            public_key, 
            SolanaCurrencyUnit.SOL, 
            SolanaNetwork.DEVNET
        )
    )     
    # Creates a test candy machine with 2 available to mint         
    candy_machine_id = \
        BLOCKCHAIN_API_RESOURCE.create_test_candy_machine(           
        secret_recovery_phrase=secret_recovery_phrase,        
        derivation_path=derivation_path,        
        passphrase=pass_phrase,        
        network=SolanaNetwork.DEVNET    
    )     
    print(candy_machine_id)    
    url_to_view = \
        f"https://explorer.solana.com/address/{candy_machine_id}?" \
        f"cluster={network.value}"    
    print(url_to_view)  
if __name__ == '__main__':    
    example()
```

The output will look something like this:

```
camera random swing ladder solution always pupil tip confirm radar design banner
DWJUVDB2GLux9zW47h81fz9cxi6ZujcQVCHytGLHHQZW
{'balance': 0.045, 'network': 'devnet', 'unit': 'sol'}
2U2d19rGeYNg5DgH1PiQvXS9dfvKLwSTxa7MFXpvUX6w
https://explorer.solana.com/address/2U2d19rGeYNg5DgH1PiQvXS9dfvKLwSTxa7MFXpvUX6w?cluster=devnet
```