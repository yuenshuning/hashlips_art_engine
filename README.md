## 6.HashLips NFT

1. Mint an NFT for free on [Mintable.app](https://mintable.app/). Aother website called [opensea]() is more popular but it's not possible to sell for free.

2. How to create an NFT collection

   1. [HashLips Art Engine](https://github.com/HashLips/hashlips_art_engine) Code description

      1. `layers/`: basic assets of image generation and fusion

      2. `src/config.js`: configuration of image generation. The `layersOrder` object describes the order of generative images (the order matters), where the values of key-value pairs <u>should correspond to</u> the folder name of `layers/` one by one

      3. If we want to set the rarity of layers, set the file name of layers as `xxx#weight.png`, otherwise, the weight is equal to default weight (1). See `src/main.js/getRarityWeight`

      4. If we want to run two or more different configurations at the same time, we need to grow the amount of `growEditionSizeTo`

         ```js
         const layerConfigurations = [
           {
             // 5 eye images
             growEditionSizeTo: 5,
             layersOrder: [ { name: "Background" }, { name: "Eyeball" }, { name: "Eye color" }, { name: "Iris" }, { name: "Shine" }, { name: "Bottom lid" }, { name: "Top lid" } ]
           }, {
             // 3 bear images
             growEditionSizeTo: 8,
             layersOrder: [ { name: "bear_background" }, { name: "bear_body" }, { name: "bear_clothes" }, { name: "bear_hair" } ]
           }
         ];
         ```

      5. If we want to change the image baseUri from defalut `ipfs://NewUriToReplace` to other value, change the `baseUri` in `src/config.js`

      6. Customize the description of each image by changing the `description` in `src/config.js`

      7. Shuffle the generated images by setting the `shuffleLayerConfigurations` in `src/config.js` to `true`

      8. Resize the size of generated images by changing the `format` in `src/config.js`

      9. Setting a base color as background by setting the `background` in `src/config.js`

      10. Run `node index.js` to generate images according to `src/config.js` and `layers/` assets.

      11. Run `node utils/xxx,js` to use the utils

      12. The generative images and corresponding JSON files (metadata) locate in `build/`

   2. Metadata is neccessary, because metadata is the information of a particular image. It's in the format of JSON object. That is why we generate images with JSON files.

   3. Upload our generated images (JSON files cannot be uploaded) to [pinata](https://app.pinata.cloud/) ipfs hosting server, and get an unique image CID. Go to `src/config.js`, change the `baseUri` to `ipfs://{image CID}`, run `node utils/update_info.js` to <u>only</u> update the JSON files (do not re-generate images)

   4. Upload our generated JSON files to [pinata](https://app.pinata.cloud/) ipfs hosting server, and get an unique metadata CID.

   5. Upload our hidden_image and hidden_metadata the same way, and get unique hidden_image CID and hidden_metadata CID.

   6. [HashLips NFT Contract](https://github.com/HashLips/hashlips_art_engine) Code description

      1. Change the `maxSupply` in `SimpleNft_flat.sol` to the amount of images we generated
      2. Change other variables such as `cost`, `maxMintAmount`, `paused`, `revealed`
      3. make sure the solidity compile matches the pragma
      4. make sure we are on `Injected Web3` environment
      5. configure the Metamask and choose test net (rinkp/ganache)
      6. make sure we choose the right contract on remix to deploy (Bear.sol) and set parameters. Note: the `_initBaseURI` should be `ipfs://{metadata CID}/`, the `_initNotRevealedUri` should be ipfs://`{hidden_metadata CID}/`
      7. Before click the `transact` button, click the small `copy` icon left to it, and save the information to local .txt file. We need this information to verify the contract.

3. 

