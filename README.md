# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos: 
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted (shared with you after the contest) 

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---
# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [x] Create a PR to this repo with the below changes:
- [x] Provide a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [x] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [x] Please have final versions of contracts and documentation added/updated in this repo **no less than 24 hours prior to contest start time.**
- [x] Be prepared for a 🚨code freeze🚨 for the duration of the contest — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the contest. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)


---

## ⭐️ Sponsor: Edit this README

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [x] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2022-08-foundation#readme))
  - [x] When linking, please provide all links as full absolute links versus relative links
  - [x] All information should be provided in markdown format (HTML does not render on Code4rena.com)
- [ ] Under the "Scope" heading, provide the name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Optional / nice to have: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] See also: [this checklist in Notion](https://code4rena.notion.site/Key-info-for-Code4rena-sponsors-f60764c4c4574bbf8e7a6dbd72cc49b4#0cafa01e6201462e9f78677a39e09746)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Frankencoin contest details
- Total Prize Pool: $60,500 USDC 
  - HM awards: $42,500 USDC 
  - QA report awards: $5,000 USDC 
  - Gas report awards: $2,500 USDC 
  - Judge awards: $6,000 USDC 
  - Lookout awards: $4,000 USDC 
  - Scout awards: $500 USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-04-frankencoin/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts April 12, 2023 20:00 UTC 
- Ends April 17, 2023 20:00 UTC 

## Hello Wardens 👋

Yes! You're here! Awesome. We are super excited for your journey on unpacking and dissecting our code. We will do our best to be available around the clock for all your questions, small, big, silly or severe.

You'll find the most important links & communication channels at the bottom 👇 

We wish you luck on your audit.

Happy hunting 🕵

_Frankencoin team_

## Automated Findings / Publicly Known Issues 

Automated findings output for the contest can be found [here](add link to report) within an hour of contest opening.

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

[ ⭐️ SPONSORS ADD INFO HERE ]

# Introduction to Frankencoin

## Motivation

_Democratization of Money Creation_: Today, money is created by central banks and multiplied by commercial banks, thereby indirectly determine the resource allocation in the economy. Like every system that relies on central planning, this
unpreventable leads a misallovation of resources, typically an overinvestment in government bonds and real estate, and underinvestment in innovation and growth.

Our vision is that anyone with eligible collateral can create their own money. We hope a decentralization the process of 
money creation and to unlock dormant growth and prosperity.

## What is Frankencoin?

The Frankencoin (ZCHF), an oracle-free, collateralized Swiss Franc stablecoin that is intended to track the value of the Swiss Franc. Our mission is to create a stablecoin that addresses the current issues faced by existing stablecoins. These issues include trusting centralized third parties, dependence on centralized oracles, and limitations in the types of collateral available to mint the stablecoin.

It's governance is decentralized, with anyone being able to propose new minting mechanisms and anyone who contributed more than 3% to the stability reserve being able to veto them. So far, the Frankencoin has two approved minting mechanism. Both are accessible through the [frontend](https://frankencoin.com). One is a simple swap contract to convert XCHF (CryptoFranc, fiat-collateralized stablecoin) into ZCHF and back. The other is a novel collateralized minting mechanism based on auctions. 

Unlike the minting mechanisms of other collateralized stablecoins, Frankencoin's auction-based mechanism does not depend on external oracles. It is very flexible with regards to the used collateral. In principle, it supports any collateral with sufficient availability on the market. However, its liquidation mechanism is slower than that of other collaterlized stablecoins, making it less suitable for highly volatile types of collateral. The name is inspired by the system's self-governing nature.

# Overview

The focus of this audit are the smart contracts that comprise key parts of the Frankecoin app, namely the minting of Frakencoin tokens (ZCHF), the purchase of reserve pool shares, and the stablecoin conversion between XCHF and ZCHF. It is vital that there are no possibilities to mint infinite Frankencoins and that no one's deposited collateral can be stolen. There is a [Gitbook documentation page](https://docs.frankencoin.com/) with much more additional information.

# Scope / Contracts

The following are the relevant source files. The list excludes interfaces and test files.

| Contract | SLOC | Purpose
| ----------- | ----------- | ----------- |
| [contracts/Frankencoin.sol](contracts/Position.sol) | 137 | The Frankencoin ERC20 token, supporting external minters, keeping track of the minter reserve, and depending on the equity contract for governance. |
| [contracts/Equity.sol](contracts/Equity.sol) | 142 | The Frankencoin Pool Shares ERC20 token with time-weighted vetoing and a built-in market maker for issuance and redemption. |
| [contracts/MintingHub.sol](contracts/MintingHub.sol) | 190 | A minter that can create and challenge positions. |
| [contracts/Position.sol](contracts/Position.sol) | 227 | Holds the collateral of an individual user for minting fresh Frankencoins. |
| [contracts/StablecoinBridge.sol](contracts/StablecoinBridge.sol) | 48 | A contract to swap other stablecoins with the same base currency 1:1. |
| [contracts/ERC20.sol](contracts/ERC20.sol) | 78 | An ERC20 token with support for inifite allowances and ERC677 notifications. |
| [contracts/ERC20PermitLight.sol](contracts/ERC20PermitLight.sol) | 51 | A lightweight ERC20 permit contract. |
| [contracts/MathUtil.sol](contracts/MathUtil.sol) | 26 | Mathematical utility functions. |
| [contracts/Ownable.sol](contracts/Ownable.sol) | 21 | The usual Ownable contract. |
| [contracts/PositionFactory.sol](contracts/PositionFactory.sol) | 29 | A factory for creating new or cloning existing positions. |
| Total | 949 |  |

Lines of code have been counted with the command

```shell
.\cloc-1.96.exe contracts --exclude-dir=test --by-file --not-match-f='II*'
```

# Points of Interest

To function properly, the Frankencoin depends as much on economic incentives as it does on the security of the smart contracts themselves.

The following are the three cornerstones for making sure the system is robust.

* It should not be possible to mint Frankencoins in a way that the FPS holders disapprove.
* The FPS issuance and redemption mechanism should be path-independent, an attacker should not be possible to drain capital from the equity contract through repeated interaction with the issuance and redemption mechanism.
* Collateralized minting should be safe. I.e. it is impossible for an attacker to steal someone's collateral, it is impossible for a position owner to mint beyond the limit of a position, and it is impossible for a position owner to withdraw collateral without having repaid the corresponding amount of previously minted Frankencoins.

## Out of scope

All the files in the [contracts/test](contracts/test) folder are only used for testing and out of scope for this audit.

The findings of the previous audit that we decided not to fix should not be reported again.

# Additional Context

There are two mechanisms of particular interest, the issuance and redemption of pool shares, as well as the collateral auction mechanism.

## Auction Mechanism

The novel idea behind the auction mechanism is to two piles of collateral for sale, one from the owner of the position
and one from the challenger that questions the soundness of the position. The final price determines which of the two 
the bidder gets. If the price is high enough, the bidder gets the collateral of the challenger. If the price is too low,
the bidder gets the collateral from the position owner. This ensures that the position owner has no incentive to overbid
in order to avert a challenge. Doing so would be costly for them as they would have to buy collateral above the market
price from the challenger as often as the challenger repeats it. The only pre-condition for this to work is that the
collateral asset is available on the market.

## Pool Shares

Anyone can acquire new pool shares at any time through an AMM-inspired mechanism. If the Frankencoin system was a
bank, the FPS tokens would be its shares. They automatically become more valuable as the system earns fees and liquidation
proceeds, but decline in value in case of losses. The bonding curve for issuance and redemption is set such that the
market cap of all pool shares is always three times the equity reserves of the system, with the supply being proportional
to the cubic root of the market cap and the price being proportional to the cubic root squared. The mathematical 
implications of having such a bonding curve are beyond the scope of this audit, but it should nonetheless be made sure
that no one can manipulate this system to their advantage.

## Scoping Details 
```
- Public code repo: [github.com/FrankencoinZCHF](github.com/FrankencoinZCHF)
- How many contracts are in scope?: 15
- Total SLoC for these contracts?: 949
- How many external imports are there?: 0  
- How many separate interfaces and struct definitions are there for the contracts within scope?: 6
- Does most of your code generally use composition or inheritance?: Composition
- How many external calls?: 0
- What is the overall line coverage percentage provided by your tests?: 98%
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:  true 
- Please describe required context: There is a [research paper](https://www.snb.ch/n/mmr/reference/sem_2022_06_03_maire/source/sem_2022_06_03_maire.n.pdf) describing the game theory behind the auction mechanism.
- Does it use an oracle?: No
- Does the token conform to the ERC20 standard?: Yes, both tokens.
- Are there any novel or unique curve logic or mathematical models?: There are two novel logics in the system. The first one is an auction mechanism to liquidate collateral. The second one is a built-in bonding curve when creating or redeeming equity tokens.
- Does it use a timelock function?: Yes, for example FPS tokens can only be redeemed after 90 days.
- Is it an NFT?: No
- Does it have an AMM?: Kind of. It offers an AMM-like possibility to mint and redeem Frankencoin Pool Shares (FPS).
- Is it a fork of a popular project?: No  
- Does it use rollups?: No
- Is it multi-chain?: No
- Does it use a side-chain?: No
- Describe any specific areas you would like addressed. E.g. Please try to break XYZ.": It is vital that there are no possibilities to mint infinite Frankencoins and that no one's deposited collateral can be stolen. The bidding process has been carefully designed with game theory in mind such that it does not pay off to manipulate the auction.
- Do you have a preferred timezone for communication?: CET
```

# Tests

Assuming that you have [node](https://nodejs.org/) already installed, you can install [Hardhat](https://hardhat.org/hardhat-runner/docs/getting-started#overview) and all the other dependencies with the following two commands:

```shell
npm install --save-dev hardhat
npm install
```

Next you can compile the code, run the test suite or calculate test coverage with the following commands. The 'test' command includes a gas usage report.

```shell
npx hardhat compile
npx hardhat test
npx hardhat coverage
```

Note that the version of the Frankencoin currently deployed on mainnet and connected to the frontend is slightly outdated.

# Links
[Frankencoin App](https://frankencoin.com/)

[Documentation](https://docs.frankencoin.com/)

[Blog](https://frankencoin.substack.com/)

[Forum](https://github.com/Frankencoin-ZCHF/FrankenCoin/discussions)

[Telegram](https://t.me/frankencoinzchf)