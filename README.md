# Bootstrap libp2p DHT using my own node

Hi guys,

I am trying to deploy a private DHT locally (as a starter) using libp2p independetenly from ipfs.
Souce code of the node is available at https://github.com/lrahmani/snipet-routed-host-bootstrap. 
I wrote it by piecing together different snippets from libp2p official examples.

The issue I am facing is that I cannot find peers using the dht other than the one I bootstraped from. This doesn't 
happen when I use ipfs public DHT.

## Setup when bootstrapping using ipfs public DHT node 

- first, get the code
- then, build it
- run a node with ipfs bootstrap
- run a second node with ipfs bootstrap and instruct it to send 'Hello World' to the first one
- Message is received by node 1 and successfully echoed back to node 2

## Setup when bootstrapping using my own node

- run a node 
- run a second node with the one of first node's multiaddrs
- run a third node with one of first node's multiaddrs and instruct it to send `Hello World` to the second node
- Third node cannot find the second peer using its `peer.ID`

Could you please instruct me on what I am doing wrong?

Thanks a lot
