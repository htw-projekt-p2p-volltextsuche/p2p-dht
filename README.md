# p2p-dht

Part of a project at HTW to build a distributed full-text-search. [See the rest
of the project.](https://github.com/htw-projekt-p2p-volltextsuche)

This repo implements the DHT that hosts the entries of the inverted index for
the [full-text-search component](https://github.com/htw-projekt-p2p-volltextsuche/fulltext-search).

The [slides](https://github.com/htw-projekt-p2p-volltextsuche/vortrag/blob/master/slides.pdf)
of our closing presentation include an explaination of how this whole project
fits together and takes a deeper look into what this component does if you want
to know more.

## Design

**Entire Project**

- [Architechture Discussion and Diagramms](https://github.com/htw-projekt-p2p-volltextsuche/planning-and-design/tree/main/architecture)
- [User Stories](https://github.com/htw-projekt-p2p-volltextsuche/planning-and-design/projects/1)
- [Our Presentation including an elaboration on two design decisions](https://github.com/htw-projekt-p2p-volltextsuche/vortrag/blob/master/slides.pdf)

**This Component**

- [API Specification](docs/openapi.yaml), [API Examples](docs/routes.md)
- [Framework Choice and Data Distribution Design](docs/data-distribution-design.md#readme)

## Run

Install:

```sh
npm install
```

Development:

```sh
npm start
```

Production:

```sh
npm run start-prod
```

## Configure

Configured via environmental variables in `variables.env`.
See: [`variables.env.example`](variables.env.example).

### `HTTP_PORT`

Port for the http server to run on.

Default: `8090`

### `HTTP_LIMIT`

The maximum request body size.

Default: `10mb`

See: [body-parser](https://www.npmjs.com/package/body-parser#limit-1)

### `PEER_PORT`

Port for the p2p node to run on.

Default: `8070`

### `PEER_IP`

IP for the p2p node to run on.

Default: `127.0.0.1` (localhost)

### `PEER_LIST`

List of peers, required for joining the network. Leave blank on first peer, that
is "creating" the network.

Default: empty

Format:
  - [multiaddr](https://multiformats.github.io/js-multiaddr/#what-is-multiaddr)
    incl. ipfs address <!--Todo: describe how to get the ipfs address-->
  - seperated by commas

Example:

```sh
PEER_LIST=/ip4/127.0.0.1/tcp/8071/ipfs/QmdW3RF4Yq4acYc4bgUmxeuJQLb2mQpQmMuDTGir5gQcYM, /ip4/127.0.0.1/tcp/8072/ipfs/QmPP5pdu6Dh93DL7LnQkKU2x8m4BoSrQswjQR5q26PMneg
```

You have to fill this list manually with the multiaddresses of the other peers.
You do this by configuring a peer through the above options (ports and ip) and
running the `npm run addr` script to show you that specific peers multiaddress,
which can then be used in another peers `PEER_LIST` to connect to this peer.

### `PEER_STORAGE`

Storage location on disk for the data of the DHT.

Default: `/tmp/datastore`

### `PEER_REDUNDANCY`

Specifies on how many different remote nodes a value should be stored.

Default: 2

### `PEER_MPLEX_SIZE`

Specify the max message size of the p2p net's
[multiplexer](https://github.com/libp2p/js-libp2p-mplex).

Default: `10mb`

### `DEBUG`

Enable logging in the p2p network.

Set it to:

```
libp2p:dht:Q*,libp2p:dht:rpc:get-value:*
```

## Scripts

**Multiaddress:**

Print the multiaddress used for connection to the node:

```sh
npm run addr
```

**Debug:**

Print the environmental variables that are being read in:

```sh
npm run debug
```

**Node testing:**

Run a node for testing:

```sh
npm run spawn 1
```

**Generate peerId locally:**

Create the `peerId.json` locally.

```sh
npm run local-peerid
```
