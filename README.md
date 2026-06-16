# 🏦 Global Standard Bank: Real-Time Fraud Engine (`RAM`)

## 1. Executive Summary & Scope
Global Standard Bank (GSB) serves approximately 1 million users globally across various banking and insurance services. Currently operating on a legacy batch-processing model, GSB is upgrading to a next-generation real-time fraud detection system in FY 2026 to enhance customer security and mitigate financial exposure.

## 2. Problem Statement (As-Is State)
GSB currently relies on a legacy batch processing system, 'RAVAN', for fraud detection. 
* **Delayed Execution:** The system analyzes and categorizes transactions in a bulk batch at the end of the day (5:00 PM).
* **Customer Friction:** Because detection is not real-time, the system relies on rigid rules that frequently block legitimate customers from using their cards while traveling.
* **Financial Vulnerability:** The delay allows malicious actors to exploit the system via Account Takeover (ATO) and Card-Not-Present (CNP) fraud. 
* **Business Impact:** During FY 26-27, GSB suffered financial losses exceeding $42 million due to these fraudulent transactions.

## 3. As-Is Process Flow
Currently, authorization requests are processed by the Customer's Bank and sent to local storage. They sit in a queue until 5:00 PM everyday, at which point 'RAVAN' evaluates the transaction risk and flags items for human review. By this time, funds have often already settled.
<img width="564" height="573" alt="as is flow fraud engine" src="https://github.com/user-attachments/assets/2ea430a6-6148-40f1-880e-e4ec7b212890" />

## 4. Proposed Solution (To-Be State)
To tackle these fraudulent transactions, GSB is implementing a real-time, event-based processing engine: **RAM**.
* **Performance Metric:** The system will evaluate transactions in under 200 milliseconds.
* **Execution:** 'RAM' executes business logic the millisecond a payment is triggered, making an immediate system call to approve, decline, or hold the transaction.

## 5. Core Business Rules Matrix
The following matrix outlines the initial rule conditions the `RAM` engine will evaluate in real-time.

| Rule ID | Trigger Condition (System Logic) | Real-Time Action | Reason Code (System Log) |
| :--- | :--- | :--- | :--- |
| **BR-01** | `Payee_History_Count == 0` | **HOLD** (Route to 24hr Queue) | `UNKNOWN_PAYEE` |
| **BR-02** | `Profile_Updated_Timestamp` < 30 mins AND `Transfer_Amount > $500` | **FLAG FOR REVIEW** | `ATO_PROFILE_VELOCITY` |
| **BR-03** | `Login_Location` != `Historical_Location_List` OR `New_Device_ID == TRUE` | **FLAG FOR REVIEW** | `ANOMALOUS_LOGIN_CONTEXT` |
