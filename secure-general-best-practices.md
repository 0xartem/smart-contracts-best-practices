# Secure Software Engineering Techniques

## Upgrading Broken Contracts

- Upgrade strategy
  - Have a registry contract that holds the address of the latest version of the contract
  - Hava a contract that forwards calls and data onto the lates version of the contract
    - Relies on the `fallback` function in `Relay` contract to forward the calls to a target contract using `delegatecall`
    - Storage, current address and balance still refer to the calling contract, only the code is taken from the called address
    - Referred as `Proxy` Patterns, but also knows as `Router`, `Dispatcher` and `Relay`
- Modularization, separation between components
  - Separate complex logic from your data storage, so you don't recreate all the data in order to change functionality
- Have a secure way for parties to decide to upgrade the code
  - Approved by a single trusted party
  - A group of people
  - A vote of the stakeholders

## Circuit Breakers (Pause contract)

- Ciruit breakers stop execution when certain conditions are met, and can be useful when new errors are discovered
- Can be triggerd by trusted parties or have programmatic rules that automatically trigger the circuit breaker

## Speed Bumps (Delay contract actions)

- Speed bumps slow down actions, so that if if malicious actions occur, there is time to recover

## Rate Limiting

- Rate limiting halts or requires approval for substantial changes
  - A depositor may be allowed to only withdraw a certain amount or percantage over a certain time period

## Contract Rollout

- Substantial testing
  - 100% test coverage
  - Deploy on your own testnet
  - Deploy on a public testnet with testing & bug bounty
  - Allow various players to play with the contract volume during exhaustive 
  - testing
  - Deploy on the mainnet in beta, with limits to the amounts at risk
- Automatic deprecation
  - An alpha contract may work for several weeks and then automatically shut down all actions, except for the final withdrawal
- Restrict the amount of Ether per user/contract (in the early stages)

## Bug Bounty Programs
