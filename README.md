# Tetcore-to-Tetcore Bridge Relay

The bridge relay is a process that connects to running Tetcore nodes and sends data over the Tetcore-to-Tetcore bridge. The process communicates with the nodes over the JSON-RPC interface and reads data from the relays information required by the `bridge` pallet using runtime calls and writes data to the modules by constructing and submitting extrinsics.

For more details, see the [design document](doc/design.md).

## Status

This is a not-in-progress prototype.

**The implementation efforts continue in [Tetsy Bridges](https://github.com/tetcoin/tetsy-bridges-common) repository.**

## Running in development

Run two development Tetcore chains:

```bash
> TMPDIR=(mktemp -d)
> cd $TMPDIR
> tetcore build-spec --dev > red-spec.json
> cp red-spec.json blue-spec.json
# Modify the chain spec in an editor so that the genesis hashes of the two chains differ.
# For example, double one of the balances in '$.genesis.runtime.balances.balances'.
> tetcore --chain red-spec.json --alice --base-path ./red --port 30343 --ws-port 9954
> tetcore --chain blue-spec.json --alice --base-path ./blue --port 30353 --ws-port 9964
```

Now run the bridge relay:

```
> target/release/tetcore-bridge --base-path ./relay \
    --rpc-url ws://localhost:9954 \
    --rpc-url ws://localhost:9964
```
