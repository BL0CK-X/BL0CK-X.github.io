## How to get the config public key of a Metaplex candy machine on Solana

You might need to retrieve the config public key of a candy machine. This might allow you to perform some function, such as retrieving data about the candy machine’s setup.
To do so, we can use the Blockchain API. Specifically, we will use the <a target="_blank" target="https://docs.theblockchainapi.com/#tag/Solana-NFT/paths/~1v1~1solana~1nft~1candy_machine~1config/post">“Get the candy machine config address” endpoint</a>. You can find more information about this endpoint on <a target="_blank" href="https://docs.theblockchainapi.com/#tag/Solana-NFT/paths/~1v1~1solana~1nft~1candy_machine~1config/post">the docs</a>.

First, install the Python wrapper.

```pip install theblockchainapi```

To get the config public key, you’ll need:
- An API key pair (get one for free <a target="_blank" href="https://dashboard.theblockchainapi.com/">here</a>)
- The candy machine ID (see how to get one <a target="_blank" href="https://medium.com/@josh.wolff.7/how-to-get-a-metaplex-candy-machine-id-d8f39d67162c">here</a>)

After getting your API key ID and your API secret key, call the `get_candy_machine_config_public_key` function.

We are citing <a target="_blank" href="https://github.com/BL0CK-X/the-blockchain-api/tree/main/examples/solana-candy-machine/get-config-address">the example found here</a>.

```
from theblockchainapi import TheBlockchainAPIResource, SolanaNetwork 
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
    the_goat_society_candy_machine_id = \
        "9htmDvW58pjCMQdjFbovo8cGBZviDfeP3j7DKnikHEy5"    
    config_address = \
        BLOCKCHAIN_API_RESOURCE.get_candy_machine_config_public_key( 
        candy_machine_id=the_goat_society_candy_machine_id,        
        network=SolanaNetwork.MAINNET_BETA    
    )    
    print(f"Config Address: {config_address}")  

if __name__ == '__main__':    
    example()
```

The output will look something like this:

```
Config Address: FVbP4uormTqtmR8uQS2YrfKEfoVoQfGEzznCMPTQMZoQ
```