# the-on-chain-dudes
"the dudes" is the first interactive profile picture NFT and now it's on chain!

## Contract
ETH mainnet contract address: 0x65182b158B6eef8A8bfd186b7bd72Fa4c4234dd0

## Getting Script from Contract
You can easily fetch the script from the mainnet contract. This script can be used to display any "the dude" on modern web browsers. Script exposes the rendering hiearchy and the objects so you can actually modify/customise it as well.

Calling this method of the contract will return you the full JS script:
```solidity
contract.getScriptChunk(0, 20)
```

Here you can see how you can fetch the script and display any dude on the browser. Please navigate to [examples](https://github.com/the-dudes-nft/the-on-chain-dudes/tree/main/example) for the full example.
```javascript
const theOnChainDudesABI = [{ "inputs": [{ "internalType": "string[]", "name": "chunks", "type": "string[]" }], "name": "addScriptChunks", "outputs": [], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [{ "internalType": "uint256", "name": "_chunkIndex", "type": "uint256" }, { "internalType": "uint256", "name": "_chunkSize", "type": "uint256" }], "name": "getScriptChunk", "outputs": [{ "internalType": "string", "name": "", "type": "string" }, { "internalType": "bool", "name": "", "type": "bool" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "getScriptChunkLength", "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "stateMutability": "view", "type": "function" }, { "inputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "name": "scriptChunks", "outputs": [{ "internalType": "string", "name": "", "type": "string" }], "stateMutability": "view", "type": "function" }]
const theOnChainDudesAddress = "0x65182b158B6eef8A8bfd186b7bd72Fa4c4234dd0"
const web3 = new Web3(new Web3.providers.HttpProvider("{provider}"));

let theDudeController;
function loadOnChainDudes() {
    const theOnChainDudes = new web3.eth.Contract(
        theOnChainDudesABI,
        theOnChainDudesAddress
    );
    
    const result = await theOnChainDudes.methods.getScriptChunk(0, 20).call()
    const script = result[0]
    const wrapped = '(function() { ' + script + 'return theOnChainDudes }())'
    const canvas = document.getElementById("canvas")

    const TheOnChainDudes = eval(wrapped);
    theDudeController = TheOnChainDudes.init(canvas)
    
    theDudeController.loadDude("018441")
    theDudeController.resizeScene(window.innerWidth, window.innerHeight)
    console.log("Attributes", theDudeController.contents.theDude.context.attributes);
}

window.addEventListener("resize", function () {
    if (!theDudeController) { return }
    theDudeController.resizeScene(window.innerWidth, window.innerHeight)
})

loadOnChainDudes()
```