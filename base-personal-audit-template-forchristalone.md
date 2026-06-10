---
title: TSwap Audit Report
author: Emmanuel Orimadegun
date: June 10, 2026
---

<!-- Cover Page Section -->
<div class="cover-page" style="text-align: center; page-break-after: always; padding-top: 5cm; box-sizing: border-box;">
  
  <!-- Relative path to your logo (Centered perfectly with fixed margin syntax) -->
  <img src="./logo.png" style="width: 50%; margin: 0 auto 2cm auto; display: block;" alt="Logo">

  <h1 style="font-size: 3rem; font-weight: bold; margin-bottom: 1cm;">TSwap Audit Report</h1>
  
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
  - [Puppy Raffle](#puppy-raffle)
- [Disclaimer](#disclaimer)
- [Risk Classification](#risk-classification)
- [Audit Details](#audit-details)
  - [Repo](#repo)
  - [Scope](#scope)
  - [Issues found](#issues-found)
- [Findings](#findings)
  - [HIGH](#high)
    - [\[H-1\]](#h-1)
  - [MEDIUM](#medium)
    - [\[M-1\]](#m-1)
  - [LOW](#low)
    - [\[L-1\]](#l-1)
  - [Informational / Non-Critical](#informational--non-critical)
    - [\[I-1\]](#i-1)
  - [Gas (Optional)](#gas-optional)

# Protocol Summary

## Puppy Raffle

This project is to enter a raffle to win a cute dog NFT. The protocol should do the following:

1. Call the `enterRaffle` function with the following parameters:
   1. `address[] participants`: A list of addresses that enter. You can use this to enter yourself multiple times, or yourself and a group of your friends.
2. Duplicate addresses are not allowed
3. Users are allowed to get a refund of their ticket & `value` if they call the `refund` function
4. Every X seconds, the raffle will be able to draw a winner and be minted a random puppy
5. The owner of the protocol will set a feeAddress to take a cut of the `value`, and the rest of the funds will be sent to the winner of the puppy.

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
git clone https://github.com/Cyfrin/4-puppy-raffle-audit
cd 4-puppy-raffle-audit
make
```

## Scope

```
./src/
└── PuppyRaffle.sol
```

- Commit Hash: 2a47715b30cf11ca82db148704e67652ad679cd8

<!-- ## Scope

```
./src/
└── PasswordStore.sol
```

## Roles

- Owner: The user who can set the password and read the password.
- Outsides: No one else should be able to set or read the password.

# Executive Summary

_i spent X hours with Z auditors using Y tools, etc._ -->

## Issues found

| Severity | Number of issues founr |
| -------- | ---------------------- |
| High     | 5                      |
| Medium   | 4                      |
| Low      | 0                      |
| Info     | 8                      |
| Gas      | 1                      |
| Total    | 18                     |

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

