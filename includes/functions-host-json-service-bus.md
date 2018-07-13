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
|maxConcurrentCalls|16|Het maximale aantal gelijktijdige oproepen aan de callback die de bericht-pomp moet starten. Standaard verwerkt de Functions-runtime meerdere berichten tegelijkertijd. Instellen om de runtime voor het verwerken van alleen een één wachtrij of onderwerp bericht op een tijdstip, `maxConcurrentCalls` op 1. | 
|prefetchCount|N.v.t.|De standaard PrefetchCount die wordt gebruikt door de onderliggende MessageReceiver.| 
|autoRenewTimeout|00:05:00|De maximale tijdsduur waarbinnen de vergrendeling van het bericht automatisch wordt vernieuwd.| 
