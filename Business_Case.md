# Business Case – Azure Sentinel + Python Threat Hunting PoC

Modern cloud environments generate large volumes of identity and activity logs. Without targeted analysis, these logs become “write-only” data: stored for compliance, but rarely used to proactively detect threats.

This proof of concept addresses the need for:

- **Proactive detection** of suspicious or anomalous sign-ins.
- **Hands-on hunting** against real sign-in data using KQL and automation.
- **Demonstrable integration** between Microsoft Sentinel/Log Analytics and Python-based tooling.

The business value of this PoC includes:

- Showing how a security team can move from passive monitoring to **hypothesis-driven threat hunting**.
- Demonstrating that identity logs (such as Entra ID `SigninLogs`) can highlight risky behavior earlier in the kill chain.
- Providing a repeatable pattern that can be extended to other log sources (e.g., `AuditLogs`, `SecurityEvent`, application logs).

This PoC is intentionally scoped to a single hunting scenario so that the architecture, queries, and automation can be clearly explained and reused in future projects.
