# Smart Contract Development Best Practices


## Use a Development Environment

Use a development environment such as Truffle (alternatively, Embark, Buidler dapp.tools) to get productive, fast.

Truffle supports plugins that offer additional features. For example, truffle-security provides smart contract security verification. truffle-plugin-verify publishes your contracts on blockchain explorers. You can also create custom plugins.

Likewise, Buidler supports a growing list of plugins for Ethereum smart contract developers.

Whichever development environment you use, picking a good set of tools is a must.

## Develop Locally

Run a local blockchain for development with Ganache (or Ganache CLI) to speed up your iteration cycle.

## Use Static Analysis Tools

- Lint Solidity code with solhint and Ethlint. Solidity linters are similar to linters for other languages (e.g. JSLint.) They provide both Security and Style Guide validations.

- Security analysis tools identify smart contract vulnerabilities. These tools run a suite of vulnerability detectors and prints out a summary of any issues found. Developers can use this information to find and address vulnerabilities throughout the implementation phase. Options include: Mythril · Slither · Manticore · MythX · Echidna · Oyente

## Use Pre-Commit Hooks

Make your developer experience seamless by setting up Git hooks with husky. Pre-commit hooks let you run your linters before every commit.

## Understand Security Vulnerabilities

Smart contract development demands a completely different mentality than web development. ‘Move fast and break things’ does not apply here. You need to invest lots of resources upfront to write bug-free software. As a developer, you must:

1. Be familiar with Solidity security vulnerabilities, and

2. Understand Solidity design patterns such as pull vs push payments and Checks-Effects-Interactions, amongst others.

3. Use defensive programming techniques: static analysis and unit tests.

4. Audit your code.

## Write Unit Tests

Uncover bugs and unexpected behaviour early with a comprehensive test suite.

## Measure Test Coverage

Writing tests is not enough; Your test suite must reliably catch regressions. Test Coverage measures the effectiveness of your tests.

## Set up Continuous Integration

Once you have a test suite, run it as frequently as possible

## Security Audit Your Contracts

Security audits help you uncover unknowns in your system that defensive programming techniques (linting, unit tests, design patterns) miss.

## Bring in External Auditors

Major protocols in the Ethereum space hire (expensive) security auditors who dive deep into their codebase to find potential security holes. These auditors use a combination of proprietary and open-source static analysis tools.

At the end of the process, you get a report that summarizes the auditors’ findings and recommended mitigations. You can read audit reports by ChainSecurity, OpenZeppelin, Consensys Diligence, and TrailOfBits to learn what kind of issues are found during a security audit.

## Use Audited, Open Source Contracts

Secure your code from Day 1 by using battle-tested open-source code that has already passed security audits. Using audited code reduces the surface area you need to audit later on.

OpenZeppelin Contracts is a framework of modular, reusable smart contracts written in Solidity. 

### OpenZeppelin Notes

- You should always use the library from these published releases: copy-pasting library source code into your project is a dangerous practice that makes it very easy to introduce security vulnerabilities in your contracts.

- Use Upgradable contracts

## Launch on a Public Testnet

Before you launch your protocol on the Ethereum mainnet, consider launching on a testnet.  
During the testnet phase, organize a bug bounty program.

## Consider Formal Verification

Formal verification is the act of proving or disproving the correctness of an algorithm against a formal specification, using formal methods of mathematics.

## Store Keys in a Secure Manner

Store private keys of Ethereum accounts in a secure manner. Here are a few suggestions:

- Generate entropy safely.
- Do NOT post or send your seed phrase anywhere. If it’s a must, use an encrypted communication channel such as Keybase Chat.
- Do use hardware wallets such as a Ledger.
- Do use a multi-signature wallet (Gnosis Safe) for particularly sensitive accounts.

## Make It Open Source

Smart contracts enable permissionless innovation that lets anyone build and innovate on them. That is what blockchains are really useful for: public, programmable and verifiable computation.  
To attract developers you need to show that you won’t change the rules of the game later on. Open sourcing your code inspires confidence.

## Prioritize Developer Experience

The developer experience (DevEx) of your protocol is paramount. Make it easy for other developers to build on your protocol with developer-friendly APIs.

*💡 For the longest time, integrating payments was really hard. Early payments companies lacked modern code bases, and things like APIs, client libraries and documentation were virtually non-existent. Stripe made it easy for developers to add payments to their software. They are now incredibly successful.  
The 0x protocol is probably the gold standard when it comes to developer experience.*

Here are two suggestions to start:

- Provide contract SDKs and sample code
- Write good documentation

The user experience of your developer portal, the completeness of the API documentation, the ease with which people can search for the right solution for their use case, and the speed at which developers can start calling your contracts are all critical for adoption to happen.

Community engagement also plays an important part.

- How do developers find you?
- Where do you connect with developers?
- What makes your project attractive to build on?

Building an active community around your project will help drive adoption in the long term.

## Provide Contract SDKs

Writing and maintaining robust, client libraries for many programming languages is non-trivial. Having SDKs available helps developers build on your protocol.

*💡 Some projects go one step further and provide fully-functional codebases that you can run and deploy. For example, the 0x Launch Kit provides decentralized exchanges that works out-of-the-box.*

## Write Good Documentation

Building on open source software reduces development time, but comes with a tradeoff: learning how to use it takes time. Good documentation reduces the time developers spend learning.

There are many types of documentation:

- High-level explainers describe in plain-English what your protocol does. Explain in clear terms what the capabilities of your protocol. This section enables decision makers to evaluate whether or not your product serves their use cases.
- Tutorials go into more details: step-by-step instructions and explanations of what the various components are and how to manipulate them to achieve a certain goal. Tutorials should strive to be clear, concise and evenly spaced across steps. Use plenty of code examples to encourage copy/pasting.
- API Reference document the technical details of your smart contracts, functions, and parameters.

## Build CLI Tools and Runbooks

Runbooks are codified procedures to achieve a specific outcome. Runbooks should contain the minimum information necessary to successfully perform the procedure.

Build internal CLI tools and runbooks to improve operations. With smart contracts, this is usually a script containing one or more contract calls that performs a business operation.

*💡 To get started, pick an effective manual process, implement it in code, and trigger automated execution where appropriate.*

## Set up Event Monitoring

Efficient and effective management of contract events is necessary for operational excellence. An event monitoring system for your smart contracts keeps you notified of real-time changes in the system.

You can roll out your own monitoring backend with web3.js or use a dedicated service such as Dagger, Blocknative Notify, Tenderly, or Alchemy Notify.

## On Building DApp Backends

DApps need a way to read and transform data from smart contracts. However, on-chain data aren’t always stored in an easy-to-read format. Reading contract data directly from an Ethereum node is sometimes too slow for user-facing web and mobile apps. Instead, you need to index the data into a more accessible format.

theGraph offers a hosted GraphQL indexing service for your smart contracts. Queries are processed on a decentralized network that ensures that data remains open and that DApps continue to run no matter what.

Alternatively, you can build your own indexing service. This service would communicate with an Ethereum node and subscribe to relevant contract events, perform transformations, and save the result in a read-optimized format.

*💡 The lack of regulatory clarity in jurisdictions across the world means that at the flip of a hat, control can become liability. To address this, making parts of your system decentralized and non-custodial can help reduce that liability.*

## On Building DApp Frontends

A frontend application allows users to interact with smart contracts. Examples include the Augur and Compound apps. DApp frontends are usually hosted in a centralized server, but can also be hosted on the decentralized IPFS network to further introduce decentralization and reduce liability.

Frontend dApps load smart contract data from an Ethereum node through client libraries such as web3.js and ethers.js.

Libraries such as Drizzle, web3-react, and subspace offer higher-level features that simplify connecting to web3 providers and reading contract data.

## Strive for Usability

Crypto has a usability problem. Gas fees and seed phrases are intimidating for new users. Fortunately, the crypto user experience is improving at a rapid pace.

Meta Transactions and the Gas Stations Network offers a solution to the gas fee problem. Meta transactions allow services to pay gas fees on behalf of users, removing the need for users to hold Ether. Meta transactions also lets users pay fees in other tokens instead of ETH. These improvements are made possible through clever use of cryptographic signatures. With GSN, these meta transactions are distributed across a network of relayers who pays the gas.

Hosted wallets and smart contract wallets remove the need for browser extensions and seed phrases. Projects under this category include Fortmatic, Portis, Bitski, SquareLink, Universal Login, Torus, Argent, and walletconnect.

*💡 Consider using the web3modal library to add support for major wallets.*

## Build with Other Protocols in Mind

Ethereum has created a digital finance stack. Financial protocols are building on top of each other, powered by the permissionless and composable nature of smart contracts.

Each protocol provides the foundation for other protocols to build more sophisticated products.

If Ethereum is the Internet of Money, Decentralized Finance protocols are Money Legos.

Don’t reinvent the wheel in isolation. Instead of forking a clone of an existing protocol, can you build something for or with the pieces that already exist? Compared to building on centralized platforms, access to smart contracts cannot be taken away from under you.

## Understand Systemic Risks

When you’re building on DeFi, you must assess whether a protocol / currency adds more value than risk.

1. Smart Contract Risk
Smart contracts can have bugs. Always consider the possibility that a bug is found in the protocols you depend on.  
Smart contract risk compounds as more protocols are composed together, similar to how SLA scores are calculated. Because of the permissionless composability of smart contracts, a single flaw cascades into all dependent systems.

2. Counterparty Risk
How is a protocol governed? Some governance models may give direct control over funds or attack vectors to the governance architecture which could expose control and funds.
Different protocols have different degrees of decentralization and control. Be wary of protocols with a small community and limited track record.

3. Mitigating Risk
Mitigate your overall risk exposure by following these basic principles:

- Interact only with audited smart contracts.
- Interact only with liquid currencies that has a significant community and product.
- Purchase smart contract insurance.

## Participate in dev communities

Smart contract development is evolving rapidly, with new tools and standards launching from talented teams all over the world.

Keep up with the latest developments in the space by visiting online Ethereum communities: ETH Research, Ethereum Magicians, r/ethdev, OpenZeppelin Forum, and the EIPs Github repo.

## Subscribe to newsletters

Newsletters are a great way to stay up-to-date with the Ethereum ecosystem. I recommend subscribing to Week in Ethereum and EthHub Weekly.