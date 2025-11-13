# Risks and Mitigations

| Risk ID | Risk Description                                                | Impact                                   | Likelihood | Mitigation                                                                                  |
|--------:|------------------------------------------------------------------|------------------------------------------|-----------|---------------------------------------------------------------------------------------------|
| R1      | Limited log sources (only SigninLogs ingested)                  | Important activity may be missed         | Medium    | Expand ingestion to include AuditLogs, SecurityEvent, and key application logs.            |
| R2      | Manual execution of Python script                               | Hunts may not run consistently           | Medium    | Schedule runs via automation (e.g., Azure Functions, Logic Apps, or a CI/CD pipeline).     |
| R3      | False positives / noisy results                                 | Analyst fatigue or missed real threats   | Medium    | Iterate on KQL logic; refine filters and focus on high-risk scenarios.                     |
| R4      | Excessive access to the workspace                               | Unauthorized access to sensitive logs    | Low/Med   | Enforce least-privilege RBAC; regularly review and audit access assignments.               |
| R5      | Lack of integration with incident response workflows            | Slow response to detected threats        | Medium    | Integrate results with Sentinel incidents or ticketing systems as a follow-on enhancement. |
| R6      | Cost growth if ingestion and retention are not tuned            | Higher-than-expected Azure costs         | Low       | Set sensible retention, monitor ingestion volume, and use budgets/alerts.                  |
