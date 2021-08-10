# Known Attacks

## Reentrancy

**External contracts can take over the control flow and and make state changes**

- Reentrancy on a Single Function. Involves functions that can be called repeatedly before the first invocation is finished


```javascript
// INSECURE
mapping (address => uint) private userBalances;

function withdrawBalance() public {
    uint amountToWithdraw = userBalances[msg.sender];
    (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); // At this point, the caller's code is executed, and can call withdrawBalance again
    require(success);
    userBalances[msg.sender] = 0;
}
```

- Cross-function Reentrancy. Usintg two different functions that share the same state (same contract or multiple contracts)


```javascript
// INSECURE
mapping (address => uint) private userBalances;

function transfer(address to, uint amount) {
    if (userBalances[msg.sender] >= amount) {
       userBalances[to] += amount;
       userBalances[msg.sender] -= amount;
    }
}

function withdrawBalance() public {
    uint amountToWithdraw = userBalances[msg.sender];
    (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); // At this point, the caller's code is executed, and can call transfer()
    require(success);
    userBalances[msg.sender] = 0;
}
```

### Pitfalls & Solutions of Reentrancy
  - Not only avoid calling external functions too soon. Also avoid calling functions which call external functions
  - Mark untrusted functions in addition to the fix making reentrancy impossible
  - Use mutexes to lock state so it can only be changed by the owner of the lock. Be very careful implementing mutexes


## Front-Running

**An observer can act on a transaction included in the mempool before being executed. For example: front-run a buy order on a decentralized exchange**

- Displacement. Run a malicious transaction before the target transaction to displace it
  - e.g. *Register a domain name first*
  - Done by increasing the `gasPrice`
- Insertion. Run a malicious transaction tefore the target transaction is executed.
  - e.g. *Buy asset first to sell it at the price of the target transaction*
  - Done by increasing the `gasLimit`
- Suppression. Run a malicoius transaction to delay execution of the target transaction.
  - e.g. *First winner in a game*
  - Multiple transactions with high `gasPrice` and `gasLimit`

### Mitigations

The best remediation is to **remove the benefit of front-running in your application**. Mainly, by removing the importance of transaction ordering time.
- Transaction Ordering. Implement batch operations (for example batch auctions)
- Confidentiality. Use "commit and reveal" scheme
- Cost-related limits. Specify maximum and minimum price range on a trade


## Timestamp Difference

**Timestamp fo the block can be manipulated by the miner**

## Integer Overflow and Underflow

**If a balance reaches the maximum uint value (2^256) it will circle back to zero which checks for the condition.**

### Mitigations
- Use `SafeMath.sol`
- Solidity >=0.8.0 supports safe checks out of the box

## DOS with Unexpected revert

- If an attacker bids with a SC which has a fallback function that reverts any payment, the attacker can win any auction
```javascript
// INSECURE
contract Auction {
    address currentLeader;
    uint highestBid;

    function bid() payable {
        require(msg.value > highestBid);

        require(currentLeader.send(highestBid)); // Refund the old leader, if it fails then revert

        currentLeader = msg.sender;
        highestBid = msg.value;
    }
}
```

- Iterate through an array to pay users (e.g. crowdfunding contract)
```javascript
address[] private refundAddresses;
mapping (address => uint) public refunds;

// bad
function refundAll() public {
    for(uint x; x < refundAddresses.length; x++) { // arbitrary length iteration based on how many addresses participated
        require(refundAddresses[x].send(refunds[refundAddresses[x]])) // doubly bad, now a single failure on send will hold up all funds
    }
}
```

### Mitigations
Pull over push payments

## DOS with Block Gas Limit
- Gas Limit DoS on a Contract via Unbounded Operations
  - By paying out to everyone at once, you risk running into the block gas limit. An attacker can maniuplate the amount of gas needed (e.g. add a bunch of addresses)
- Gas Limit DoS on the Network via Block Stuffing
  - Even if your contract does not contain an unbounded loop, an attacker can prevent other transactions from being included in the blockchain for several blocks by placing computationally intensive transactions with a high enough gas price


## Insufficient Gas Greifing

This attack may be possible on a contract which accepts generic data and uses it to make a call another contract (a 'sub-call') via the low level address.call() function, as is often the case with multisignature and transaction relayer contracts.

### Mitigations
- Implement logic requiring forwarders to provide enough gas to finish the subcall
- Permit only trusted accounts to relay the transaction

## Forcibly Sending Ether to a Contract

It is possible to forcibly send Ether to a contract without triggering its fallback function.

The selfdestruct contract method allows a user to specify a beneficiary to send any excess ether. selfdestruct does not trigger a contract's fallback function.

[SWC Registry](https://swcregistry.io/)
[Source](https://consensys.github.io/smart-contract-best-practices/known_attacks/)