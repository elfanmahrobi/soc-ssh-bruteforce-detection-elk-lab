# Controlled SSH Failure Simulation

## Safety Requirement

Run these steps only from Kali Linux `192.168.56.122` against the owned Ubuntu lab server `192.168.56.123`.

## Baseline

First, perform one authorized successful login and capture the accepted-authentication event.

```bash
ssh <lab-user>@192.168.56.123
```

## Generate Failed Events

Use a nonexistent test account and intentionally enter an incorrect password. Repeat manually five times within one minute.

```bash
ssh socfake@192.168.56.123
```

This manual method creates enough events to validate the lab threshold while keeping the simulation controlled.

## Local Verification

On the Ubuntu monitored server:

```bash
sudo journalctl -u ssh --since "10 minutes ago" | grep -E "Failed password|Invalid user|Accepted"
```

Depending on the Ubuntu logging configuration, also check:

```bash
sudo grep -E "Failed password|Invalid user|Accepted" /var/log/auth.log | tail -n 30
```

## Expected Result

The log set should contain the source `192.168.56.122`, the target username, timestamps, and failed authentication messages. Confirm the same events are visible in Kibana before creating an alert.
