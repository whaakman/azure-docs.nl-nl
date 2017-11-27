```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Eigenschap  |Standaard | Beschrijving |
|---------|---------|---------| 
|maxConcurrentCalls|16|Het maximum aantal gelijktijdige oproepen aan de callback die de bericht-pomp moet starten. De runtime van Functions verwerkt standaard meerdere berichten tegelijkertijd. Stel rechtstreeks runtime slechts een enkele wachtrij of onderwerp bericht tegelijkertijd wordt verwerkt, `maxConcurrentCalls` op 1. | 
|prefetchCount|N.v.t.|De standaard PrefetchCount die wordt gebruikt door de onderliggende MessageReceiver.| 
|autoRenewTimeout|00:05:00|De maximale tijdsduur waarbinnen de vergrendeling van het bericht automatisch wordt vernieuwd.| 
