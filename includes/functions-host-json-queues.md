```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|maxPollingInterval|60000|Het maximale interval in milliseconden tussen polls van de wachtrij.| 
|visibilityTimeout|0|Het interval tussen nieuwe pogingen bij het verwerken van een bericht is mislukt.| 
|batchSize|16|Het aantal berichten op te halen en parallel worden verwerkt. Het maximum is 32.| 
|maxDequeueCount|5|Het aantal pogingen voor het verwerken van een bericht voordat u deze verplaatst naar de wachtrij verontreinigd.| 
|newBatchThreshold|batchSize/2|De drempelwaarde waarmee een nieuwe batch van berichten worden opgehaald.| 
