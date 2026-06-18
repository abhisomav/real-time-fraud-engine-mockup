# Non Functional Requirements

NFR-PERF-01: The RAM engine must process the transaction payload and return an API response (Approve, Decline, or Hold) within a maximum latency of 200 milliseconds. If the 200ms limit is breached, the engine must release the payload and allow the system to default the transaction to the manual review queue, ensuring the customer's payment screen does not freeze.


NFR-SCAL-02: The engine must maintain its 200ms response time SLA during peak load events, specifically supporting a sustained throughput of up to 3,000 concurrent Transactions Per Second (TPS). The system must process this volume without dropping payloads or degrading the customer experience


NFR-AVAIL-03: The RAM engine must support zero-downtime deployment strategies. System maintenance and software patching must not result in unmonitored transaction windows, ensuring the engine maintains an overall availability SLA of 99.99%.

