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