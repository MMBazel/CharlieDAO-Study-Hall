**Answers to most common Crypto Questions by CharlieDAO**

Q1 - So what exactly happens when I sign a transaction, end-to-end? e.g. I initiate a swap on Uniswap. Does my transaction request go to a provider assigned by Metamask? Or is it one set up by the DApp?

_You sign transaction, it becomes a viable transaction in the public "mempool" - the pool of transactions transmitted via the nodes peer to peer (P2P) "gossip" so nodes are aware of what is available to be added to a block. My understanding is MetaMask uses your RPC specifications to transmit this (thus, for certain private transactions, you would use the flashbots RPC to transmit it to reduce MEV viability). This is a giant topic, but this is my limited understanding._

Q2 - If Ethereum is the “World Computer” does that mean every node runs the code for my transaction? At what point do they all do this? Does everything stop (e.g. mining, block creation) until they’ve all ran the code for every transaction on the block?

_When you create a new full node - it does verify every transaction ever since the genesis block. But light clients only verify the headers of past transactions and then continue forward. Not every node runs every transaction - they compete to construct blocks of transactions they verified and then have that block get added (Proof of Work Consensus). Once a block if verified, the verification process is less computationally intensive - because the work has already been proven. But my understanding is nodes strategically accept valid blocks (without doing the full verification) in order to continue focusing on constructing their own blocks & extract MEV._

Q3 - Is there a difference between an ethereum validator node and a miner? 

_Nodes are separate from miners; but typically all miners also run nodes. You can run a node for data reasons separate from mining for profit reasons. _

Q4 - If roll-ups and other layer 2s are just side chains for Ethereum mainnet, why is it that my transactions (e.g. using MATIC on Polygon) happen on a different network (the Polygon network)? If they truly were side chains, wouldn’t a transaction I did on polygon propogate to Ethereum mainnet somehow?

_Roll-ups are different than side chains. Arbitrum & Optimism requiring bridging because they use a validation scheme that allows 7 days for transactions to be contested (i.e. withdrawing from them requires 7 days)- but all transactions on their network are "rolled up" and combined into call data that is submitted to the mainnet. This makes them a "true" L2 - in that the state of accounts is verifiable using only mainnet calldata (this "security inheritance" argument). While Polygon uses a different scheme I don't totally understand, but it essentially "checkpoints" itself onto Ethereum- but not by bundling all transactions and submitting them as call data. True Alt L1 chains like Avalanche, Fantom, BSC, etc. don't make any calls to mainnet nor submit data to mainnet; so they are not L2s. In my mind, Polygon is its own chain with its own token, but for marketing purposes sets itself up to break if mainnet breaks LOL. That's how a lot of devs in the space describe it._

Q5 - What are the practical differences between deploying a smart contract to the following places? I’m assuming that optimizations / sacrifices are made to make e.g. testing easier and faster, but I’d love those differences enumerated. mainnet testnet my own local node (e.g. geth) “hardhat”

_Local is for contract in isolation; hardhat/ganache/etc. often use an API (alchemy, infura) to access a formal mainnet fork (someone literally copy/pastes all of ethereum so you can interact with other contracts, e.g., interfacing with tiemstamped copy of production AAVE). testnet is similar to above, but more closely resembles production environment- AAVE will copy its contracts onto testnets to support developer interoperability, but those contracts will of course have totally different amounts of money & interest rates, etc. mainnet is production_


Q6 So I sign a transaction using Metamask. 
- Where is this public mempool physically stored? With everyone running nodes?
- How does it get to the mempool? You mentioned MetaMask using your RPC specifications, does that mean Metamask takes care of that by default?

_A pending tx is sent to a node via your RPC specifications (i.e. you could pay to use a private RPC and have your transactions protected from some kinds of MEV) . The nodes then gossip the pending transaction via P2P (so that multiple nodes have overlapping transactions in their set of available transactions - the collective of these available tx is called the 'mempool' which is queried for transactions) - see here: https://www.quicknode.com/guides/defi/how-to-access-ethereum-mempool_

Q7. Hmm, I understand that, because of proof-of-work, you can verify blocks without actually having to find the appropriate hash (i.e. do the work). But I was under the impression all the nodes have to actually do the computation. There's no way to prove that you did the correct computation without actually doing it......... right? Otherwise why would you ever have more than one node executing any given transaction?

_Miners do computation in parallel, many miners will compute the same transactions as they organize them into blocks (and ensure they are valid, e.g., that slippage is within tolerance for a Uniswap trade), but only the one that makes the winning block will get the block emission. Some blocks can be valid, but not be part of the longest blockchain (Greedy Heaviest Object something something "GHOST") this makes them "orphans"._

Q8. I'd figure you have to run a node if you want to mine, no? Is all you need access to the mempool? I figure you need some kind of eth client to mine, and that makes you a node.

_Yeah actually I think all miners do run a minimum type of node, but I think they can run light or full nodes; then there's also archive nodes. The multiple clients & multiple node types is a whole thing._

Q9. Interesting! So if I switch my network to Arbitrum or Optimism, I can move around any ERC-20 tokens I have on mainnet?

_No, I actually thought this too, I thought the L2s were at the dApp level, but it doesn't feel different than bridging to Fantom or Avalanche. You bridge, you do the tx available on the L2 dapps, then you bridge back. I think the key distinction is that if the L2 broke (including the bridge), mainnet would have the calldata for all the tx done on the L2 and be able to sort out a kind of "unbridging" of funds in case of a massive arbitrum catastrophe. But my honest opinion is that until L2s actually feel better than alt L1s like Avalanche, people aren't going to value the security much. Of course what's interesting about L2s is that the transaction cost per user decreases as there are more users. Which is inverse from an alternative L1 (which use their own fee markets as opposed to sharing mainnet fees)_
