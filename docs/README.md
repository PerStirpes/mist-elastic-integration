# mist Integration

## Overview

Explain what the integration is, define the third-party product that is providing data, establish its relationship to the larger ecosystem of Elastic products, and help the reader understand how it can be used to solve a tangible problem.
Check the [overview guidelines](https://www.elastic.co/guide/en/integrations-developer/current/documentation-guidelines.html#idg-docs-guidelines-overview) for more information.

## Datastreams

client_events includes

-   client-join
-   client-info
-   client-sessions

device_events include

-   device-events
-   device updowns

mx-edge

-   mx-edge events

nac_events includes

-   nac-accounting
-   nac-events

audits

alarms

### Mist Events

Mist network events

## Requirements

Elastic Agent must be installed. For more information, refer to [these instructions](https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html).

#### Installing and managing an Elastic Agent:

You have a few options for installing and managing an Elastic Agent:

#### Install a Fleet-managed Elastic Agent (recommended):

With this approach, you install Elastic Agent and use Fleet in Kibana to define, configure, and manage your agents in a central location. We recommend using Fleet management because it makes the management and upgrading of your agents considerably easier.

#### Install Elastic Agent in standalone mode (advanced users):

With this approach, you install Elastic Agent and manually configure the agent locally on the system where it’s installed. You are responsible for managing and upgrading the agents. This approach is reserved for advanced users only.

#### Install Elastic Agent in a containerized environment:

You can run Elastic Agent inside a container, either with Fleet Server or standalone. Docker images for all versions of Elastic Agent are available from the Elastic Docker registry, and we provide deployment manifests for running on Kubernetes.

You need Elasticsearch for storing and searching your data and Kibana for visualizing and managing it.
You can use our hosted Elasticsearch Service on Elastic Cloud, which is recommended, or self-manage the Elastic Stack on your own hardware.

The requirements section helps readers to confirm that the integration will work with their systems.
Check the [requirements guidelines](https://www.elastic.co/guide/en/integrations-developer/current/documentation-guidelines.html#idg-docs-guidelines-requirements) for more information.

## Setup

Point the reader to the [Observability Getting started guide](https://www.elastic.co/guide/en/observability/master/observability-get-started.html) for generic, step-by-step instructions. Include any additional setup instructions beyond what’s included in the guide, which may include instructions to update the configuration of a third-party service.
Check the [setup guidelines](https://www.elastic.co/guide/en/integrations-developer/current/documentation-guidelines.html#idg-docs-guidelines-setup) for more information.

### Enabling the integration in Elastic:

#### Create a new integration from a ZIP file (optional)

1. In Kibana, go to **Management** > **Integrations**.
2. Select **Create new integration**.
3. Select **Upload it as a .zip**.
4. Upload the ZIP file.
5. Select **Add to Elastic**.

### Install the integration

1. In Kibana, go to **Management** > **Integrations**.
2. In **Search for integrations** search bar, type mist.
3. Click the **mist** integration from the search results.
4. Click the **Add mist** button to add the integration.
5. Add all the required integration configuration parameters.
6. Click **Save and continue** to save the integration.

### Collecting logs from HTTP endpoint

Specify the address and port that will be used to initialize a listening HTTP server that collects incoming HTTP POST requests containing a JSON body. The body must be either an object or an array of objects. Any other data types will result in an HTTP 400 (Bad Request) response. For arrays, one document is created for each object in the array.

### TLS/SSL Configuration (Optional)

To enhance security, configure the server with TLS/SSL settings. This ensures secure communication between clients and the server. Below is an example of how to configure these settings:

```yml
ssl.certificate: "/etc/pki/server/cert.pem"
ssl.key: "/etc/pki/server/cert.key"
```

ssl.key: The private key used by the server to decrypt data encrypted with its corresponding public key, as well as to sign data to authenticate its identity.
ssl.certificate: The server's certificate, used to verify its identity. It contains the public key and is validated against a Certificate Authority (CA) to establish an encrypted connection with clients.

In the input settings, include any relevant SSL Configuration and Secret Header values depending on the specific requirements of your endpoint. You may also configure additional options such as certificate, keys, supported_protocols, and verification_mode. Refer to the [Elastic SSL Documentation](https://www.elastic.co/guide/en/beats/filebeat/current/configuration-ssl.html#ssl-server-config) for further details.

## Troubleshooting (optional)

-   If some fields appear conflicted under the `logs-*` or `metrics-*` data views, this issue can be resolved by [reindexing](https://www.elastic.co/guide/en/elasticsearch/reference/current/use-a-data-stream.html#reindex-with-a-data-stream) the impacted data stream.

Provide information about special cases and exceptions that aren’t necessary for getting started or won’t be applicable to all users. Check the [troubleshooting guidelines](https://www.elastic.co/guide/en/integrations-developer/current/documentation-guidelines.html#idg-docs-guidelines-troubleshooting) for more information.

### Troubleshooting HTTP endpoint

If you encounter an error while ingesting data, it might be due to the data collected over a long time span. Generating a response in such cases may take longer and might cause a request timeout if the `HTTP Client Timeout` parameter is set to a small duration. To avoid this error, it is recommended to adjust the `HTTP Client Timeout` and `Interval` parameters based on the duration of data collection.

```
{
  "error": {
    "message": "failed eval: net/http: request canceled (Client.Timeout or context cancellation while reading body)"
  }
}
```

## Reference

Provide detailed information about the log or metric types we support within the integration. Check the [reference guidelines](https://www.elastic.co/guide/en/integrations-developer/current/documentation-guidelines.html#idg-docs-guidelines-reference) for more information.

## Logs

### Mist Events

Mist client events

**ECS Field Reference**

Please refer to the following [document](https://www.elastic.co/guide/en/ecs/current/ecs-field-reference.html) for detailed information on ECS fields.

## Mist Client Events (info | join | session)

| Mist field (JSON)             | ECS field created by pipeline                                      | Notes                                                     |
| ----------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------- |
| `client_family`               | `device.model.identifier`                                          |                                                           |
| `client_manufacture`          | `device.manufacturer`                                              |                                                           |
| `client_model`                | `device.model.name`                                                |                                                           |
| `client_os`                   | `device.os.version`                                                |                                                           |
| `ssid`                        | `network.name`                                                     |                                                           |
| `next_ap`                     | `destination.mac`                                                  |                                                           |
| `ap`                          | `server.mac` & `observer.mac`                                      | duplicated                                                |
| `ap_name`                     | `observer.product`                                                 | _recommend mapping to `observer.name` to avoid overwrite_ |
| `event_id`                    | `transaction.id`                                                   |                                                           |
| `service`                     | `service.name`                                                     |                                                           |
| `client_hostname`, `hostname` | `client.hostname`                                                  |                                                           |
| `duration`                    | `event.duration` (sec → ns)                                        |                                                           |
| `disconnect`                  | `event.end` (sec → ms)                                             |                                                           |
| `id`                          | `event.id`                                                         |                                                           |
| `count`                       | `event.sequence`                                                   |                                                           |
| `connect`                     | `event.start` (sec → ms)                                           |                                                           |
| `reason`                      | `event.reason`                                                     |                                                           |
| `component`                   | `event.provider`                                                   |                                                           |
| `model`                       | `observer.product`                                                 | may be overwritten by `ap_name`                           |
| `device_type`                 | `observer.type`                                                    |                                                           |
| `client_username`             | `client.user.name`                                                 | (mapped twice)                                            |
| `client_ip`, `ip`             | `client.ip` (typed)                                                |                                                           |
| `mac`                         | `client.mac`                                                       |                                                           |
| `src_ip`                      | `source.ip`                                                        |                                                           |
| `site_name`                   | `organization.name`                                                |                                                           |
| `org_id`                      | `organization.id`                                                  |                                                           |
| _Geo IP enrich_               | `source.geo`, `source.as.*`, `destination.geo`, `destination.as.*` |                                                           |

---

## Mist Device Events (+ up/down)

| Mist field                           | ECS field                      | Notes                 |
| ------------------------------------ | ------------------------------ | --------------------- |
| `site_name`                          | `organization.name`            |                       |
| `org_id`                             | `organization.id`              |                       |
| `ap`                                 | `observer.mac`                 |                       |
| `audit_id`                           | `event.id`                     |                       |
| `reason`                             | `event.code`                   |                       |
| `device_type`                        | `observer.type`                |                       |
| _(script)_ `device_name` / `ap_name` | `observer.name`                | device_name preferred |
| _(constant)_                         | `observer.vendor = "juniper"`  |                       |
| _(tag)_                              | `event.category = ["network"]` |                       |
| `mac`                                | — (removed)                    | duplicate             |

---

## Mist Alarms

| Mist field  | ECS field                                                          | Notes                     |
| ----------- | ------------------------------------------------------------------ | ------------------------- |
| `last_seen` | `event.end`                                                        | ISO-8601 parsed           |
| `start`     | `event.start`                                                      | epoch ms                  |
| `id`        | `event.id`                                                         |                           |
| `type`      | `event.code`                                                       |                           |
| `org_id`    | `organization.id`                                                  |                           |
| `hostnames` | `host.hostname`                                                    |                           |
| `macs`      | `client.mac`                                                       |                           |
| `servers`   | `server.ip` (typed)                                                |                           |
| `site_name` | `organization.name`                                                |                           |
| _(script)_  | `event.kind = alert`                                               |                           |
| _(script)_  | `event.category`, `event.type`, `event.severity`                   | from `group` + `severity` |
| _Geo IP_    | `source.geo`, `source.as.*`, `destination.geo`, `destination.as.*` |                           |

---

## Mist Audit Events

| Mist field   | ECS field                                                          | Notes |
| ------------ | ------------------------------------------------------------------ | ----- |
| `src_ip`     | `source.ip` (typed)                                                |       |
| `org_id`     | `organization.id`                                                  |       |
| `id`         | `event.id`                                                         |       |
| `admin_name` | `user.full_name`                                                   |       |
| `user_agent` | `user_agent.original`                                              |       |
| `@timestamp` | _(left as-is by Cribl)_                                            |       |
| _Geo IP_     | `source.geo`, `source.as.*`, `destination.geo`, `destination.as.*` |       |

---

## Mist NAC Authentication Events (`nac_events`)

| Mist field (JSON) | ECS field created by pipeline                                                                             | Notes                                                   |
| ----------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `nas_ip`          | `observer.ip` (typed)                                                                                     | Geo-enriched → `observer.geo`, `observer.as.*`          |
| `mac`             | `client.mac`                                                                                              |                                                         |
| `device_mac`      | `observer.mac`                                                                                            |                                                         |
| `nas_vendor`      | `observer.vendor`                                                                                         |                                                         |
| `org_id`          | `organization.id`                                                                                         |                                                         |
| `username`        | `user.name`                                                                                               |                                                         |
| `text`            | `event.reason`                                                                                            |                                                         |
| `nacrule_id`      | `rule.id`                                                                                                 |                                                         |
| `idp_role`        | `user.roles`                                                                                              |                                                         |
| _(enrichment)_    | `observer.as.asn → observer.as.number`<br>`observer.as.organization_name → observer.as.organization.name` | after GeoIP                                             |
| _(pipeline)_      | `event.category = ["authentication"]`                                                                     | plus `"network"` for `NAC*CLIENT*[PERMIT         DENY]` |
| _(pipeline)_      | `event.type = "allowed"` / `"denied"`                                                                     | based on `nac_events.type`                              |
| _(pipeline)_      | `event.outcome = "success"` / `"failure"`                                                                 |                                                         |
| \_(related)\*     | `related.ip`, `related.user`                                                                              | adds observer IP / client MAC / observer MAC / username |

---

## Mist NAC Accounting Events (`nac_accounting`)

| Mist field (JSON) | ECS field created by pipeline                                                                             | Notes                                                               |
| ----------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `nas_ip`          | `observer.ip` (typed)                                                                                     | Geo-enriched                                                        |
| `client_ip`       | `client.ip` (typed)                                                                                       | Geo-related (added to `related.ip`)                                 |
| `type`            | `event.code`                                                                                              | (`NAC_ACCOUNTING_*`)                                                |
| `mac`             | `client.mac`                                                                                              |                                                                     |
| `username`        | `user.name`                                                                                               |                                                                     |
| `nas_vendor`      | `observer.vendor`                                                                                         |                                                                     |
| `org_id`          | `organization.id`                                                                                         |                                                                     |
| `device_mac`      | `observer.mac`                                                                                            |                                                                     |
| `ap`              | `observer.mac`                                                                                            | overwrites `device_mac` if both present                             |
| _(enrichment)_    | `observer.as.asn → observer.as.number`<br>`observer.as.organization_name → observer.as.organization.name` |                                                                     |
| _(pipeline)_      | `event.category = ["network"]`                                                                            |                                                                     |
| _(pipeline)_      | `event.type` = `"start"` / `"end"` / `"info"`                                                             | based on `type`                                                     |
| _(pipeline)_      | `event.outcome = "success"`                                                                               |                                                                     |
| \_(related)\*     | `related.ip`, `related.user`                                                                              | adds observer IP / client IP / client MAC / observer MAC / username |
| _(cleanup)_       | removes `nas_ip`, `client_ip` from `mist.nac_accounting` payload                                          |

\* **related.\* fields** are appended to help with cross-event correlation; they are not direct renames.

**Exported fields**

| Field                 | Description               | Type             |
| --------------------- | ------------------------- | ---------------- |
| @timestamp            | Event timestamp.          | date             |
| data_stream.dataset   | Data stream dataset name. | constant_keyword |
| data_stream.namespace | Data stream namespace.    | constant_keyword |
| data_stream.type      | Data stream type.         | constant_keyword |
| event.dataset         | Event dataset             | constant_keyword |
| event.module          | Event module              | constant_keyword |

# TODO

## nac events ingest pipeline

`nac-accounting`

[x] \"type\":\"NAC_ACCOUNTING_STOP\" maps to event.code

`nac-events`

[x] type": "NAC_CLIENT_DENY", mapped to event.code
[x] `text` maps to mist.nac_events.message example `\"text\":\"No policy rules are hit, rejected by implicit deny\"`

## device events ingest pipeline

`device-events`
[x] `text` maps to `mist.device_events.message` example `\"text\":\"_USR_SESSION_HELD: MAC-RADIUS User 0 session with MacAddress interface mge-0/0/38.0 vlan (null) is held\"`
[x] device events | has `type` `"type\":\"SW_USR_SESSION_HELD\"`

---

`device-updowns`

[x] device updowns `"ev_type": "NOTICE",`
[x] `"type": "AP_RESTARTED",` mapped to event.code
[x] device updowns reason mapped to mist.device_events.message

---

## alarms ingest pipeline

[xs] type, "type\":\"infra_dhcp_failure\" maps to `event.code`

## audits ingest pipeline

[x] message maps to audit_events.message `\"message\":\"Login with Role \\\"_NET_ENG_WI-FI_NA_NG\\\"\",`

## client events ingest pipeline

`client-info`
[ ] has no message

`client-join`
[ ] has no message

`client-session`
[ ] termination reason

## mx-edge ingest pipeline

[x] type `"type": "TT_INACTIVE_VLANS"`,

## Misc

[ ] drop "agent_id_status": "missing"
[ ] @timestamp is cribl ingested timestap

-   change to ingested at cribl
    [ ] parse orgininal timestamp
