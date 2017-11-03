De volgende tabel geeft een lijst van quota en limieten specifiek voor [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Zie voor meer informatie over prijzen van Event Hubs [prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limiet | Bereik | Type | Gedrag wanneer overschreden | Waarde |
| --- | --- | --- | --- | --- |
| Nummer van event hubs per naamruimte |naamruimte |Statisch |De volgende aanvragen voor het maken van nieuwe event hub worden geweigerd. |10 |
| Aantal partities per event hub |Entiteit |Statisch |- |32 |
| Aantal consumergroepen per event hub |Entiteit |Statisch |- |20 |
| Aantal AMQP-verbindingen per naamruimte |naamruimte |Statisch |De volgende aanvragen voor aanvullende verbindingen wordt geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |5,000 |
| Maximale grootte van Event Hubs-gebeurtenis|Systeeminstellingen |Statisch |- |256 kB |
| Maximale grootte van de naam van een event hub |Entiteit |Statisch |- |50 tekens |
| Het aantal niet-epoche ontvangers per klantengroep |Entiteit |Statisch |- |5 |
| Maximale bewaarperiode van gebeurtenisgegevens |Entiteit |Statisch |- |1-7 dagen |
| Maximum aantal Throughput Units |naamruimte |Statisch |De limiet is overschreden doorvoer eenheid zorgt ervoor dat uw gegevens worden beperkt en genereert een  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. U kunt een groter aantal doorvoereenheden voor een standaard aanvragen laag met indiening een [ondersteuningsaanvraag](/azure/azure-supportability/how-to-create-azure-support-request). [Aanvullende doorvoereenheden](../articles/event-hubs/event-hubs-auto-inflate.md) zijn beschikbaar in blokken van 20 op basis van de vastgelegde aankoop. |20 |
| Aantal autorisatieregels per naamruimte |naamruimte|Statisch |De volgende aanvragen voor het maken van de regel autorisatie worden geweigerd.|12 |
