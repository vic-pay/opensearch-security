# Opensearch SIGMA rules

This page contains basics of usage SIGMA rules in Opensearch

## Rule sample

Create `Log type` for sample rule:

1) Log in to Opensearch Dashboards (https://localhost:5601 if you use docker-compose from this repo)
2) Folow **OpenSearch Plugins -> Security Analytics -> Detectors -> Log types**
3) Push **Create log type** and fill parameters:

        Name: test-log-type
        Category: Other

Create `Detector`:

1) Folow **OpenSearch Plugins -> Security Analytics -> Detectors**
2) Push **Create Detector** and fill parameters:

        Name: test-detector
        Data source: test-index
        Log type: Test Log Type

Create `Detection rule`:

1) Folow **OpenSearch Plugins -> Security Analytics -> Detectors -> Detection rules**
2) Push **Create detection rule**
3) Push **YAML Editor** and use code:

        id: Lf2CTY4BNQromJYse0fm
            logsource:
            product: test-log-type
            title: test rule
            description: test rule
            tags: []
            falsepositives: []
            level: high
            status: experimental
            references: []
            author: username
            detection:
            condition: Selection_1
            Selection_1:
                message|contains:
                - test

## Verification

Create index with document:

1) Folow **Management -> Dev Tools**
2) Run request:

```
POST test-index/_doc/
{
  "@timestamp": "2024-04-05T12:19:00+03",
  "message": "test message",
  "logsource": {
    "product": "test"
  }
}
```

Check alerts: Folow **OpenSearch Plugins -> Security Analytics**