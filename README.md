# Azure Windows Security Monitoring Lab

Hands-on Azure blue-team lab using Windows Server 2022, Azure Monitor Agent (AMA), a Data Collection Rule (DCR), Log Analytics and Microsoft Sentinel.

## Architecture

Windows Server VM (vm-win-security) -> AMA + DCR (dcr-windows-security) -> Log Analytics (law-windows-security) -> Microsoft Sentinel (KQL + analytics incidents)

## Verified results (2026-11-16)

- Windows Server Security events collected in the `SecurityEvent` table.
- Four event types verified: 4624 successful logon, 4625 failed logon, 4688 process creation, 4720 account creation.
- A 24-hour summary query returned 126 successful logons, 1 failed logon, 643 process creation events, and 4 account creation events. These are historical snapshot counts, not continuous rates.
- Failed-to-successful authentication correlation identified the controlled RDP test pattern for the lab account.
- Process investigation identified Windows and Azure management processes; a controlled PowerShell marker was visible via a matching `CommandLine` query.
- Command-line coverage snapshot: 9 / 739 process creation events populated `CommandLine` (~1.2%).
- A Sentinel analytics rule titled Windows - New Local User Account Created generated four visible incidents at Medium severity. Incident 110079 contained one alert, one recorded activity, and zero mapped entities.

## Investigations

1. Failed logons (4625): detailed events, frequency and timeline.
2. Failed then successful logons (4625 -> 4624): correlate by account and time window, interpret source and logon type.
3. Process execution (4688): process frequency, parent-child context, PowerShell / CMD, command-line visibility.
4. Account creation (4720): actor, target, counts, timeline and authorized-test context.

See [`kql-queries/`](kql-queries/) for runnable queries and [`screenshots/`](screenshots/) for evidence.

## Detection and assessment

A scheduled Sentinel detection for account creation was demonstrated through generated incidents. The underlying scheduled rule settings (frequency, threshold, full deployed query and entity mapping configuration) were not captured, so this repository does not claim to verify those values. The account creation was part of an authorized lab; no compromise was established. The missing incident entity mapping and sparse command-line collection are documented limitations.

## Reproduction outline

1. Deploy a Windows Server 2022 VM with restricted RDP and a cost budget alert.
2. Create Log Analytics Workspace, enable Microsoft Sentinel, and install Azure Monitor Agent.
3. Create a DCR and associate the VM; configure selected Security Event IDs.
4. Enable required audit policies and generate harmless authentication, process and local account events.
5. Query the `SecurityEvent` table and build a Sentinel scheduled analytics rule.
6. Review alerts, export evidence and remove test resources.

## Evidence and privacy

Screenshots are limited snapshots from a controlled lab. Review every screenshot for identifiers before publishing. See [`docs/evidence-register.md`](docs/evidence-register.md) and [`docs/security-and-cost.md`](docs/security-and-cost.md).

## Report

The full technical report is provided separately as a DOCX and PDF in the downloadable deliverables.
