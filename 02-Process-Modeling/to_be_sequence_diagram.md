# 🏗️ Process Modeling: To-Be Architecture

## Real-Time Engine Sequence Diagram
The following sequence diagram outlines the real-time data flow and decision logic of the `RAM` fraud engine. The system is constrained by a strict Non-Functional Requirement (NFR) to execute this entire loop in under 200 milliseconds to prevent payment gateway timeouts.

```mermaid
sequenceDiagram
    participant C as Customer 
    participant PG as Payment Gateway
    participant RAM as RAM Fraud Engine
    participant DB as Core Banking DB

    C->>PG: Initiates Transfer ($500+)
    PG->>RAM: Streams Transaction Payload (JSON)
    
    rect rgb(200, 220, 240)
        Note right of RAM: Sub-200ms Execution
        RAM->>DB: Fetch Payee & Profile History
        DB-->>RAM: Return Historical Data
        RAM->>RAM: Execute Business Rules (BR-01, BR-02)
    end
    
    alt is Fraudulent (Rule Triggered)
        RAM-->>PG: HOLD Transaction (Status: 24_HOUR_HOLD)
        RAM->>DB: Log Event (ATO_PROFILE_VELOCITY)
    else is Legitimate
        RAM-->>PG: APPROVE Transaction
    end
```
