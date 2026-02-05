# Solidity Lottery (Chainlink VRF v2.5 + Automation) — Foundry

A production-style raffle/lottery smart contract built with **Foundry** and **Chainlink VRF v2.5**, designed to showcase real-world patterns: custom errors, events, deterministic tests, deployment scripts, and CI.

Repo: https://github.com/GN25/Solidity-Lottery

## What this project does

- **Players enter** by paying an `entranceFee`.
- The raffle stays **OPEN** for a configured `interval`.
- When the interval passes and there are players + balance, Chainlink Automation-style logic triggers a winner selection.
- A **verifiably random winner** is chosen via **Chainlink VRF v2.5**, the pot is paid out, and the raffle resets.

## Tech stack

- **Solidity** (`^0.8.19`)
- **Foundry** (`forge`, `cast`, `anvil`) for development/testing
- **Chainlink VRF v2.5** (verifiable randomness)
- **Automation-compatible** `checkUpkeep/performUpkeep` pattern
- **GitHub Actions** CI: formatting, build, and tests

## Contracts & scripts

- Core contract: [src/Raffle.sol](src/Raffle.sol)
	- `enterRaffle()` with custom errors + event emission
	- `checkUpkeep()` and `performUpkeep()` for upkeep gating
	- `fulfillRandomWords()` picks winner, resets state, and pays out (Checks-Effects-Interactions)

- Deployment: [script/DeployRaffle.s.sol](script/DeployRaffle.s.sol)
	- Deploys the raffle and wires VRF subscription flow when needed

- VRF subscription utilities: [script/Interactions.s.sol](script/Interactions.s.sol)
	- Create subscription, fund subscription, and add the raffle as a consumer

- Network configuration (Sepolia + local Anvil mocks): [script/HelperConfig.s.sol](script/HelperConfig.s.sol)

## Local quickstart

Prereqs:

- Foundry installed: https://book.getfoundry.sh/getting-started/installation

Install deps:

```bash
forge install
```

Run tests:

```bash
forge test -vvv
```

Start a local chain:

```bash
make anvil
```

In another terminal, deploy locally:

```bash
make deploy
```

## Sepolia deployment

Create your `.env` from the example:

```bash
cp .env.example .env
```

Fill in:

- `SEPOLIA_RPC_URL`
- `PRIVATE_KEY` (never commit this)
- `ETHERSCAN_API_KEY` (optional, used for `--verify`)

Deploy:

```bash
make deploy ARGS="--network sepolia"
```

## CI

GitHub Actions runs:

- `forge fmt --check`
- `forge build --sizes`
- `forge test -vvv`

Workflow: [.github/workflows/test.yml](.github/workflows/test.yml)

## Notes

- The test suite uses Anvil time/block manipulation (`vm.warp`, `vm.roll`) and VRF mocks to validate winner selection and payout behavior.
- This is a learning/portfolio project intended to demonstrate smart contract engineering practices.
