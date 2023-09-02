Hyperledger is an open source project where we can be able to develop blockchain on it. It has many frameworks and tools in order to build that.
The very famous one is Hyperledger Fabric.

# Hyperledger Fabric

Let's say there are 5 nodes A , B , C , D , E in a blockchain network. If A and D are competitors, whatever transactions done between A and B, D can able to see it since they have same ledger.
In order to mitigate that A and B can have a subnet. Moreover, Fabric is a private blockchain. We call that subnets as Channels in Fabric.

If we want to enroll to this network, we have to request to MSP (Membership Service Provider).

To make a transaction in the newtork and store data, we've to write Chaincode in the Fabric. (Same as Smart Contract in Blockchain). Chaincode is not a language it's a program. We can able to implement it in Java, Js, Golang. Fabric is built with Go.

Fabic divided the ledger into two parts:
- World State
- Transaction Log

World State is used for storing the current state. 
Transaction log is used for storing the history of transaction.

## Hyperledger Fabric (Private Blockchain)

In a network of peers, each peer must have ledger and chaincode (Smart Contract). To participate in the private blockchain network, not only the certificate from CA helps. There is a service called MSP (Membership Service Provider) will decide who can be part of network and what are their roles. 

The orderer peer is responsible for adding the transaction log into block which will be further added to blockchain. In a network of A,B,C,D when A and B made transaction, it should not visible for other peers. So that we can able to create channels. For example if A needs to send transaction to B, A and B can be one channel. If A needs to send transaction to C, A and C can be one channel








