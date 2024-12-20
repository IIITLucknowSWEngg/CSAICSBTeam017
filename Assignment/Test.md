# Test Cases for Google Pay v1.0

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

---

## 1. Introduction

This document details the test cases for Google Pay v1.0, covering functional and non-functional aspects. Each use case is tested for standard and alternative flows to ensure compliance with requirements.

## 2. Test Cases for Use Cases

### 2.1 Send Money (UC-01)

**Test Cases:**

| Test Case ID | Description                         | Input Parameters             | Expected Result                            |
| ------------ | ----------------------------------- | ---------------------------- | ------------------------------------------ |
| TC-UC01-01   | Successful money transfer           | Valid recipient, amount      | Transaction successful notification sent   |
| TC-UC01-02   | Invalid recipient details           | Invalid recipient            | Prompt user to re-enter recipient details  |
| TC-UC01-03   | Insufficient balance                | Valid recipient, high amount | Insufficient funds error message displayed |
| TC-UC01-04   | Session expired before confirmation | Inactive session             | Redirect to login screen                   |

---

### 2.2 Pay Bill (UC-02)

**Test Cases:**

| Test Case ID | Description                        | Input Parameters               | Expected Result                           |
| ------------ | ---------------------------------- | ------------------------------ | ----------------------------------------- |
| TC-UC02-01   | Successful bill payment            | Valid utility provider, amount | Bill payment successful notification sent |
| TC-UC02-02   | Unsupported utility provider       | Invalid provider ID            | Unsupported provider error displayed      |
| TC-UC02-03   | Payment failure during transaction | Valid inputs, gateway issue    | Payment failed notification sent          |
| TC-UC02-04   | Session timeout during action      | Inactive session               | Redirect to login screen                  |

---

### 2.3 Process Payment (UC-03)

**Test Cases:**

| Test Case ID | Description                    | Input Parameters            | Expected Result                   |
| ------------ | ------------------------------ | --------------------------- | --------------------------------- |
| TC-UC03-01   | Successful QR code payment     | Valid QR code, amount       | Payment success notification sent |
| TC-UC03-02   | Invalid QR code                | Corrupted QR code           | Invalid QR code error displayed   |
| TC-UC03-03   | Payment failure                | Valid QR code, system error | Payment failed notification sent  |
| TC-UC03-04   | Session expired during payment | Inactive session            | Redirect to login screen          |

---

### 2.4 Manage Merchant Subscription (UC-04)

**Test Cases:**

| Test Case ID | Description                            | Input Parameters           | Expected Result                              |
| ------------ | -------------------------------------- | -------------------------- | -------------------------------------------- |
| TC-UC04-01   | Approve merchant subscription          | Valid merchant application | Merchant approved, Token ID assigned         |
| TC-UC04-02   | Reject incomplete merchant application | Incomplete application     | Request additional details notification sent |
| TC-UC04-03   | Deny merchant subscription             | Invalid merchant details   | Merchant denial notification sent            |

---

### 2.5 Process Transaction (UC-05)

**Test Cases:**

| Test Case ID | Description                           | Input Parameters             | Expected Result                          |
| ------------ | ------------------------------------- | ---------------------------- | ---------------------------------------- |
| TC-UC05-01   | Successful transaction processing     | Valid payment details        | Transaction successful notification sent |
| TC-UC05-02   | Invalid credentials during processing | Invalid token/credentials    | Access denied error displayed            |
| TC-UC05-03   | Transaction gateway failure           | Valid details, gateway issue | Payment failed notification sent         |

---

### 2.6 Refund Transaction (UC-06)

**Test Cases:**

| Test Case ID | Description                       | Input Parameters               | Expected Result                          |
| ------------ | --------------------------------- | ------------------------------ | ---------------------------------------- |
| TC-UC06-01   | Successful refund processing      | Valid transaction ID           | Refund successful notification sent      |
| TC-UC06-02   | Refund request for invalid ID     | Invalid transaction ID         | Refund denied notification sent          |
| TC-UC06-03   | Partial refund processing         | Valid transaction ID, amount   | Partial refund successful notification   |
| TC-UC06-04   | Refund processing during timeout  | Valid transaction ID, timeout  | Refund pending notification sent         |

## 3. Non-Functional Test Cases

### 3.1 Performance Testing

| Test Case ID | Description                              | Input Parameters        | Expected Result                         |
| ------------ | ---------------------------------------- | ----------------------- | --------------------------------------- |
| TC-NF01-01   | Transaction processing under normal load | Concurrent 1,000 users  | Transactions processed within 2 seconds |
| TC-NF01-02   | Transaction processing under peak load   | Concurrent 10,000 users | Transactions processed within 5 seconds |

---

### 3.2 Security Testing

| Test Case ID | Description          | Input Parameters         | Expected Result                 |
| ------------ | -------------------- | ------------------------ | ------------------------------- |
| TC-NF02-01   | Test data encryption | Payment data             | Data encrypted end-to-end       |
| TC-NF02-02   | Penetration testing  | Simulated attack vectors | No unauthorized access achieved |

---

### 3.3 Usability Testing

| Test Case ID | Description            | Input Parameters             | Expected Result                     |
| ------------ | ---------------------- | ---------------------------- | ----------------------------------- |
| TC-NF03-01   | Interface usability    | User navigation              | Easy and intuitive navigation       |
| TC-NF03-02   | Multi-language support | Selected language preference | Interface displays correct language |

---

