# Solidity Lottery (Raffle) 🎰

A decentralized, provably fair lottery/raffle smart contract built with Solidity and Foundry. This project implements Chainlink VRF v2.5 for verifiable randomness and Chainlink Automation for automated winner selection.

## About

This project demonstrates a production-ready raffle smart contract with the following features:

- **Provably Fair Randomness**: Uses Chainlink VRF (Verifiable Random Function) to ensure fair and tamper-proof winner selection
- **Automated Execution**: Leverages Chainlink Automation (formerly Keepers) for automatic winner picking when conditions are met
- **Time-Based Rounds**: Configurable time intervals between lottery rounds
- **Entry Fee System**: Users pay an entrance fee to participate in the raffle
- **Winner Takes All**: The entire prize pool is awarded to the randomly selected winner
- **State Management**: Implements proper state transitions (OPEN/CALCULATING) to prevent interference during winner selection

## Built With

- **Foundry**: Modern, fast Ethereum development framework
- **Solidity ^0.8.19**: Smart contract programming language
- **Chainlink VRF v2.5**: Verifiable random number generation
- **Chainlink Automation**: Decentralized automation network
- **OpenZeppelin**: Security-audited contract standards

## Prerequisites

Before you begin, ensure you have the following installed:

- [Foundry](https://book.getfoundry.sh/getting-started/installation)
- [Git](https://git-scm.com/downloads)
- An Ethereum wallet with testnet ETH
- [Chainlink VRF Subscription](https://vrf.chain.link/)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/GN25/Solidity-Lottery.git
cd Solidity-Lottery
```

2. Install dependencies:
```bash
make install
```

Or manually:
```bash
forge install cyfrin/foundry-devops@0.2.2
forge install smartcontractkit/chainlink-brownie-contracts@1.1.1
forge install foundry-rs/forge-std@v1.8.2
forge install transmissions11/solmate@v6
```

3. Set up your environment variables:
```bash
cp .env.example .env
```

Edit `.env` with your configuration:
```
SEPOLIA_RPC_URL=your_alchemy_or_infura_url
PRIVATE_KEY=your_private_key
ETHERSCAN_API_KEY=your_etherscan_api_key
```

## Usage

### Build

Compile the smart contracts:
```bash
forge build
```

### Test

Run the test suite:
```bash
forge test
```

Run tests with gas reporting:
```bash
forge test --gas-report
```

Run tests with verbosity:
```bash
forge test -vvvv
```

### Format

Format Solidity files:
```bash
forge fmt
```

### Local Development

Start a local Anvil node:
```bash
make anvil
```

### Deploy

#### Deploy to Local Network (Anvil):
```bash
make deploy
```

#### Deploy to Sepolia Testnet:
```bash
make deploy ARGS="--network sepolia"
```

### Chainlink VRF Setup

1. Create a VRF subscription:
```bash
make createSubscription ARGS="--network sepolia"
```

2. Fund your subscription at [vrf.chain.link](https://vrf.chain.link)

3. Add your contract as a consumer:
```bash
make addConsumer ARGS="--network sepolia"
```

## Smart Contract Architecture

### Raffle.sol

The main raffle contract with the following key functions:

- `enterRaffle()`: Allows users to enter the raffle by paying the entrance fee
- `checkUpkeep()`: Chainlink Automation compatible function to check if it's time to pick a winner
- `performUpkeep()`: Triggered by Chainlink Automation to request a random number
- `fulfillRandomWords()`: Callback function that receives the random number and selects the winner

### State Variables

- `i_entranceFee`: Minimum ETH required to enter
- `i_interval`: Time between lottery rounds
- `s_players`: Array of participants
- `s_raffleState`: Current state (OPEN/CALCULATING)
- `s_recentWinner`: Address of the last winner
- `s_lastTimeStamp`: Timestamp of the last winner selection

## Testing

The project includes comprehensive tests in `test/unit/RaffleTest.t.sol`:

- Unit tests for all contract functions
- Integration tests with Chainlink VRF mocks
- Edge case testing
- State transition verification

Run specific tests:
```bash
forge test --match-test testEnterRaffle
forge test --match-contract RaffleTest
```

## Security Considerations

- Implements checks-effects-interactions pattern
- Uses custom errors for gas efficiency
- Proper state management to prevent reentrancy
- Immutable variables for gas optimization
- Events emitted for all state changes

## Gas Optimization

- Custom errors instead of require strings
- Immutable and constant variables where possible
- Efficient storage patterns
- Optimized loops and data structures

## Project Structure

```
├── src/
│   └── Raffle.sol              # Main raffle contract
├── script/
│   ├── DeployRaffle.s.sol      # Deployment script
│   ├── HelperConfig.s.sol      # Network configuration
│   └── Interactions.s.sol      # Chainlink interaction scripts
├── test/
│   ├── unit/
│   │   └── RaffleTest.t.sol    # Unit tests
│   └── mocks/
│       └── LinkToken.sol       # Mock LINK token
├── lib/                        # Dependencies
├── Makefile                    # Build automation
└── foundry.toml               # Foundry configuration
```

## Makefile Commands

| Command | Description |
|---------|-------------|
| `make install` | Install dependencies |
| `make build` | Compile contracts |
| `make test` | Run tests |
| `make deploy` | Deploy to Sepolia |
| `make anvil` | Start local node |
| `make createSubscription` | Create Chainlink VRF subscription |
| `make addConsumer` | Add contract as VRF consumer |
| `make snapshot` | Generate gas snapshots |
| `make format` | Format Solidity code |

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License.

## Resources

- [Foundry Documentation](https://book.getfoundry.sh/)
- [Chainlink VRF](https://docs.chain.link/vrf/v2/introduction)
- [Chainlink Automation](https://docs.chain.link/chainlink-automation/introduction)
- [Cyfrin Updraft](https://updraft.cyfrin.io/)

## Author

**Guillem Navarra**

## Acknowledgments

- Patrick Collins and the Cyfrin team for educational resources
- Chainlink for providing decentralized oracle infrastructure
- The Foundry team for the excellent development framework

---

⭐ If you found this project helpful, please give it a star!
