# Test task for Cryptographer Researcher

## Description
  
Create a prototype of an Ethereum crypto wallet (or provide enough technical and practical documentation for it to be created) that uses Multi-Party Computation to sign transactions. The prototype shall fulfil the following requirements: 

- For every wallet, 3 key shares are generated 

- Any 2 of the 3 shares can sign a transaction 

- A private key is never generated to create a new wallet 

- The private key doesn’t need to be reconstructed to sign a transaction 

 
 

To show the functionality of the prototype, build a simple app that offers the following functionality: 

- Create a new crypto wallet - the app returns the 3 shares and the public key associated with this Ethereum wallet. 

- Sign a message - the app takes as input 2 key shares and a message and generates a signature hash corresponding to the wallet’s public key (e.g. we can verify it for example via https://etherscan.io/verifiedSignatures# ). 

 
 

Extra credit - add an option to choose an arbitrary number of key shares to be generated and an arbitrary threshold for how many key shares are required to sign a transaction. For example, we could then generate 5 shares and require 3 of them to sign. 

 
 
## Resolution 
 

### Initial considerations:  

I’m full-time employee and I have a toddler to take care of so I have little time to comply with this, my intention is to iterate thru the solution along the days instead of waiting a long time to send a solution. 

 
 

### Solution:  

I was a bit puzzled about this request as it seems that the problem itself has been already solved elsewhere, but I liked to give you a solution. 

 
 

The grounds of the solution are threshold signatures where a pre-defined sub-set of “n” parts of a total of “m” can be used to sign the request. 

 
 

There’s a company called Web3Auth (https://web3auth.io/) that already provides a commercial solution to this, so a wallet can be created and the user can control several of the shares and he can either use them in different devices or share with 3rd parties for later working with them (https://web3auth.io/docs/overview/key-management/technical-architecture/#proactive-secret-sharing-with-threshold-signature-schemes ) 

 
 

But if we would like to create a product of our own I would suggest staring the research with some of the public libraries available in the net: 

- https://github.com/bnb-chain/tss-lib 

- https://github.com/ing-bank/threshold-signatures 

 
 

I choose these two because they have been reviewed by a security firm and therefore a good start to get a safer product.  

Indeed both of them have sample code that would be itself the solution to the task. 

Moreon I’d like to add some considerations from the task and what I remember of the interview. 

 
 

1. A private key is never generated to create a new wallet. 

	This is the kind of solution that directly relates to BIP-32 standard (https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) The hierarchical deterministic wallets (or "HD Wallets"). I’ve been working with HDWallets several months this year and I’ve implemented some use cases with them (unfortunately nowadays not yet public code). The problem with this is that TSS generates new keys but does not generate entropy for the HDWallet, it means that you can have one of both: either TSS or HDWallet or key derivation. 

 
 

2. I recall talking about wallet recovery. This can be achieved with the classical tools:  

- passphrase memorization or cold storage, BIP-39 is very useful here (https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) 

- Sharing the secret (private key or passphrase) via method like Shamir secret sharing (https://medium.com/@keylesstech/a-beginners-guide-to-shamir-s-secret-sharing-e864efbf3648) there’s even an node.js library that implements it already (https://www.npmjs.com/package/shamirs-secret-sharing) 

### Final notes

I’d like to point out that it would be also nice to follow the new eIdas 2.0 framework evolution as some of the requirements for the European Digital Identity wallet include references to **device** and **cloud signature** of documents and if could affect to the product development. I’ve been working with Digital Identity solutions for the last 3 years. 
