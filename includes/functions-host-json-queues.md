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
|maxPollingInterval|60000|Het maximale interval in milliseconden tussen de wachtrij worden opgevraagd.| 
|visibilityTimeout|0|Het tijdsinterval tussen nieuwe pogingen bij het verwerken van een bericht is mislukt.| 
|batchSize|16|Het aantal Wachtrijberichten waarmee de Functions-runtime gelijktijdig worden opgehaald en parallel worden verwerkt. Als het getal dat wordt verwerkt opgehaald omlaag naar de `newBatchThreshold`, de runtime opgehaald van een andere batch en start deze berichten worden verwerkt. Zodat het maximum aantal gelijktijdige berichten worden verwerkt per functie `batchSize` plus `newBatchThreshold`. Deze limiet geldt afzonderlijk voor elke wachtrij-geactiveerde functie. <br><br>Als u voorkomen dat de parallelle uitvoering voor berichten in een wachtrij ontvangen wilt, kunt u instellen `batchSize` op 1. Deze instelling elimineert echter gelijktijdigheid alleen zo lang uw functie-app wordt uitgevoerd op een enkele virtuele machine (VM). Als de functie-app wordt geschaald naar meerdere virtuele machines, kan één exemplaar van elke wachtrij-geactiveerde functie uitvoeren in elke virtuele machine.<br><br>De maximale `batchSize` is 32. | 
|maxDequeueCount|5|Het aantal keren dat probeert een bericht verwerken voordat u deze verplaatst naar de wachtrij onverwerkbare.| 
|newBatchThreshold|batchSize/2|Wanneer het aantal berichten gelijktijdig worden verwerkt met deze eigenschap omlaag naar het volgende nummer, wordt een andere batch in de runtime opgehaald.| 



