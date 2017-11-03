Verzameling van aangepaste metingen. Deze verzameling om te rapporteren meting die is gekoppeld aan de telemetrie-item met de naam gebruiken. Er zijn typische gebruiksvoorbeelden:
- de grootte van Afhankelijkheidstelemetrie nettolading
- het aantal items van wachtrij verwerkt door telemetrie aanvragen
- tijd die klant nodig was voor het voltooien van de stap in de wizard stap voltooiing gebeurtenis telemetrie.

U kunt een query [aangepaste metingen](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in analytische gegevens van toepassing:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Aangepaste metingen zijn gekoppeld aan de telemetrie-item waartoe ze behoren. Ze zijn onderworpen aan sampling met de telemetrie-item dat u deze metingen bevat. Gebruiken voor het bijhouden van een meting met een waarde die onafhankelijk van andere typen telemetrie [metrische telemetrie](../articles/application-insights/app-insights-api-custom-events-metrics.md).

De maximale lengte: 150
