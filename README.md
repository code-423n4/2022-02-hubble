# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos:
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted.

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest is over and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest setup

## ⭐️ Sponsor: Provide contest details

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Add all of the code to this repo that you want reviewed
- [ ] Create a PR to this repo with the above changes.

---

# Contest prep

## ⭐️ Sponsor: Contest prep
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2021-06-gro/blob/main/README.md))
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 8 hours prior to contest start time.**
- [ ] Ensure that you have access to the _findings_ repo where issues will be submitted.
- [ ] Promote the contest on Twitter (optional: tag in relevant protocols, etc.)
- [ ] Share it with your own communities (blog, Discord, Telegram, email newsletters, etc.)
- [ ] Optional: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] Designate someone (or a team of people) to monitor DMs & questions in the C4 Discord (**#questions** channel) daily (Note: please *don't* discuss issues submitted by wardens in an open channel, as this could give hints to other wardens.)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Hubble contest details
- $71,250 USDC main award pot
- $3,750 USDC gas optimization award pot
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-02-hubble-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts February 17, 2022 00:00 UTC
- Ends February 23, 2022 23:59 UTC

This repo will be made public before the start of the contest. (C4 delete this line when made public)

[ ⭐️ SPONSORS ADD INFO HERE ]

# Hubble Exchange
Perpetual futures exchange on Avalanche.

## Contract Overview
Here is the list of main contracts in the protocol.

| Contract Path                       | Lines of Code |
| ----------------------------------- | ------------- |
| `contracts/ClearingHouse.sol`       |  355          |
| `contracts/MarginAccount.sol`       |  621          |
| `contracts/AMM.sol`                 |  739          |
| `contracts/InsuranceFund.sol`       |  120          |
| `contracts/Oracle.sol`              |  173          |
| `contracts/Registry.sol`            |  25           |
| `contracts/VUSD.sol`                |  76           |
| `contracts/curve-v2/Swap.vy`        |  1170         |
| `contracts/curve-v2/CurveMath.vy`   |  265          |
| `contracts/curve-v2/Views.vy`       |  126          |

## System Overview
There are 4 major entities in the system at a high level

* `ClearingHouse`: This contract is used for opening/closing positions, adding/removing liquidity, liquidation for all AMMs
* `MarginAccount`: Used for accounting purposes, keeps track of the margin of all users, liquidating margin account, settling bad debt.
* `AMM`: This is a virtual AMM that consists of two components. One component is the `Swap.vy` which is inspired from curve-v2 [triCrypto2](https://curve.fi/tricrypto2) pool but modified for two coins and the other is `AMM.sol` which consists of functionalities required for perp exchange.
* `InsuranceFund`: Used to settle bad debts

There are two types of user personas in the system
* Takers: takes long/short position on leverage
* Makers: add/remove liquidity in the pool on leverage

A user can be both a taker as well a maker. For more information about makers, refer to [this](https://medium.com/hubbleexchange/makers-in-hubble-vamm-c2dbae445ed9) blog.


## Dev setup guideline
### One-Time vyper setup
Vyper compilation with hardhat takes a ton of time and is performed on every run (no caching). Therefore, we place .vy files outside the contracts directory and manually compile and dump the abi and bytecode in files that are then picked up in the tests.

```
python3 -m venv venv
source venv/bin/activate
pip install vyper==0.2.12
npm run vyper-compile
```

### Compile
```
npm run vyper-compile && npm run compile
```
