# Test Cases for Google Pay

## Table of Contents

1. [Introduction](#1-introduction)
2. [Test Cases for Use Cases](#2-test-cases-for-use-cases)
   - [Send Money (UC-01)](#21-send-money-uc-01)
   - [Pay Bill (UC-02)](#22-pay-bill-uc-02)
   - [Process Payment (UC-03)](#23-process-payment-uc-03)
   - [Manage Merchant Subscription (UC-04)](#24-manage-merchant-subscription-uc-04)
   - [Process Transaction (UC-05)](#25-process-transaction-uc-05)
   - [Refund Transaction (UC-06)](#26-refund-transaction-uc-06)
3. [Non-Functional Test Cases](#3-non-functional-test-cases)
   - [Performance Testing](#31-performance-testing)
   - [Security Testing](#32-security-testing)
   - [Usability Testing](#33-usability-testing)
4. [Conclusion](#4-conclusion)

---

## 1. Introduction

This document outlines the test cases for Google Pay v1.0, covering both functional and non-functional testing to validate the application’s compliance with requirements. These test cases span different use cases, from sending money to processing transactions and merchant subscriptions. They ensure that Google Pay delivers a seamless and secure user experience.

---

## 2. Test Cases for Use Cases

### 2.1 Send Money (UC-01)

**Test Cases:**

| Test Case ID | Description                         | Input Parameters                         | Expected Result                                                                |
| ------------ | ----------------------------------- | ---------------------------------------- | ------------------------------------------------------------------------------ |
| TC-UC01-01   | Successful money transfer           | Valid recipient details, valid amount    | Transaction successfully processed, notification sent confirming transfer     |
| TC-UC01-02   | Invalid recipient details           | Invalid recipient phone number or email | Prompt the user to re-enter valid recipient details with error message shown |
| TC-UC01-03   | Insufficient balance                | Valid recipient, high transfer amount    | Error message “Insufficient funds” displayed; no transfer processed           |
| TC-UC01-04   | Session expired before confirmation | Inactive session due to extended inactivity | User redirected to login screen with a session timeout error message          |
| TC-UC01-05   | Recipient’s bank details invalid    | Valid recipient, invalid bank details    | Error message indicating "Recipient’s bank details are incorrect" displayed  |
| TC-UC01-06   | Successful transfer with wallet payment | Wallet balance, recipient, valid amount   | Transfer confirmed; recipient receives funds instantly via Google Pay wallet |

---

### 2.2 Pay Bill (UC-02)

**Test Cases:**

| Test Case ID | Description                        | Input Parameters                    | Expected Result                                                               |
| ------------ | ---------------------------------- | ------------------------------------ | ----------------------------------------------------------------------------- |
| TC-UC02-01   | Successful bill payment            | Valid utility provider, correct amount | Bill payment successfully processed; confirmation sent to user and provider   |
| TC-UC02-02   | Unsupported utility provider       | Invalid provider ID or unsupported provider | Error message “Unsupported utility provider” displayed                        |
| TC-UC02-03   | Payment failure during transaction | Correct provider and payment amount, but gateway issue | Payment failed notification with error code displayed                          |
| TC-UC02-04   | Session timeout during action      | Inactive session due to timeout      | Session expired message shown; user redirected to login screen                |
| TC-UC02-05   | Duplicate bill payment             | Bill payment duplicate submission    | Warning message shown; no duplicate payment processed                         |

---

### 2.3 Process Payment (UC-03)

**Test Cases:**

| Test Case ID | Description                    | Input Parameters                     | Expected Result                                                        |
| ------------ | ------------------------------ | ------------------------------------ | ---------------------------------------------------------------------- |
| TC-UC03-01   | Successful QR code payment     | Valid QR code, valid amount          | Payment successful notification shown and transaction recorded         |
| TC-UC03-02   | Invalid QR code                | Corrupted or expired QR code         | Error message “Invalid QR code, please try again” displayed            |
| TC-UC03-03   | Payment failure                | Valid QR code, system/server error   | Payment failed message displayed with instructions to retry            |
| TC-UC03-04   | Session expired during payment | Inactive session                     | User redirected to login page with session expired error message       |
| TC-UC03-05   | Invalid amount with QR code    | Valid QR code, invalid payment amount | Error message displayed indicating invalid payment amount              |

---

### 2.4 Manage Merchant Subscription (UC-04)

**Test Cases:**

| Test Case ID | Description                            | Input Parameters                      | Expected Result                                                            |
| ------------ | -------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------- |
| TC-UC04-01   | Approve merchant subscription          | Valid merchant application details    | Merchant successfully approved, assigned Token ID for further processing   |
| TC-UC04-02   | Reject incomplete merchant application | Incomplete or missing application data | Error message requesting additional merchant details                       |
| TC-UC04-03   | Deny merchant subscription             | Invalid merchant details or fraud     | Notification sent to merchant, subscription request denied                 |
| TC-UC04-04   | Merchant subscription status inquiry   | Merchant ID, valid request            | Status response with approval or denial message                           |

---

### 2.5 Process Transaction (UC-05)

**Test Cases:**

| Test Case ID | Description                           | Input Parameters                       | Expected Result                                                               |
| ------------ | ------------------------------------- | -------------------------------------- | ----------------------------------------------------------------------------- |
| TC-UC05-01   | Successful transaction processing     | Valid payment details                  | Transaction processed successfully with confirmation sent to the user        |
| TC-UC05-02   | Invalid credentials during processing | Invalid token or credentials           | “Access denied” error displayed; transaction not processed                    |
| TC-UC05-03   | Transaction gateway failure           | Valid payment details, gateway issue   | Payment failure notification sent to user, retry option available            |
| TC-UC05-04   | Duplicate transaction detected        | Identical transaction details          | Duplicate transaction message shown, no second transaction processed          |

---

### 2.6 Refund Transaction (UC-06)

**Test Cases:**

| Test Case ID | Description                       | Input Parameters                    | Expected Result                                                               |
| ------------ | --------------------------------- | ----------------------------------- | ----------------------------------------------------------------------------- |
| TC-UC06-01   | Successful refund processing      | Valid transaction ID, refund amount  | Refund processed successfully, confirmation sent to user                      |
| TC-UC06-02   | Refund request for invalid ID     | Invalid transaction ID               | “Refund request denied” message shown with error details                      |
| TC-UC06-03   | Partial refund processing         | Valid transaction ID, partial amount | Partial refund successfully processed, confirmation message displayed          |
| TC-UC06-04   | Refund processing during timeout  | Valid transaction ID, timeout        | “Refund pending” message displayed with instructions to wait or retry         |
| TC-UC06-05   | Refund request during fraud check | Pending fraud review on transaction  | Refund request temporarily denied, fraud check in progress                    |

---

## 3. Non-Functional Test Cases

### 3.1 Performance Testing

| Test Case ID | Description                              | Input Parameters                  | Expected Result                                                      |
| ------------ | ---------------------------------------- | --------------------------------- | -------------------------------------------------------------------- |
| TC-NF01-01   | Transaction processing under normal load | 1,000 concurrent users           | Transactions processed within 2 seconds, no significant delays      |
| TC-NF01-02   | Transaction processing under peak load   | 10,000 concurrent users          | Transactions processed within 5 seconds, with no system downtime    |
| TC-NF01-03   | Transaction throughput under stress      | 20,000 concurrent transactions   | Response time of 5-10 seconds, ensuring no server crashes or errors |

---

### 3.2 Security Testing

| Test Case ID | Description                | Input Parameters                  | Expected Result                                         |
| ------------ | -------------------------- | ---------------------------------- | ------------------------------------------------------- |
| TC-NF02-01   | Test data encryption       | Payment details (encrypted data)   | Data encrypted using industry-standard encryption methods |
| TC-NF02-02   | Penetration testing        | Simulated cyber attack (SQL injection, etc.) | No unauthorized access to payment systems or data    |
| TC-NF02-03   | Authentication bypass test | Invalid login attempts             | Account lockout or multi-factor authentication required to prevent unauthorized access |

---

### 3.3 Usability Testing

| Test Case ID | Description               | Input Parameters                   | Expected Result                                                    |
| ------------ | ------------------------- | ----------------------------------- | ------------------------------------------------------------------ |
| TC-NF03-01   | Interface usability       | User navigation through app        | Smooth and intuitive navigation; minimal learning curve            |
| TC-NF03-02   | Multi-language support    | Selected language preference       | Interface displays correct language without bugs or glitches       |
| TC-NF03-03   | User feedback collection  | Interaction with customer survey   | Positive feedback with ease of use and accessibility               |

---

## 4. Conclusion

The above test cases have been designed to validate Google Pay for both functionality and non-functionality. These cases cover a wide range of scenarios from basic operations like sending money and paying bills to edge cases involving error handling, performance under load, and security concerns. Additionally, all usability aspects are considered
