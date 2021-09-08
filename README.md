# Token-Sale-With-Royalty-Plutus-Contract

A repository of publicly verifiable token sale with royalty contracts. This will be the storage solution since it is easily attainable and usable. A more decentralized solution will come in the future but for now this will be the solution.

The repository will work in a very similar fashion to the Cardano Foundation's token registry. It will allow users to add their contribution to the repository so everyone can use a collection of trusted smart contracts. The goal is to create a micro-ecosystem of contracts that are simple enough to read and quick to compile.

To add a token sale contract to the repository: please fork the repo, add in the contract addition, public verification key, and compiled plutus script into the contracts folder following the directions below then use a pull request to add in your contribution into the smart contract repository when finished.

The folder containing the contract to be compiled will be placed in the contracts folder. The name of the folder will be the pubkeyhash of the seller, allowing a quick search method for a single seller, followed by an under score then the price of the shop in lovelace. Please see the example_contract_addition folder for an example addition.

# Usage

After forking the repo, clone the repo onto your system. Make a copy of the default folder into the contracts folder. Change the name of the copied folder into the pubKeyHash of the wallet that will be receiving payments for tokens sold followed by the price of tokens inside this contract. Additions will not be granted if the naming scheme is not followed. Place a copy of the seller's vkey, the artist's vkey, and the compiled plutus script, into the copied folder.

A user will only need to edit the token-sale-with-royalty/src/TokenSaleWithRoyal.hs file. The haskell code requires only two changes, any other changes will result in a failed contract addition. The original artist key hash and royalty amount will be determined by the token being sold thus can be verified if changed at compile time. The user will only need to update the pubkeyhash of the seller and the price of the token. After the changes are made to the TokenSaleWithRoyalty.hs file, the user will need to compile the Haskell into Plutus such that other users can verify the correct output as well as have the correct plutus script file for usage.

## Finding the public key hash

The user will be required to get the pubKeyHash using the cardano-cli. This will be used to name the folder inside the contracts folder.

```bash
cardano-cli address key-hash --payment-verification-key-file FILE # Filepath of the payment verification key.
# or use this command depending on the use case
cardano-cli address key-hash --payment-verification-key STRING    # Payment verification key (Bech32-encoded)
# For additional help please check out: cardano-cli address key-hash --help
```

## The Haskell to change

Look for this section of code inside the token-sale-with-royalty/src/TokenSaleWithRoyalty.hs file, starts around line 134. The file to be edited should be inside the folder copied into the contracts folder. Please only change the values of the seller address and the cost.

The default file:

```hs
validator :: Plutus.Validator
validator = Scripts.validatorScript (typedValidator ts)
    where ts = TokenSaleParams { tsSellerAddress  = pubKeyHashAddress "SELLER_PUB_KEY_HASH_HERE" -- update to new seller
                               , tsRoyaltyAddress = pubKeyHashAddress "ARTIST_PUB_KEY_HASH_HERE" -- get original artist
                               , tsCost           = TOTAL_LOVELACE_COST_HERE                     -- Token Cost
                               , tsRoyalty        = ROYALTY_AMOUNT_HERE                          -- royalty  = 1 / % of returns
                               }
```                               

An example of a properly filled out plutus script:

```hs
validator :: Plutus.Validator
validator = Scripts.validatorScript (typedValidator ts)
    where ts = TokenSaleParams { tsSellerAddress  = pubKeyHashAddress "d3fb616e18dcb9476f5167ab60b14a6954a14e0ad650cfa864d05d41" -- update to new seller
                               , tsRoyaltyAddress = pubKeyHashAddress "a26dbea4b3297aafb28c59772a4ef2964ebffb3375b5de313947e6c8" -- get original artist
                               , tsCost           = 35000000                                                                     -- Token Cost
                               , tsRoyalty        = 10                                                                           -- royalty  = 1 / % of returns
                               }
```
The example above forces the buyer to pay the 31.5 ADA to the sellers address and 3.5 ADA to the original artist's address before the buyer can spend the token utxo. The total price is 35 ADA and the royalty is 10% of the total price.

## Compiling the plutus script

Inside the pubKeyHash_lovelaceAmount folder will be the cabal.project file and the token-sale-with-royalty directory. Change the directory into token-sale-with-royalty and run the commands below.

```bash
cabal clean
cabal build -w ghc-8.10.4
cabal run token-sale-with-royalty
```

This will produce a compiled plutus script.

The folder should be named correctly and contain haskell code that when compiled following the instructions above will result in the correct plutus script. A correctly named folder will have the format pubKeyHash_lovelaceAmount.

To get the smart contract address use the cardano-cli command below.

```bash
cardano-cli address build --mainnet --payment-script-file FILE # Filepath of the plutus script.
```

To get the wallet address from a verificaiton key use the cardano-cli command below.

```bash
cardano-cli address build --mainnet --payment-verification-key-file FILE # Filepath of the plutus script.
```

All additions that follow the format and can be compiled into the correct Plutus script will be added into the repository.
