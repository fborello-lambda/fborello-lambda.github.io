+++
title = "Ethereum"
date = 2024-05-01

[taxonomies]
tags=["ethereum", "programming"]
+++

## Ethereum

## [Glossary](https://ethereum.org/en/glossary/)

The Ethereum organization proposes some specs or improvements `EIPs` , this specs are what **_Architecture_** means for hardware.

Updates are released when a certain block height is met. All execution clients agree on the height.

[Accounts](https://ethereum.org/en/developers/docs/accounts/) → Difference between `Externally Owned Account` and `Contract Account`.
[selfdestruct - Redeploy a smart contract on the same address - Ethereum Stack Exchange](https://ethereum.stackexchange.com/questions/142626/redeploy-a-smart-contract-on-the-same-address)

[ERC-1271: Standard Signature Validation Method for Contracts](https://eips.ethereum.org/EIPS/eip-1271)

Types of Tx accepted by zkSync → [EIP-712: Typed structured data hashing and signing](https://eips.ethereum.org/EIPS/eip-712)

- [EIP712 is here: What to expect and how to use it | by Koh Wei Jie | MetaMask | Medium](https://medium.com/metamask/eip712-is-coming-what-to-expect-and-how-to-use-it-bb92fd1a7a26)
- Gas calculation → [What is EIP-1559? | Coinbase Help](https://help.coinbase.com/en/coinbase/getting-started/crypto-education/eip-1559)

System Contracts → Contracts that provide general purpose code, some interact with the L1 like `Diamond`.
Contract’s events:

> You should emit an event anytime something occurs in your smart contract that some system outside the blockchain should be aware of so that the outside system may listen for such occurrences

[What are Paymasters?](https://www.stackup.sh/blog/what-are-paymasters)

[Gas Refund](https://eips.ethereum.org/EIPS/eip-3529)

[ommer blocks](https://ethereum.org/en/glossary/#ommer)

[optional access lists](https://eips.ethereum.org/EIPS/eip-2930)

Rollups:

- Optimistic
- zk Rollups
  - Validium → Proofs are not stored on the blockchain.
  - Sometimes, a merkletree that contains all the transactions hashes is built and then the root composes the zk Proof that is later stored on the L1.

Rollup operation requires the assistance of an operator, who rolls transactions together, computes a zero-knowledge proof of the correct state transition, and affects the state transition by interacting with the rollup contract.

[Sharding](https://ethereum.org/es/roadmap/danksharding/)

zkEVM → Solves the problem of creating a general purpose verifier. Taking into account that the `circuit` (Arithmetization of the problem) has to be fixed in order to verify proofs, the idea is to have a so called `Virtual Machine` that operates like a processor whose proof takes into account each step of the execution.

- [TinyRAM](https://www.scipr-lab.org/doc/TinyRAM-spec-2.000.pdf) inspires the zkSync’s zkEVM
- How is the zkEVM compatible with Solidity? Solidity → compiles to → Yul → compiles to LLVM → Then the bytecode is interpreted by the zkEVM.

## EVM

[EVM Deep Dives: The Path to Shadowy Super Coder 🥷 💻 - Part 1](https://noxx.substack.com/p/evm-deep-dives-the-path-to-shadowy)

[EVM Deep Dives: The Path to Shadowy Super Coder 🥷 💻 - Part 2](https://noxx.substack.com/p/evm-deep-dives-the-path-to-shadowy-d6b?s=r)

## MISC

- [ethereum - How does the opcode JUMP work in the EVM Stack? - Stack Overflow](https://stackoverflow.com/questions/44602539/how-does-the-opcode-jump-work-in-the-evm-stack)
- [creationcode - What is the difference between bytecode, init code, deployed bytecode, creation bytecode, and runtime bytecode? - Ethereum Stack Exchang](https://ethereum.stackexchange.com/questions/76334/what-is-the-difference-between-bytecode-init-code-deployed-bytecode-creation)
- [Merkle Patricia Trie | ethereum.org](https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie)
- [Verkle trees | ethereum.org](https://ethereum.org/en/roadmap/verkle-trees)
- Opcodes:
  - [Ethereum Foundation Opcodes](https://ethereum.org/en/developers/docs/evm/opcodes/)
  - [Opcodes interactive](https://ethervm.io/)
  - [David J. Pearce (Disassembling EVM Bytecode (the Basics))](https://whileydave.com/2023/01/04/disassembling-evm-bytecode-the-basics/)
- Logs
  - [Understanding event logs on the Ethereum blockchain | by Luit Hollander | MyCrypto | Medium](https://medium.com/mycrypto/understanding-event-logs-on-the-ethereum-blockchain-f4ae7ba50378)
  - [Ethereum Logs and Events - What are Event Logs on the Ethereum Network](https://moralis.io/ethereum-logs-and-events-what-are-event-logs-on-the-ethereum-network/)
  - [go ethereum - Understanding logs and log blooms - Ethereum Stack Exchange](https://ethereum.stackexchange.com/questions/12553/understanding-logs-and-log-blooms) (Important)
- Gas
  - [maxPriorityFeePerGas vs maxFeePerGas](https://docs.alchemy.com/docs/maxpriorityfeepergas-vs-maxfeepergas)
  - [EIP-1559 Gas Fees: Base Fee, Priority Fee, & Max Fee](https://www.blocknative.com/blog/eip-1559-fees)

[How to convert eth gas price in gwei to $. : r/ethereum](https://www.reddit.com/r/ethereum/comments/pxze1i/how_to_convert_eth_gas_price_in_gwei_to/):

```sh
1 ETH = 1e18 wei

1 Gwei (for giga wei) = 1e9wei = 0.000000001 ETH

A standard transaction costs 21_000 gas.

Assume gas price 131 gwei

131 gwei/gas * 21_000 gas = 2_751_000 gwei = 0.002751 ETH
```

## Yul | EVM's "Assembly" lang

- earn yul:
  - [Docs](https://docs.soliditylang.org/en/latest/yul.html)
  - [learn-yul](https://github.com/andreitoma8/learn-yul)

## ZK Rollups

Blogposts:

- [Hyperchain Blog](https://blog.matter-labs.io/introduction-to-hyperchains-fdb33414ead7)
- [Hyperscaling-zkSync](https://docs.zksync.io/zk-stack/concepts/hyperchains-hyperscaling.html)

## Solidity

- Following the [REMIX IDE tutorial](https://remix.ethereum.org/)

Kick off Contract:

```solidity
// SPDX-License-Identifier: MIT // <-- License
pragma solidity ^0.8.0; // <-- Specify version

contract SimpleStorage {
    // State variable, stored in the Blockchain
    uint public n; // It is initialized to 0

    // A transaction is needed to change the state variable.
    function set(uint _n) public {
        n = _n;
    }

    // To read the variable you don't need to send a transaction.
    function get() public view returns (uint) {
        return n;
    }

    // Pure functions neither read nor write the Blockchain's state
    function add(uint i, uint j) public pure returns (uint) {
        return i + j;
    }
}
```

- Solidity interprets the `Contract` keyword as the “entry point”, just like the `main` function in a programming language.
- Smart Contracts are atomic, if one operation fails, the entire operation is reverted without altering the Blockchain’s state.
- The variables that are outside `functions` are stored in the Blockchain. And are called `State variables` . A scheme of “getters” and “setters” is used.
  - If the function takes inputs like our `set` function (line 9), you must specify the parameter types and names. A common convention is to use an underscore as a prefix for the parameter name to distinguish them from state variables.
- `pure` and `view` functions don’t modify the Blockchain’s state.
  "The following statements are considered modifying the state:
  1. Writing to state variables / Reading from state variables.
  2. Emitting events.
  3. Creating other contracts.
  4. Using selfdestruct.
  5. Sending Ether via calls.
  6. Calling any function not marked view or pure.
  7. Using low-level calls.
  8. Using inline assembly that contains certain opcodes."
  9. Accessing `address(this).balance` or `<address>.balance`.
  10. Accessing any of the members of block, tx, msg (with the exception of `msg.sig` and `msg.data`).
- `Modifiers` and `Constructors`
  - A `Constructor` is called the first time the contract is deployed.
  - A `Modifier` is used to change the behavior of a function, in other words, it checks some parameters and executes the function. An `_` (underscore) is used to symbolize the function’s code: The code you place before the underscore in the modifier will be executed before the code in the body of the modified function. The code after the underscore will be executed after the code in the body of the modified function. [require](https://docs.soliditylang.org/en/latest/control-structures.html#error-handling-assert-require-revert-and-exceptions) keyword.
- Function visibilty:
  - private: only inside the contract
  - internal: inside the contract and by child contract
  - public: inside contract, child contract and other contracts or transactions
  - external: only called by other contracts or transactions, state variables can not be external
- [What are the virtual and override keywords in Solidity? - Ethereum Stack Exchange](https://ethereum.stackexchange.com/questions/78572/what-are-the-virtual-and-override-keywords-in-solidity)
- DataTypes
  - Arrays
  - Enums
  - Structs
  - Mappings (Used to store values related to an `address`)
    - `mapping(address => uint) balance;`
    - Accessing: `balance[_addr];`
    - Nested mappings are allowed.
- DataLocations
  - [When to use Storage vs. Memory vs. Calldata in Solidity](https://docs.alchemy.com/docs/when-to-use-storage-vs-memory-vs-calldata-in-solidity)
  - Storage: stores the data permanently on the Blockchain
    - Variables located inside a `contract` but outside `functions` are called State variables, and are stored in `storage` .
  - Memory: Values stored in `memory` are only stored temporarily and are not on the blockchain, variables inside `functions` are stored in memory, sometimes the keyword is needed to specify which type of DataLocation is needed.
  - Calldata: stores function arguments. The data is stored temporarily and cannot be mutated.
  - Assignments:
    - Memory to Memory: This creates a reference.
    - “Global” Storage to “Local” Storage: This creates a reference.
    - Storage and Memory/Calldata: This creates a copy.
  - `.selector` [go ethereum - explanation of appending .selector in solidity smart contracts - Ethereum Stack Exchange](https://ethereum.stackexchange.com/questions/72687/explanation-of-appending-selector-in-solidity-smart-contracts)

[SPDX licenses](https://spdx.org/licenses/)

[Cheatsheet](https://docs.soliditylang.org/en/latest/cheatsheet.html#global-variables)
