---
title: Werken met de change feed processor-bibliotheek in Azure Cosmos DB
description: Met behulp van de Azure Cosmos DB-change feed processor-bibliotheek.
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rafats
ms.openlocfilehash: 9d427a8001112e4994597b86579d85156f94a870
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629161"
---
# <a name="using-the-azure-cosmos-db-change-feed-processor-library"></a>Met behulp van de Azure Cosmos DB-change feed processor-bibliotheek

De [Azure Cosmos DB change feed processor-bibliotheek](sql-api-sdk-dotnet-changefeed.md) helpt u bij verwerking van gebeurtenissen verdelen over meerdere consumenten worden gedistribueerd. Deze bibliotheek vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken.

Het belangrijkste voordeel van change feed processor-bibliotheek is dat u hoeft te beheren van elke partitie en vervolgtoken en u hoeft op te vragen voor elke container handmatig.

De change feed processor-bibliotheek vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken. Hiermee worden beheerd automatisch lezen wijzigingen over meerdere partities met behulp van een lease-mechanisme. Zoals u in de volgende afbeelding, ziet als u twee clients die van de change feed processor-bibliotheek gebruikmaken wordt gestart, verdelen zij het werk onderling. Als u doorgaat met het verhogen van het aantal clients, houden ze het werk onderling verdelen.

![Met behulp van Azure Cosmos DB change feed processor-bibliotheek](./media/change-feed-processor/change-feed-output.png)

De linker-client eerst is gestart en het starten van de bewaking van alle partities, en vervolgens de tweede client die is gestart, en vervolgens de eerste van een aantal van de tweede client-leases laten gaan. Dit is een efficiënte manier om werk tussen clients en andere computers te distribueren.

Als u twee zonder server Azure functions dezelfde container bewaking en het gebruik van de dezelfde lease hebt, krijgt de twee functies mogelijk verschillende documenten, afhankelijk van hoe de processor-bibliotheek voor het verwerken van de partities besluit.

## <a name="implementing-the-change-feed-processor-library"></a>Implementatie van de wijziging feed processor-bibliotheek

Er zijn vier belangrijke onderdelen van de implementatie van de change feed processor-bibliotheek: 

1. **De bewaakte container:** de bewaakte container heeft de gegevens op basis waarvan de wijzigingenfeed is gegenereerd. Alle toevoegingen en wijzigingen in de bewaakte container worden weerspiegeld in de wijzigingenfeed van de container.

1. **De container lease:** de lease container coördinaten voor het verwerken van de feed over meerdere werknemers wijzigen. Een afzonderlijke container wordt gebruikt voor het opslaan van de leases met één lease per partitie. Het is nuttig voor het opslaan van deze container lease op een ander account met de schrijfregio dichter in de buurt waar de change feed processor wordt uitgevoerd. Een lease-object bevat de volgende kenmerken:

   * Eigenaar: Hiermee geeft u de host die eigenaar is van de lease.

   * Voortzetting: Geeft de positie (vervolgtoken) in de feed voor een bepaalde partitie wijzigen.

   * Tijdstempel: Laatste keer dat de lease is bijgewerkt. de tijdstempel kan worden gebruikt om te controleren of de lease is verlopen.

1. **De processor host:** elke host wordt bepaald hoeveel partities verwerken op basis van hoeveel andere instanties van hosts actieve leases hebben.

   * Wanneer een host wordt gestart, krijgt deze leases om de werkbelasting in balans tussen alle hosts. Een host wordt leases, regelmatig vernieuwd, zodat de lease actief blijven.

   * Een host-controlepunten lezen voor de laatste vervolgtoken naar de lease voor elk. Een host controleert gelijktijdigheid om veiligheid te garanderen, de ETag voor elke update van de lease. Andere strategieën controlepunt worden ook ondersteund.

   * Bij het afsluiten, een host alle leases worden vrijgegeven, maar blijft de voortzetting van informatie, zodat het lezen van de opgeslagen controlepunt later kan worden hervat.

   Het aantal hosts kan op dit moment niet groter zijn dan het aantal partities (lease).

1. **De consument:** consumenten of werknemers, worden threads die de wijzigingenfeed verwerking gestart door elke host uitvoeren. Elke host processor kan meerdere consumenten hebben. Elke consument leest de wijziging feed van de partitie die is toegewezen aan en ontvangt van de host van wijzigingen en verlopen van leases.

Om verder te begrijpen hoe deze vier onderdelen van de wijzigingenfeed processor werk samen, bekijk een voorbeeld in het volgende diagram. De bewaakte verzameling documenten worden opgeslagen en 'Plaats' gebruikt als de partitiesleutel. We zien dat de blauwe partitie enzovoort documenten met het veld 'Plaats' uit "A-E" bevat. Er zijn twee hosts, elk met twee consumenten van de vier partities parallel lezen. De pijlen geven de consumenten lezen van een specifieke positie in de feed wijzigen. In de eerste partitie vertegenwoordigt de donkerder blauw ongelezen wijzigingen terwijl de lichtblauw de wijzigingen al lezen op de wijzigingenfeed vertegenwoordigt. De hosts de leaseverzameling gebruiken voor het opslaan van een waarde 'voortzetting' om de positie van de huidige lezen voor elke consument bij te houden.

![Change feed processor-voorbeeld](./media/change-feed-processor/changefeedprocessor.png)

### <a name="change-feed-and-provisioned-throughput"></a>Wijzigingenfeed en ingerichte doorvoer

U betaalt voor ru's gebruikt, omdat de verplaatsing van gegevens in en uit Cosmos containers altijd ru's worden verbruikt. Door de container lease verbruikte ru's in rekening worden gebracht.

## <a name="additional-resources"></a>Aanvullende resources

* [Azure Cosmos DB change feed processor-bibliotheek](sql-api-sdk-dotnet-changefeed.md)
* [Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Aanvullende voorbeelden op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

## <a name="next-steps"></a>Volgende stappen

U kunt nu doorgaan naar meer informatie over de wijzigingenfeed in de volgende artikelen:

* [Overzicht van de wijzigingenfeed](change-feed.md)
* [Manieren om te lezen wijzigingenfeed](read-change-feed.md)
* [Met behulp van de change feed met Azure Functions](change-feed-functions.md)