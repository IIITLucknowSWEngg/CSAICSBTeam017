# Google Pay System Diagrams

---

# 1. System Context Diagram - Google Pay

```
@startuml
title System Context Diagram for Google Pay

actor User as U
actor Merchant as M
actor Bank as B
actor PaymentNetwork as PN

package "Google Pay System" {
    [Google Pay Mobile App] as GPApp
    [Google Pay Backend Services] as GPBackend
}

U --> GPApp : Makes Payment Request
M <-- GPApp : Payment Confirmation
GPApp --> GPBackend : Sends Payment Details
GPBackend --> PN : Routes Payment
PN --> B : Processes Transaction
B --> PN : Sends Transaction Status
PN --> GPBackend : Updates Payment Status
GPBackend --> GPApp : Notifies Payment Status
GPApp --> U : Displays Payment Status

@enduml


```

![System Context](./Diagrams/context.png)



### Description  
- **Users**:  
  - **Retail investors**: Interact with the Google Pay app to make payments.  
  - **Merchants**: Receive payments through the Google Pay app.  
- **External Systems**:  
  - **Payment Networks**: Handle routing and processing of payments to banks.  
  - **Banks**: Process the financial transactions.  
- **Components**:  
  - **Google Pay Mobile App**: Allows users to initiate payments and receive payment statuses.  
  - **Google Pay Backend Services**: Handles routing payment requests and updating the status.  

---

# 2. Container Diagram 

```plantuml

@startuml
actor "User" as User
actor "Merchant" as Merchant
actor "Admin" as Admin

package "Frontend Layer" {
    [GPay App (Web/Mobile)] as Frontend
}

package "Backend Layer" {
    [Backend API] as BackendAPI
    [User Management Service] as UserService
    [Payment Processing Service] as PaymentService
    [Notification Service] as NotificationService
    [Analytics Service] as AnalyticsService
}

package "External Services Layer" {
    [Banking API] as BankingAPI
    [Merchant Gateway] as MerchantGateway
    [Notification Provider] as NotificationProvider
}

package "Data Layer" {
    [User Database] as UserDB
    [Transaction Database] as TransactionDB
    [Merchant Database] as MerchantDB
}

User --> Frontend : Login, Add Cards, Make Payments
Merchant --> Frontend : Receive Payments
Admin --> BackendAPI : Manage Users, Transactions

Frontend --> BackendAPI : API Requests
BackendAPI --> UserService : User Authentication
BackendAPI --> PaymentService : Payment Processing
BackendAPI --> NotificationService : Notifications
BackendAPI --> AnalyticsService : Usage Analytics

PaymentService --> BankingAPI : Process Transactions
PaymentService --> MerchantGateway : Route Merchant Payments

NotificationService --> NotificationProvider : Push Notifications

UserService --> UserDB : Save/Update User Info
PaymentService --> TransactionDB : Save Transactions
MerchantGateway --> MerchantDB : Log Merchant Data
@enduml

```

![Container Diagram](./Diagrams/Container.png)


### Description  
- **Users**:  
  - **Retail investors**: Use the mobile app to log in, add payment methods, and make payments.  
  - **Merchants**: Receive payments through the app interface.  
  - **Admins**: Manage users and transaction data through the backend API.  
- **External Systems**:  
  - **Banking API**: Handles transaction processing with banks.  
  - **Merchant Gateway**: Routes merchant payments for completion.  
  - **Notification Provider**: Sends notifications about transaction statuses.  
- **Components**:  
  - **Frontend Layer**: The **Google Pay App** (mobile/web) allows user interaction.  
  - **Backend Layer**: Includes various services like **User Management**, **Payment Processing**, **Notification Service**, and **Analytics Service** that interact with databases and external APIs.  
  - **Data Layer**: Stores user, transaction, and merchant data.  

---



# 3. Component Diagram - Google Pay Payment Processing

```plantuml
@startuml

@startuml
title Class-based Component-Level Design for G-Pay

' Main Class - GPay
class GPay {
  - userAccounts
  - transactionHistory
  - notifications
  --
  + initializeApp()
  + authenticateUser()
  + manageWallet()
  + initiateTransaction()
}

' Interfaces
interface PaymentInterface {
  + initiatePayment(amount, method)
  + verifyPayment(transactionID)
  + refundPayment(transactionID)
}

interface WalletInterface {
  + addMoney(amount)
  + transferFunds(toAccount, amount)
  + getBalance()
}

interface NotificationInterface {
  + sendNotification(userID, message)
  + scheduleReminder(userID, dateTime)
  + fetchNotifications(userID)
}

' Elaborated Design Classes
class Payment {
  - amount
  - method
  - status
  --
  + processPayment()
  + verifyTransaction()
  + issueRefund()
}

class Wallet {
  - balance
  - linkedAccounts
  --
  + deposit()
  + withdraw()
  + checkBalance()
}

class Notification {
  - notificationID
  - userID
  - type
  --
  + send()
  + fetchAll()
  + deleteNotification()
}

' Relationships
GPay --> Payment : manages
GPay --> Wallet : interacts with
GPay --> Notification : notifies

Payment ..> PaymentInterface : implements
Wallet ..> WalletInterface : implements
Notification ..> NotificationInterface : implements
@enduml

```

![Component Diagram](./Diagrams/component2.png)

```plantuml

@startuml
title G-Pay Traditional Component-Level Design

' Main Component
component "G-Pay System" as GPay

' Subcomponents
component "User Authentication" as Auth
component "Transaction Management" as Transaction
component "Wallet Management" as Wallet
component "Notification System" as Notification

' Lower-Level Components for Transaction Management
component "Initiate Payment" as InitPayment
component "Verify Payment" as VerifyPayment
component "Refund Payment" as RefundPayment

' Lower-Level Components for Wallet Management
component "Add Money" as AddMoney
component "Transfer Funds" as TransferFunds
component "Check Balance" as CheckBalance

' Lower-Level Components for Notification System
component "Send Notification" as SendNotification
component "Schedule Reminder" as ScheduleReminder
component "Fetch Notifications" as FetchNotifications

' Connections
GPay --> Auth
GPay --> Transaction
GPay --> Wallet
GPay --> Notification

Transaction --> InitPayment
Transaction --> VerifyPayment
Transaction --> RefundPayment

Wallet --> AddMoney
Wallet --> TransferFunds
Wallet --> CheckBalance

Notification --> SendNotification
Notification --> ScheduleReminder
Notification --> FetchNotifications
@enduml

```

![Component Diagram](./Diagrams/component1.png)

### Description  
- **Users**:  
  - **Retail investors**: Use the app to manage payments, fund transfers, and view notifications.  
- **External Systems**:  
  - **None explicitly**: The diagram focuses on internal components.  
- **Components**:  
  - **GPay**: The main class that interacts with users, manages accounts, and processes transactions.  
  - **Payment**, **Wallet**, and **Notification** classes handle specific operations like initiating payments, managing balances, and sending notifications.  
  - **Interfaces**: Abstract operations for payments, wallet management, and notifications, allowing easy integration and operation with the underlying system.

---




# 3. Deployment Diagram - Google Pay

```plantuml
@startuml
title Deployment Diagram - Google Pay System

' Nodes
node "User's Mobile Device" {
    [Google Pay Mobile App] <<Mobile App>>
}

node "User's PC" {
    [Google Pay Web App] <<Web App>>
}

node "Admin's PC" {
    [Admin Web Dashboard] <<Web App>>
}

node "Application Server" {
    [Authentication Service] <<Service>>
    [Transaction Service] <<Service>>
    [Payment Service] <<Service>>
    [Fraud Detection Service] <<Service>>
    [Notification Service] <<Service>>
    [User Management Service] <<Service>>
}

node "Database Server" {
    database "User Database" as UserDB
    database "Transaction Database" as TxnDB
    database "Payment Database" as PaymentDB
}

cloud "Third-Party Services" {
    [Payment Gateway] <<External Service>>
    [Card Network (Visa/Mastercard)] <<External Service>>
    [Bank API] <<External Service>>
}

' Connections
[Google Pay Mobile App] --> [Authentication Service] : "Login/Signup"
[Google Pay Mobile App] --> [Transaction Service] : "Initiate Transactions"
[Google Pay Mobile App] --> [Payment Service] : "Process Payments"
[Google Pay Mobile App] --> [Fraud Detection Service] : "Monitor Transactions"
[Google Pay Mobile App] --> [Notification Service] : "Receive Notifications"
[Google Pay Mobile App] --> [User Management Service] : "Manage Account"

[Google Pay Web App] --> [Authentication Service] : "Login/Signup"
[Google Pay Web App] --> [Transaction Service] : "Initiate Transactions"
[Google Pay Web App] --> [Payment Service] : "Process Payments"
[Google Pay Web App] --> [Fraud Detection Service] : "Monitor Transactions"
[Google Pay Web App] --> [Notification Service] : "Receive Notifications"
[Google Pay Web App] --> [User Management Service] : "Manage Account"

[Admin Web Dashboard] --> [User Management Service] : "Manage Users"
[Admin Web Dashboard] --> [Transaction Service] : "Monitor Transactions"
[Admin Web Dashboard] --> [Payment Service] : "Monitor Payments"
[Admin Web Dashboard] --> [Fraud Detection Service] : "View Fraud Alerts"

[Transaction Service] --> TxnDB : "Store Transaction Records"
[User Management Service] --> UserDB : "Store User Data"
[Payment Service] --> PaymentDB : "Store Payment Data"
[Fraud Detection Service] --> TxnDB : "Analyze Transaction Data"

[Payment Service] --> [Payment Gateway] : "Process Payments"
[Payment Gateway] --> [Card Network (Visa/Mastercard)] : "Authorize Card Payments"
[Payment Gateway] --> [Bank API] : "Authorize Bank Payments"

@enduml

```
![Deployment Diagram](./Diagrams/Deployment.png)

### Description  
- **Users**:  
  - **Retail investors**: Access Google Pay through either mobile or web apps to make payments and manage accounts.  
  - **Admins**: Use the Admin Web Dashboard to monitor transactions, manage users, and view fraud alerts.  
- **External Systems**:  
  - **Payment Gateway**: Routes payment processing requests to card networks and banks.  
  - **Card Network (Visa/Mastercard)**: Authorizes card transactions.  
  - **Bank API**: Authorizes and processes bank payments.  
- **Components**:  
  - **User's Mobile Device & PC**: Host the **Google Pay Mobile App** and **Google Pay Web App** for user interaction.  
  - **Application Server**: Runs services like **Authentication**, **Payment**, **Fraud Detection**, **Notification**, and **User Management**.  
  - **Database Server**: Hosts databases for user, transaction, and payment data.  
  - **Third-Party Services**: Handle payment authorization and processing.
