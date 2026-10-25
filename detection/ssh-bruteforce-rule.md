# SSH Brute-Force Detection Rule

## Rule Metadata

- **Name:** Linux SSH Brute-Force Attempts
- **Description:** Detect repeated failed SSH authentication attempts from the same source IP against the monitored Ubuntu server.
- **Rule type:** Threshold
- **Severity:** Medium
- **Risk score:** 47
- **Runs every:** 1 minute
- **Additional look-back:** 1 minute

## Query

```text
host.ip:"192.168.56.123" and event.category:authentication and event.outcome:failure
```

Fallback during early pipeline testing:

```text
message:"Failed password"
```

## Threshold

- Group by: `source.ip`
- Threshold: `5`

## Investigation Guide

1. Validate source IP and destination host.
2. Review targeted usernames.
3. Count failures and establish the event window.
4. Search for successful authentication from the same source.
5. Review other events from the source IP.
6. Determine whether the activity is authorized lab testing.
7. Contain using the documented UFW procedure if required.
8. Preserve evidence and complete the incident report.

## False Positives

- User typing an outdated password repeatedly
- Automated management systems with stale credentials
- Misconfigured scripts or scheduled jobs
- Expected authentication testing inside the laboratory

## MITRE ATT&CK

- T1110 - Brute Force
- T1110.001 - Password Guessing
