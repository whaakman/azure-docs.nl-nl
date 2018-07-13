```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|maxBatchSize|64|Het maximale aantal ontvangen gebeurtenissen per ontvangen lus.|
|prefetchCount|N.v.t.|De standaard PrefetchCount die wordt gebruikt door de onderliggende EventProcessorHost.| 
|batchCheckpointFrequency|1|Het aantal batches van gebeurtenissen te verwerken voordat u een controlepunt van de cursor Event hub maakt.| 
