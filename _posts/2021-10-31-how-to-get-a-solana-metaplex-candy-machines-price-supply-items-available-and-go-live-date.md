### How to get a Solana Metaplex candy machine’s price, supply, items available, and go live date

To get info about a Metaplex candy machine, we will use <a target="_blank" href="https://theblockchainapi.com">the Blockchain API</a>. Specifically we will use the "Get a Metaplex candy machine's details" endpoint, documented <a target="_blank" href="https://docs.theblockchainapi.com/#operation/solanaGetCandyMachineDetails">here</a>.

First, create and save an API key pair <a target="_blank" href="https://dashboard.theblockchainapi.com/api-keys?blog=direct-get-candy-details">here</a>.

Now retrieve the candy machine ID of the candy machine. For some help on how to do this, see this article.

Install the Python wrapper.

```pip install theblockchainapi```

Finally, let’s consider this example. In this example, we call the `get_candy_machine_info` function.

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
    candy_machine_data = \
        BLOCKCHAIN_API_RESOURCE.get_candy_machine_info(  
        candy_machine_id=the_goat_society_candy_machine_id,       
        network=SolanaNetwork.MAINNET_BETA    
    )    
    print(f"Candy Machine Data: {config_address}")

if __name__ == '__main__':    
    example()
```

The output will look something like this:

```
{
    'go_live_date': 1635343002.0,  # The mint start time (Unix time)
    'items_available': 8888.0,  # The number of items available in the collection (not the number of items available to mint)
    'price': 100000000.0,  # The price in Lamports (1 billion Lamports per SOL)
    'uuid': 'FVbP4u'
}
```