# **Test Documentation for Google Pay**

## **Overview**
This document outlines the test scenarios, test cases, and expected outcomes for key features of the Google Pay application, focusing on functionality, security, and user experience.

---

## **Test Scenarios and Results**

### **1. User Registration**
#### **Description**: A user signs up for Google Pay by providing valid details.

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | Valid name, email, phone number   | User is successfully registered and redirected to the dashboard.                    | Pass                |
| **Error Path**       | Invalid phone number              | Registration fails, showing an error message: "Invalid phone number."               | Pass                |
| **Abused Path**      | SQL injection in name or email    | Registration is prevented; an error message is displayed: "Invalid input detected." | Pass                |

---

### **2. User Login**
#### **Description**: A registered user logs into their Google Pay account.

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | Valid email and password          | User is successfully logged in and redirected to the main dashboard.                | Pass                |
| **Error Path**       | Incorrect password                | Login fails, displaying an error message: "Incorrect password."                     | Pass                |
| **Abused Path**      | Multiple failed login attempts    | Account is temporarily locked after 5 failed attempts, with an email alert sent.    | Pass                |

---

### **3. Add Payment Method**
#### **Description**: A user adds a new card or bank account to their Google Pay account.

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | Valid card details                | Payment method is successfully added and verified.                                  | Pass                |
| **Error Path**       | Invalid card number               | Addition fails, showing an error: "Invalid card number."                            | Pass                |
| **Abused Path**      | Using stolen card details         | Card addition fails, and an alert is sent to the user.                              | **Fail**: Alert not sent; refer Bug ID: #GPay-001. |

---

### **4. Make Payment**
#### **Description**: A user makes a payment to another user or merchant.

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | Valid recipient and amount        | Payment is processed, and the recipient receives the funds.                         | Pass                |
| **Error Path**       | Insufficient balance              | Payment fails, showing "Insufficient funds."                                       | Pass                |
| **Abused Path**      | Overload with rapid transactions  | Account is flagged for suspicious activity and restricted.                          | Pass                |

---

### **5. Transaction History**
#### **Description**: A user views their payment history in the app.

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | No filters applied                | All transactions are displayed in chronological order.                              | Pass                |
| **Error Path**       | System delay                      | Displays "Unable to retrieve transactions at this time. Please try again."          | Pass                |
| **Abused Path**      | Requesting history for >5 years   | Limits the request to available data within 5 years, preventing overload.           | Pass                |

---

### **6. Notifications**
#### **Description**: Notify users about payments, offers, or security alerts.

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | Successful payment notification   | Notification includes payment confirmation and recipient details.                   | Pass                |
| **Error Path**       | Delivery failure                  | Logs the issue and retries the notification delivery.                               | Pass                |
| **Abused Path**      | Excessive notifications (spam)    | Limits notifications to essential messages and alerts the user.                     | Pass                |

---

### **7. Manage Security**
#### **Description**: User updates security settings (e.g., enabling biometric authentication).

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | Enable fingerprint authentication | Biometric authentication is enabled and tested successfully.                        | Pass                |
| **Error Path**       | Unresponsive fingerprint scanner  | Displays an error message: "Unable to register fingerprint."                        | Pass                |
| **Abused Path**      | Multiple failed biometric attempts | User is required to use an alternative login method (e.g., password).               | Pass                |

---

### **8. Rewards & Cashback**
#### **Description**: A user redeems rewards or receives cashback for eligible transactions.

| **Test Case**       | **Input**                          | **Expected Output**                                                                 | **Result**          |
|----------------------|------------------------------------|-------------------------------------------------------------------------------------|----------------------|
| **Happy Path**       | Eligible payment                  | Rewards or cashback are applied to the user’s account balance.                      | Pass                |
| **Error Path**       | Ineligible transaction            | Cashback is not applied, displaying "Not eligible for rewards."                     | Pass                |
| **Abused Path**      | Attempt to redeem rewards twice   | Prevents duplicate redemption, logging the attempt.                                 | Pass                |

---

## **Summary of Results**
- **Total Test Cases**: 24  
- **Passed**: 23  
- **Failed**: 1  
  - Bug ID: #GPay-001 (Alert not sent for stolen card attempt during payment method addition).

### **Next Steps**
- Address Bug ID: #GPay-001 in the next sprint.
- Perform additional testing to ensure alert system reliability.
- Conduct load testing for scalability of key features.

---
