# real-time-fraud-engine-mockup
A mock real-time event-driven fraud detection rules engine.
# 🏦 Global Standard Bank: Real-Time Fraud Engine (`RAM`)

## 1. Executive Summary & Scope
[cite_start]Global Standard Bank (GSB) serves approximately 1 million users globally across various banking and insurance services[cite: 3]. [cite_start]Currently operating on a legacy batch-processing model, GSB is upgrading to a next-generation real-time fraud detection system in FY 2026 to enhance customer security and mitigate financial exposure[cite: 4].

## 2. Problem Statement (As-Is State)
[cite_start]GSB currently relies on a legacy batch processing system, 'RAVAN', for fraud detection[cite: 6]. 
* [cite_start]**Delayed Execution:** The system analyzes and categorizes transactions in a bulk batch at the end of the day (5:00 PM)[cite: 7].
* [cite_start]**Customer Friction:** Because detection is not real-time, the system relies on rigid rules that frequently block legitimate customers from using their cards while traveling[cite: 8].
* [cite_start]**Financial Vulnerability:** The delay allows malicious actors to exploit the system via Account Takeover (ATO) and Card-Not-Present (CNP) fraud[cite: 9]. 
* [cite_start]**Business Impact:** During FY 26-27, GSB suffered financial losses exceeding $42 million due to these fraudulent transactions[cite: 10].

## 3. As-Is Process Flow
<img width="564" height="573" alt="as is flow fraud engine" src="https://github.com/user-attachments/assets/c80ba6f3-97b1-4978-b078-a259e39cd868" />

*(See `/docs/as_is_process.png` for the detailed BPMN model)*
Currently, authorization requests are processed by the Customer's Bank and sent to local storage. [cite_start]They sit in a queue until 5:00 PM everyday, at which point 'RAVAN' evaluates the transaction risk and flags items for human review. By this time, funds have often already settled.

## 4. Proposed Solution (To-Be State)
[cite_start]To tackle these fraudulent transactions, GSB is implementing a real-time, event-based processing engine: **RAM**[cite: 14].
* [cite_start]**Performance Metric:** The system will evaluate transactions in under 200 milliseconds[cite: 15].
* [cite_start]**Execution:** 'RAM' executes business logic the millisecond a payment is triggered, making an immediate system call to approve, decline, or hold the transaction[cite: 15].

## 5. Core Business Rules Matrix
[cite_start]The following matrix outlines the initial rule conditions the `RAM` engine will evaluate in real-time[cite: 16, 17].

| Rule ID | Trigger Condition (System Logic) | Real-Time Action | Reason Code (System Log) |
| :--- | :--- | :--- | :--- |
| **BR-01** | `Payee_History_Count == 0` | **HOLD** (Route to 24hr Queue) | `UNKNOWN_PAYEE` |
| **BR-02** | `Profile_Updated_Timestamp` < 30 mins AND `Transfer_Amount > $500` | **FLAG FOR REVIEW** | `ATO_PROFILE_VELOCITY` |
| **BR-03** | `Login_Location` != `Historical_Location_List` OR `New_Device_ID == TRUE` | **FLAG FOR REVIEW** | `ANOMALOUS_LOGIN_CONTEXT` |
