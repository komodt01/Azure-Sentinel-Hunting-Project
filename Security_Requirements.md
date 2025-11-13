# Security Requirements
_Project:_ **Sentinel Threat Hunting PoC**  
_Generated:_ 2025-11-13  
_Cloud:_ Azure

## 1. Security Objectives

- Use identity and activity logs to proactively **detect suspicious sign-in behavior**.
- Preserve **confidentiality and integrity** of log data.
- Ensure that access to the log workspace and queries is **properly authenticated and authorized**.
- Provide enough visibility to support **incident investigation** and follow-on analytics.

## 2. Derived Requirements

| Category           | Requirement                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------|
| Identity & Access  | Use Azure AD identities and role-based access control (RBAC) for workspace and Sentinel access. |
| Identity & Access  | Avoid hard-coded secrets; rely on `DefaultAzureCredential` where possible.                      |
| Data Protection    | Use TLS for all communication with Azure services; protect workspace data at rest.             |
| Monitoring & Logs  | Ingest `SigninLogs` into Log Analytics; optionally include `AuditLogs` and other key sources.  |
| Governance         | Clearly separate roles for Sentinel admin, workspace admin, and read-only hunting users.       |
| Tooling Security   | Keep the Python script free of secrets; allow only necessary permissions for querying logs.    |

## 3. Controls Implemented

- **Authentication and Authorization**
  - Python script uses `DefaultAzureCredential`, leveraging existing identity rather than embedded keys.
  - Workspace access governed via Azure RBAC roles.

- **Log Ingestion**
  - Microsoft Entra ID `SigninLogs` are connected to a Log Analytics workspace.
  - Sentinel is enabled on the workspace for consolidated security context.

- **Data Protection**
  - All calls to Azure Monitor use HTTPS.
  - Workspace and Sentinel data benefit from Azure’s built-in encryption at rest.

- **Operational Controls**
  - KQL queries are versionable and can be maintained as code alongside the Python script.
  - Manual runbook (`manual.md`) defines how to execute the hunt consistently.

## 4. Residual Risks & Mitigations

- **Limited log coverage**
  - PoC may only ingest `SigninLogs`.  
  - *Mitigation:* For production, expand to `AuditLogs`, `SecurityEvent`, and other relevant sources.

- **Manual analysis**
  - Hunting results require analyst review; no automatic remediation.  
  - *Mitigation:* Convert mature queries into Sentinel analytics rules or automate via playbooks.

- **False positives / false negatives**
  - Initial KQL queries may be simplistic.  
  - *Mitigation:* Iterate on KQL patterns, tune thresholds, and incorporate additional context (location, device, risk).

- **RBAC misconfiguration**
  - Overly broad access could expose sensitive logs.  
  - *Mitigation:* Follow least-privilege RBAC patterns and regularly review role assignments.
