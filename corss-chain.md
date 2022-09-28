# Cross Chain

## Generic Process:

### 1. Run bnk-node, create channel bind with two committees

### 2. Start two network to cross
```
yarn run mercury 
yarn run mars
```
### 3. Copy anchor/.env.example => anchor/.env for Initializing, get ISSUER_ADDRESS and SETTER_ADDRESS from address:
```
npx hardhat address --public-key 0x04a3d538322f2a971795b63a0656b945b7ab6e99641d837f9a3720a4a7bfbbd4e9552a5f41a371dd79854e72cac3aba0919e6467aff47d19afae6066084fcece78
```
Note: just for Initializing, needn't bind with committee

### 4. Deploy factory contract
```
npx hardhat deploy --network mercury 
npx hardhat deploy --network mars
```
### 5. Insert issuer with committee's pk into mercury && mars
```
npx hardhat --network  mercury factory --address 0x4f399620858D515277ecD25879857C1Bd591F286 --issuer cid1's pk
npx hardhat --network  mars factory --address 0xd0302A60296433793E0977a265b290092A3D8006 --issuer cid2's pk
```

Note: after insert issuer, it will generate anchor addresses, bind target committee with target anchor(bnk-node: active committee)

### 6. Run monitor server and relayer server 
```
./carbon/e2e/integrate.sh start
```


## Lock Process

### 1. Add funds to target account: 
```
npx hardhat run --network mars ./scripts/drop.ts
```
### 2. Start Lock, will forge to target account
```
npx hardhat run --network mercury ./scripts/lock.ts
```
Note: check target account's token number with: 
```
npx hardhat --network mars balance --account <target account>  --token <committee's ahchor address connect with mars>
```

## Melt Process

Due to melt.ts default deployer is 0x8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD in mars, we need to forge tokens for deployer

### 1. Fix lock.ts: 
```
extra.addr => 8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD

npx hardhat run --network mercury ./scripts/lock.ts
```

Note: check deploer's token number with: 
```
npx hardhat --network mars balance --account 0x8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD  --token <committee's ahchor address connect with mars>
```

### 2. Add funds to target account: 
```
npx hardhat run --network mercury ./scripts/drop.ts
```

### 3. Start melt
```
npx hardhat run --network mars ./scripts/melt.ts
```
Note: check deploer's token number with:
```
npx hardhat --network mars balance --account 0x8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD  --token <committee's ahchor address connect with mars>
```
check target account's balance with:
```
npx hardhat --network mercury balance --account <melt.ts: extra.addr>
```

## Forge Process

Due to forge's default deployer is 0x8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD in mars is issuer,  default anchor is the latest anchor in mars, we need insert issuer with 0x8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD,
### 1. Insert new issuer: 
```
npx hardhat --network  mars factory --address 0xd0302A60296433793E0977a265b290092A3D8006 --issuer 0x8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD
```

### 2. Start forge:
```
npx hardhat run --network mars ./scripts/forge.ts
```

Note: check account's token number with: 
```
npx hardhat --network mars balance --account 0x8a7B9Bf0AA9B10cfe0B738e1c1020038Ac095bAD  --token <anchor addr generate in Forge step1>
```

   

   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   