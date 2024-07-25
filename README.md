# Solidity-Interview-Questions-And-Answers
You might as well Hire me, since I've already answered these :) 
#### What is the difference between private, internal, public, and external functions?
Private functions are only accessible to the contract that defines it.

Internal functions are only accessible to the contract that defines it and any child contracts that inherit it.

External functions are only accessible externally by other contracts and are not visible inside the contracts that define them. External uses less gas than public.

Public functions are accessible to the contract that defines it and to any external contract or third party that calls it. The public is the default visibility.</summary></details>

#### Approximately, how large can a smart contract be?
EIP-170 set the contract code size limit to 0x6000 bytes (i.e 24,576 bytes or approx 24kb) which results in a contract creation failure with an out-of-gas error. It was introduced to address the slight quadratic vulnerability in Ethereum and prevent DOS denial of service attacks.

Although the nodes are compensated with gas fees for the computational work that they perform when executing a transaction, there are additional costs associated with maintaining and interacting with the smart contract that are not directly covered by the gas fees.

Storage Costs: storing the contract’s bytecode and state on the blockchain incurs storage costs for the nodes. As the size of the contract grows, so does the storage cost for the nodes

Disk Access and Preprocessing: reading the contract’s bytecode from disk and preprocessing it for execution on the virtual machine also imposes computational overhead on nodes.

Merkle Proof Generation: Generating Merkle proofs for contract state verification , especially for large contracts or complex state structures.

Gas fees do not adequately cover these long-term costs associated with maintaining and interacting with smart contracts. This creates a potential imbalance where the deployer of a contract may incur relatively low transaction costs while imposing a significantly higher computational burden on the nodes after deployment.

Without a maximum contract size or other mechanisms to limit the computational workload imposed by smart contracts, there is a risk that malicious actors could deploy large contracts or design contract interactions in a way that maximizes computational work for nodes, effectively conducting a Denial-of-Service (DoS) attack on the network.

Even with a limited contract size, the attack is only deterrant, not impossible. The attacker will have to create multiple large contracts , multiplying the cost and making it more difficult to execute a coordinated attack effectively.

#### What is the difference between create and create2?
EVM has 2 opcodes to create a new contract address. Create and Create2.They are both used to determine the address of the smart contract even before it is deployed.

CREATE: A new contract can take the hash of the sender address and nonce.

Precisely, the last 20 bytes of keccak256 of RLP encoding of the creator’s contract address and a nonce. Every account has an associated nonce; for regular accounts, it is increased on every transaction. For contract accounts, it is increased on every contract creation. Since nonces can’t be reused and must be sequential, it’s almost possible to predict the address where the next created contract will be deployed, onyl if no other transaction happens before that which is an undesirable property of counterfactual systems, says openzeppelin. So, CREATE is not reliable to predict the address accurately.

CREATE2:

The address of the new contract created is by taking the keccak256 hash of contract offset constant 0xFF, deployer address, salt(an arbitrary value provided by the deployer), and contract initialization code. Since the deployer provides the salt value, it’s theoretically possible to choose a desirable address by adjusting the salt value.

#### What major change with arithmetic happened with Solidity 0.8.0?
Arithemetic operations revert on underflow and overflow . They are checked by default default and Panic(0x11) error is throw and the call iss reverted.

#### What special CALL is required for proxies to work?
Delegate call is required for proxy to work. delecate call executes code in another contract in the context of the contract that calls it.For this to work, the storage layout must be preserved. When A executes delegatecall to B, B’s code is executed with the context of A’s storage, A’s msg.sender and A’s msg.value. The implementation contract can be upgraded without the smart contract having to change the address of the proxy.

#### Prior to EIP-1559, how do you calculate the dollar cost of an Ethereum transaction?
Prior to EIP-1559,The miner received 100% of the gas cost.

Dollar Cost = (Gas Price * Gas Used / 1e9) * Ether Exchange Rate

Gas used: The amount of gas consumed by the transaction.

Gas price: The price per unit of gas specified by the sender in Gwei (1 Gwei = 0.000000001 ETH).

1e9: is 10 to the power 9,for conversion from gwei to ether.

Ether Exchange Rate: The current exchange rate of Ether to US dollars.

#### After EIP-1559, how do you calculate the dollar cost of an Ethereum transaction?
Dollar Cost = (Base Fee + Priority Fee) * Gas Used / 1e9 * Ether Exchange Rate

Base Fee(Gwei): The minimum fee required to include a transaction in a block.Its dynamically adjusted based on network congestion.The fee is then burned.

Priority Fee:(Tip) The amount paid to miners per gas to incentivize them to include the transaction. Users specify this fee.

Gas Used(units): The amount of gas that can be used for the transaction.

Ether Exchange Rate: The current exchange rate of Ether to US dollars.

#### What are the challenges of creating a random number on the blockchain?
Blockchain is deterministic. They must arrive at the same state after processing same set of transactions and all of this data is public. So, arriving at random number is challenging. althought attempts to generate random numbers from blocknumber and block timestamp can be used for limited randomness, they also pose the risk of miners poentially manipulating the process if they have significant hashing power.

#### What is the difference between a Dutch Auction and an English Auction?
A Dutch auction starts with a high price that gradually decreases until a buyer accepts the price. An English auction starts with a low price that increases as bidders competitively bid higher amounts until no higher bids are made.

#### What is the difference between transfer and transferFrom in ERC20?
A transfer function allows the sender to transfer tokens directly to another address. The sender's account must have enough balance to cover the transfer amount.
A transferFrom function allows an approved third party to transfer tokens from one address to another. It is used in conjunction with the approve function, where the token owner allows a spender to withdraw multiple times up to a certain amount.

#### Which is better to use for an address allowlist: a mapping or an array? Why?
Mapping is better because it is gas & time efficient and scaleable. Mappings provide O(1) time complexity for lookups, additions, and deletions.With mapping, we can directly check if a value is in allowlist or not. But to check it on an array, we will have to loop through it completely.

#### Why shouldn’t tx.origin be used for authentication?
tx.origin refers to the original sender of a transaction, while msg.sender refers to the immediate sender of a message within a contract. so, an attacker could phish a user to authnticate as tx.origin in a target contract.

#### What hash function does Ethereum primarily use?
It uses Keccak-256 hash function primariyl for core functions like generating addresses, creating cryptographic proofs, and ensuring the integrity of data. Note that it also uses other hash functions like RIPEMD-160,SHA-256,Blake2b for limited purposes.

#### How much is 1 gwei of Ether?
One gwei is equal to 0.000000001 ($1\times10^{-9}$) Ether.

#### How much is 1 wei of Ether?
One wei is equal to 0.000000000000000001 Ether ($1\times10^{-18}$) Ether. Wei is the smallest unit of Ether.

#### What is the difference between assert and require?
Assert is used for input validation, to 'check' the state before execution of a statement. Assert is used in testing to check for invariants and prevent conditions that should never be possible.

#### What is a flash loan?
Flash loan is a loan faciliated with a smart contract to be repaid within the same transaction.

#### What is the check-effects pattern?
It's a security pattern that protects control flow against reentrancy. Check-check the state of the contract,caller Effects-update the global state  Interactions-if the checks pass, perform external call.

#### What is the minimum amount of Ether required to run a solo staking node?
It's 32 ETH.

#### What is the difference between fallback and receive?
Receive is a special function executed when ether is sent to a contract and the msg.data is empty.
Fallback is a special function executed when a function that does not exist is called or if ether is sent to the contract where receive function doesn't exist or the msg.data is empty.

#### How do you send Ether to a contract that does not have payable functions, or a receive or fallback?
By providing target address as parameter to a selfdestruct call from another contract.The selfdestruct forces the target contract to receive ether. Although the opcode is depreciated, the attack is still possible using previous versions.

#### what is the gas limit per block?
30M gas

####  What is reentrancy?
An attack vector where the execution flow is transferred to an external contract allowing a function to be called recursively.

#### What prevents infinite loops from running forever?
gas,when gas limit is exceeded, the execution stops.

#### What is the difference between view and pure?
View - functions that can read from blockchain,but can't modify the state
Pure- functions that cannot read from blockchain and can't modify the state

#### What is the difference between transferFrom and safeTransferFrom in ERC721?
#### How can an ERC1155 token be made into a non-fungible token?
#### What is access control and why is it important?
#### What does a modifier do?
#### What is the largest value a uint256 can store?








  












