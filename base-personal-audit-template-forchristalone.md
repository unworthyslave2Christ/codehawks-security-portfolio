---
title: ThunderLoan Audit Report
author: Emmanuel Orimadegun
date: June 10, 2026
---

<!-- Cover Page Section -->
<div class="cover-page" style="text-align: center; page-break-after: always; padding-top: 5cm; box-sizing: border-box;">
  
  <!-- Relative path to your logo (Centered perfectly with fixed margin syntax) -->
  <!-- <img src="./logo.png" style="width: 50%; margin: 0 auto 2cm auto; display: block;" alt="Logo"> -->

  <h1 style="font-size: 3rem; font-weight: bold; margin-bottom: 1cm;">ThunderLoan Audit Report</h1>
  
  <p style="font-size: 1.5rem; margin-bottom: 2cm;">Version 1.0</p>
  
  <!-- <p style="font-size: 1.5rem; font-style: italic; margin-bottom: 4cm;">The-Cross.io</p> -->
  
  <p style="font-size: 1.2rem;">June 10, 2026</p>


  <p style="font-size: 1.0rem;">Prepared by: Emmanuel Orimadegun</p>

  
</div>

<!-- Your report starts here! -->



<!-- Prepared by: [Emmanuel Orimadegun](https://the-cross.io) -->
<!-- Lead Auditors: Emmanuel  -->

# Table of Contents

- [Table of Contents](#table-of-contents)
- [Protocol Summary](#protocol-summary)
  - [ThunderLoan](#thunderloan)
- [Disclaimer](#disclaimer)
- [Risk Classification](#risk-classification)
- [Audit Details](#audit-details)
  - [Repo](#repo)
  - [Scope](#scope)
  - [Roles](#roles)
  - [Issues found](#issues-found)
- [Findings](#findings)
  - [HIGH](#high)
    - [\[H-1\]  Mixing up variable location causes storage collisions in `ThunderLoan::s_flashLoanFee` and `ThunderLoan::s_currentlyFlashLoaning`](#h-1--mixing-up-variable-location-causes-storage-collisions-in-thunderloans_flashloanfee-and-thunderloans_currentlyflashloaning)
    - [\[H-2\]  Unnecessary `updateExchangeRate` in `deposit` function incorrectly updates `exchangeRate` preventing withdraws and unfairly changing reward distribution](#h-2--unnecessary-updateexchangerate-in-deposit-function-incorrectly-updates-exchangerate-preventing-withdraws-and-unfairly-changing-reward-distribution)
    - [\[H-3\]  By calling a flashloan and then `ThunderLoan::deposit` instead of `ThunderLoan::repay` users can steal all funds from the protocol](#h-3--by-calling-a-flashloan-and-then-thunderloandeposit-instead-of-thunderloanrepay-users-can-steal-all-funds-from-the-protocol)
    - [\[H-4\]  getPricceofOnePoolTokenInWeth uses the TSwap price which doesn't accout for decimals also fee precision is 18 decimals](#h-4--getpricceofonepooltokeninweth-uses-the-tswap-price-which-doesnt-accout-for-decimals-also-fee-precision-is-18-decimals)
  - [MEDIUM](#medium)
    - [\[M-1\]  Centralization risk for trusted owners](#m-1--centralization-risk-for-trusted-owners)
    - [Impact:](#impact)
    - [\[M-2\]  Centralized owners can brick redemptions by disapproving of  specific token](#m-2--centralized-owners-can-brick-redemptions-by-disapproving-of--specific-token)
    - [\[M-3\]  Fee on transfer, rebase etc.](#m-3--fee-on-transfer-rebase-etc)
  - [LOW](#low)
    - [\[L-1\] Empty Function Body - Consider commenting why](#l-1-empty-function-body---consider-commenting-why)
    - [\[L-2\] Initializers could be front-run](#l-2-initializers-could-be-front-run)
    - [\[L-3\] Missing critial event emissions](#l-3-missing-critial-event-emissions)
  - [Informational](#informational)
    - [\[I-1\] Poor Test Coverage](#i-1-poor-test-coverage)
    - [\[I-2\] Not using `__gap[50]` for future storage collision mitigation](#i-2-not-using-__gap50-for-future-storage-collision-mitigation)
    - [\[I-3\] Different decimals may cause confusion. ie: AssetToken has 18, but asset has 6](#i-3-different-decimals-may-cause-confusion-ie-assettoken-has-18-but-asset-has-6)
    - [\[I-4\] Doesn't follow https://eips.ethereum.org/EIPS/eip-3156](#i-4-doesnt-follow-httpseipsethereumorgeipseip-3156)
  - [Gas](#gas)
    - [\[GAS-1\] Using bools for storage incurs overhead](#gas-1-using-bools-for-storage-incurs-overhead)
    - [\[GAS-2\] Using `private` rather than `public` for constants, saves gas](#gas-2-using-private-rather-than-public-for-constants-saves-gas)
    - [\[GAS-3\] Unnecessary SLOAD when logging new exchange rate](#gas-3-unnecessary-sload-when-logging-new-exchange-rate)
  
<!-- Cover Page Section -->
<div style="page-break-after: always; padding-top: 5cm;"/>

# Protocol Summary

## ThunderLoan

The ⚡️ThunderLoan⚡️ protocol is meant to do the following:

Give users a way to create flash loans
Give liquidity providers a way to earn money off their capital
Liquidity providers can deposit assets into ThunderLoan and be given AssetTokens in return. These AssetTokens gain interest over time depending on how often people take out flash loans!

# Disclaimer

I as an independent auditor make all effort to find as many vulnerabilities in the code in the given time period, but holds no responsibilities for the findings provided in this document. A security audit by the me is not an endorsement of the underlying business or product. The audit was time-boxed and the review of the code was solely on the security aspects of the Solidity implementation of the contracts.

# Risk Classification

|            |        | Impact |        |     |
| ---------- | ------ | ------ | ------ | --- |
|            |        | High   | Medium | Low |
|            | High   | H      | H/M    | M   |
| Likelihood | Medium | H/M    | M      | M/L |
|            | Low    | M      | M/L    | L   |

I used the [CodeHawks](https://docs.codehawks.com/hawks-auditors/how-to-evaluate-a-finding-severity) severity matrix to determine severity. See the documentation for more details.

# Audit Details

## Repo

```
git clone https://github.com/Cyfrin/6-thunder-loan-audit
cd 6-thunder-loan-audit
make 
```

## Scope

```
#-- interfaces
|   #-- IFlashLoanReceiver.sol
|   #-- IPoolFactory.sol
|   #-- ITSwapPool.sol
|   #-- IThunderLoan.sol
#-- protocol
|   #-- AssetToken.sol
|   #-- OracleUpgradeable.sol
|   #-- ThunderLoan.sol
#-- upgradedProtocol
    #-- ThunderLoanUpgraded.sol
```

- Solc Version: 0.8.20
- Chain(s) to deploy contract to: Ethereum
- ERC20s:
  - USDC
  - DAI
  - LINK
  - WETH

- Commit Hash: 8803f851f6b37e99eab2e94b4690c8b70e26b3f6



## Roles

Owner: The owner of the protocol who has the power to upgrade the implementation.
Liquidity Provider: A user who deposits assets into the protocol to earn interest.
User: A user who takes out flash loans from the protocol.

<!-- # Known Issues -->

<!-- # Executive Summary

_i spent X hours with Z auditors using Y tools, etc._ --> -->

## Issues found

| Severity | Number of issues founr |
| -------- | ---------------------- |
| High     | 4                      |
| Medium   | 3                      |
| Low      | 3                      |
| Info     | 4                      |
| Gas      | 2                      |
| Total    | 16                     |

<div style="page-break-after: always; break-after:page; padding-top: 2cm;"></div>

# Findings

## HIGH

### [H-1]


## MEDIUM

### [M-1]


## LOW

### [L-1]

## Informational / Non-Critical

### [I-1] 

## Gas (Optional)

// TODO

