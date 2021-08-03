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
  - Name variables, methods & contract interfaces that interacting may be unsafe (`UntrustedBank`)
  - Check-effects-interactions patter. Avoid state changes after external calls. Mitigates Reentrancy risks.
  - Use `.call()`. Don't use `transfer()` and `send()`. Keep Reentrancy risks in mind, the `call()` does NOT mitigate it.
  - Handle errors from external calls
  - Use **"pull"** over **"push"** for external calls. Try to isolate each external call into its own transaction that can be initiated by the recipient of the call. Use for payments.
  - Don't `delegatecall` to untrusted code.


- Ether can be sent to any account. Contract addresses can be precomputed, ether can be sent to an address before the contract is deployed.

[Source](https://consensys.github.io/smart-contract-best-practices/general_philosophy/)