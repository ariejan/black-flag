# Black Flag

[![Build Status](https://travis-ci.org/ariejan/black-flag.svg?branch=master)](https://travis-ci.org/ariejan/black-flag)

Black Flag offers a multi-node regtest setup, including a fully operational
bitcore-node, including insight and bitcoin-wallet-service. This means you 
can use [CoPay](https://copay.io/) as a client on your regtest network.

Black Flag is intended for development purposes on `regtest` only. Things
are designed to be cleaned, wiped and restarted. Do not alter the 
configuration to run `livenet` because you will lose Bitcoin if you do.

## Getting started

It's recommended you run Black Flag locally. 

Because Black Flag is fully dockerized, except a few convenience scripts,
all you need is Docker and Docker Compose. The CoPay Chrome App is 
recommended, but not required.

Clone this repository and use `docker-compose` to create the
necessary docker images and start all nodes. 

```
git clone https://github.com/ariejan/black-flag.git
cd black-flag
docker-compose build
docker-compose start
```

This will spin up three containers:

 * node_one: The first node running `bitcoind` in regtest mode.
 * node_two: The second node running `bitcoind` in regtest mode.
 * insight: Running insight, insight-api and bws

This will also expose Insight and Bitcore Wallet Service (BWS) on the 
following ports:

 * Insight: http://localhost:3001/insight
 * Insight API: http://localhost:3001/insight-api/version
 * Bitcore Wallet Service API: http://localhost:3232/bws/api/v1/version

## Mining Blocks

Regtest allows you to generate new blocks fast. Bitcoin mined remains
locked for 100 blocks. So if you mine your 1ste block, you'll need to
mine another 100 to unlock the 50BTC mined. Luckily, this is quite 
painless. 

For convenience, use the `one` and `two` scripts to execute `bitcoin-cli`
commands to either `node_one` or `node_two`. 

To mine (or generate) 101 new blocks, run this command from the black flag
directory:

```
./one generate 101
```

## Bootstrap 

To get up and running fast, you can use `bootstrap`. It will generate
200 blocks, transfer 100 BTC to `mxqYXyf4NiFYaLGkvL1zMtYgcqbXbeLFg6` and 
create 10 confirmations.

See how to setup Copay and import the wallet with the following 
recovery phrase:

```
ask square affair adjust gauge giant that hole gate egg bitter sustain
```

## Setup Copay

Copay can be configured per wallet. This means you can add a wallet 
and configure it to use your new Black Flag regtest bitcoin network. 

 1. Create a 'New personal wallet'
 2. Under 'Advanced options'
   1. Wallet Service URL: `http://localhost:3232/bws/api`
   2. Testnet: Yes

If you want to use the Bootstrapped wallet, choose 'Import wallet' instead,
enter the recovery phrase above and make adjust the 'Testnet' and 'Wallet
Service URL' options the same as when creating a new wallet.

## Wipe blockchain

In case you want to start from scratch you can wipe regtest blockchain
data and restart all nodes. Use `./wipe` to do this for you.

If you really messed things up, stop all nodes, rebuild them from
scratch and start everything up again.

```
docker-compose down
docker-compose build --no-cache
docker-compose start
```

Note: your Copay wallet will get confused if previously known transactions
disappear from the blockchain. It's best to delete the wallet and create
a new one (or import again with a recover phrase).

## Credits

Development by [Ariejan de Vroom](https://www.devroom.io)

Inspired by

 * [bitcoin-testnet-box](https://github.com/freewil/bitcoin-testnet-box).
 * jeshan's [bitcore-node](https://github.com/jeshan/bitcore-node)

## Donations are very welcome!

Donations keep me motivated to continue working on Black Flag. 

 * BTC: `159j4gLD5oKXj6yfjTSfihmjArekcPRcBU`
 * ETH: `0x9A30b3456Ac8f818807ae15BCa0b881035D75eA4`
 

