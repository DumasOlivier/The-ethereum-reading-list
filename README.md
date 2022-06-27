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

[ERC777 🏗️](https://docs.openzeppelin.com/contracts/4.x/erc777)

[ERC1155 🏗️](https://docs.openzeppelin.com/contracts/4.x/erc1155)

## Smart contracts

### Memory keyword

[What does the keyword "memory" do exactly? 🏗️](https://ethereum.stackexchange.com/questions/1701/what-does-the-keyword-memory-do-exactly)

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
