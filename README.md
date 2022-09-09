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

## Update 1

Regarding threshold signatures: 2 out of 2 is just a specific case of n out of m, so the same principles apply to both cases, indeed I really doubt that any code would work different both 2/2 or n/m, it would raise concerns on the security of the algorithm and that has been already proved.


I've been reviewing ZenGo (https://zengo.com/) to find out a few interesting bits:

1. **Different levels of thereshold**. ZenGo uses 2 out of 2 but they public library https://github.com/ZenGo-X/multi-party-ecdsa is built to work with n out of m (explicit example of multi-party: https://github.com/ZenGo-X/multi-party-ecdsa/tree/master/src/protocols) . This is a deseign decision, if the raise the threshold lets say to 3/5 there 5 shares have to be securely stored and the 3 parts that need to be collected should be accesible to the user anytime he needs to sign a transaction. They even have an example to deploy and test in which 3 shares are used.
2. **Security**. https://zengo.com/security-in-depth/ they base all the securitiy un 2 out of 2 shares of threshold signatures. Not bad, just pointing it out.
3. **Recovery**. The recovery of the user shard is done via biometric scan of the face, nice idea and Apple has demostrated that it works most of the time (some cases where a user could unlock other user device rose in some asian market a few ago, it not a big issue). This idea or similar could cope with the "A private key is never generated to create a new wallet" requirement given in the TestTask. Anyway it would require a Biometric security module as biometrics usually work with some tolerances that have to be handled prior to generate always the same key.
   1. An alternative to this may be the use of ID scanning SDKs. I've seen a couple of them *ReadID* and *Entrust* that can use NFC capabilities in a phone to scan the National ID / Passport and that information could be used for **KYC** and maybe part of initial key generation.
5. **Product security audit**. They passed four of them between 2019 and 2020, Kudelski was passed twice the same that was used in the two libraries I pointed in the first part of the report.
6. Indeed I'd like to progress further with https://github.com/cryptochill/tss-ecdsa-cli that mentions "Includes support for HD keys (BIP32). HD support based on https://github.com/trepca/multi-party-ecdsa/tree/hd-support." and it could be very nice with the biometric seed.
