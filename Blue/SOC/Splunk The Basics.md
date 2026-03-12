---
date: 2026-03-10
tags:
  - defensive-security
  - splunk
  - siem
---
# Splunk Components
---
![](../../PNG%20PDF%20JPEG/attachment-03102026.png)

## Splunk Forwarder
This is essentially Splunk's agent that is installed on an endpoint to collect data and send it back to the Splunk instance.

The forwarder collects the data from log sources and sends it to the Splunk Indexer.
## Splunk Indexer
This processes data it receives from the forwarders. It parses and normalizes data into field-value pairs, categorizes it, and stores the results as events.

The data, normalized by the Indexer, can be searched using the Search Head.
## Splunk Search Head
Users can search indexed logs using SPL (Search Processing Language). A search request is sent to the indexer and the relevant events are returned as field-value pairs.

# Adding Data
---
Data source categories that Splunk can ingest:
![](../../PNG%20PDF%20JPEG/attachment-03102026-1.png)
