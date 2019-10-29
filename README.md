# How to convert private key to WIF

### 0. Overview

  ```
  WIF = base58check encode ([version byte][private key][checksum])
  
  version byte = 80 for mainnet, ef for testnet and regtest
 
  checksum = first 4 bytes of double SHA256 of private key
  ```

### 1. Add version byte

In this example

* private key = 619c335025c7f4012e556c2a58b2506e30b8511b53ade95ea316fd8c3286feb9

  ```
  > export PRIV_KEY=619c335025c7f4012e556c2a58b2506e30b8511b53ade95ea316fd8c3286feb9
  ```

Add version byte in front of private key (ef for regtest)
  
  ```
  > export VER=ef
  
  > echo ${VER}${PRIV_KEY}
  ef619c335025c7f4012e556c2a58b2506e30b8511b53ade95ea316fd8c3286feb9
  ```

Network | Version Byte (Hex)
:-----: | :----------------:
mainnet | 80
testnet | ef
regtest | ef

### 2. Compute checksum (Double SHA256)

  ```
  > echo ${VER}${PRIV_KEY} -n | xxd -r -p | openssl dgst -sha256 -binary | openssl dgst -sha256
  5ea6574663729d86cdc55bc9b7b47eda13a7ae8e4c7bc7084e248f8ddd755cbc
  ```

Take the first 4 bytes of the double SHA256 hash

  ```
  5ea65746
  
  > export CHECKSUM=5ea65746
  ```

### 3. Append checksum

  ```  
  > echo ${VER}${PRIV_KEY}${CHECKSUM}
  ef619c335025c7f4012e556c2a58b2506e30b8511b53ade95ea316fd8c3286feb95ea65746
  ```

### 4. Convert to Base58Check ([encoder](http://lenschulwitz.com/base58))

  ```
  92KuV1Mtf9jTttTrw1yawobsa9uCZGbfpambH8H1Y7KfdDxxc4d
  ```

### 5. Verify (Optional)

In bitcoin-qt console

  ```
  > importprivkey 92KuV1Mtf9jTttTrw1yawobsa9uCZGbfpambH8H1Y7KfdDxxc4d "test-priv-key"
  null
  
  > getaddressesbyaccount "test-priv-key"
  [
    "mi7uHKSho5sj9EwN8Tat4GuLu5ZjbJqT4Q"
  ]
  ```

### References

[Wallet Import Format - Bitcoin Wiki](https://en.bitcoin.it/wiki/Wallet_import_format)

### Bitcoin donations welcome: 3BJbCUdJ8mhZn9B2Ncz1JBBEbe1R8816AW
### Litcoin donations welcome: MQRLB7gYHBAZuY8erwbMTguG24CBAwgwo4
### Dash coin donations welcome: XsXNddLuVLc1b1Q2DDqniSvwYbpq7F2BLe
