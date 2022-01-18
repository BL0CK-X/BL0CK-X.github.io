## How to format the off-chain metadata for an NFT on Solana

### Creating an NFT and some background

You can create an NFT with our endpoint <a href="https://docs.blockchainapi.com/#operation/solanaCreateNFT" target="_blank">here</a>.

Note that an NFT has a few on-chain attributes for metadata:
<li>The `name`</li>
<li>The `symbol`</li>
<li>The `uri`</li>
<li>The `creators`, `update_authority`, `primary_sale_happened`, and `seller_fee_basis_points`</li>

The `uri` is the URL that contains all of the off-chain data, such as attributes, the image, etc. Thus, in Solana, the image is NOT stored on Solana's blockchain.

The attribute `nft_upload_method` offers two options: `LINK` and `S3`.

The `S3` will automatically upload your NFT off-chain data to our S3 bucket and the `uri` of the NFT to our S3 link. You can use this for testing purposes, but we don't recommend using it for live NFTs, although you can.

The `LINK` method directly uploads the link you provide as a value for the attribute for `nft_url` as the `uri` of the NFT. 
However, for the image to show and the metadata to be properly recognized by Solana platforms, then you must format the file you uploaded correctly. 
First, it must be JSON formatted. Second, it must follow the format as outlined in the next section.

### The JSON Schema

See this JSON schema example for how to format the JSON for your NFT.

```json
{
    "name": "Solflare X NFT",
    "symbol": "",
    "description": "Celebratory Solflare NFT for the Solflare X launch",
    "seller_fee_basis_points": 0,
    "image": "https://www.arweave.net/abcd5678?ext=png",
    "animation_url": "https://www.arweave.net/efgh1234?ext=mp4",
    "external_url": "https://solflare.com",
    "attributes": [
        {
        "trait_type": "web",
        "value": "yes"
        },
        {
        "trait_type": "mobile",
        "value": "yes"
        },
        {
        "trait_type": "extension",
        "value": "yes"
        }
    ],
    "collection": {
        "name": "Solflare X NFT",
        "family": "Solflare"
    },
    "properties": {
        "files": [
            {
                "uri": "https://www.arweave.net/abcd5678?ext=png",
                "type": "image/png"
            },
            {
                "uri": "https://watch.videodelivery.net/9876jkl",
                "type": "unknown",
                "cdn": true
            },
            {
                "uri": "https://www.arweave.net/efgh1234?ext=mp4",
                "type": "video/mp4"
            }
        ],
        "category": "video",
        "creators": [
            {
                "address": "SOLFLR15asd9d21325bsadythp547912501b",
                "share": 100
            }
        ]
    }
}
```

### What the JSON fields do

<li>description - Human readable description of the asset.</li>
<li>image - URL to the image of the asset. PNG, GIF and JPG file formats are supported. You may use the ?ext={file_extension} query to provide information on the file type.</li>
<li>animation_url - URL to a multi-media attachment of the asset. The supported file formats are MP4 and MOV for video, MP3, FLAC and WAV for audio and GLB for AR/3D assets. You may use the ?ext={file_extension} query to provide information on the file type.</li>
<li>external_url - URL to an external application or website where users can also view the asset.</li>
<li>properties.category - Supported categories: image (png, gif, svg, jpg), video (mp4, mov), audio (mp3, flac, wav), vr (glb, gltf)</li>
<li>properties.files - Object array, where an object should contain the uri and type of the file that is part of the asset. The type should match the file extension. The array will also include files specified in image and animation_url fields, and any other that are associated with the asset. You may use the ?ext={file_extension} query to provide information on the file type.</li>
<li>attributes - Object array, where an object should contain trait_type and value fields. value can be a string or a number.</li>

This section was informed by <a href="https://medium.com/metaplex/metaplex-metadata-standard-45af3d04b541">this article</a>.