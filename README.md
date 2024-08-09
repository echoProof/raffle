# Raffle Smart Contract

## Overview

This Raffle smart contract allows users to enter a raffle and randomly selects a winner. The contract utilizes Chainlink's VRF (Verifiable Random Function) to ensure randomness in the winner selection process. Additionally, a fee of 1% is transferred to a specified address each time a winner is selected.

### Features

- **Random Winner Selection**: Uses Chainlink VRF for secure random number generation.
- **Entrance Fee**: Users can enter the raffle by paying a specified entrance fee.
- **Fee Distribution**: 99% of the prize goes to the winner, and 1% is sent to a specified fee address.
- **Automation**: Integrates with Chainlink Keepers for automatic winner selection based on a specified time interval.

## Prerequisites

- Solidity v0.8.19
- [Foundry](https://book.getfoundry.sh/) for testing and deployment
- A .env file for configuration (example.env provided)

## Installation

1. Clone this repository:

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Install the required dependencies:

   ```bash
   make install
   ```

3. Create a `.env` file in the root directory of the project and add the following environment variables:

   ```env
    # Example .env file
    ANVIL_RPC=http://127.0.0.1:8545
    SEPOLIA_RPC=
    ETHERSCAN_API_KEY=
   ```

   ## Configuration

The contract constructor takes several parameters:

- `subscriptionId`: Chainlink VRF subscription ID
- `interval`: Time interval between raffle draws
- `entranceFee`: Cost to enter the raffle
- `feeAddress`: Address to receive the 1% fee

In the `HelperConfig.s.sol` file, there are 3 variables that need to be set:

```solidity
address public FOUNDRY_DEFAULT_SENDER = ;
address public TESTNET_DEFAULT_SENDER = ;
address public FOUNDRY_FEE_ADDRESS = ;
```

## Contract Structure

### Main Contract: `Raffle`

- **Constructor**: Initializes the contract with required parameters including subscription ID, gas lane, entrance fee, etc.
- **Functions**:
  - `enterRaffle()`: Allows users to enter the raffle by sending ETH.
  - `checkUpkeep()`: Called by Chainlink Keepers to check if the conditions for upkeep are met.
  - `performUpkeep()`: Requests random words from Chainlink VRF.
  - `fulfillRandomWords()`: Handles the logic for transferring the prize to the winner and fees to the fee address.
  - Various getter functions to retrieve contract state and configuration.

### Error Handling

The contract includes custom error messages to handle different scenarios, including:

- `Raffle__UpkeepNotNeeded`
- `Raffle__TransferFailed`
- `Raffle__SendMoreToEnterRaffle`
- `Raffle__RaffleNotOpen`
- `Raffle__TransferFeeFailed`

## Usage

1. Deploy the contract on your desired network (Sepolia, Mainnet, or Local) using Foundry.
2. Users can enter the raffle by calling the `enterRaffle()` function and sending the required entrance fee.
3. The contract will automatically select a winner based on the specified time interval using Chainlink Keepers.

## Testing

To run the tests, ensure you have Foundry set up and execute the following command:

```bash
forge test
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any enhancements or bug fixes.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.