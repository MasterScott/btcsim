btcsim
=======

[![Build Status](https://travis-ci.org/conformal/btcsim.png?branch=master)]
(https://travis-ci.org/conformal/btcsim)

btcsim implements a simulation test driver using the `simnet` network provided
by `btcd`. It launches the required nodes, manages them, runs the simulation
and cleans up when done, all by itself, so there's no manual setup/teardown
involved. btcsim uses `btcrpcclient` to interface with the RPC servers.

The input is read from a CSV file with the following headers:

    | block height | minimum utxos required | transactions required |

Example:

    20000,40000,20000
    20001,60000,30000

Based on the input, the minimum required number of utxos are generated by
creating transactions using the existing utxos and dividing them until the
required number is reached.

The required number of transactions are generated by simulating transactions
between multiple actors and using the utxos generated previously.

Mining is controlled using the RPC clients to ensure that the blocks are
generated only when required i.e. when the minimum required number of utxos and
transactions are created.

## Components

### Node

This is the first `btcd` node that is launched. It acts as the server for all
Actors and as a peer for the miner.

### Actor

An Actor simulates a wallet "Agent" by launching a `btcwallet` instance which
connects to the node server. It is responsible for generating
transactions among itself as well as other actors. Multiple actors can be
launched simultaneously to simulate a large load due to a heavy multi-user
system.

### Miner

The Miner launches a `btcd` instance and simulates a mining node. It is
connected to the node server using the `addnode` RPC call. It is
responsible for collecting transactions and mining them when required.

## Installation

btcsim depends on `btcd` and `btcwallet`, so install those first

```bash
$ go get github.com/conformal/btcd
```

```bash
$ go get github.com/conformal/btcwallet
```

Now install btcsim

```bash
$ go get github.com/conformal/btcsim
```

## Usage

Invoking the command `btcsim` without any flags initializes a single actor
simulation, using a default linear curve of 0-10,000 transactions per block:

```bash
$ btcsim
```

For more options, see:

```bash
$ btcsim --help
```

## License

Package btcsim is licensed under the liberal ISC License.
