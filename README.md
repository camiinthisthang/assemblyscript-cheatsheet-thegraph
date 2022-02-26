# assemblyscript-cheatsheet-thegraph
A living resource to help developers get started with assemblyscript. Throwing some headscratchers I faced in here so that you all can get around more easily. 

- Can't use JS array methods (forEach, map, filter, etc.) in your mappings

- Use double equal signs for comparisons, not triple equal sign. 
 ``` js
 if (trait == "Eyes") {
  lady.eyeColor = value;
}
```
- Add node modules to git.ignore before trying to push your subgraph to Github, it'll get stuck if you try to push everything.

<img width="184" alt="Screen Shot 2022-02-26 at 12 19 17 AM" src="https://user-images.githubusercontent.com/15346823/155835939-eda66ce7-7917-4fb7-8dae-e603ce22bf2e.png"> <img width="305" alt="Screen Shot 2022-02-26 at 12 19 34 AM" src="https://user-images.githubusercontent.com/15346823/155835954-215e3f9f-30dd-4f04-8a72-4019abcd405d.png">

- Errors about using a variable before assigning it a value usually mean that you’re not using the correct scope (move your variable into the block above)

- You can do something similar to ```console.log``` to print statements and help you debug. There is an API for this, but I have found that the easiest thing is to just create string variables and print them out after steps where you think the program may be breaking to see if the control even gets to that line. Be sure to import ```log``` in your mappings.ts file to use this API.
``` js
if (trait === "hair") {
  let hairMessage = "trait equals hair";
  log.info("inside trait equals hair: {}", [hairMessage]);
  lady.hairStyle = value;
  let hairConfirm = "hair was saved to lady";
  log.info("after save: {}", [hairConfirm]);
}
````

- To read a value from a JSON object like metadata from IPFS:
Example metadata [from a CryptoCoven Witch](https://ipfs.io/ipfs/QmZHKZDavkvNfA9gSAg7HALv8jF7BJaKjUc9U2LSuvUySB/25.json).
``` json
{"description":"You are a WITCH from the silence between bells. You whisper your secrets to mummified heads. Your magic spawns from a dolphin's first swim. You live in a cottage grown from saplings and vines. GET UNDER THE DUST!","external_url":"https://www.cryptocoven.xyz/witches/25","image":"https://cryptocoven.s3.amazonaws.com/376afcf596b6efd88c689272ece54a5b.png","name":"butternut seed","background_color":"","attributes":[{"trait_type":"Skin Tone","value":"Night"},{"trait_type":"Body Shape","value":"Chiseled"},{"trait_type":"Hair (Front)","value":"Curtain"},{"trait_type":"Hair Color","value":"Platinum"},{"trait_type":"Eyebrows","value":"Medium Flat (Black)"},{"trait_type":"Mouth","value":"Pursed (Red)"},{"trait_type":"Background","value":"Taupe"},{"trait_type":"Top","value":"Open Collar with Bow (White)"},{"trait_type":"Facewear","value":"Simple Septum Ring (Silver)"},{"trait_type":"Earrings","value":"Basic Piercings (Silver)"},{"trait_type":"Eye Style","value":"Babylash"},{"trait_type":"Eye Color","value":"Green"},{"trait_type":"Archetype of Power","value":"Hag"},{"trait_type":"Sun Sign","value":"Pisces"},{"trait_type":"Moon Sign","value":"Gemini"},{"trait_type":"Rising Sign","value":"Gemini"},{"display_type":"number","trait_type":"Will","value":8,"max_value":9},{"display_type":"number","trait_type":"Wit","value":8,"max_value":9},{"display_type":"number","trait_type":"Wiles","value":3,"max_value":9},{"display_type":"number","trait_type":"Wisdom","value":3,"max_value":9},{"display_type":"number","trait_type":"Wonder","value":4,"max_value":9},{"display_type":"number","trait_type":"Woe","value":5,"max_value":9}],"coven":{"id":25,"name":"butternut seed","type":"hag","description":{"intro":"You are a WITCH from the silence between bells.","hobby":"You whisper your secrets to mummified heads.","magic":"Your magic spawns from a dolphin's first swim.","typeSpecific":"You live in a cottage grown from saplings and vines.","exclamation":"GET UNDER THE DUST!"},"skills":{"will":8,"wit":8,"wiles":3,"wisdom":3,"wonder":4,"woe":5},"birthChart":{"sun":"pisces","moon":"gemini","rising":"gemini"},"styles":[{"id":"recEXIkcO3w1OHe79","attribute":"base","name":"chiseled","color":"night"},{"id":"recqEEzSzfCGhCXPe","attribute":"hair-bangs","name":"curtain","color":"platinum"},{"id":"rec8dTF6Lj23oM21v","attribute":"eyebrows","name":"medium-flat","color":"black"},{"id":"rec8N8kMirlJDNdzo","attribute":"mouth","name":"pursed","color":"red"},{"id":"recnk9WLnMtCbPz8R","attribute":"background","name":"solid","color":"taupe"},{"id":"recaZJr1la3mb4IlQ","attribute":"body-under","name":"open-collar-with-bow","color":"white"},{"id":"recAYGBePrCdrGOPb","attribute":"facewear","name":"simple-septum-ring","color":"silver"},{"id":"recIA3JjkB3mu02UR","attribute":"earrings","name":"basic-piercings","color":"silver"},{"id":"rec5Z7ZXbmV75dzNI","attribute":"eyes","name":"babylash","color":"green"}],"hash":"376afcf596b6efd88c689272ece54a5b"}}
```
> Tip: Copy/paste this data into an [online JSON formatter to be able to view the object more clearly.](https://jsonformatter.curiousconcept.com/#about)
<img width="829" alt="Screen Shot 2022-02-26 at 12 34 41 AM" src="https://user-images.githubusercontent.com/15346823/155836417-f6845bc8-21a0-42b8-8a5d-0fbc348c4ef0.png">

``` js
let metadata = ipfs.cat(ipfshash + token.tokenURI);
    if (metadata) {
      const value = json.fromBytes(metadata).toObject()
      if (value) {
        const image = value.get('image')
        const name = value.get('name')
        const description = value.get('description')
        const externalURL = value.get('external_url')

        const coven = value.get('coven')
        if (coven) {
          let covenData = coven.toObject()
          const type = covenData.get('type')
          if (type) {
            token.type = type.toString()
          }

          const birthChart = covenData.get('birthChart')
          if (birthChart) {
            const birthChartData = birthChart.toObject()
            const sun = birthChartData.get('sun')
            const moon = birthChartData.get('moon')
            const rising = birthChartData.get('rising')
            if (sun && moon && rising) {
              token.sun = sun.toString()
              token.moon = moon.toString()
              token.rising = rising.toString()
            }
          }
        }
       }
      }
```

- To read metadata from an array of objects in IPFS:
Example data from [Fame Lady Squad metadata.](https://ipfs.io/ipfs/QmTngWTnURuyiz1gtoY33FKghCiU2uQusXpnUc36QJNKsY/1002)
``` json
{"name": "Fame Lady #1002", "description": "Fame Lady Squad is the first ever generative all-female avatar collection on the Ethereum blockchain. Yes, we are THE community who took over a project to write our own story. This is NFT history. This is HERstory. FLS are 8888 distinctive Ladies made up of millions of fierce trait combinations. Community = Everything. Commercial IP rights of each Lady NFT belong to its owner.", "image": "ipfs://QmRRRcbfE3fTqBLTmmYMxENaNmAffv7ihJnwFkAimBP4Ac/1002.png", "attributes": [{"trait_type": "Skin", "value": "Olive"}, {"trait_type": "Background", "value": "White"}, {"trait_type": "Face Expression", "value": "Pokerface"}, {"trait_type": "Eyebrows", "value": "Stylish"}, {"trait_type": "Eyes", "value": "Pink"}, {"trait_type": "Nose Piercing Lower", "value": "Silver Ring"}, {"trait_type": "Body", "value": "Small Chain"}, {"trait_type": "Clothes", "value": "Green Dress"}, {"trait_type": "Hair", "value": "Straight Black"}, {"trait_type": "Elf Ear", "value": "Elf"}]}
```
> Tip: Copy/paste this data into an [online JSON formatter to be able to view the object more clearly.](https://jsonformatter.curiousconcept.com/#about)
<img width="936" alt="Screen Shot 2022-02-26 at 12 41 14 AM" src="https://user-images.githubusercontent.com/15346823/155836588-b64c9b20-bdd5-4b5f-a0cc-3bd5566c01d2.png">

``` js
let metadata = ipfs.cat(ipfsHash + lady.tokenURI);
    if (metadata) {
      const value = json.fromBytes(metadata).toObject();
      if (value) {
        const name = value.get("name");
        if (name) {
          lady.name = name.toString();
        }
      }
      let attributes: JSONValue[];
      let ladyAttributes = value.get("attributes"); //this is our array of objects
      if (ladyAttributes) {
        attributes = ladyAttributes.toArray();

        for (let i = 0; i < attributes.length; i++) { // loop through array of objects and turn each element into an object format to read in the values correctly
          let item = attributes[i].toObject();
          let trait: string;
          let traitName = item.get("trait_type");
          if (traitName) {
            trait = traitName.toString();
            let value: string;
            let traitValue = item.get("value");
            if (traitValue) {
              value = traitValue.toString();
              if (trait == "Hair") {
                lady.hairStyle = value;
              }
              if (trait == "Skin") {
                lady.skinColor = value;
              }
              if (trait == "Eyes") {
                lady.eyeColor = value;
              }
              if (trait == "Face Expression") {
                lady.faceExpression = value;
              }
            }
          }
        }
      }
    }
```
