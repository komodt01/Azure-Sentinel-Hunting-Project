# Design Overview
_Project:_ **Sentinel Threat Hunting PoC**  
_Generated:_ 2025-11-13  
_Cloud:_ Azure  
_Key Services/Areas:_ Microsoft Sentinel, Log Analytics, Entra ID, Python, KQL

## Business Requirements

- Enable targeted threat hunting against Azure sign-in data (`SigninLogs`).
- Use **Azure-native** log collection (Log Analytics + Sentinel) instead of ad-hoc log storage.
- Automate execution of hunting queries using **Python** and the Azure Monitor Query SDK.
- Keep the design simple enough to run in a lab subscription while still reflecting real-world practices.

Success is defined as:

- Being able to run KQL queries via Python against recent sign-in data.
- Surfacing potentially suspicious activity (e.g., odd sign-ins, unusual locations) for investigation.
- Documenting the approach so it can be reused by security teams.

## Architecture Summary

- **Identity & Logs**
  - Microsoft Entra ID generates **signin and audit events**.
  - Diagnostic settings or connectors route these events into a **Log Analytics workspace**.

- **SIEM Layer**
  - **Microsoft Sentinel** is enabled on top of the workspace to provide security context, workbooks, and advanced analytics (optional but recommended).

- **Hunting Tooling**
  - A **Python script** (`sentinel_query.py`) uses:
    - `DefaultAzureCredential` for authentication.
    - `LogsQueryClient` to run KQL queries against the workspace.
  - The script queries tables like `SigninLogs` and prints results for review.

- **Analyst Interaction**
  - A security analyst runs the script, inspects results, and refines KQL queries.
  - Over time, successful hunting patterns can be turned into **Sentinel analytics rules** or automated playbooks.

## Design Trade-offs

- **Scope vs. depth**
  - Focused on identity-centric hunting (sign-in activity) instead of ingesting every possible log source.
  - Keeps the PoC manageable while still demonstrating strong security value.

- **Manual execution vs. full automation**
  - Script is run manually to keep the pattern explicit and explainable.
  - In production, the logic could be integrated into scheduled jobs, playbooks, or SOAR tooling.

- **Lab-scale deployment vs. production readiness**
  - Designed for a single subscription/workspace.
  - A real deployment would use multiple workspaces, regions, and access boundaries.

## Cost Estimate (Lab Scale)

- Log Analytics Workspace: small ingestion volume (primarily sign-in data).
- Sentinel: pay-as-you-go based on ingested data and enabled features.
- No additional compute required beyond the analyst’s machine or a small VM for running the script.

At modest lab volumes, this PoC can typically be run for **low tens of USD per month**, depending on log retention and ingest volume.

## Well-Architected Pillar Mapping

| Pillar             | Design Decision                                            | Rationale                                      |
|--------------------|------------------------------------------------------------|------------------------------------------------|
| Security           | Centralize identity logs; hunt with KQL + Python          | Improve threat detection and investigation     |
| Operational Excellence | Use Log Analytics and Sentinel as managed platforms   | Reduce overhead vs. self-hosted logging stack  |
| Performance        | Query-focused script and KQL optimizations                | Quick response when running targeted hunts     |
| Cost Optimization  | Scope log sources and retention to PoC needs              | Control ingestion and storage costs            |
