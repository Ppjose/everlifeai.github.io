# EverLife.AI Token Sale Flow

## Background and Prerequisites

This document details the Token Sale Flow as outlined in the EverLife.AI Whitepaper which is available at https://everlife.ai/.

EverLife.AI has chosen Stellar as it's platform for Token Sale.

This article assumes a basic understanding of [Stellar Accounts](https://www.stellar.org/developers/guides/concepts/accounts.html) and the principles behind [Stellar Smart Contracts](https://www.stellar.org/developers/guides/walkthroughs/stellar-smart-contracts.html). To be able to follow the details and verify the steps an intermediate to expert level of Stellar is required.

## Purpose

Creation of a fairly simple, controlled and secure Token Sale flow which can verified by external parties.

### Bounty Hunt

We believe that openness and cooperation is paramount to security. We encourage anyone to contribute with corrections, improvements and simplifications to the flow. Contributions will be eligible for rewards within the EverLife.AI Bounty Program.

## Overview

This document covers the following phases which make up the EverLife.AI Token Sale Flow:

1. Genesis Phase
1. Distribution Phase
1. Private Sale
1. Pre Sale
1. Public Sale
1. Contribution Flow
1. Post Token Sale
1. Bonus Distribution  

## Notation and Terminology

We use `M` to denote a million, `1 000 000`, units.

Account references are represented by a two or three letter/digit code, i.e. `GA`. Transactions are referenced by a name followed by the `tx` suffix, i.e. `CONCLUSIONtx` or `RFUNDtx`.

EVER is the token/asset being sold during the Token Sale.

Bonuses are awarded at different levels during the different phases of the Token Sale. Investors coming onboard earlier in the process will receive a higher bonus. The bonuses are rewarded in EVER tokens which will be distributed after a successful conclusion of the Token Sale.

## Global parameters

Global parameters are defined prior to the start of the Token Sale.

`D` denotes the Token Sale conclusion date. `time:D-80` means 80 days before Token Sale conclusion.

## Variables

Variables are set based on the outcome in different phases.

`Z` amount of EVER sold during Private Sale.
`XZ` amount of XLM raised during Private Sale.
`Y` amount of EVER sold during Pre Sale.
`XY` amount of XLM raised during Pre Sale.
`X` amount of EVER sold during Public Sale.
`XX` amount of XLM raised during Public Sale.
`V` amount of XLM representing the Soft Cap as defined in USD.

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

### Trustor Accounts

Trustor accounts are controlled by external trusted parties. These accounts are used as signers to safeguard the Token Sale process and ensure fair play. Some crucial steps in the flow require certain trustors or a minimum number of them to sign of on them for the process to be able to proceed.

Account | Description
--------|------------
`TP` | Trusted Party Account. Controlled by a main trustor who will sign of on Token Sale success or failure.
`TA1` ... `TA10` | Trustor Accounts. Ten accounts controlled by leading community members which will safeguard the long term control of the `GA` account and finalize the release of the EVER tokens for trading after Token Sale conclusion.

### Intermediate Accounts

Accounts used during Token Sale to provide accounting transparency and investor security.

Account | Description
--------|------------
`EA` | Escrow Account. EVER tokens will be offered from this and account and this is where funds from the contributors will be deposited during the Token Sale.
`BA` | Burn Account. Any unsold tokens will be transferred from `EA` to the Burn Account upon Token Sale conclusion.

### Contributor Accounts

Accounts related to the individual contributor/investor.

Account | Description
--------|------------
`CA1` | Contributor’s existing Stellar Account, from where contribution in XLM will be made (typically a Stellar Wallet). Always controlled by the contributor.
`CA2` | Contributor’s Token Sale Account, created specifically for Token Sale participation. Used to purchase and hold the EVER tokens until the Token Sale conclusion. If Token Sale is successfully concluded the EVER funds are released and may be merged with `CA1`. If Token Sale fails a the EVER tokens will be exchanged back and the contribution in XLM will be refunded back to this account and control handed back to the contributor. <br><br> _Note:_ The Contributor controls the secret key for this account at all times. The secret key is never transferred to Token Sale servers or known to Token Sale parties.

---

## 1. Genesis Phase <sup>[1](#note_1)</sup>

1. Setup of `GA` and the EVER asset.

2. Genesis 500M in `GA`.

###### Notes

<a name="note_1">1</a>: [Issuing Assets on Stellar](https://www.stellar.org/developers/guides/issuing-assets.html)

---

## 2. Distribution Phase (`time:D-80`)

Setting up and funding the distribution account.

1. Creation of `DA`.

2. `GA` sends 500M EVER to `DA`

3. The `GA` account is configured as follows:
  - Account flag `AUTHORIZATION_REQUIRED = true` - _Note:_ Only accounts with explicit authorization from `GA` will be able to receive EVER funds.
  - Account flag `AUTHORIZATION_REVOCABLE = true` - _Note:_ During the Token Sale the `GA` will be able to revoke the authorization for contributors accounts in order to prevent trading until the Token Sale is successfully concluded.
  - The trustor accounts `TA1`...`TA10` are added as signers of the `GA` account, each with a signing weight of 1.
  - Update master key (`GA`) weight to 11.
  - Update low threshold to 11. - _Note:_ `GA` can still perform low threshold operations alone, trustors can not perform operations without `GA`. `ALLOW_TRUST` is a low threshold operation.
  - Update medium and high thresholds to 15. - _Note:_ `GA` and four of the trustors will be required to sign for medium or high operations. This guards against more EVER being created without trustor approvals.

Creation of accounts `EA`, `TA`, `CR`, `RA` and `BOA`. These accounts are controlled by EverLife.AI. Each of them create a trustline for EVER to `GA` and receive authorisation from `GA`.

Perform EVER distribution from `DA` to the following, as per the EverLife.AI whitepaper:

1. Escrow for Token Sale sale `EA` - 200M (40%)
1. Team reserved `TA` -  75M (15%)
1. Company reserved `CR` - 100M (20%)
1. Rewards and bonuses `RA` - 115M (23%)
1. Bounty `BOA` - 10M (2%)

---

## 3. Private Sale (`time:D-75`)

Direct private investor interactions.

Let `Z` denote the amount of EVER tokens sold, and distributed from `EA` during the Private Sale. Investor XLM funds are sent to `EA`.

_Note:_ Once EVER is sold from `EA` to Private Sale contributors their trustline authorisation will be revoked, to prevent any pre Token Sale trading taking place.

---

## 4. Pre Sale (`time:D-45`)

### Establishing the Pre Sale exchange rate [<a name="note_2">2</a>]

The exchange rate of 1 USD to 10 EVER is used to establish the intermediate rate of XLM to EVER to be used during the Pre Sale. The rate is calculated based on the average exchange rate, on a major exchange, for XLM to USD during the last seven days.

### Setup of Token Sale Escrow Trust Mechanism as a Smart Contract

Now it is time to setup the `EA` account so that trustor approvals will be required to complete or abort the Token Sale according to the rules defined. This we will accomplish with some configuration of the `EA` account and two pre-signed transactions.

1. `EA` adds `TP` as a signer with weight 1.

2. `EA` sets low, medium and high thresholds to 2 - _`EA` and `TP` now both have weight 1 and need to jointly sign all operations to be performed on the `EA` account._

At the start of Pre Sale we will define the soft cap in XLM based in the exchange rate<sup>[2](#note_2)</sup> established.

Let `V` denote the amount of XLM representing the soft cap of USD $2 million.

We will now create two pre-signed and time locked Token Sale success transaction, signed by both `EA` and `TP`. The first transaction can be used to successfully conclude the Token Sale only if the soft cap has been reached. If the soft cap is not reached within the time limits defined the funds in the `EA` account will be made accessible for refunding offers after Token Sale conclusion.

3. Token Sale success transaction `CONCLUSIONtx` is created and stored by trusted party and EverLife.AI:
  - Source account: `EA`
  - Sequence number: `CONCLUSIONtx#` = &lt;next available&gt; + 3 (We need to reserve two sequence numbers for the offers to sell EVER during Pre and Public Sale, plus one more for the post Token Sale burn operation).
  - Time lock: minimum time = `time:D`, maximum time = 0
  - Signers: `EA` and `TP`
  - Operations:
    - Send `V` XLMs to `DA` - _Note:_ This will fail if we have not reached our soft cap, thus causing the rest of the operations in the transaction not to be applied.
    - Remove `TP` as a signer.
    - Reset operation thresholds to defaults.
    - Merge account with `DA`. - _Note:_ On completion the `EA` account will be completely merged with `DA` and `EA` will be marked as removed from the ledger.


4. Token Sale Pre Sale, time locked, refund transaction `PRE_REFUNDtx` is created and stored by both trusted parties and EverLife.AI:
  - Source account: `EA`
  - Sequence number: `PRE_REFUNDtx#` = `CONCLUSIONtx#` + 1 (Note: `CONCLUSIONtx` will cause the sequence number to have been incremented even if the operations fails).
  - Time lock: minimum time = `time:D+7`, maximum time = `time:D+14`
  - Signers: `EA` and `TP`
  - Operations:
    - Create offer to sell XLM and buy EVER at the rate<sup>[2](#note_2)</sup> established for Pre Sale.

The transaction `PRE_REFUNDtx` guarantees investors that they will be offered to sell their EVER back to `EA` for XLM, between `time:D+7` and `time:D+14`, should the Token Sale not be concluded within the specified time frame.

_Note:_ Transaction `PRE_REFUNDtx` will need to be adjusted with the actual amount of XLM being deposited into `EA` during the Pre Sale, `XY`. This requires the cooperation of `EA` and `TP` which both need to sign the updated version, which will reuse the same sequence number.

_Option:_ We could set up an array of transaction, each offering a smaller amount of XLM for EVER, to be submitted one by one until the amount of XLM is exhausted within the size of the refund transactions.

At this stage the `EA` account is guarded during the rest of the Token Sale since `TP` will be required to sign any transactions which can affect it. If the Token Sale is underfunded the transaction will fail and XLM funds will remain in `EA` so that time locked refund transactions may be applied.

### Publishing the Pre Sale Offer

The Pre Sale is capped at 100M EVER so now we publish the offer to sell a maximum of that many tokens, depending on how much was sold during the Private Sale.

1. `EA` publishes an offer `PRE_OFFERtx` to sell a maximum of 100M EVER tokens for XLMs at the Pre Sale exchange rate<sup>[2](#note_2)</sup>.
  - Source account: `EA`
  - Sequence number: `PRE_OFFERtx#` = &lt;next available&gt;
  - Time lock: minimum time = `time:D-45`, maximum time = `time:D-30`
  - Signers: `EA` and `TP`
  - Operations:
      - Create offer to sell `max(100M, 200M - Z)` EVER and buy XLM at the rate<sup>[2](#note_2)</sup> established for Pre Sale.

Contributors that are whitelisted for the Pre Sale and have passed KYC/ALM checks are now invited to buy EVER as described in the Contribution Flow, as outlined below.

Finally, let `Y` denote the number of EVER tokens sold, and distributed from `EA` during the Pre Sale. Let `XY` denote the number of XLM raised during Pre Sale.

---

## 5. Public Sale (`time:D-30`)

### Establishing the Public Sale exchange rate [<a name="note_3">3</a>]

The rate for the Public Sale is established using the same process as used for the Pre Sale<sup>[2](#note_2)</sup>.

### Setting up the refund transaction for the Public sale

Since we are using different exchange rates for Pre and Public sale we need to stage the refunds. The Public Sale refund window will follow the Pre Sale refund windows and start at `time:D+14`, with no maximum time limit specified.

4. Token Sale Public Sale, time locked, refund transaction `PUB_REFUNDtx` is created and stored by both trusted parties and EverLife.AI:
  - Source account: `EA`
  - Sequence number: `PUB_REFUNDtx#` = `PRE_REFUNDtx#` + 1
  - Time lock: minimum time = `time:D+14`, maximum time = 0
  - Signers: `EA` and `TP`
  - Operations:
    - Create offer to sell XLM and buy EVER at the rate<sup>[3](#note_3)</sup> established for Public Sale.

_Note:_ Transaction `PUB_REFUNDtx` will need to be adjusted with the actual amount of XLM being deposited into `EA` during the Public Sale, `XX`. This requires the cooperation of `EA` and `TP` which both need to sign the updated version, which will reuse the same sequence number.

### Publishing the Public Sale Offer

The Pre Sale is capped at 100M EVER so now we publish the offer to sell a maximum of that many tokens, depending on how much was sold during the Private Sale.

1. `EA` publishes an offer `PUB_OFFERtx` to sell the remaining EVER tokens for XLMs at the Public Sale exchange rate<sup>[3](#note_3)</sup>.
  - Source account: `EA`
  - Sequence number: `PUB_OFFERtx#` = `PRE_OFFERtx#` + 1
  - Time lock: minimum time = `time:D-30`, maximum time = `time:D`
  - Signers: `EA` and `TP`
  - Operations:
      - Create offer to sell remaining EVER and buy XLM at the rate<sup>[3](#note_3)</sup> established for Public Sale.

Contributors that are whitelisted for the Public Sale and have passed KYC/ALM checks are now invited to buy EVER as described in the Contribution Flow, as outlined below.

Finally, let `X` denote the number of EVER tokens sold, and distributed from `EA` during the Public Sale. Let `XX` denote the number of XLM raised during Public Sale.

---

## 6. Contribution Flow

The contribution flow is used for both Pre and Public sale phases.

1. Contributor signs up and goes through the KYC/AML process.

2. Upon KYC Approval, an e-mail is sent to the Contributor, containing a link to the contribution dashboard.

3. Contributor uses the dashboard to initiate a new contribution:
  - An offline account `CA2` is created in the Browser. They public and secret keys of `CA2` is displayed to the Contributor in the browser and must be kept safe by the Contributor, it will be required to perform a refund.

_Note:_ The secret key to `CA2` is never handled by or known to EverLife.AI.

4. Contributor uses her/his existing wallet account `CA1` to send the contribution amount + 3 XLMs (for account activation and Stellar transaction fees) to `CA2`.


5. When the contribution dashboard detects that XLM funds have reached `CA2`, an option is presented to buy EVER tokens. Upon selecting the buy option the following transaction, named `CA_FUNDtx` is created and submitted from the browser:
  - Source account: `CA2`
  - Sequence number: `CA_FUNDtx#` = &lt;next available&gt;
  - Signers: `CA2`
  - Operations:
    - Add `EA` as signer for `CA2` with weight 1.
    - Create trustline for the EVER asset against `GA`.
    - Set master weight to 1.
    - Set low, medium and high threshold to 2 - _Note:_ Any operation on CA2 now requires joint signatures from EA and CA2.

The contribution made is now held in escrow. Next three more transactions are created in the browser to conclude the flow.

6. Transaction `CA_BUYtx` represents the buying of EVER tokens, it is created in the browser and signed there by `CA2` before being sent to the EverLife.AI servers:
  - Source account: `CA2`
  - Sequence number: `CA_BUYtx#` = `CA_FUNDtx#` + 1
  - Signers: `CA2` in browser, `EA` on server side
  - Operations:
    - Create offer to buy EVER for XLM according to the contribution amount and the current exchange rate offered (may be different for Pre Sale and Public Sale).

Once received by EverLife.AI servers transaction `CA_BUYtx` is validated, signed by `EA` and submitted to SDEX.

The two concluding transactions are also created in the browser and sent to EverLife.AI server to be signed but they are not submitted at this time. Both are stored as signed XDR envelopes on EverLife.AI servers, and also shared with the Contributor who is encouraged to store them safely as a backup.

Depending on the Token Sale outcome one of these two transactions are submitted, either to claim the EVER asset bought or perform the refund in case of Token Sale failure.

7. Transaction `CA_CLAIMtx` which will be used to return full control of `CA2`, and   the EVER tokens it holds, to the Contributor once the Token Sale is successfully concluded:
  - Source account: `CA2`
  - Sequence number: `CA_CLAIMtx#` = `CA_BUYtx#` + 1
  - Time lock: minimum time = `time:D`
  - Signers: `CA2` in browser, `EA` on server side
  - Operations:
    - Set master weight to 2.
    - Remove `EA` as signer.
    - Reset operation thresholds to defaults.


8. Transaction `CA_REFUNDtx` which will be used to refund the XLM contributed for the EVER bought in case of Token Sale failure:
  - Source account: `CA2`
  - Sequence number: `CA_REFUNDtx#` = `CA_BUYtx#` + 1 (_Note:_ Since the sequence number is equal to `CA_CLAIMtx#` only one of these two transactions may be used.)
  - Signers: `CA2` in browser, `EA` on server side
  - Time lock: _The time lock will be different depending on if the contribution was made during Pre och Public sale since EVER might have been sold at different rates._
    - Pre Sale: minimum time = `time:D+7`, maximum time = `time:D+14`
    - Public Sale: minimum time = `time:D+14`, maximum time = 0
  - Signers: `CA2` in browser, `EA` on server side
  - Operations:
    - Create offer to sell the obtained amount of EVER and for the contributed amount XLM. (_Note:_ At the rate established for the Pre Sale or Public sale respectively.)

---

## 7. Token Sale conclusion (`time:D`)

### Cleanup

First any unsold EVER tokens need to be sent to the burn account `BA` and the trustline from `EA` to `GA` need to be revoked in preparation of conclusion. (_Note:_ This is required for the `CONCLUSIONtx` to be successfully applied since it includes a merge account operation.)

1. We submit the `BURNtx` to perform this cleanup:
  - Source account: `EA`
  - Sequence number: `CONCLUSIONtx#` - 1
  - Signers: `EA` and `TP`
  - Operations:
    - Send any remaining EVER to `BA`
    - Remove trustline for EVER to `GA`

_Note:_ The `BA` account is used for accounting purposes. The burnt amount EVER will then be then be sent back to `GA` where will stay out of circulation and therefore reduce the total supply.

### Conclusion

The Token Sale is concluded by submission of the `CONCLUSIONtx` transaction (created during the setup of the Pre Sale), held by EverLife.AI and a trusted party `TP`. Once submitted it will determine the outcome.

### On Success

The `CONCLUSIONtx` transaction has successfully transfers the amount of XLM as defined for soft cap and then merges `EA` with `DA` which now holds all the contributed funds and is controlled solely by EverLife.AI.

We now need to remove the requirement for authorisation to use the EVER asset, to allow trading. At the same time we want to ensure that no more EVER can be created.

1. The release transaction `RELEASEtx` is submitted to enable free trading of EVER and prevent any further asset creation:
  - Source account: `GA`
  - Sequence number: &lt;next available&gt;
  - Signers: `GA` and any 4 of the trustors `TA1` ... `TA10`.
  - Operations:
    - Set flag: `AUTHORIZATION_REQUIRED` = false
    - Set flag: `AUTHORIZATION_REVOCABLE` = false
    - Remove signers: `TA1` ... `TA10`.
    - Update master weight to 1.
    - Update low threshold to 1.
    - Update medium and high thresholds to 2.

_Note:_ Once submitted and applied `RELEASEtx` locks the `GA` account from further operations (medium or high).

The contributor dashboard will now present the option of claiming the EVER deposited in the respective `CA2` accounts, by submitting the `CA_CLAIMtx` which returns full control of that account to each contributor.

#### Exchange rate Bonus computations

Average rate of USD to XLM up to Token Sale conclusion at `time:D` is used to compute an exchange bonus, according to the EverLife.AI white paper. If the calculated exchange rate is more favourable than the rates used during Pre Sale or Public Sale, then additional bonus is credited to the accounts in EVER. _Note:_ The bonus tokens are distributed at `time:D+30`.

### On Failure

If the `CONCLUSIONtx` transaction failed to transfer the soft cap amount from `EA` to `DA`. The pre signed transactions `PRE_REFUNDtx` and `PUB_REFUNDtx` are now submitted in that order according to their time locks.

This causes the Contributor specific pre signed refund transactions `CA_REFUNDtx` to be enabled since the XML asset is still in `EA`. The Contributors dashboard will now show the option of submitting these refund transactions according to the stages of refunding, first Pre Sale, then Public Sale.

_Note:_ Since the Contributor is encouraged to store her/his own copy of the refund transactions they could be submitted independently of the dashboard as well.

## 8. Bonus Distribution (`time:D+30`)

Bonuses in EVER are now distributed from `RA` to `CA2` accounts according to the rules defined in the EverLife.AI white paper. Bonuses are assigned as compensation for USD to XLM fluctuations during the Token Sale period as well as an extra incentive for early bird contributions.
