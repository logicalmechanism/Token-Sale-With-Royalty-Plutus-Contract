# Python Helper Scripts

This folder contains scripts to be used with the token sale smart contract.

These are basic templates for any user to easily interact with the token sale with royalty smart contracts. They are meant to be copied into a seperate folder where the cardano-cli is on path as it typically is when installed from source. The user will need to fill in the correct lines near the bottom of the files.

The suggested folder structure for interacting with a given smart contract is:

```
folder/
    -> tmp/
    -> wallet/
        wallet.addr
        wallet.skey
        wallet.vkey
    -> scripts/
        __init__.py
        start_royalty_contract.py
        end_royalty_contract.py
        split_utxo.py
        transaction.py
    seller.vkey
    artist.vkey
    script.plutus
```

This simple structure will allow a user to start or end any compiled Plutus script. Inside helper python files is a __name__ == "__main__" statement that contains a set of parameters that need to be filled out. The code is designed to exit if any of the information does not work.

Please note it is extremely important that any token inside a smart contract contains Datum. Any UTxOs without a Datum will remain stuck inside a smart contract forever.

# Splitting UTxO For Collateral

```py
    # The wallet address can be opened from file or hardcoded.

    # with open("/path/to/wallet/payment.addr", "r") as read_content: WALLET_ADDR = read_content.read().splitlines()[0]
    WALLET_ADDR      = "wallet_address_here"
    WALLET_SKEY_PATH = "/path/to/wallet/payment.skey"
    
    # Tmp folder for transactions - This stores protocol params, etc
    TMP = "/path/to/tmp/folder/"
    
    # The required collateral for the contract.
    COLLATERAL = 17000000
```

# Starting A Token Sale With Royalty Contract

# Ending A Token Sale With Royalty Contract