# Connect Metamask

## Compare normal chain with BoolNetwork in compatible with ethereum
## Common: 
![rocess](images/connection0.png)

## BoolNetwork:
![rocess](images/connection1.png)

## Install the MetaMask Extension and create one account
![rocess](images/connection2.png)

## Start bnk-node
```
git clone https://github.com/ABMatrix/boolnetwork.git
cd boolnetwork/node
cargo build --release
./target/release/bnk-node --dev --tmp --rpc-port 9933
```

## Link chain with polkadot.js apps:
```
https://polkadot.js.org/apps/#/explorer
```
### Note: need switch to Local Node
```
ws://127.0.0.1:9944
```

## Link EVM with MetaMask
### 1. Enter Settings
![rocess](images/connection4.png)

### 2. Select Networks and add New Network
![rocess](images/connection6.png)

### 4. RPC URL is binding with bnk-node, Chain ID is defined in the node runtime
![rocess](images/connection7.png)

### 5. Select BoolNetwork
![rocess](images/connection8.png)

## Create new account in Substrate using Private Key for MetaMask's account
### 1. Select Account Details and export Private Key
![rocess](images/connection9.png)

### 2. Type MetaMask password
![rocess](images/connection10.png)

### 3. Copy the Private Key and use it to create new account in substrate
![rocess](images/connection11.png)

![rocess](images/connection12.png)

![rocess](images/connection13.png)

### Check new account in substrate, account id is same as metamask address
![rocess](images/connection14.png)

## Deposit balance from Substrate into MetaMask

### *There are two options to deposit:

### *Option1: Transfer by metamask: 
### 1. Add development account(prefunds account) to metamask
```
alith sk: 0x5fb92d6e98884f76de468fa3f6278f8807c48bebc13595d45af5bdc4da702133
```
![rocess](images/connection15.png)

### 2. Send some balance from Alith to Account1
![rocess](images/connection16.png)

![rocess](images/connection17.png)

![rocess](images/connection18.png)

### 3. Waiting for transaction complete
![rocess](images/connection19.png)

![rocess](images/connection20.png)

### 4. Check Account1's balance
![rocess](images/connection21.png)

![rocess](images/connection22.png)

### *Option2: Transfer by Polkadot.js apps:

### 1. Send balance by Alith
![rocess](images/connection23.png)

### 2. Check Event and Account1
![rocess](images/connection24.png)

![rocess](images/connection25.png)

## Create contract and call it in EVM
### 1. Create a contract by sending transaction
![rocess](images/connection26.png)

![rocess](images/connection27.png)

### 2. Get contract address
![rocess](images/connection28.png)

### Note: This contract deploy some tokens to the creator

### 3. Check the contract address code storage
![rocess](images/connection29.png)

### 4. Get the index hash about creator address

```
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8"   -d '{"jsonrpc":"2.0","id":1,"method":"eth_getIndex","params": ["0x1D2A512EaE004AE78bA2353fc2d3596FCfb95F0E", "0x00"]}'
{"jsonrpc":"2.0","result":"0xd5d6d8faa68d702d04eb9fbf19f017ff823fa4fef6abc5df39b31c12035a8d8b","id":1}
```

### 5. Check token number about the creator address
![rocess](images/connection30.png)

### 6. Transfer some token to target address(0x7E6F241a60C9D557a412B66845bfe10CCFD82E4A)
![rocess](images/connection31.png)

![rocess](images/connection32.png)

### Note: transfer number is "0x00000000000000000000000000000000000000000000000000000000000000dd"

### 7. Get the index hash about recipient address

```
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8"   -d '{"jsonrpc":"2.0","id":1,"method":"eth_getIndex","params": ["0x7E6F241a60C9D557a412B66845bfe10CCFD82E4A", "0x00"]}'
{"jsonrpc":"2.0","result":"0x34902750012c0610301474fd3c9af861bc73615fd2e034fc43d5a5231cad2fac","id":1}
```

### 8. Check token number about the recipient address
![rocess](images/connection33.png)

## Use precompile to connect with substrate
### 1. Copy Mining.sol(node/pallets/evm/precompile/mining) to Remix IDE

![rocess](images/connection34.png)

### 2. Compile Mining.sol

![rocess](images/connection36.png)

### 3. Select Deploy tap, Select Environment with "Injected Provider - MetaMask" and It will choose the metamask account
### Copy the address in Mining.sol to "At Address" and click it
![rocess](images/connection37.png)

### 4. Now user can send substrate transaction and query substrate storage by the contract(ig. call pallet-mining tx-function: create)
![rocess](images/connection38.png)

### 5. Check event in substrate chain
![rocess](images/connection39.png)

### 6. Query pallet-mining storage: provider_id, it will change to "1"
![rocess](images/connection40.png)






