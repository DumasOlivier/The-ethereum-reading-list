# The Ethereum reading list

Reading list of interesting articles and their highlights used to learn Solidity and blockchain concepts.

![Ethereum hero](https://ethereum.org/static/28214bb68eb5445dcb063a72535bc90c/9019e/hero.webp)
_ethereum.org_

### How to contribute ?
 - Add useful articles and their highlights
 - Summarize the articles where you see a 🏗️
 - Create an issue to request an article explaining concepts that you want to learn

## Blockchain addresses

<details>
<summary><a href="https://info.etherscan.com/what-is-an-ethereum-address/">What is an ethereum address ?</a></summary>

* There are two types of addresses in Ethereum: Externally Owned Address (EOA) and Contract Address.

* Externally Owned Address refers to an account with a public and private key pair that holds your funds : a 42-character hexadecimal address derived from the last 20 bytes of the public key controlling the account with 0x appended in front.
 
* Contract address refers to the address hosting a collection of code on the Ethereum blockchain that executes functions.

</details>

<details>
<summary><a href="https://unblock.net/what-is-a-blockchain-address/">What is a blockchain address ?</a></summary>

* When Bitcoin was first created it had the ability to send Bitcoin payments directly to IP addresses. However, Bitcoin’s developers soon realized that this could be vulnerable to man-in-the-middle attacks, so they removed the feature, and it hasn’t been restored to date.

* Once Bitcoin abandoned the Pay to IP idea the developers switched to the Pay To Public Key Hash 
 
* The format of an address (Bank account nuber, SWIFT code, postal address) doesn’t matter at all, what matters is that it serves its purpose of helping to locate a specific location – physical or virtual.  
 
* It makes no difference what algorithms you use to generate the address, how you manipulate the public key, or what the resulting address actually looks like. However, it is important to note that the method used to create an address can have implications on usability, privacy and security.
 
* Since the beginning, users of Bitcoin were perplexed when seeing Ethereum addresses, which are long hexadecimal strings beginning with 0x, like this: 0x0eb81892540747ec60f1389ec734a2c0e5f9f735.
 
* The address creation method used by Ethereum isn’t so different from Bitcoin. It begins with the private key, which uses ECDSA to create a public key, just like Bitcoin. The public key is then hashed using Keccak-256, which gives us a 32-byte string. Ethereum drops the first 12 bytes, and the 20 bytes left over yield a 40 character hexadecimal address, to which is added a 0x prefix

* There’s another critical difference with Ethereum addresses, and that’s a lack of checksum. With Ethereum there’s no protection from typing just one character wrong and having your funds gone forever.
 
* Ethereum developers are somewhat partial to the ICAP format, which is base58 and uses checksums just like Bitcoin and other cryptocurrencies. The really potentially attractive feature of the ICAP format though is that it is compatible with another existing format – the International Bank Account Number (IBAN) system. This means all the existing banking software and systems can understand and interact with these ICAP Ethereum addresses.  

</details>

[Ethereum Accounts 🏗️](https://ethereum.org/en/developers/docs/accounts)

## Ethereum Name Service

## Mining

<details>
<summary><a href="https://ethereum.org/en/developers/docs/consensus-mechanisms/pow/mining-algorithms/">Mining Algorithms (🏗️)</a></summary>
<br>
TODO
</details>

## Security

<details>
<summary><a href="https://ethereum.org/en/developers/docs/smart-contracts/security/">Smart contract Security 🏗️</a></summary>
<br>
 * An audit is no longer sufficient as the only security consideration. Security starts before you write your first line of smart contract code, security starts with proper design and development processes.

 * Follow the development process [describe here](https://ethereum.org/en/developers/docs/smart-contracts/security/#smart-contract-development-process)
 
 * Beware of attacks detailed in the following articles 👇

 * You can use security tools such as Slither, Mythril, Securify ... Double check [the list in the article](https://ethereum.org/en/developers/docs/smart-contracts/security/#smart-contract-security) to find the one that fits your needs.
</details>


[Smart contract Security Best Practices 🏗️](https://consensys.github.io/smart-contract-best-practices/)

[Reetrancy attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/reentrancy/)

[Oracle manipulation attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/oracle-manipulation/)

[Frontrunning attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/frontrunning/)

[Timestamp Dependence attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/timestamp-dependence/)

[Insecure arithmetic attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/insecure-arithmetic/)

[Denial of service attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/denial-of-service/)

[Griefing attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/griefing/)

[Force feeding attack 🏗️](https://consensys.github.io/smart-contract-best-practices/attacks/force-feeding/)

## Scaling

[Scaling 🏗️](https://ethereum.org/en/developers/docs/scaling/)

## Zero Knowledge proof

[ZK Starter pack 🏗️](https://ethresear.ch/t/zero-knowledge-proofs-starter-pack/4519)

[Topic Sampler (ZK proof) 🏗️](https://0xparc.notion.site/Pre-program-Topic-Sampler-46456f891dc541a7a780c79f8ced463c)

[ZK learning group all materials 🏗️](https://0xparc.notion.site/ZK-Learning-Group-1-All-Materials-0e35a14a11894c0895f84a1642c429ad)

#### Zero knowledge explained by Vitlaik Buterin

_To read from 1 to 3_

[1 - Quadratic Arithmetic Programs: from Zero to Hero 🏗️](https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649)

[2 - Exploring Elliptic Curve Pairings 🏗️](https://medium.com/@VitalikButerin/exploring-elliptic-curve-pairings-c73c1864e627)

[3 - Zk-SNARKs: Under the Hood 🏗️](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6)
