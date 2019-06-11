<!-- File format adr/adr-0000-project-keyword-YYYY-MM-DD.md -->

# ADR 9999: Global load balancing for Empire GKE clusters

## Status

- [x] active
- [ ] rejected
- [ ] deprecated
- [ ] superseded

## Context

The introduction of the Empire infrastructure (GKE clusters) requires the implementation of global load balancing.
In more detail, Empire infrastructure is trying to consolidate the countless kubernetes clusters the devops/sre needs to manage.<br>
With this consolidation, we are gaining better resource utilisation, safer and unified clusters.

Because this effectively means that we are putting all our eggs in the same basket, we decided (see adr-0007-global-gke-clusters) to deploy the applications to more than one cluster, which achieves - at first - continental and later global high availability.

For this continental/global high availability, we require a solution, which is capable of load balancing the incoming requests to the multiple clusters, which are running our applications.

## Decision

After contacting CloudFlare's sales team, we realised, that global load balancing is not cheap, so the SREs were investigating other options. Akamai and AWS Route53.

It turned out, that with AWS Route53 we are capable of achieving almost the same results for about the 10% of the cost compared to CloudFlare, so the decision is to go with AWS Route53 instead of CloudFlare's Intelligent failover/Load balancing.

## Consequences

CloudFlare's and Akamai's DNS load balancing solution implementation differs from AWS Route53's.

CloudFlare and Akamai creates a single record and they programmatically send different IPs (based on the weights specified) to the clients when they do the resolution. While AWS Route53's solution is depending on the records TTL values.

~~This means, that if a client is doing heavy DNS querry caching, they won't do a fresh resolution on the hostname, when its TTL expired, but their set DNS servers will return the cached value.~~

~~This can cause unforeseen outages for clients - in case of an Empire GKE cluster outage - who's DNS server is not repsecting these TTL values.~~

The above lines are incorrect, but left them in for information, because some may reach the same conclusion as I did before.

After testing CloudFlare and AWS R53 together it turned out, that CloudFlare is respecting AWS's TTL, so load balancing works perfectly. <br>Besides this, because CLoudFlare is obscuring the real records (CNAME and A) from the end users, they constantly get the same resolution results from CloudFlare and CloudFlare does the following resolutions from AWS R53.

### Technical recommendation

Because we are going to use high frequency health checking with AWS Route53 (1 origin will receive ~2-3 requests / second), we should create a log exclusion in stackdriver for our GKE container log ingestion.

This is recommended to decrease the noise in the logs and the amount we are billed (Stackdriver log ingestion).

Example:

If we have 12 applications, each with 3 environments (dev, staging, prod) and with 2 origins and we calculate 2 health check requests / origin and each packet is ~1500 bytes, then the cost of ingesting these "pointless" log entries every month is

12 (# of applications) * 3 (# of environments) * 2 (# of origins) * 2 (# of requests/second) * 1500 (size of the log entry in bytes) = **$260.7 / month**

#### Exclusion filter

```
resource.type="k8s_container"
textPayload:"Amazon-Route53-Health-Check-Service"
```
