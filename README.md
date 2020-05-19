# Bootstrap libp2p DHT using my own node: failed to find any peer in table

Hi guys,

I am trying to deploy a private DHT using libp2p independetenly from ipfs.
Souce code of the node is available at https://github.com/lrahmani/snipet-routed-host-bootstrap. 
I wrote it by piecing together different snippets from libp2p official examples (mostly routed echo).

The issue I am facing is that I cannot find peers using the dht other than the one I bootstraped from. This doesn't 
happen when I use ipfs public DHT.

## Setup when bootstrapping using ipfs public DHT node 

- first, get the code

      git clone https://github.com/lrahmani/snipet-routed-host-bootstrap.git && cd snipet-routed-host-bootstrap
      
- then, build it

      go build
      
- run the node with an ipfs public node as bootstrap

      ./node -l 9000 -b /ip4/104.131.131.82/tcp/4001/ipfs/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ

      2020/05/19 06:22:32 Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR bootstrapping to QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:22:33 context.Background bootstrapDialSuccess QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:22:33 bootstrapped with QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:22:33 context.Background bootstrapDial Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:22:33 My ID is : Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR
      2020/05/19 06:22:33 I can be reached at:
      2020/05/19 06:22:33 /ip4/127.0.0.1/tcp/9000/p2p/Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR
      2020/05/19 06:22:33 /ip4/192.168.178.21/tcp/9000/p2p/Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR
      2020/05/19 06:22:33 /ip4/192.168.122.1/tcp/9000/p2p/Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR
      2020/05/19 06:22:33 /ip4/172.17.0.1/tcp/9000/p2p/Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR
      2020/05/19 06:22:33 listening for connections

- run a second node with an ipfs public node as bootstrap, and instruct it to dial the first one by its `peer.ID`
      
      ./node -l 9001 -b /ip4/104.131.131.82/tcp/4001/ipfs/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ -d Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR
      
      2020/05/19 06:32:38 QmXn5ha1dWwDRsqpbzXg8ioCnK52irZSLv8qaMgdLHHZoc bootstrapping to QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:32:39 context.Background bootstrapDialSuccess QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:32:39 bootstrapped with QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:32:39 context.Background bootstrapDial QmXn5ha1dWwDRsqpbzXg8ioCnK52irZSLv8qaMgdLHHZoc       QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
      2020/05/19 06:32:39 My ID is : QmXn5ha1dWwDRsqpbzXg8ioCnK52irZSLv8qaMgdLHHZoc
      2020/05/19 06:32:39 I can be reached at:
      2020/05/19 06:32:39 /ip4/127.0.0.1/tcp/9001/p2p/QmXn5ha1dWwDRsqpbzXg8ioCnK52irZSLv8qaMgdLHHZoc
      2020/05/19 06:32:39 /ip4/192.168.178.21/tcp/9001/p2p/QmXn5ha1dWwDRsqpbzXg8ioCnK52irZSLv8qaMgdLHHZoc
      2020/05/19 06:32:39 /ip4/192.168.122.1/tcp/9001/p2p/QmXn5ha1dWwDRsqpbzXg8ioCnK52irZSLv8qaMgdLHHZoc
      2020/05/19 06:32:39 /ip4/172.17.0.1/tcp/9001/p2p/QmXn5ha1dWwDRsqpbzXg8ioCnK52irZSLv8qaMgdLHHZoc
      2020/05/19 06:32:39 DEBUG looking for peer Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR
      2020/05/19 06:32:40 DEBUG found addresses for peer Qma4U9ix2caGVy1HLYDwpHe6WURv4A5trukY686UrZ8yqR:
      2020/05/19 06:32:40 [/ip4/192.168.178.21/tcp/9000 /ip4/192.168.122.1/tcp/9000 /ip4/172.17.0.1/tcp/9000 /ip4/217.155.39.193/tcp/9000]
      2020/05/19 06:32:40 opening stream
      2020/05/19 06:32:40 read reply: "Hello, world!\n"

      
- Message is received by node 1 and successfully echoed back to node 2
      
      # node 1
      2020/05/19 06:32:40 Got a new stream!
      2020/05/19 06:32:40 read: Hello, world!
      
## Setup when bootstrapping using my own node

- run a first node 

      ./node -l 9000
      
      2020/05/19 06:43:13 My ID is : Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:13 I can be reached at:
      2020/05/19 06:43:13 /ip4/127.0.0.1/tcp/9000/p2p/Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:13 /ip4/192.168.178.21/tcp/9000/p2p/Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:13 /ip4/192.168.122.1/tcp/9000/p2p/Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:13 /ip4/172.17.0.1/tcp/9000/p2p/Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:13 listening for connections


- run a second node with the first node as bootstrap (use one of its multiaddrs)
      
      ./node -l 9001 -b /ip4/127.0.0.1/tcp/9000/p2p/Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:26 QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf bootstrapping to Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:26 context.Background bootstrapDialSuccess Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:26 bootstrapped with Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:26 context.Background bootstrapDial QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:43:26 My ID is : QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      2020/05/19 06:43:26 I can be reached at:
      2020/05/19 06:43:26 /ip4/127.0.0.1/tcp/9001/p2p/QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      2020/05/19 06:43:26 /ip4/192.168.178.21/tcp/9001/p2p/QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      2020/05/19 06:43:26 /ip4/192.168.122.1/tcp/9001/p2p/QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      2020/05/19 06:43:26 /ip4/172.17.0.1/tcp/9001/p2p/QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      2020/05/19 06:43:26 listening for connections
      
- run a third node with the first node as bootstrap, and instruct it to dial the second node
      
      ./node -l 9002 -b /ip4/127.0.0.1/tcp/9000/p2p/Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH -d QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      
      2020/05/19 06:46:36 Qmdy8EQbFPTAuS8FmAU7BVfh1YRio3zB7K47RbCSb6EFZw bootstrapping to Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:46:36 context.Background bootstrapDialSuccess Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:46:36 bootstrapped with Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:46:36 context.Background bootstrapDial Qmdy8EQbFPTAuS8FmAU7BVfh1YRio3zB7K47RbCSb6EFZw Qme1tMLGtBfbWibguT8VaKdPdLgVrtrs6xW1mSHM6KpdkH
      2020/05/19 06:46:36 My ID is : Qmdy8EQbFPTAuS8FmAU7BVfh1YRio3zB7K47RbCSb6EFZw
      2020/05/19 06:46:36 I can be reached at:
      2020/05/19 06:46:36 /ip4/127.0.0.1/tcp/9002/p2p/Qmdy8EQbFPTAuS8FmAU7BVfh1YRio3zB7K47RbCSb6EFZw
      2020/05/19 06:46:36 /ip4/192.168.178.21/tcp/9002/p2p/Qmdy8EQbFPTAuS8FmAU7BVfh1YRio3zB7K47RbCSb6EFZw
      2020/05/19 06:46:36 /ip4/192.168.122.1/tcp/9002/p2p/Qmdy8EQbFPTAuS8FmAU7BVfh1YRio3zB7K47RbCSb6EFZw
      2020/05/19 06:46:36 /ip4/172.17.0.1/tcp/9002/p2p/Qmdy8EQbFPTAuS8FmAU7BVfh1YRio3zB7K47RbCSb6EFZw
      2020/05/19 06:46:36 DEBUG looking for peer QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      2020/05/19 06:46:36 DEBUG Didn't found addresses for peer QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf
      2020/05/19 06:46:36 DEBUG found addresses for peer QmQacZT1cWoaYV4oUwaRduj4Kdt6JUEzTqX5M82AJD3REf:
      2020/05/19 06:46:36 []
      2020/05/19 06:46:36 opening stream
      2020/05/19 06:46:36 failed to find any peer in table

- third node *instantaneously* returns `failed to find any peer in table`

Could you please instruct me on how to solve this issue?

Thanks a lot
