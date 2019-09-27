## OpenSea ERC-1155 Starter Contracts

### About ERC-1155.

This is a  simple sample ERC-1155 contract for the purposes of demonstrating integration with the [OpenSea](https://opensea.io) marketplace for crypto collectibles. We include:
- A script for minting items.
- A factory contract for making sell orders for unminted items (allowing for **gas-free and mint-free presales**).
- A configurable lootbox contract for selling randomized collections of ERC-1155 items.

Additionally, this contract whitelists the proxy accounts of OpenSea users so that they are automatically able to trade the ERC-1155 items on OpenSea (without having to pay gas for an additional approval). On OpenSea, each user has a "proxy" account that they control, and is ultimately called by the exchange contracts to trade their items. (Note that this addition does not mean that OpenSea itself has access to the items, simply that the users can list them more easily if they wish to do so)

## Requirements

### Node version

Either make sure you're running a version of node compliant with the `engines` requirement in `package.json`, or install Node Version Manager [`nvm`](https://github.com/creationix/nvm) and run `nvm use` to use the correct version of node.

## Installation

Run
```bash
yarn
```

## Deploying

### Deploying to the Rinkeby network.

1. You'll need to sign up for [Infura](https://infura.io). and get an API key.
2. Using your API key and the mnemonic for your Metamask wallet (make sure you're using a Metamask seed phrase that you're comfortable using for testing purposes), run:

```
export INFURA_KEY="<infura_key>"
export MNEMONIC="<metmask_mnemonic>"
yarn truffle migrate --network rinkeby
```

### Deploying to the mainnet Ethereum network.
```
yarn truffle migrate --network live
```

### Troubleshooting

#### It doesn't compile!
Install truffle locally: `yarn add truffle`. Then run `yarn truffle migrate ...`.

You can also debug just the compile step by running `yarn truffle compile`.

#### It doesn't deploy anything!
This is often due to the truffle-hdwallet provider not being able to connect. Go to infura.io and create a new Infura project. Use your "project ID" as your new `INFURA_KEY` and make sure you export that command-line variable above.

### Minting tokens.

After deploying to the Rinkeby network, there will be a contract on Rinkeby that will be viewable on [Rinkeby Etherscan](https://rinkeby.etherscan.io). For example, here is a [recently deployed contract](https://rinkeby.etherscan.io/address/0xeba05c5521a3b81e23d15ae9b2d07524bc453561). You should set this contract address and the address of your Metamask account as environment variables when running the minting script:

```
export OWNER_ADDRESS="<my_address>"
export NFT_CONTRACT_ADDRESS="<deployed_contract_address>"
export NETWORK="rinkeby"
node scripts/mint.js
```

Note: When running the minting script on mainnet, your environment variable needs to be set to `mainnet` not `live`.  The environment variable affects the Infura URL in the minting script, not truffle. When you deploy, you're using truffle and you need to give truffle an argument that corresponds to the naming in truffle.js (`--network live`).  But when you mint, you're relying on the environment variable you set to build the URL (https://github.com/ProjectOpenSea/opensea-creatures/blob/master/scripts/mint.js#L54), so you need to use the term that makes Infura happy (`mainnet`).  Truffle and Infura use the same terminology for Rinkeby, but different terminology for mainnet.  If you start your minting script, but nothing happens, double check your environment variables.