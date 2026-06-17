# ⚙️ Functional Requirements (UI & Dashboard)

The following functional requirements dictate the behavior of the internal Fraud Analyst Dashboard. These requirements provide the UI execution layer for the transactions intercepted by the `RAM` engine's core business rules.

### Analyst Dashboard Logic

| Req ID | UI Component | Functional Requirement (System Behavior) | Traces To |
| :--- | :--- | :--- | :--- |
| **FR-UI-01** | Transaction Queue | The dashboard must query and display all transactions currently in the `24_HOUR_HOLD` status, sorted oldest to newest. | **ST-01, ST-02** |
| **FR-UI-02** | Risk Context | The UI must display the triggered Reason Code (e.g., `ATO_PROFILE_VELOCITY`) alongside the Transaction ID. | **ST-01, ST-02** |
| **FR-UI-03** | Approve Button | Clicking **[Approve]** must change status to `APPROVED` and trigger an API call to the Payment Gateway to release funds. | **ST-01, ST-02** |
| **FR-UI-04** | Decline Button | Clicking **[Decline]** must change status to `DECLINED_FRAUD` and automatically lock the `Customer_Profile`. | **ST-01, ST-02** |

*(Note: Wireframe mockups corresponding to these UI components are linked in the project wiki/assets).*
