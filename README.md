# Account Abstraction smart contracts

The suite of Account Abstraction (AA) smart contracts, based on [Bicomony open-source](https://github.com/bcnmy/scw-contracts).

## Smart Contracts

- **BaseSmartAccount.sol**: An abstract contract implementing the EIP4337 IWallet interface.
- **Proxy.sol**: A lightweight proxy upgradeable through the UUPS pattern.
- **SmartAccountFactory.sol**: This factory contract manages the deployment of Smart Account (Account Abstraction).
- **SmartAccount.sol**: The primary implementation contract for a Smart Account (Account Abstraction).
- **EntryPoint.sol**: Implements the EIP4337 Entry Point contract.
- **StakeManager.sol**: A stake manager for wallet and paymaster deposits/stakes.
- **Executor.sol**: A helper contract facilitating calls and delegate calls to dapp contracts.
- **FallbackManager.sol**: Manages a fallback handler for delegate calls.
- **ModuleManager.sol**: Adopts the Gnosis Safe module manager pattern.
- **DefaultCallbackHandler.sol**: Handles hooks to respond to token receipts.
- **MultiSend.sol & MultiSendCallOnly.sol**: Facilitates batching multiple transactions into one.
- **VerifyingSingletonPaymaster.sol**: A paymaster that uses an external service for transaction validation.
- **PaymasterHelpers.sol**: A library essential for decoding paymaster data and context.

## Prerequisites

- [NodeJS v20 LTS](https://nodejs.org/en/blog/release/v20.9.0) with yarn installed.
- [Hardhat latest version](https://hardhat.org/)

## Setup

- Install `yarn`
```sh
$ npm install -g yarn
```

- Create a `.secret` file at the root to store your mnemonic.
**Note**: Never commit this file.
```shell
$ echo "your mnemonic here" > .secret
```

- Create a `.env` from template and populate it with necessary secrets and credentials.
```sh
$ cp .env.example .env
```

- Install dependencies
```sh
$ yarn
```

## Compile

- Compile smart contracts
```sh
$ npx hardhat clean && npx hardhat compile
```

## Test

- Execute unit tests
```sh
$ npx hardhat test
```

- Test with gas report:
```sh
$ REPORT_GAS=true npx hardhat test
```

- Generate code coverage report:
```sh
$ npx hardhat coverage
```

- Execute Bundler Integration Tests:
```sh
$ yarn bundler-test
```

## Deploy

- Deploy `deployer` contract
```sh
$ npx hardhat run scripts/deployer-contract.deploy.ts --network derachain
```
you should note that `Deployed new Deployer Contract at 0x... on chain derachain: 20240801` is printed out without errors.

- Populate deployed address from previous step to `.env` file
```sh
DEPLOYER_CONTRACT_ADDRESS_PROD="0x..."
```

- Change the seed values in `scripts/utils/index.ts` with current datetime

```ts
export const DEPLOYMENT_SALTS_PROD: DeploymentSaltsType = {
  ENTRY_POINT: "DARECHAIN_ENTRY_POINT_V1_YYYYMMDD",
  MULTI_SEND: "DARECHAIN_MULTI_SEND_V1_YYYYMMDD",
  WALLET_FACTORY: "DARECHAIN_PROD_WALLET_FACTORY_V1_YYYYMMDD",
  WALLET_IMP: "DARECHAIN_PROD_WALLET_IMP_V1_YYYYMMDD",
  SINGELTON_PAYMASTER: "DARECHAIN_PROD_SINGLETON_PAYMASTER_V1_YYYYMMDD",
  ECDSA_REGISTRY_MODULE: "DARECHAIN_PROD_ECDSA_REGISTRY_MODULE_V1_YYYYMMDD",
  MULTICHAIN_VALIDATOR_MODULE: "PROD_MULTICHAIN_VALIDATOR_MODULE_V1_YYYYMMDD",
  PASSKEY_MODULE: "PROD_PASSKEY_MODULE_V1_YYYYMMDD",
  SESSION_KEY_MANAGER_MODULE: "PROD_SESSION_KEY_MANAGER_MODULE_V2_YYYYMMDD",
  SESSION_KEY_MANAGER_MODULE_V2: "PROD_SESSION_KEY_MANAGER_MODULE_V2_YYYYMMDD",
  BATCHED_SESSION_ROUTER_MODULE: "PROD_BATCHED_SESSION_ROUTER_MODULE_V1_YYYYMMDD",
  ERC20_SESSION_VALIDATION_MODULE: "PROD_ERC20_SESSION_VALIDATION_MODULE_V2_YYYYMMDD",
  SMART_CONTRACT_OWNERSHIP_REGISTRY_MODULE: "PROD_SMART_CONTRACT_OWNERSHIP_REGISTRY_MODULE_V1_YYYYMMDD",
};
```

- Deploy other contracts
```sh
$ npx hardhat run scripts/deploy.ts --network derachain
```
you should note that fully suite of smart contracts is printed out without errors. For example:
```json
Deployed Contracts:  {
  "EntryPoint": "0x3dF0697D2446f86a37F0efa445c620Cb935F164c",
  "SmartAccount": "0x3aCfe599036370435becB228321E8d37C81d2a12",
  "SmartAccountFactory": "0xCe3068038C7200EEDeE95D63Bf1607977F4c32e3",
  "VerifyingPaymaster": "0x573E1d7ea4bcd28C77372f3EfaC599D59f470c8E",
  "EcdsaOwnershipRegistryModule": "0x9102Fa334996E08cB7FdE9Dfe8DD9A73C7b9f4c2",
  "MultichainValidatorModule": "0x6EE21DB40b4869e37Cc99aB0C3240f7949521cEe",
  "PasskeyModule": "0x3D386612da26794B805e30Ccf84a25ea66716c09",
  "SessionKeyManagerModule": "0x2Cdf767D5e44916fd8F03D4072e551CCd72563C0",
  "ERC20SessionValidationModule": "0x9c07103dEF14a6642E02781939c4A8267f69098D",
  "SmartContractOwnershipRegistryModule": "0xE367A9D88f5debDACF7141FfCE49a14CD01b4B4D"
}
```

## Verify
The verification of smart contracts should be conducted in the following sequence.

- Deployer
```sh
$ npx hardhat verify <deployer-address> --network derachain
```

- EntryPoint
```sh
$ npx hardhat verify <entrypoint-address> --network derachain
```

- SmartAccount template
```sh
$ npx hardhat verify <sa-template-address> <entry-point-address> --network derachain
```

- SmartAccount Factory
```sh

- Verify smart contracts on DERA chain
```sh
$ npx hardhat verify <contract-address> --network derachain
```

or with constructor arguments
```sh
$ npx hardhat verify <contract-address> <constructor-arg1> <constructor-arg2> ... --network derachain
```