# Security Best Practices


## General rules

### Prepare for failure
- Pause the contract
- Limit amount of funds at risk
- Bugfix, improvements upgrade strategy

### Rollout Carefully
- Full tests coverage, add tests against new attack vectors
- Bug bounties & alpha testing
- Rollout in phases

### Keep Contracts Simple
- Create simple logic in contracts
- Single responsibiliy. Keep contracts & functions small
- Use battle-tested code & tools
- Clarify vs performance. Prefer clarity if possible
- Minimize blockchain usage in your system. Be reasonable

### Stay up to Date
- Check contracts if new bugs discovered
- Upgrade all to latest versions
- Adopt new security techniques

### Keep in Mind Blockchain Properties
- Be careful about external contract calls
- Public functions can be called by anyone in any order
- Private data is actually viewable
- Keep gas costs and block gas limit in mind
- Timestamps are imprcise on a blockchain
- Randomness is non-trivial

### Simplicity vs Complexity
- Rigid vs Upgradable
- Monolithic vs Modular
- Duplication vs Reuse


## Secure Development Recommendations

- External Calls
  - Don't use external contract calls when possible
  - Name variables, methods & contract interfaces that may interact with unsafe calls (`UntrustedBank`)
  - Check-effects-interactions patter. Avoid state changes after external calls. Mitigates Reentrancy risks.
  - Use `.call()`. Don't use `transfer()` and `send()`. Keep Reentrancy risks in mind, the `call()` does NOT mitigate it.
  - Handle errors from external calls
  - Use **"pull"** over **"push"** for external calls. Try to isolate each external call into its own transaction that can be initiated by the recipient of the call. Use for payments.
  - Don't `delegatecall` to untrusted code.


- Ether can be sent to any account
  - Contract addresses can be precomputed, ether can be sent to an address before the contract is deployed.
  - Be careful with coding conditions that strictly check the balance of a contract 

- On-chain data is public
  - Don't require users to publish information too early if privacy is at stake (games, auctions)
  - Use commitment scheme strategy

- Some particpants can stop participating
  - Don't depend on a specific party actions to process claims or refunds
  - Bypass non-acting participants (time limit etc.)
  - Add economic incentive to act/submit information

- Beware of negation of the most negative signed integer

- Solidity-specific recommendations
  - Enforce invariants with `assert()`. Combine with pausing and upgrading contracts (to fix a failing assertion)
  - Use `assert()`, `require()`, `revert()` properly
  - Use modifiers only for checks
  - Rounding with integer division. Use multiplier or store numberator and denominator (to calculate off-chain)
  - Tradeoff between **Interfaces** & **Abstract Classes**
  - Keep fallback functions simple & check data length
  - Explicitly mark payable functions and state variables
  - Explicitly mark visibility in functions and state variables
  - Lock pragmas to specific compiler version
  - Use events to monitor contracts activity
  - Be aware that'Built-ins' can be shadowed
  - Avoid using `tx.origin`. Never use it for authorization purposes
  - Be aware of timestamp manipulation
    - miner manipulation
    - it is safe to use a `block.timestamp` if your time-dependant event can vary by 15 sec. and maintain integrity
    - Avoid using `block.number` as a timestamp
  - Be careful with multiple inheritance. "Diamond" inheritance resolution
  - Use interface type instead of the address for type safety (compile-time check)
  - Avoid using `extcodesize` to check for EOA