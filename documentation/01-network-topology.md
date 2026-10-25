# Network Topology

## Environment

The lab runs on Oracle VirtualBox using the isolated Host-Only network `192.168.56.0/24`.

| Host | Role | Address | Main Services |
|---|---|---:|---|
| ELK Server | Centralized SIEM | `192.168.56.121` | Elasticsearch, Logstash, Kibana |
| Kali Linux | Authorized attack simulator | `192.168.56.122` | SSH client and analysis tools |
| Ubuntu Server | Monitored Linux endpoint | `192.168.56.123` | OpenSSH, Filebeat, UFW |

## Traffic Flow

1. Kali sends controlled SSH requests to Ubuntu on TCP/22.
2. Ubuntu records authentication events.
3. Filebeat sends logs to Logstash on TCP/5044.
4. Logstash processes and forwards events to Elasticsearch.
5. Kibana provides search, visualization, detection, and investigation.
6. UFW is used for temporary source-IP containment.

## Security Boundaries

- Only the Host-Only network is used for attack simulation.
- No public target is included.
- Testing is limited to personally controlled machines.
- Credentials, tokens, and passwords must not be committed to GitHub.
