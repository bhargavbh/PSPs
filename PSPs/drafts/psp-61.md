# Polkadot ChainSpec

* **PSP Number:** 61
* **Authors:** @bhargavbh
* **Status:** Draft
* **Created:** 2023-05-22
* **Reference Implementation:** [Substrate ChainSpec](https://docs.rs/sc-chain-spec/latest/sc_chain_spec/struct.GenericChainSpec.html)

## Summary
This standard describes the ChainSpec format in JSON for Host implementations to connect and communicate with the Polkadot network. The ChainSpec consists of a 


## References
- [Implemetation in Substrate](https://github.com/paritytech/substrate/blob/master/client/chain-spec/src/lib.rs)

## Motivation

Host implementations (Validators/ Full-Nodes/ Light-Clients) need information to identify, communicate and connect to the network. This is loosely termed as chain specifications. A standard format for presenting and exchanging this information would streamline the process for Hosts to read, manipulate and share this data regarding the chain configuration. 
This is not included in the protocol specification because each Host may have additional parameters/data which they would like to be added in the chain specification. Genesis state, one of the mandatory fields for the protocol has been already described in the Protocol Specification. 


## Specification

### ChainSpec format

A chainSpec consists of the following fields for the client specifications:

```json
 // The human readable name of the chain.
 "name": "Polkadot",
 // The id of the chain.
 "id": "polkadot",
 // The chain type of this chain.
 // Possible values are `Live`, `Development`, `Local`.
 "chainType": "Live",
 // A list of multi addresses that belong to boot nodes of the chain.
 "bootNodes": [
   "/dns/polkadot-connect-0.parity.io/tcp/443/wss/p2p/12D3KooWEPmjoRpDSUuiTjvyNDd8fejZ9eNWH5bE965nyBMDrB4o",
    "/dns/polkadot-connect-1.parity.io/tcp/443/wss/p2p/12D3KooWLvcA24g6sT9YTaQyinwowMbLF5z7iMLoxZpEiV9pSmNf",
    "/dns/dot-bootnode.stakeworld.io/tcp/30310/p2p/12D3KooWAb5MyC1UJiEQJk4Hg4B2Vi3AJdqSUhTGYUqSnEqCFMFg",
    "/dns/p2p.3.polkadot.network/tcp/30333/p2p/12D3KooWGJQysxrQcSvUWWNw88RkqYvJhH3ZcDpWJ8zrXKhLP5Vr",
    "/dns/boot.stake.plus/tcp/30333/p2p/12D3KooWKT4ZHNxXH4icMjdrv7EwWBkfbz5duxE5sdJKKeWFYi5n"
 ],
 // Optional list of "multi address, verbosity" of telemetry endpoints.
 // The verbosity goes from `0` to `9`. With `0` being the mode with the lowest verbosity.
 "telemetryEndpoints": [
   [
     "wss://telemetry.polkadot.io/submit/",
      0
   ]
 ],
 // Optional networking protocol id that identifies the chain.
 "protocolId": "dot",
 // Optional fork id. Should most likely be left empty.
 // Can be used to signal a fork on the network level when two chains have the
 // same genesis hash.
 "properties": {
   "tokenDecimals": 10,
   "tokenSymbol": "DOT"
 },
  ```


It can additionally include extensions to the client specifications like:

```json
 // List of known Bad Block header hashes that are not imported by the Host. Value is null if there are no Bad Blocks.
 "badBlocks": [
    "0x15b1b925b0aa5cfe43c88cd024f74258cb5cfe3af424882c901014e8acd0d241",
    "0x2563260209012232649ab9dc003f62e274c684037de499a23062f8e0e816c605"
  ],
 // List of known fork Block header hashes to be imported at the particular block heights. Value is null if there are no fork blocks. 
"forkBlocks": [
    ("01e94e1e7e9cf07b3b0bf4e1717fce7448e5563901c2ef2e3b8e9ecaeba088b1", 1492283),
    ("ddc4323c5e8966844dfaa87e0c2f74ef6b43115f17bf8e4ff38845a62d02b9a9", 1492436),
    ()"38ba115b296663e424e32d7b1655cd795719cef4fd7d579271a6d01086cf1628", 1492586)
  ]
  ```

The genesis state it the format described in the Polkadot Specifications is also included in the chainpec. The genesis state can be set as either `raw`, `stateRotHash`, or through runtime mechanism like [`Genesis Configurations`](https://docs.substrate.io/build/genesis-configuration/):
```json
 // The genesis declaration of the chain.
 //
 // `runtime`, `raw`, `stateRootHash` denote the type of the genesis declaration.
 //
 // These declarations are in the following formats:
 // - `runtime` where the genesis state is declared by a runtime through methods like `GenesisConfig`  and opaque to the node.
 // - `raw` is a `json` object with two fields `top` and `children_default`. Each of these
 //   fields is a map of `key => value`. These key/value pairs represent the genesis storage.
 // - `stateRootHash` is a single hex encoded hash that represents the genesis hash. The hash
 //   type depends on the hash used by the chain.
 
 "genesis": { "runtime": {} },

 // Optional map of `block_number` to `wasm_code`.
 //
 // The given `wasm_code` will be used to substitute the on-chain wasm code starting with the
 // given block number until the `spec_version` on-chain changes. The given `wasm_code` should
 // be as close as possible to the on-chain wasm code. A substitute should be used to fix a bug
 // that can not be fixed with a runtime upgrade, if for example the runtime is constantly
 // panicking. Introducing new runtime apis isn't supported, because the node
 // will read the runtime version from the on-chain wasm code. Use this functionality only when
 // there is no other way around it and only patch the problematic bug, the rest should be done
 // with a on-chain runtime upgrade.
 "codeSubstitutes": [],
```




## Copyright

Each PSP must be labeled as placed in the [public domain](https://creativecommons.org/publicdomain/zero/1.0/).
