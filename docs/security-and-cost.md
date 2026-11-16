# Security, cost and cleanup

- VM access: RDP 3389 should be limited to the operator's current public IP address and removed when not needed.
- The budget is an alert, not a billing hard cap.
- When idle, stop/deallocate the virtual machine. Disks and some network resources may continue to incur costs.
- The Sentinel free trial shown in evidence ran from 2026-11-16 until 2026-12-17 23:59 UTC; this is historical, not a statement of current trial eligibility.
- Revoke/disable the test analytics rule after the lab to avoid unnecessary alerts.
- Before cleanup, export KQL, screenshots, and the report. Then delete the disposable resource group if no longer needed, after double-checking contents.
- Never publish passwords, subscription IDs, tenant IDs, VM public IPs, or unredacted personally identifiable data.
