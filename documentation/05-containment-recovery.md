# Containment and Recovery

## Pre-Containment Check

On Ubuntu `192.168.56.123`:

```bash
sudo ufw status numbered
```

Ensure SSH remains allowed from a trusted management source before changing firewall rules. Use the VirtualBox console as a recovery path.

## Temporary Containment

Block the Kali lab source:

```bash
sudo ufw insert 1 deny from 192.168.56.122 to any port 22 proto tcp
sudo ufw status numbered
```

## Verify Containment

From Kali:

```bash
nc -zv -w 3 192.168.56.123 22
ssh -o ConnectTimeout=5 <lab-user>@192.168.56.123
```

Record the result and confirm the ELK pipeline remains operational.

## Recovery

List the numbered rules:

```bash
sudo ufw status numbered
```

Delete only the temporary deny rule by its current number:

```bash
sudo ufw delete <rule-number>
```

Verify recovery:

```bash
sudo ufw status numbered
```

From Kali:

```bash
nc -zv 192.168.56.123 22
```

Document the containment start, verification, rule removal, and recovery timestamps.
