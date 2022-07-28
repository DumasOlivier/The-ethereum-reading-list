# The Ethereum reading list

<div align="center">
  <img src="https://ethereum.org/static/4d030a46f561e5c754cabfc1a97528ff/3ba1a/impact_transparent.webp" width="40%" />
</div>

Reading list of interesting articles and their highlights used to learn Solidity and blockchain concepts.

Note : All illustrations are from the [official ethereum website.](https://ethereum.org/en/)

### How to contribute ?

- Add useful articles and their highlights
- Summarize the articles where you see a 🏗️
- Create an issue to request an article explaining concepts that you want to learn

## Blockchain addresses

<details>
<summary><a href="https://info.etherscan.com/what-is-an-ethereum-address/">What is an ethereum address ?</a></summary>

- There are two types of addresses in Ethereum: Externally Owned Address (EOA) and Contract Address.

- Externally Owned Address refers to an account with a public and private key pair that holds your funds : a 42-character hexadecimal address derived from the last 20 bytes of the public key controlling the account with 0x appended in front.

- Contract address refers to the address hosting a collection of code on the Ethereum blockchain that executes functions.

</details>

<details>
<summary><a href="https://unblock.net/what-is-a-blockchain-address/">What is a blockchain address ?</a></summary>

- When Bitcoin was first created it had the ability to send Bitcoin payments directly to IP addresses. However, Bitcoin’s developers soon realized that this could be vulnerable to man-in-the-middle attacks, so they removed the feature, and it hasn’t been restored to date.

- Once Bitcoin abandoned the Pay to IP idea the developers switched to the Pay To Public Key Hash

- The format of an address (Bank account nuber, SWIFT code, postal address) doesn’t matter at all, what matters is that it serves its purpose of helping to locate a specific location – physical or virtual.

- It makes no difference what algorithms you use to generate the address, how you manipulate the public key, or what the resulting address actually looks like. However, it is important to note that the method used to create an address can have implications on usability, privacy and security.

- Since the beginning, users of Bitcoin were perplexed when seeing Ethereum addresses, which are long hexadecimal strings beginning with 0x, like this: 0x0eb81892540747ec60f1389ec734a2c0e5f9f735.

- The address creation method used by Ethereum isn’t so different from Bitcoin. It begins with the private key, which uses ECDSA to create a public key, just like Bitcoin. The public key is then hashed using Keccak-256, which gives us a 32-byte string. Ethereum drops the first 12 bytes, and the 20 bytes left over yield a 40 character hexadecimal address, to which is added a 0x prefix

- There’s another critical difference with Ethereum addresses, and that’s a lack of checksum. With Ethereum there’s no protection from typing just one character wrong and having your funds gone forever.

- Ethereum developers are somewhat partial to the ICAP format, which is base58 and uses checksums just like Bitcoin and other cryptocurrencies. The really potentially attractive feature of the ICAP format though is that it is compatible with another existing format – the International Bank Account Number (IBAN) system. This means all the existing banking software and systems can understand and interact with these ICAP Ethereum addresses.
</details>

<details>
<summary><a href="https://ethereum.org/en/developers/docs/accounts">Ethereum Accounts</a></summary>

 * Accounts can be user-controlled (Externally-owned accounts) or deployed as smart contracts (Smart Contract accounts).

- Both can receive, hold and send ETH and tokens & Interact with deployed smart contracts

- Key differencies

  > Externally-owned accounts
  >
  > - Creating an account costs nothing
  > - Can initiate transactions
  > - Transactions between externally-owned accounts can only be ETH/token transfers
  >   <br/>
  >   Contract accounts
  >
  > - Creating a contract has a cost because you're using network storage
  > - Can only send transactions in response to receiving a transaction
  > - Transactions from an external account to a contract account can trigger code which can execute many different actions, such as transferring tokens or even creating a new contract

- Accounts have four fields :

  - **nonce :** Number of transactions sent from the account. In a contract account, this represents the number of contracts created by the account.
  - **balance :** The number of wei owned by this address.
  - **codeHash :** This hash refers to the code of an account on the Ethereum virtual machine (EVM). It cannot be changed, unlike the other account fields. For externally owned accounts, the codeHash field is the hash of an empty string.

  - **storageRoot (or storage hash):** A 256-bit hash of the root node of a Merkle Patricia trie that encodes the storage contents of the account (a mapping between 256-bit integer values), encoded into the trie as a mapping from the Keccak 256-bit hash of the 256-bit integer keys to the RLP-encoded 256-bit integer values.

- The public key is generated from the private key using the _Elliptic Curve Digital Signature Algorithm_.

- The public address for an account is built by taking the last 20 bytes of the Keccak-256 hash of the public key and adding 0x to the beginning.

- It is possible to derive new public keys from your private key but you cannot derive a private key from public keys.

- A contract address comes from the creator's address and the number of transactions sent from that address (the “nonce”).

</details>

## Merkle Tree

What is it and how does it work 🏗️

## Hash functions

Introduction to hash functions 🏗️

SHA256 hashing function 🏗️

## The Ethereum Virtual Machine

<details>
  <summary><a href="https://ethereum.org/en/developers/docs/evm/">Ethereum Virtual Machine</a></summary>

  - Exist as one single entity maintained by thousands of connected computers running an Ethereum client

  - At any given block in the chain, Ethereum has one and only one 'canonical' state.

  - The EVM is what defines the rules for computing a new valid state from block to block.

  -  Instead of a distributed ledger (like Bitcoin), Ethereum is a distributed state machine.

### FROM LEDGER TO STATE MACHINE

  > Ethereum's state is a large data structure which holds not only all accounts and balances, but a machine state, which can change from block to block according to a pre-defined set of rules, and which can execute arbitrary machine code.

<div align="center">
  <img src="https://ethereum.org/static/e8aca8381c7b3b40c44bf8882d4ab930/302a4/evm.png" width="80%" />
</div>

### THE ETHEREUM STATE TRANSITION FUNCTION

  - The EVM behaves as a mathematical function would: Given an input, it produces a deterministic output.

```Solidity
  //Given an old valid state (S) and a new set of valid transactions (T), the Ethereum state transition function Y(S, T) produces a new valid output state S'
  Y(S, T)= S'
```

#### State

  - The state of the EVM is an enormous data structure called a [modified Merkle Patricia Trie](https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/).

  - This modified Merkle Patricia Trie keeps all accounts linked by hashes and reducible to a single root hash stored on the blockchain.

#### Transactions

  - Transactions are cryptographically signed instructions from accounts.

  - There are two types of transactions : those which result in message calls and those which result in contract creation.

  - Contract creation results in the creation of a new contract account containing compiled smart contract bytecode.

#### EVM INSTRUCTIONS

  - The EVM executes as a stack machine with a depth of 1024 items.

  - Each item is a 256-bit word, which was chosen for the ease of use with 256-bit cryptography (such as Keccak-256 hashes or secp256k1 signatures).

  - During execution, the EVM maintains a transient memory, which does not persist between transactions.

  - Compiled smart contract bytecode executes as a number of EVM opcodes, which perform standard stack operations like XOR, AND, ADD, SUB, etc.

<div align="center">
  <img src="https://ethereum.org/static/9628ab90bfd02f64cf873446cbdc6c70/302a4/gas.png" width="80%" />
</div>

### EVM IMPLEMENTATIONS

- All implementations of the EVM must adhere to the specification described in the Ethereum Yellowpaper.

- All Ethereum clients include an EVM implementation.

</details>

<details>
  <summary><a href="https://hackmd.io/@Nhlanhla/SyIOqbUyK">Explaining core system Ethereum Virtual Machine</a></summary>

  ### What is EVM (Ethereum Virtual Machine) ?

  - The EVM allows smart contract code to run by compiling to EVM bytecode.

  - EVM keeps each node systemically isolated from others in order to avoid security risks (if one node is compromised, it does not influence any other nodes and blockchain network).

  ### Why do we need EVM ?

  - Enables smart contract (solidity) to be executed on any computer (OS agnostic).

  - works as a middle layer between a smart contract and operating system.

  <div align="center">
    <img src="https://i.imgur.com/8o8dKbn.jpg" width="60%">
  </div>

  - If we do not have EVM, we need to develop respective compilers for each operating system.

  ### EVM bytecode

  - EVM byte code is compiled from Solidity and executed on EVM.

  > [Flow]
  >
  > Source Code -> Bytecode -> Machine Code
  >
  > Source Code: File written in programming language such as Java, Solidity.
  >
  > Bytecode: Compiled from source code and run on Virtual Machine such as JVM, EVM
  >
  > Machine Code: Code that only operating system can read. Bytecode is converted to Machine Code and finally executed.

  ### What is Opcodes ?

  - Opcodes (Operation code) is a set of instruction code for computers.

  - Internally, EVM converts "EVM bytecode" to "Ethereum Virtual Machine Opcodes" to execute specific tasks.

  - A developer also can directly write opcode with Solidity Assembly. But it might be a lot harder to understand than solidity.

  ### Contract ABI (Application Binary Interface)

  - Interface between two program modules.

  - In Ethereum, Contract ABI is a standard defined as data structure and functions with JSON array format, to interact with a smart contract from external application.

  ### Gas and EVM

  - Fuel that is consumed for computation.

  - Amount of Gas in each instruction is clearly defined in [Ethereum Yellow Paper](http://paper.gavwood.com/)

  ### What is a Client

  - Ethereum client is a software that implements EVM (Geth: Go-Ethereum, Parity: Rust implementation etc...)

  - Provides EVM and other functions such as a command-line interface to interact Ethereum blockchain.

  - The client has two types: full node client and lightweight client.

  - Full node client downloads all blockchain data on disk and receives blocks and transaction to validate.

  - Lightweight client downloads only blockchain header in order to save disk space.

  #### Full Node Client

  - Stores the full blockchain data available on disk and can serve the network with any data on request.

  - Receives new transactions and blocks while participating in block validation.

  -  Verifies all blocks and states.

  - Stores recent state only for more efficient initial sync.

  - All state can be derived from a full node.

  - Once fully synced, stores all state moving forward similar to archive nodes (more below).

 #### Lightweight Client

  - Stores the header chain and requests everything else on demand.

  - Can verify the validity of the data against the state roots in the block headers

</details>

## Tokens

<details>
  <summary><a href="https://docs.openzeppelin.com/contracts/4.x/erc20">ERC20</a></summary>

  - An ERC20 token contract keeps track of fungible tokens: any one token is exactly equal to any other token.

  - Each Token is exactly the same (in type and value) than another Token.

  - It's quite easy to build your own ERC20 token contract using Openzepplin standards :

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract GLDToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("Gold", "GLD") {
        _mint(msg.sender, initialSupply);
    }
}
```

  - About decimals :
> Often, you’ll want to be able to divide your tokens into arbitrary amounts: say, if you own 5 GLD, you may want to send 1.5 GLD to a friend. Unfortunately, Solidity and the EVM do not support this behavior: only integer (whole) numbers can be used, which poses an issue. You may send 1 or 2 tokens, but not 1.5.

- To work around this, ERC20 provides a decimals field, which is used to specify how many decimal places a token has.

- Decimals is only used for display purposes. All arithmetic inside the contract is still performed on integers.

- You’ll probably want to use a decimals value of 18, just like Ether and most ERC20 token contracts in use.

```solidity
// Send 5 tokens using a token contract with 18 decimals :

transfer(recipient, 5 * (10 ** 18));
```

</details>

<details>
  <summary><a href="https://docs.openzeppelin.com/contracts/4.x/erc721">ERC721</a></summary>

  - ERC721 is a standard for representing ownership of non-fungible tokens, where each token is unique.

  - Oppenzepplins standards makes it easier to implement an ERC721 contract by using it's ERC721URIStorage contract :

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract GameItem is ERC721URIStorage {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    constructor() ERC721("GameItem", "ITM") {}

    function awardItem(address player, string memory tokenURI)
        public
        returns (uint256)
    {
        uint256 newItemId = _tokenIds.current();
        _mint(player, newItemId);
        _setTokenURI(newItemId, tokenURI);

        _tokenIds.increment();
        return newItemId;
    }
}
```

  - Metadata are used to store most of the token properties that makes it unique. It can be stored on the blockchain directly but it's cheaper to do this offchain by using IPFS to store your JSON document and only store the url that points to this document onchain.

  - The structure of the metadata is important if you want to see your tokens / NFTs listed on opensea for example, you can see in the [official opensea guidelines on how to structure your metadata here](https://docs.opensea.io/docs/metadata-standards#metadata-structure).

  - Example of metadata structure :

```json
{
  "description": "Friendly OpenSea Creature that enjoys long swims in the ocean.",
  "external_url": "https://openseacreatures.io/3",
  "image": "https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png",
  "name": "Dave Starbelly",
  "attributes": [
    {
      "trait_type": "Base",
      "value": "Starfish"
    },
    {
      "trait_type": "Eyes",
      "value": "Big"
    }
  ],
}
```

</details>

<details>
  <summary><a href="https://docs.openzeppelin.com/contracts/4.x/erc777">ERC777</a></summary>

  - ERC777 is a standard for fungible tokens, and is focused around allowing more complex interactions when trading tokens.

  - The standard is designed for improvements in `decimals`, minting, and burning with proper events, but its main feature is receive hook.

  > A hook is simply a function in a contract that is called when tokens are sent to it, meaning accounts and contracts can react to receiving tokens.

  - It enables atomic purchases using tokens, rejecting reception of tokens by reverting the hook call, and redirecting the received tokens to other addresses becomes easier.

  - The contracts that obey ERC777 standards are required to implement the hook present in contract in order to receive tokens.

  > The implementation of the hook makes sure that no token can get stuck in a contract that is unaware of ERC777 protocol, which happens a lot in case of ERC20.

  **Note:** The ERC777 standard is backwards compatible with ERC20, meaning one can interact with these tokens as if they are ERC20, using the standard functions and adding the sending hook.

  ### Constructing an ERC777 token contract
  ERC777 protocol includes all the standards of ERC20 and additionally the hook.

  ```solidity
  // contracts/GLDToken.sol
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.0;

  import "@openzeppelin/contracts/token/ERC777/ERC777.sol";

  contract GLDToken is ERC777 {
    constructor(uint256 initialSupply, address[] memory defaultOperators)
        ERC777("Gold", "GLD", defaultOperators)
    {
        _mint(msg.sender, initialSupply, "", "");
    }
  }

  ```

  - Both `name` and `symbol` are assigned, but not `decimals`. The ERC777 specification mandates that decimals always returns a fixed value of 18, so there’s no need to set it ourselves.

  ```solidity
  > GLDToken.balanceOf(deployerAddress)
  1000
  ```

  ```solidity
  > GLDToken.transfer(otherAddress, 300)
  > GLDToken.send(otherAddress, 300, "")
  > GLDToken.balanceOf(otherAddress)
  600
  > GLDToken.balanceOf(deployerAddress)
  400
  ```

  ### Sending tokens to contracts
  The token transfer might revert in case of failed transfer. The token is returned back with the below message. This prevent tokens from being locked in forever.

  ```
  ERC777: token recipient contract has no implementer for ERC777TokensRecipient
  ```

</details>

[ERC1155 🏗️](https://docs.openzeppelin.com/contracts/4.x/erc1155)

## Smart contracts

### Memory keyword

<details>
<summary><a href="https://ethereum.stackexchange.com/questions/1701/what-does-the-keyword-memory-do-exactly">What does the keyword "memory" do exactly?</a></summary>

<br/>

_This is a Stack exchange answer from [eth](https://ethereum.stackexchange.com/users/42/eth), it was already a summary from the eth documentation and I found that it was worth reading fully like that :_

> The Ethereum Virtual Machine has three areas where it can store items.
>
> The first is “storage”, where all the contract state variables reside. Every contract has its own storage and it is persistent between function calls and quite expensive to use.
>
>The second is “memory”, this is used to hold temporary values. It is erased between (external) function calls and is cheaper to use.
>
>The third one is the stack, which is used to hold small local variables. It is almost free to use, but can only hold a limited amount of values.
>
>For almost all types, you cannot specify where they should be stored, because they are copied everytime they are used.
>
>The types where the so-called storage location is important are structs and arrays. If you e.g. pass such variables in function calls, their data is not copied if it can stay in memory or stay in storage. This means that you can modify their content in the called function and these modifications will still be visible in the caller.
>
>There are defaults for the storage location depending on which type of variable it concerns (source):

```solidity
// state variables are always in storage
// function arguments are always in memory
// local variables of struct, array or mapping type reference storage by default
// local variables of value type (i.e. neither array, nor struct nor mapping) are stored in the stack
```

</details>

### Deploying smart contracts

<details>
<summary><a href="https://ethereum.stackexchange.com/questions/101336/what-is-the-benefit-of-using-create2-to-create-a-smart-contract">What is the benefit of using create2() to create a smart contract?</a></summary>
<br/>

_This answer from [Batın Evirgen](https://ethereum.stackexchange.com/users/70574/bat%c4%b1n-evirgen) was copy pasted directly as it's super clear and short like that._

CREATE opcode is used by default when deploying smart contracts. The deployed contract address is calculated like this.

```
keccak256(senderAddress, nonce)
```

CREATE2 opcode is introduced later and allows you to predetermine the contract address. Contract address is computed like this.

```
keccak256(0xFF, senderAddress, salt, bytecode)
```

`0xFF` parameter is a constant to prevent collision with CREATE opcode.

`salt` parameter is a value sender sends when deploying contract.

`bytecode` parameter is, you probably guessed it, the bytecode of the smart contract you want to deploy.

If you want to predetermine the contract address before deploying, you can simply loop through different `salt` values and select the one you like (or want).

A great example of using CREATE2 can be seen here. [Application of CREATE2 opcode](https://www.reddit.com/r/ethdev/comments/a43tzv/example_of_how_to_use_the_new_create2_opcode_in/ebdeu00/)

Edit: Creating a contract with new keyword requires you to know the contract's source code. After creating a contract with new keyword, it will return the created contract's address.

This also uses CREATE opcode behind to create the contract.

</details>

<details>
<summary><a href="https://docs.openzeppelin.com/cli/2.8/deploying-with-create2">Deploying Smart Contracts Using CREATE2</a></summary>
<br>

- The CREATE2 opcode gives us the ability predict the address where a contract will be deployed, without ever having to do so.

- There are two major ways in which a smart contract can be deployed: with the CREATE and CREATE2 flows.

#### With `CREATE` :

> Smart contracts can be created both by other contracts (using Solidity’s `new` keyword) and by regular accounts. In both cases, the address for the new contract is computed the same way: as a function of the sender’s own address and a nonce.

```solidity
new_address = hash(sender, nonce)
```

> For regular accounts, the `nonce` is increased on every transaction, while for contract accounts it is increased on every contract creation. This means it is possible to predict the address where the next created contract will be deployed, but only if no other transactions happen before then - an undesirable property for counterfactual systems.

#### With `CREATE2` :

- The whole idea behind this opcode is to make the resulting address independent of future events.

```solidity
// New addresses are a function of:
// - 0xFF, a constant that prevents collisions with CREATE
// - The sender’s own address
// - A salt (an arbitrary value provided by the sender)
// - The to-be-deployed contract’s bytecode

new_address = hash(0xFF, sender, salt, bytecode)
```

#### Using CREATE2 from the Openzeppelin's CLI :

- Because CREATE2 is an EVM opcode, it is normally only usable by smart contracts and not external accounts.

- However, the OpenZeppelin CLI provides a handy way of running CREATE2-like deployments directly from the terminal.

- [Check the official documentation of how to do so here](https://docs.openzeppelin.com/cli/2.8/deploying-with-create2)

</details>

### Upgradeable contracts

[Transparent Implementation  🏗️](#)

[UUPS Implementation  🏗️](#)

## Ethereum Name Service

## Mining

<details>
<summary><a href="https://ethereum.org/en/developers/docs/consensus-mechanisms/pow/mining-algorithms/">Mining Algorithms (🏗️)</a></summary>
<br>
TODO
</details>

## Security

<details>
<summary><a href="https://ethereum.org/en/developers/docs/smart-contracts/security/">Smart contract Security</a></summary>

Also inspired from this really good article [Protect Your Solidity Smart Contracts From Reentrancy Attacks](https://medium.com/coinmonks/protect-your-solidity-smart-contracts-from-reentrancy-attacks-9972c3af7c21)

- An audit is no longer sufficient as the only security consideration. Security starts before you write your first line of smart contract code, security starts with proper design and development processes.

- Follow the development process [describe here](https://ethereum.org/en/developers/docs/smart-contracts/security/#smart-contract-development-process)

- Beware of attacks detailed in the following articles 👇

- You can use security tools such as Slither, Mythril, Securify ... Double check [the list in the article](https://ethereum.org/en/developers/docs/smart-contracts/security/#smart-contract-security) to find the one that fits your needs.
</details>

<details>
<summary><a href="https://consensys.github.io/smart-contract-best-practices/attacks/reentrancy/">Reetrancy attack</a></summary>

- A reentrancy attack occurs when a function makes an external call to another untrusted contract and this untrusted contract makes a recursive call back to the original function in an attempt to drain funds.

- Reentrancy on a Single Function

  - The first version of this bug to be noticed involved functions that could be called repeatedly, before the first invocation of the function was finished.

```solidity
// INSECURE
mapping (address => uint) private userBalances;

function withdrawBalance() public {
    uint amountToWithdraw = userBalances[msg.sender];
    (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); // At this point, the caller's code is executed, and can call withdrawBalance again
    require(success);
    userBalances[msg.sender] = 0;
}

// SECURE : Make sure you don't call an external function until you've done all the internal work you need to do:
mapping (address => uint) private userBalances;

function withdrawBalance() public {
    uint amountToWithdraw = userBalances[msg.sender];
    userBalances[msg.sender] = 0;
    (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); // The user's balance is already 0, so future invocations won't withdraw anything
    require(success);
}
```

- Cross-function Reentrancy

  - Attack possible when a vulnerable function shares state with another function that has a desirable effect for the attacker.

```solidity
// Here withdraw() calls the attacker’s fallback() function same as with the single function reentrancy attack.
// The difference is the fallback() function makes a call to transfer instead of recursively calling withdraw().
// Because the balance has not been set to 0 before this call, the transfer function can transfer a balance that has already been spent.

function transfer(address to, uint amount) external {
    if (balances[msg.sender] >= amount) {
        balances[to] += amount;
        balances[msg.sender] -= amount;
    }
}
function withdraw() external {
    uint256 amount = balances[msg.sender];
    require(msg.sender.call.value(amount)());
    balances[msg.sender] = 0;
}
```

- Prevent reentrancy attacks

  - Finishing all internal work (ie. state changes) first, and only then calling the external function.

  - Avoid calling functions which call external functions.

  - When it's possible for you, it's considered safer to use send() and transfer() instead of call() because they are limited to 2,300 gas.

  - Use the Checks-effects-interactions pattern
    > This pattern defines the order in which you should structure your functions.
    >
    > First perform any checks, which are normally assert and require statements, at the beginning of the function.
    >
    > If the checks pass, the function should then resolve all the effects to the state of the contract.
    >
    > Only after all state changes are resolved should the function interact with other contracts. By calling external functions last, even if an attacker makes a recursive call to the original function they cannot abuse the state of the contract.

- Mutex

  - In more complex situations such as protecting against cross-function reentrancy attacks it may be necessary to use a mutex.

  - A mutex places a lock on the contract state. Only the owner of the lock can modify the state.

  - OpenZeppelin has it’s own mutex implementation you can use called ReentrancyGuard. [Please check the documentation here.](https://docs.openzeppelin.com/contracts/4.x/api/security#ReentrancyGuard)

</details>

[Reetrancy attack via a modifier 🏗️](https://medium.com/valixconsulting/solidity-smart-contract-security-by-example-03-reentrancy-via-modifier-fba6b1d8ff81)

[Oracle manipulation attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/oracle-manipulation/)

[Frontrunning attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/frontrunning/)

[Timestamp Dependence attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/timestamp-dependence/)

[Insecure arithmetic attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/insecure-arithmetic/)

[Denial of service attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/denial-of-service/)

[Griefing attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/griefing/)

[Force feeding attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/force-feeding/)

## Bridges

## Scaling / Layer 2

<details>
<summary><a href="https://ethereum.org/en/developers/docs/scaling/">Scaling</a></summary>

- The main goal of scalability is to increase transaction speed (faster finality), and transaction throughput (high transactions per second), without sacrificing decentralization or security

## On-chain scaling

### Sharding

- Split a database horizontally to reduce network congestion and increase transactions per second by creating new chains, known as “shards.”

- This will also lighten the load for each validator who will no longer be required to process the entirety of all transactions across the network.

## Off-chain scaling

### Layer 2 scaling

- Scale your application by handling transactions off the Ethereum Mainnet while taking advantage of the decentralized security model of Mainnet.

- Generally speaking, transactions are submitted to these layer 2 nodes instead of being submitted directly to layer 1 (Mainnet).

- For some solutions the layer 2 instance then batches them into groups before anchoring them to layer 1, after which they are secured by layer 1 and cannot be altered.

#### Rollups

- Rollups perform transaction execution outside layer 1 and then the data is posted to layer 1 where consensus is reached. It exists two types of Rollups (see detailed articles in other sections) :

- Optimistic Rollups : Assumes transactions are valid by default and only runs computation, via a fraud proof, in the event of a challenge.

- Zero-knowledge Rollups : Zero-knowledge rollups: runs computation off-chain and submits a validity proof to the chain.

#### State channels

- State channels allow participants to transact x number of times off-chain while only submitting two on-chain transactions to the Ethereum network.

- Participants must lock a portion of Ethereum's state, like an ETH deposit, into a multisig contract.

- Locking the state in this way is the first transaction and opens up the channel. The participants can then transact quickly and freely off-chain.

- When the interaction is finished, a final on-chain transaction is submitted, unlocking the state.

- The two types of channels are currently state channels and payment channels.

#### Sidechains

- Independent EVM-compatible blockchain which runs in parallel to Mainnet.

- These are compatible with Ethereum via two-way bridges, and run under their own chosen rules of consensus, and block parameters.

- Several sidechains are existing such as Polygon PoS, Skale, and Rootstock.

#### Plasma chain

- Separate blockchain that is anchored to the main Ethereum chain, and uses fraud proofs (like optimistic rollups) to arbitrate disputes.

- Sometimes referred to as "child" chains as they are essentially smaller copies of the Ethereum Mainnet.

- [More on plasma chains in the official documentation here.](https://ethereum.org/en/developers/docs/scaling/plasma)

#### Validium

- A Validium chain uses validity proofs like zero-knowledge rollups but data is not stored on the main layer 1 Ethereum chain.

- This can lead to 10k transactions per second per Validium chain and multiple chains can be run in parallel.

- Off-chain transactions executed on the validium chain are verified via a smart contract on the base Ethereum layer using zero-knowledge proofs, which can be SNARKs or STARKs.

- [More on Validium chains in the official documentation here.](https://ethereum.org/en/developers/docs/scaling/validium/)

</details>

[Sharding 🏗️](https://ethereum.org/en/upgrades/sharding/)

## Oracles

<details>
<summary><a href="https://ethereum.org/en/developers/docs/oracles/">Oracles</a></summary>

## What is an Oracle?

- Oracles are data feeds that allow Ethereum access to off-chain information that exists in the real world. Essentially allowing you to query this data inside smart contracts. Oracles can also be used to "send" data out to the real world.

- An example of an application they would be used in, is in predicition markets. Where the settlement of a payout relies on a real world outcome, such as a the winner of a sports bout.


## Why Are They Needed?

- You might ask, why don't we just make direct calls to APIs on the internet? In the Ethereum blockchain, every node in the network needs to replay every transaction to produce the same result (reach consensus). However depending on the exact time that API is called that data would be variable. Making it impossible to reach consensus.

- Oracles mitiage this issue by utilizing a decentralized system that grabs the data off-chain and then posts that data to the blockchain in an immutable way. As it's being posted directly on the blockchain, all nodes will use this data instead of querying an API themselves.

### The Oracle Problem

- Oracles can seen to be a flawed concept, as relying on a single source of truth to provide data would not be secure. It also invalidates the concept of decentralization. This is known as "the oracle problem".

- This problem can be avoided by using a decentralize oracle that pulls data from many data sources. So that redundancy is ensured

### Security
- As mentioned, an oracle is only as secure as the data sources it's comprised of. That is why its important to use [a feed system](https://developer.makerdao.com/feeds/), such as one utilized by MakerDAO. Which collates data from many external sources.

### Architecture

- A simple example of Oracle architecture would be:
    - 1) Emit a log from a Smart Contract with a request
    - 2) Have an off-chain service subscribed to listen to these events
    - 3) The off-chain service performs the tasks defined by that log
    - 4) The off-chain service reports back to the Smart Contracy by passing in the data as a transaction

 However, as outlined by the oracle problem, this is a centralized way of implementing an oracle.

- [Chainlink OCR](https://blog.chain.link/off-chain-reporting-live-on-mainnet/) Improved upon this by having a network of nodes call different data sources and aggregating the data in a decentralized and verifable nature.

    - The nodes communicate off-chain, cryptograpgically sign their reponses, aggregate the data, and then sends one transaction on-chain that contains the result. Furthermore, it includes incentivization and penalization methods to provide a trusted  system

## Usage

- At of the time this writing, the most commonly accepted way to implement oracles is to use the readily available systems that [Chainlink](https://chain.link/) has developed. Chainlink has already deployed verifiable Oracle systems funded by the public known as [data feeds](https://data.chain.link/). Furthermore, they provide a framework to use their node structure to query customized data. See the services Chainlink offers:

    - [Get crypto price feeds in your contract](https://chain.link/solutions/defi)
    - [Generate verifiable random numbers (useful for gaming)](https://chain.link/solutions/chainlink-vrf)
    - [Call external APIs](https://docs.chain.link/docs/request-and-receive-data)

### Chainlink Data Feeds
- [How to implement Chainlink Data Feeds](https://docs.chain.link/docs/get-the-latest-price/)

### [Chainlink VRF](https://docs.chain.link/docs/chainlink-vrf/)

- Chainlink Verifiable Random Function allows a probably-fair way to verify a source of randomness in smart contracts. Provably-fair random numbers are difficult to produce inherently, as blockchains are deterministic.

- By using this Chainlink service to produce a verifiable random number, you follow the [request and receive cycle](https://docs.chain.link/docs/architecture-request-model). Where the LINK token is used as gas to pay to oracle providers for in turn, having a node produce a verifable random number.

### [Chainlink Keepers](https://docs.chain.link/docs/chainlink-keepers/introduction/)

- Inherently, smart contract can't trigger or initate their own functions at specific times, or depending on specific logic. This can only be done through a manual transaction call to a smart contract.

- Chainlink Keepers is a decentralized network of nodes that will carry out these tasks in a trust minimized and decentralized manner

- The use of this service requires that a smart contract include a [KeeperCompatibleInterface](https://docs.chain.link/docs/chainlink-keepers/compatible-contracts/)
    - checkUpkeep: Checks if the contract requires work to be done
    - performUpkeep: Performs the work on the contract, if instructed by checkUpkeep

### [Chainlink API Call](https://docs.chain.link/docs/make-a-http-get-request)

- This is a service that allows you to make custom API calls of your choosing and have the data reported on-chain in a trust minimized nature. It is to be noted that having a single oracle perform this request would be a centralized implementation

## Various Oracle Service Providers
- [Chainlink](https://witnet.io/)
- [Witnet](https://witnet.io/)
- [Provable](https://provable.xyz/)
- [Paralink](https://paralink.network/)
- [Dos.Network](https://dos.network/)
- [Band](https://bandprotocol.com/)

## Implement Your Own Oracle Solution

- See [this](https://medium.com/@pedrodc/implementing-a-blockchain-oracle-on-ethereum-cedc7e26b49e) comprehensive overview by Pedro Costa

</details>

## Zero Knowledge proof

[ZK Starter pack 🏗️](https://ethresear.ch/t/zero-knowledge-proofs-starter-pack/4519)

[Topic Sampler (ZK proof) 🏗️](https://0xparc.notion.site/Pre-program-Topic-Sampler-46456f891dc541a7a780c79f8ced463c)

[ZK learning group all materials 🏗️](https://0xparc.notion.site/ZK-Learning-Group-1-All-Materials-0e35a14a11894c0895f84a1642c429ad)

#### Zero knowledge explained by Vitlaik Buterin

_To read from 1 to 3_

[1 - Quadratic Arithmetic Programs: from Zero to Hero 🏗️](https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649)

[2 - Exploring Elliptic Curve Pairings 🏗️](https://medium.com/@VitalikButerin/exploring-elliptic-curve-pairings-c73c1864e627)

[3 - Zk-SNARKs: Under the Hood 🏗️](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6)

<br />

![Ethereum hero](https://ethereum.org/static/28214bb68eb5445dcb063a72535bc90c/9019e/hero.webp)
