# Upgrades Mix

- [Upgrades Mix](#upgrades-mix)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
- [Usage](#usage)
  - [Scripts](#scripts)
  - [Test](#test)
  - [Linting](#linting)
  - [License](#license)

This repo shows users how to use the Transparent Proxy pattern for upgrading smart contracts. It uses most of the code from openzeppelin's repo, and adds brownie scripts on top.

## Prerequisites

Please install or have installed the following:

- [nodejs and npm](https://nodejs.org/en/download/)
- [python](https://www.python.org/downloads/)

## Installation

1. [Install Brownie](https://eth-brownie.readthedocs.io/en/stable/install.html), if you haven't already. Here is a simple way to install brownie.

```bash
pip install eth-brownie
```

Or, if that doesn't work, via pipx

```bash
pip install --user pipx
pipx ensurepath
# restart your terminal
pipx install eth-brownie
```

2. For local testing [install ganache-cli](https://www.npmjs.com/package/ganache-cli)
   _Skip if you only want to use testnets_

```bash
npm install -g ganache-cli
```

or

```bash
yarn add global ganache-cli
```

3. Download the mix and install dependancies.

```bash
brownie bake upgrades-mix
cd upgrades
```

## Environment Variables

If you want to be able to deploy to testnets or work with mainnet-fork, do the following.

1. Set your `WEB3_INFURA_PROJECT_ID`, and `PRIVATE_KEY` [environment variables]

You can get a `WEB3_INFURA_PROJECT_ID` by getting a free trial of [Infura](https://infura.io/). You can find your `PRIVATE_KEY` from your ethereum wallet like [metamask](https://metamask.io/).

You'll also need testnet Goerli or Sepolia ETH. You can get ETH into your wallet by using the [sepolia faucets located here](https://sepoliafaucet.com/).

You'll also want an [Etherscan API Key](https://etherscan.io/apis) to verify your smart contracts. 

You can add your environment variables to the `.env` file:

```
export WEB3_INFURA_PROJECT_ID=<PROJECT_ID>
export PRIVATE_KEY=<PRIVATE_KEY>
export ETHERSCAN_TOKEN=<YOUR_TOKEN>
```

> DO NOT SEND YOUR KEYS TO GITHUB
> If you do that, people can steal all your funds. Ideally use an account with no real money in it.

5. Add a new network (sepolia)

You can add a new network in brownie using the following
```bash
brownie networks add Ethereum "Sepolia (Infura)" id="sepolia" host="https://sepolia.infura.io/v3/<YOUR_INFURA_PROJECT_ID>" chainid=11155111 explorer="https://api-sepolia.etherscan.io/api?apikey=<ETHERSCAN_YOUR_TOKEN>"
```

Otherwise, you can build, test, and deploy on your local environment.

# Usage

## Scripts

```
brownie run scripts/01_deploy_box.py
brownie run scripts/02_upgrade_box.py
```

This will:

1. Deploy a `Box` implementation contract
2. Deploy a `ProxyAdmin` contract to be the admin of the proxy
3. Deploy a `TransparentUpgradeableProxy` to be the proxy for the implementations

Then, the upgrade script will:

4. Deploy a new Box implementation `BoxV2`
5. Upgrade the proxy to point to the new implementation contract, essentially upgrading your infrastructure.
6. Then it will call a function only `BoxV2` can call

## Test

```
brownie test
```

## Linting

```
pip install black
pip install autoflake
autoflake --in-place --remove-unused-variables -r .
black .
```

## License

This project is licensed under the [MIT license](LICENSE)

