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

![System Context](./Diagrams/C.png)


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

![Component Diagram](./Diagrams/S.png)

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

![Component Diagram](./Diagrams/Component.png)

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
