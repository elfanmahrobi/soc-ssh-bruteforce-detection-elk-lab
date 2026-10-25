# Kibana Investigation Queries

Use Kibana Discover with the data view that contains Filebeat Linux events.

## Primary KQL

```text
host.ip:"192.168.56.123" and event.category:authentication and event.outcome:failure
```

## Source-Focused Query

```text
source.ip:"192.168.56.122" and event.category:authentication
```

## Message Fallback

```text
message:"Failed password" and source.ip:"192.168.56.122"
```

## Successful Login After Failures

```text
source.ip:"192.168.56.122" and event.category:authentication and event.outcome:success
```

## Fields to Display

- `@timestamp`
- `host.name`
- `host.ip`
- `source.ip`
- `user.name`
- `event.category`
- `event.action`
- `event.outcome`
- `process.name`
- `message`

> Confirm actual field availability in Discover. If structured fields are missing, use `message` during initial validation and improve parsing later.
