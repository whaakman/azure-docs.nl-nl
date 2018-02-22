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
|batchSize|16|Het aantal berichten in wachtrij plaatsen die de runtime van Functions tegelijkertijd opgehaald en parallel worden verwerkt. Wanneer het aantal verwerkte opgehaald omlaag naar de `newBatchThreshold`, de runtime opgehaald van een andere batch en start deze berichten worden verwerkt. Daarom het maximum aantal gelijktijdige berichten worden verwerkt per functie is `batchSize` plus `newBatchThreshold`. Deze beperking geldt afzonderlijk voor elke functie wachtrij geactiveerd. <br><br>Als u voorkomen dat parallelle uitvoering voor berichten ontvangen voor een wachtrij wilt, kunt u instellen `batchSize` op 1. Deze instelling voorkomt gelijktijdigheid echter alleen zolang de functie-app wordt uitgevoerd op een enkele virtuele machine (VM). Als de functie-app uitgeschaald naar meerdere virtuele machines, kan één exemplaar van elke functie wachtrij geactiveerd door elke virtuele machine uitvoeren.<br><br>De maximale `batchSize` is 32. | 
|maxDequeueCount|5|Het aantal pogingen voor het verwerken van een bericht voordat u deze verplaatst naar de wachtrij verontreinigd.| 
|newBatchThreshold|batchSize/2|Wanneer het aantal berichten dat gelijktijdig wordt verwerkt, mag dit nummer, wordt een andere batch in de runtime opgehaald.| 



