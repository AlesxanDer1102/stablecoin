# Decentralized Stablecoin System (DSC)

## Overview

The **Decentralized Stablecoin System (DSC)** is a smart contract platform designed to create and manage a decentralized stablecoin, backed by over-collateralized assets in the form of **Wrapped Bitcoin (WBTC)** and **Wrapped Ethereum (WETH)**. The stablecoin aims to maintain a 1:1 peg with USD by utilizing algorithmic mechanisms to maintain price stability. The system ensures that the minted DSC tokens are always backed by 200% collateral, providing a safety buffer to maintain the peg.

The contract allows users to deposit WBTC or WETH as collateral, mint DSC tokens, and redeem collateral. If the value of a user's collateral drops and their **Health Factor** falls below a defined threshold, their position becomes eligible for liquidation by external parties. Liquidators are incentivized with a reward for liquidating risky positions.

## Key Features

- **Over-Collateralization**: Each DSC token is backed by 200% collateral, ensuring stability and security for the system.
- **Supported Collaterals**: WBTC and WETH are accepted as collateral.
- **Stablecoin Peg**: DSC maintains a 1:1 peg with USD, based on the value of the collateral.
- **Health Factor**: Users' collateral is monitored, and if the **Health Factor** falls below a set threshold, their position is eligible for liquidation.
- **Liquidation Mechanism**: Liquidators can take action if a user's collateral value is insufficient to cover their DSC debt, earning a reward in collateral tokens.

## How It Works

1. **Deposit Collateral**: Users deposit WBTC or WETH into the system, which will be used to mint DSC tokens.
2. **Mint DSC**: The user can mint DSC tokens, which are issued based on the value of the deposited collateral. The collateralization ratio is always maintained at 200%.
3. **Maintain Health Factor**: The **Health Factor** of each user's position is tracked. It is calculated as:

   $$
   \text{Health Factor} = \frac{\text{Total Collateral Value in USD}}{\text{Amount of DSC Minted} \times 2}
   $$

   If the Health Factor drops below 1, the user’s position is at risk of liquidation.
   
4. **Liquidation**: If the Health Factor of a position falls below 1, external parties can liquidate the position by redeeming the collateral and burning the corresponding DSC tokens. Liquidators receive a reward (10% bonus) for their actions.

## Contract Functions

### `depositCollateralAndMintDSC`
Deposits collateral (WETH or WBTC) and mints DSC tokens. The amount of DSC minted is proportional to the value of the collateral deposited, ensuring that the position remains over-collateralized.

### `redeemCollateralForDsc`
Allows users to redeem their collateral by burning the equivalent amount of DSC tokens. The amount of collateral returned depends on the amount of DSC burned and the current value of the collateral.

### `liquidate`
External parties can call this function to liquidate users whose collateralization ratio falls below the required threshold. Liquidators receive a bonus for successfully liquidating a user's position.

### `getHealthFactor`
This function calculates the **Health Factor** for a specific user. The Health Factor reflects the safety of a user’s position and determines whether it is eligible for liquidation.

### `getAccountCollateralValue`
This function provides the total value of a user’s collateral in USD, helping to assess the safety of their position.

## Liquidation & Incentives

Liquidation occurs when a user’s Health Factor falls below a threshold of 1. In this case, external liquidators can step in to redeem the collateral and burn the necessary DSC to bring the user's position back into balance.

Liquidators are rewarded with **10% of the collateral value** as an incentive for taking on the risk of liquidation.

## System Requirements

- **Collateral Tokens**: The system supports Wrapped Bitcoin (WBTC) and Wrapped Ethereum (WETH) as collateral assets.
- **Collateralization Ratio**: The system requires a 200% collateralization ratio for all positions. This means that for every 1 DSC minted, the collateral must be worth at least 2 USD.
- **Health Factor Threshold**: Users whose Health Factor falls below 1 are eligible for liquidation.

## Example Scenario

### Scenario 1: Deposit and Mint DSC
1. User deposits 1 WETH (worth 2000 USD).
2. The system allows them to mint 1000 DSC (2000 USD collateral * 0.5 = 1000 DSC).
3. The collateralization ratio is 200%, which means that if the value of WETH falls, the user will be at risk of liquidation if the collateralization ratio drops below 200%.

### Scenario 2: Health Factor Drops Below 1
1. If the value of WETH drops from 2000 USD to 950 USD, the user's Health Factor would fall below 1.
2. External liquidators can liquidate the position by redeeming the WETH and burning the corresponding DSC.
3. The liquidator is rewarded with a 10% bonus in collateral for their action.

## Contract Deployment

To deploy this contract on a blockchain (e.g., Ethereum or Polygon), the following steps should be followed:

1. **Compile the contract**:
   ```bash
   forge build
   ```

2. **Deploy the contract**:
   ```bash
   forge script DeployDSC --rpc-url <RPC_URL> --private-key <PRIVATE_KEY>
   ```
   -RPC_URL : RPC of on blockchain EVM compatible
   -PRIVATE_KEY: Your private key of your wallet

4. **Interact with the contract**: Once deployed, users can interact with the system via a frontend or directly through functions like `depositCollateralAndMintDSC`, `redeemCollateralForDsc`, and `liquidate`.

## Example Code Snippets

### Deposit Collateral and Mint DSC

```solidity
// Deposit 1 WETH and mint 1000 DSC
dsce.depositCollateralAndMintDSC(address(WETH), 1e18, 1000e18);
```

### Redeem Collateral for DSC

```solidity
// Redeem 1 WETH by burning 1000 DSC
dsce.redeemCollateralForDsc(address(WETH), 1e18, 1000e18);
```

### Liquidation

```solidity
// Liquidate a user's position
dsce.liquidate(address(WETH), userAddress, 1000e18);
```

## License

This project is licensed under the **MIT License**.


