# Linux Authentication Log Onboarding

## Monitored Server

Install and configure Filebeat on `192.168.56.123`. Copy the example carefully and replace certificate paths and credentials with environment-specific values.

```bash
sudo filebeat test config -e
sudo filebeat test output -e
sudo systemctl enable --now filebeat
sudo systemctl is-active filebeat
```

## ELK Server

Place the Logstash example on `192.168.56.121`, validate it, and restart Logstash.

```bash
sudo /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
sudo systemctl restart logstash
sudo systemctl is-active logstash
sudo ss -lntp | grep 5044
```

## Validation Event

On the monitored Ubuntu Server:

```bash
logger "SOC-HOMELAB TEST: Linux authentication pipeline validation"
sudo journalctl --since "5 minutes ago" | grep "SOC-HOMELAB"
```

Then verify the event in Kibana Discover. Do not continue until logs from `192.168.56.123` are searchable.
