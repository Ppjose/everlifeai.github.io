# EverLife.AI Token Sale Flow

## Background and Prerequisites

This document details the Token Sale Flow as outlined in the EverLife.AI Whitepaper which is available at https://everlife.ai/.

EverLife.AI has chosen Stellar as it's platform for Token Sale.

This article assumes a basic understanding of [Stellar Accounts](https://www.stellar.org/developers/guides/concepts/accounts.html) and the principles behind [Stellar Smart Contracts](https://www.stellar.org/developers/guides/walkthroughs/stellar-smart-contracts.html).

### Notes on the current version

The current version of this document is a complete re-write of the original flow since the original design only supported purchases in XLM. As the project and investor relations developed the need for accepting other crypto currencies became more and more apparent.

We have therefore decided to partner with a third party payment service to integrate payment support for a multitude of crypto currencies.

The effect of this change in scope has also meant that the earlier Stellar only flow has been simplified and many of the mechanisms designed for that flow can not be applied in the general case.

## Purpose

Creation of a simple managed way of registering purchases and distributing tokens.

### Bounty Hunt

We believe that openness and cooperation is paramount to security. We encourage anyone to contribute with corrections, improvements and simplifications to the flow. Contributions will be eligible for rewards within the EverLife.AI Bounty Program.

## Overview

This document covers the following phases which make up the EverLife.AI Token Sale Flow:

1. Genesis Phase
1. Distribution Phase
1. Purchases through the Token Sale App
1. Transfer of tokens
1. Bonus Distribution  

## Notation and Terminology

EVER is the token/asset being sold during the Token Sale.

Bonuses are awarded at different levels during the different phases of the Token Sale. Investors coming onboard earlier in the process will receive a higher bonus. The bonuses are rewarded in EVER tokens which will be distributed after a successful conclusion of the Token Sale.

## Accounts

This section describes the Stellar Accounts used in the Token Sale flow.

### Key Accounts

Account | Description
--------|------------
`GA` | Genesis Account. This account creates and issues the EVER token a.k.a. the Issuing Account.
`DA` | Master distribution Account. This account receives all created EVER tokens and distributes them further. Upon a successful conclusion the invested funds in XLM are transferred to this account.
`TA` | Team/Partner/Advisor Account. Keeps tokens reserved for key project contributors.
`CR` | Company Reserve.
`RA` | Rewards Account. Rewards and bonuses are distributed from this account.
`BOA` | Bounty Account. Used for further distribution through initial bounty program.

### Contributor Accounts

Accounts related to the individual contributor/investor.

Account | Description
--------|------------
`CA1` | Contributor’s existing Stellar Account, from where contribution in XLM will be made (typically a Stellar Wallet). Always controlled by the contributor.
`CA2` | Contributor’s Token Sale Account, created specifically for Token Sale participation. Used to hold the EVER tokens until the Token Sale conclusion.<br><br> _Note:_ The Contributor controls the secret key for this account at all times. The effective secret key is never transferred to Token Sale servers or known to Token Sale parties.

---

## 1. Genesis Phase <sup>[1](#note_1)</sup>

1. Setup of `GA` and the EVER asset.

2. Genesis 500M in `GA`.

###### Notes

<a name="note_1">1</a>: [Issuing Assets on Stellar](https://www.stellar.org/developers/guides/issuing-assets.html)

---

## 2. Distribution Phase

Setting up and funding the distribution account.

1. Creation of `DA`.

2. `GA` sends 500M EVER to `DA`

3. The `GA` account is configured as follows:
  - Account flag `AUTHORIZATION_REQUIRED = true` - _Note:_ Only accounts with explicit authorization from `GA` will be able to receive EVER funds.
  - Account flag `AUTHORIZATION_REVOCABLE = true` - _Note:_ During the Token Sale the `GA` will be able to revoke the authorization for contributors accounts in order to prevent trading until the Token Sale is successfully concluded.

Creation of accounts `EA`, `TA`, `CR`, `RA` and `BOA`. These accounts are controlled by EverLife.AI. Each of them create a trustline for EVER to `GA` and receive authorisation from `GA`.

Perform EVER distribution from `DA` to the following, as per the EverLife.AI whitepaper:

1. Escrow for Token Sale sale `EA` - 200M (40%)
1. Team reserved `TA` -  75M (15%)
1. Company reserved `CR` - 100M (20%)
1. Rewards and bonuses `RA` - 115M (23%)
1. Bounty `BOA` - 10M (2%)

---

## 3. Purchases through the Token Sale App

The purchase of EVER is performed through the following steps:

1. Sign up in our token sale app at https://app.everlife.ai.
2. Submit KYC details. These details will be checked in a two step process, first by a third party service and then by the EverLife Investor Services. Once KYC is passed purchase options are available in the app.
3. Select the amount of EVER to purchase and select desired currency for payment. We accept both XLM (Stellar) and other crypto currencies (BTC, ETH).
4. Provide a Stellar destination account (`CA2`) to which the EVER can be transferred upon successful payment. *This account needs to be a valid Stellar account (funded with at least a minimum balance) and with an existing trust line to the EVER asset.*
5. If Stellar XLM is used for payment, the sender Stellar account (`CA1`) needs to be specified for us to be able to match the payment. Otherwise no extra information needs to be provided.
6. Payment instructions will be provided. A Stellar address will be shown if XLM has been selected and a payment link will be shown if another currency has been selected.
7. Status of payments and the amount of EVER purchased is shown on the token sale app dashboard. Incoming payments are periodically scanned by our backend services and the dashboard is updated at regular intervals.

---

## 5. Transfer of tokens

Once payment is verified and registered on the dashboard another backend service will handle the payment of EVER to the destination account. Payments are batched at regular intervals.

---

## 6. Bonus Distribution

EVER bonuses are distributed after completion of the token sale, according to the rules defined in the EverLife.AI white paper. Bonuses are assigned as compensation for USD to XLM/ETH fluctuations during the Token Sale period as well as an extra incentive for early bird contributions.
