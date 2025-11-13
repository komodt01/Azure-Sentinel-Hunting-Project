# Manual – How to Run the Sentinel Threat Hunting PoC

## 1. Prerequisites

- An Azure subscription.
- A **Log Analytics workspace**.
- Microsoft Entra ID sign-in data connected to that workspace (e.g., via the Entra/Sign-in logs connector or diagnostic settings).
- (Optional) Microsoft Sentinel enabled on the workspace.
- Python 3.x installed locally or on a VM.
- Azure CLI (`az`) installed, or a managed identity if running in Azure.

## 2. Python Environment Setup

Create and activate a virtual environment (optional but recommended):

   bash
   python -m venv .venv
   source .venv/bin/activate      # Linux/macOS
   .venv\Scripts\activate         # Windows

pip install azure-identity azure-monitor-query

3. Authentication

Log in to Azure from your terminal:

az login


Ensure the logged-in identity has Log Analytics Reader (or equivalent) on the workspace.

The script uses DefaultAzureCredential, which will detect your CLI login or managed identity automatically.

4. Configure sentinel_query.py

Open the script and update the workspace ID:

workspace_id = "YOUR-WORKSPACE-ID-HERE"


You can find this in the Azure Portal under:

Log Analytics Workspace → Overview → Workspace ID.

Adjust the KQL query as needed. The default is:

query = "SigninLogs | take 5"


Example: list failed sign-ins:

SigninLogs
| where ResultType != 0
| sort by TimeGenerated desc
| take 20

5. Running the Hunt

From the project directory, run:

python sentinel_query.py


If successful, you should see:

Table name(s) returned.

Row data printed as dictionaries representing each event.

6. Interpreting Results

Review the output for suspicious patterns:

High volume of failures from a single IP.

Sign-ins from unusual geolocations.

Unfamiliar user agents or client apps.

Refine the KQL query to narrow on specific signals of interest.

7. Extending the PoC

Export results to CSV or JSON for further offline analysis.

Split KQL queries into named “hunts” and parameterize them (e.g., time ranges, users).

Integrate with Sentinel:

Turn stable queries into analytics rules.

Use playbooks (Logic Apps) for response actions.

Document new hunts and their rationale in this repository as the project evolves.

