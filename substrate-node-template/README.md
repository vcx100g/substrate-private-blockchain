# Launch private network

full tutorial here
`https://docs.substrate.io/tutorials/v3/private-network/`

  - How to start and stop peer blockchain nodes.
  - How to generate your own secret key pairs.
  - How to create a custom chain specification that uses the keys you generated.
  - How to add validators to a private network that uses your custom chain specification.

Purge old chain data
```shell
./target/release/node-template purge-chain --base-path /tmp/node01 --chain local -y
```

Start the first node using the custom chain
```shell
./target/release/node-template \
--base-path /tmp/alice \
--chain local \
--alice \
--port 30333 \
--ws-port 9945 \
--rpc-port 9933 \
--node-key 0000000000000000000000000000000000000000000000000000000000000001 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator
```

_node-key is for development only_

### Local node identity

display when blockchain started

`12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX.`

## add a second validator
Enable other participants to join

```shell
./target/release/node-template purge-chain --base-path /tmp/bob --chain local -y
```
```shell
./target/release/node-template \
--base-path /tmp/node02 \
--chain ./customSpecRaw.json \
--port 30334 \
--ws-port 9946 \
--rpc-port 9934 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--rpc-methods Unsafe \
--name MyNode02 \
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX
```
change `12D3KooWLmrYDLoNTyTYtRdDyZLWDe1paxzxTw5RgjmHLfzW96SX`
to Local node identity

check apps to see it work

### Generate your own keys

in project root
```shell
./target/release/node-template key generate --scheme Sr25519 --password-interactive
```
copy the output
```
  Secret phrase:     turkey public want access hospital decide inhale tent accuse ranch shrimp effort
  Secret seed:       0x1c19a20ea6c5319f10eef9d3c567762ed51c0321121f4414e6db42461664d485
  Public key (hex):  0x4cf828a13b637e3517b6195418cf21a3017cf59e6874dbbdb9e138cffded571c
  Account ID:        0x4cf828a13b637e3517b6195418cf21a3017cf59e6874dbbdb9e138cffded571c
  Public key (SS58): 5DodDRHfjWeJDjSSXmWhaemGLscT6omuN6R4SAkowACVtdBd
  SS58 Address:      5DodDRHfjWeJDjSSXmWhaemGLscT6omuN6R4SAkowACVtdBd
```

add secret seed key to node
```shell
./target/release/node-template key inspect --password-interactive --scheme Ed25519 0x1c19a20ea6c5319f10eef9d3c567762ed51c0321121f4414e6db42461664d485
```
get output and save public key

```
  Secret Key URI     `0x1c19a20ea6c5319f10eef9d3c567762ed51c0321121f4414e6db42461664d485` is account:
  Secret seed:       0x1c19a20ea6c5319f10eef9d3c567762ed51c0321121f4414e6db42461664d485
  Public key (hex):  0x5cfa01bedf81b06fb925123548f6047820d3906e261be65aeabe01eaf548ff13
  Account ID:        0x5cfa01bedf81b06fb925123548f6047820d3906e261be65aeabe01eaf548ff13
  Public key (SS58): 5EAcXhUp48d2FtnkdKLcc2GVvsWaYA53jCSmKDE2UNiKH6dP
  SS58 Address:      5EAcXhUp48d2FtnkdKLcc2GVvsWaYA53jCSmKDE2UNiKH6dP
```

generate second key

```
  Secret phrase:       analyst shoot clay dust such toy scout pool filter give throw crack
  Secret seed:       0x403e4e7a0ecc4c3033683cdfa2406e2552c0ab04ddf5b458371c5e568d5d6673
  Public key (hex):  0xd0c5500c6d7edac71a65240ad2553f0c678db64ee4f7902c5c46b7196f383b09
  Account ID:        0xd0c5500c6d7edac71a65240ad2553f0c678db64ee4f7902c5c46b7196f383b09
  Public key (SS58): 5GnSRy7hFf5Gjs14kjttcPWnFierK59JnbPNiFTzBxnuivL7
  SS58 Address:      5GnSRy7hFf5Gjs14kjttcPWnFierK59JnbPNiFTzBxnuivL7
```

```
  Secret Key URI `0x403e4e7a0ecc4c3033683cdfa2406e2552c0ab04ddf5b458371c5e568d5d6673` is account:
  Secret seed:       0x403e4e7a0ecc4c3033683cdfa2406e2552c0ab04ddf5b458371c5e568d5d6673
  Public key (hex):  0x8a4decdab5c034b15f4a854b97ce9ae6d72540b99f961608191583c77939f372
  Account ID:        0x8a4decdab5c034b15f4a854b97ce9ae6d72540b99f961608191583c77939f372
  Public key (SS58): 5FC3cZKd8qr7njsNGzsfVReCgyp8LUekKTLJ2VNJV9PHLLBn
  SS58 Address:      5FC3cZKd8qr7njsNGzsfVReCgyp8LUekKTLJ2VNJV9PHLLBn
```

## Modify an existing chain specification

```shell
./target/release/node-template build-spec --disable-default-bootnode --chain local > customSpec.json
```

## Modify an existing chain specification

create customSpec.json
```shell
./target/release/node-template build-spec --disable-default-bootnode --chain local > customSpec.json
```

set detailed
```json
{
  "name": "My Custom Testnet",
  "aura": {
    "authorities": [
      "5CfBuoHDvZ4fd8jkLQicNL8tgjnK8pVG9AiuJrsNrRAx6CNW",
      "5EJPj83tJuJtTVE2v7B9ehfM7jNT44CBFaPWicvBwYyUKBS6"
    ]
  },
  "grandpa": {
    "authorities": [
      [
        "5CuqCGfwqhjGzSqz5mnq36tMe651mU9Ji8xQ4JRuUTvPcjVN",
        1
      ],
      [
        "5FeJQsfmbbJLTH1pvehBxrZrT5kHvJFj84ZaY5LK7NU87gZS",
        1
      ]
    ]
  },
}
```

### Convert the chain specification to use the raw format

```shell
./target/release/node-template build-spec --chain=customSpec.json --raw --disable-default-bootnode > customSpecRaw.json
```

## Start both node

```shell
./target/release/node-template \
--base-path /tmp/node01 \
--chain ./customSpecRaw.json \
--port 30333 \
--ws-port 9944 \
--rpc-port 9933 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--rpc-methods Unsafe \
--name MyNode01

./target/release/node-template \
--base-path /tmp/node02 \
--chain ./customSpecRaw.json \
--port 30334 \
--ws-port 9945 \
--rpc-port 9935 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--rpc-methods Unsafe \
--name MyNode02
```

## Launch the private network

1. You have generated or collected the account keys for at least two authority accounts.
2. You have updated your custom chain specification to include the keys for block production (aura) and block finalization (grandpa).
3. You have converted your custom chain specification to raw format and distributed the raw chain specification to the nodes participating in the private network.

## Add keys to the keystore

After you start the first node, no blocks are yet produced. The next step is to add two types of keys to the keystore for each node in the network.

secret-seed from aura key
```shell
./target/release/node-template key insert --base-path /tmp/node01 \
--chain customSpecRaw.json \
--suri <your-secret-seed> \
--password-interactive \
--key-type aura \
--scheme Sr25519
```
`0x1c19a20ea6c5319f10eef9d3c567762ed51c0321121f4414e6db42461664d485`

can also add from file
```shell
./target/release/node-template key insert --help
```

secret-seed from grandpa key
```shell
./target/release/node-template key insert --base-path /tmp/node01 \
--chain customSpecRaw.json \
--suri <your-secret-key> \
--password-interactive \
--key-type gran \
--scheme Ed25519
```
`0x1c19a20ea6c5319f10eef9d3c567762ed51c0321121f4414e6db42461664d485`

### verify key set

remember change folder directory
```shell
ls /tmp/node01/chains/ong_testnet/keystore
```

### rejoin network

```shell
./target/release/node-template \
--base-path /tmp/node01 \
--chain ./customSpecRaw.json \
--port 30333 \
--ws-port 9945 \
--rpc-port 9933 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--rpc-methods Unsafe \
--name MyNode01

./target/release/node-template \
--base-path /tmp/node02 \
--chain ./customSpecRaw.json \
--port 30334 \
--ws-port 9946 \
--rpc-port 9934 \
--telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
--validator \
--rpc-methods Unsafe \
--name MyNode02 \
--bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWF9XqqboRLkfai8JpmvWTnoRVWjvrwZUMwE6cu99JaYfX
```
replace the node identity to node01
`12D3KooWF9XqqboRLkfai8JpmvWTnoRVWjvrwZUMwE6cu99JaYfX`

bootnode need to be valid

# Host own Polkadot-JS Explorer

if not work use
`https://polkadot.js.org/apps/`

`https://github.com/polkadot-js/apps#development`

git clone then install
```shell
$ git clone https://github.com/polkadot-js/apps <optional local path>
$ yarn
```

or run docker container

_remember to use host network_
```shell
$ docker run --rm -it --name polkadot-ui -e WS_URL=ws://0.0.0.0:9945 -p 80:80 --network host jacogr/polkadot-js-apps:latest
```

if build explorer for local change
```shell
$ docker build -t jacogr/polkadot-js-apps .
```

### custom endpoints for polkadot js explorer

set development local node




