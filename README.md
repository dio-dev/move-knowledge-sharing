# Layer 1 Blockchain Ecosystems: 
## A Technical Deep Dive into EVM, Solana, and Move-based Networks

---

## Contents

1. Introduction to L1 Ecosystems
2. Types of L1 Networks
3. CAIP-2 and Interoperability
4. Network Comparisons
5. Move Language Benefits
6. DAG-Based Architecture
7. Technical Deep Dives

---

## 1. Introduction to L1 Ecosystems

<img width="878" alt="image" src="https://github.com/user-attachments/assets/70c4ff64-5afb-4aed-9ac1-80a4af6beb2d" />


Layer 1 blockchain networks serve as the foundation of blockchain technology. Each ecosystem represents a different approach to solving fundamental blockchain challenges:
- Transaction processing
- State management
- Smart contract execution
- Asset handling

These differences create unique tradeoffs in terms of:
- Scalability
- Security
- Decentralization
- Developer experience

---

## 2. Types of L1 Networks

### Account-based Systems (e.g., Ethereum)
- Uses account balances and state changes
- Sequential transaction processing
- Smart contracts with global state
- Example: Traditional bank account model

### UTXO-based Systems (e.g., Bitcoin)
- Transaction output based
- Parallel transaction processing possible
- Limited smart contract capabilities
- Example: Physical cash model

### Parallel Execution Systems (e.g., Solana, Sui)
- Concurrent transaction processing
- Different approaches to state management
- Built for high throughput
- Example: Multiple cashiers serving customers simultaneously

---

## 3. CAIP-2 and Interoperability

### What is CAIP-2?
Chain Agnostic Improvement Proposal 2 provides standardized blockchain identifiers

### Why We Need It
- Enables cross-chain applications
- Simplifies wallet integrations
- Standardizes blockchain references
- Example format: "eip155:1" for Ethereum mainnet

### Benefits
- Consistent addressing across chains
- Easier integration development
- Better user experience
- Future-proof applications

---

## 4. Network Comparisons

### Solana vs EVM Networks

| Feature | Solana | EVM Networks |
|---------|---------|--------------|
| Consensus | PoH + PoS | PoS/PoW |
| Programming | Rust | Solidity |
| Transaction Model | Parallel | Sequential |
| Block Time | Sub-second | Several seconds |
| State Management | Account-centric | Global state |

Solana process independent transactions.
<img width="996" alt="image" src="https://github.com/user-attachments/assets/891a557f-dbe9-4bdf-8448-803c231266e4" />
<img width="700" alt="image" src="https://github.com/user-attachments/assets/67e017f3-203e-41fb-bb93-442dbac732bb" />
We need to specify to which account and which resource are used so system could analize it's dependent or independent. Not developer friendly. 



### Move-based Networks vs EVM

| Feature | Move-based | EVM Networks |
|---------|------------|--------------|
| Language | Move | Solidity |
| Data Model | Object-centric | Account-based |
| Execution | Parallel | Sequential |
| Asset Handling | Native objects | Contract-based |
| Security Model | Resource-based | Guard-based |

In aptos we have Multi version data structure. You balance could have few versions.
Transactions executed in paralel optimistically. We assume there are no dependant or overlaping transactions.

<img width="797" alt="image" src="https://github.com/user-attachments/assets/65cc5284-cb17-4c8b-8e7c-8197763f6979" />

If you trying to send to multiple wallets 3 transactions. it will create 3 versions of your balance which is not writen to blockchain yet. Also they don't have access to paralel versions.
then it comes to verification part. 

<img width="600" alt="image" src="https://github.com/user-attachments/assets/bdeefc33-548d-4466-bdbd-7223348e56dc" />

if verifier see that there are different versions of balance it reject it. And those will be executed later.
And sequencer will order them to avoid unnecessary execution. 

<img width="253" alt="image" src="https://github.com/user-attachments/assets/ddc7c889-8697-4419-9b9c-560786e2e106" />

like traffic but where drivers could change line. In solana it is strict.

<img width="1059" alt="image" src="https://github.com/user-attachments/assets/62531ade-e9ad-4e0e-aa59-d7e8c96aa8be" />

<img width="915" alt="image" src="https://github.com/user-attachments/assets/d1f5bb5c-4dbd-429d-b4a4-4cdd85208097" />

<img width="1138" alt="image" src="https://github.com/user-attachments/assets/ee817511-1b28-476f-abaf-ae920839c263" />



---

## 5. Move Language Benefits

### Security Advantages

#### Resource Safety
- Resources cannot be copied or discarded
- Must be explicitly moved between storage locations
- Prevents reentrancy attacks by design

#### Type Safety
```move
struct Token has key, store {
    value: u64
}
```
- Linear type system
- Explicit resource management
- No null references

#### Attack Prevention
- Reentrancy: Prevented by design
  
  <img width="1077" alt="image" src="https://github.com/user-attachments/assets/c040c9d3-a246-4b6e-a35d-02965e3be4bf" />

- Double-spending: Impossible through resource system
- Integer overflow: Built-in checks
- Unauthorized access: Ability-based control
<img width="383" alt="image" src="https://github.com/user-attachments/assets/2932cb7e-96a0-4562-ab90-8d07b44827e4" />

```Move
// Copy = can be copied
// Drop = can be deleted
// Key = can be stored in global storage
// Store = can be stored inside other structures
struct Token has key, store {
    value: u64
}

// Compiler enforces these abilities
struct NonCopyableToken has store {
    value: u64
}
```

- MEV attack
  
    MEV (Maximal Extractable Value) or "Front-running" attack. Let me explain how it works and how different blockchains handle this security concern.
    Think of blockchain transactions like sending letters through a post office. In a regular system, letters are processed in the order they arrive. However, imagine if a postal worker could see what's in the     letters and rearrange them to their advantage before they're delivered. This is essentially what happens in MEV attacks.
    Here's how it typically works on EVM networks:
    When you submit a transaction, it first goes to the "mempool" - a waiting area for transactions before they're included in blocks. During this time, attackers (often working with validators) can:
    
    - See your pending transaction
    - Analyze what effect it will have (like a large trade that will move prices)
    - Create their own transaction with higher gas fees
    - Place their transaction before yours in the block

In aptos we have randomization of verifiers nodes. Few nodes should verify transaction. If some of node behaves differently it will have penalty(14m dollars should be locked). Randomaizing depend on stacked tokens and rating of node. 

---

## 6. Technical Deep Dives

### Aptos Implementation Details
- Rotating validator scheme
- Block-STM for parallel execution
- Flexible account model
- Key rotation capability
- Direct token ownership

### Sui Token Handling
- Object-based tokens
- Split and merge operations
- Direct ownership model
- Parallel processing
- DAG Structure 
- Causal transaction ordering

### Account Models
- EVM: Separate contract/user accounts
- Aptos: Unified resource accounts
- Sui: Object-centric ownership
- Solana: Program-derived addresses

---

## Key Takeaways

1. Each ecosystem offers unique advantages:
   - EVM: Mature ecosystem, strong developer tools
   - Solana: High performance, low costs
   - Move-based: Better asset handling, enhanced security

2. Future trends point toward:
   - Increased parallelization
   - Better security by design
   - Improved asset handling
   - Enhanced interoperability

3. Development considerations:
   - Different programming paradigms
   - Varying security models
   - Distinct performance characteristics
   - Unique state management approaches
  
---

## 7. Movement
![architecture_movement-bcaa30613d508744d2f3318a35e39a6a (1)](https://github.com/user-attachments/assets/99a5b28f-9b49-4c51-84c3-8e8e6719e9ad)


The most distinctive aspect of Movement's architecture is its modular design that separates different blockchain functions into specialized layers. Let's break this down:
### First Layer - Data Availability (DA) with Celestia:
Think of Celestia like a highly efficient filing system for blockchain data. The key innovation here is the use of DA light nodes, which act as intermediaries between the full network and Celestia. These light nodes handle two crucial operations:

- Write DA: Storing transaction data
- Read DA: Retrieving transaction data
  
This separation allows for better scalability since not every node needs to store all data.

### Transaction Flow Process:

- When a transaction is submitted through the REST API, it first enters the mempool (a waiting area for transactions)
- The system then creates batches of transactions, which is more efficient than processing them one by one
- These batches are sent to Celestia for sequencing, ensuring a proper order
- The sequenced data returns as "proto blocks" - think of these as preliminary blocks waiting to be finalized
- The MoveVM then executes these blocks
- Finally, L2 blocks are created and added to the state database

### The Settlement System:
Movement uses what's called "Fast Finality Settlement," which is particularly interesting. It works like this:

- Validator nodes actively participate in settling blocks
- They check the state root (a summary of all transactions) against Ethereum (L1)
- This creates a security checkpoint system that helps prevent incorrect state updates

### Node Types and Roles:
Movement has three distinct types of nodes, each with specific responsibilities:

Validator Nodes: Can write to DA and participate in settlement
Follower Nodes: Can write to DA but don't participate in settlement
Archival Nodes: Only read from DA and store historical data

### Fee Structure Innovation:
Movement breaks down fees into specific components:
CopyTotal Fee = DA Fee + Sequencing Fee + Execution Fee + Settlement Fee
This granular fee structure means users only pay for the exact services they use, making it more economically efficient than systems with flat fees.

### Key Advantages:

Modular Scalability: By separating DA, execution, and settlement, each component can be optimized independently
Economic Efficiency: The granular fee structure aligns costs with actual resource usage
Security: The multi-layer validation process with fast finality provides strong security guarantees
Light Node Support: The DA light node system makes it easier for more participants to join the network
