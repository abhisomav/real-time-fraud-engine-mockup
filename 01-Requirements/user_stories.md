# 📖 User Stories (Behavior-Driven Development)

The following BDD scenarios dictate the functional execution of the `RAM` real-time fraud engine. These stories map directly to the Core Business Rules Matrix.

---

## Story: ST-01 - New Payee Cooling-Off Period
**Traceability:** Maps to BR-01
**Feature:** Real-Time Transaction Holding

**Scenario:** A customer attempts to send money to a payee with no prior transaction history.
* **Given** a valid customer account is in an active state
* **And** the customer initiates a transfer to a destination account
* **And** the `Payee_History_Count` for this destination is exactly `0`
* **When** the transaction payload reaches the payment gateway
* **Then** the `RAM` system must intercept the transaction in under 200ms
* **And** route the transaction to the `24_HOUR_HOLD` queue
* **And** log the event with the reason code `UNKNOWN_PAYEE`

---

## Story: ST-02 - Velocity Check on Profile Updates
**Traceability:** Maps to BR-02
**Feature:** Real-Time Account Takeover (ATO) Prevention

**Scenario:** A customer attempts a high-value transfer immediately after changing personal details.
* **Given** a valid customer account is in an active state
* **And** the customer has successfully updated their profile information (Password, Email, Mobile Number, or Address) within the last 30 minutes
* **When** the customer attempts an outbound transfer greater than $500 AUD
* **Then** the `RAM` system must intercept the transaction
* **And** place the transaction in a `24_HOUR_HOLD` queue
* **And** log the transaction with the reason code `ATO_PROFILE_VELOCITY`

---

## Story: ST-03 - Anomalous Login Context & MFA Trigger
**Traceability:** Maps to BR-03
**Feature:** Frictionless Customer Verification

**Scenario:** A customer attempts a transaction from an unrecognized device or country.
* **Given** a valid customer account is in an active state
* **And** a new login session is established
* **And** the session `Device_ID` is unrecognized **OR** the `IP_Location` does not match the `Historical_Location_List`
* **When** the customer attempts an outbound transaction of any amount
* **Then** the `RAM` system must intercept the transaction
* **And** update the transaction status to `PENDING_MFA_VERIFICATION`
* **And** trigger a real-time OTP (One-Time Password) SMS to the customer's registered mobile number
* **And** log the event with the reason code `ANOMALOUS_LOGIN_CONTEXT`
