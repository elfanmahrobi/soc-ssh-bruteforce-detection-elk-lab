# SOC Investigation Workflow

## Triage Checklist

1. Confirm the alert rule name and severity.
2. Record the first and last event timestamps.
3. Identify the source IP.
4. Identify the destination host.
5. Identify targeted usernames.
6. Count authentication failures.
7. Check whether failures were followed by a successful login.
8. Review other activity from the same source IP.
9. Confirm the source is the authorized Kali lab VM.
10. Determine whether containment is required for the exercise.

## Investigation Questions

- Did one source target one account or several accounts?
- How many failures occurred in the detection window?
- Was there a successful login after the failures?
- Were `root`, privileged users, or nonexistent usernames targeted?
- Did the source generate other suspicious events?
- Is the behavior expected as part of the lab exercise?

## Timeline Format

```text
<UTC timestamp> - First failed SSH authentication
<UTC timestamp> - Detection threshold reached
<UTC timestamp> - Alert created
<UTC timestamp> - Analyst began triage
<UTC timestamp> - Source IP contained
<UTC timestamp> - Containment verified
<UTC timestamp> - Temporary block removed
<UTC timestamp> - Recovery verified and incident closed
```

## Evidence Handling

- Use UTC timestamps where possible.
- Sanitize passwords, tokens, and unrelated personal data.
- Preserve original screenshots.
- Record commands and findings in the incident report.
