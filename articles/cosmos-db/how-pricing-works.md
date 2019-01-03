---
title: Prijsmodel voor Azure Cosmos DB
description: In dit artikel wordt uitgelegd dat het prijsmodel van Azure Cosmos DB en hoe deze uw kostenbeheer en kosten planning vereenvoudigt.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: rimman
ms.openlocfilehash: 5651e7ee8196eec9d1b1bf20e84ed73e484afb4b
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542388"
---
# <a name="pricing-model-of-azure-cosmos-db"></a>Prijsmodel voor Azure Cosmos DB 

Het prijsmodel van Azure Cosmos DB vereenvoudigt de kosten voor het beheer en planning. Met Azure Cosmos DB betaalt u voor de ingerichte doorvoer en de opslag die u verbruikt.

* **Ingerichte doorvoer**: Ingerichte doorvoer (ook wel gereserveerde doorvoer) hebt u gegarandeerd hoge prestaties op elke schaal. Geeft u de doorvoer (RU/s) die u nodig hebt, en worden de resources die nodig zijn om te waarborgen van de geconfigureerde doorvoer op Azure Cosmos DB. U wordt gefactureerd per uur voor de maximale ingerichte doorvoer voor een bepaald uur.

   > [!NOTE]
   > Omdat de ingerichte doorvoer-model resources naar de container of de database worden, wordt u gefactureerd voor de ingerichte doorvoer, zelfs als u niet alle workloads uitvoert.

* **Opslag verbruikt**: U wordt gefactureerd een vast tarief voor de totale hoeveelheid opslagruimte (GB) voor gegevens en de indexen voor een bepaald uur.

Ingerichte doorvoer, opgegeven als [Aanvraageenheden](request-units.md) per seconde (RU/s), kunt u lezen van of gegevens schrijven naar containers of databases. U kunt [inrichten doorvoer voor een database of een container](set-throughput.md). U kunt op basis van de behoeften van uw werkbelasting, doorvoer omhoog/omlaag schalen op elk gewenst moment. Prijzen voor Azure Cosmos DB is elastisch en staat in verhouding met de doorvoer die u op een database of een container configureert. De minimale doorvoer en opslag-waarden en de stappen van de schaal geeft een uitgebreid scala aan prijs versus elasticiteit spectrum aan alle segmenten van klanten, van aan grootschalige containers op kleine schaal. Elke database of een container wordt gefactureerd op uurbasis voor de ingerichte doorvoer in de eenheden van 100 RU/s, met een minimum van 400 RU/s, en de opslagruimte in GB. In tegenstelling tot de ingerichte doorvoer en opslag wordt in rekening gebracht op basis van verbruik. U moet dat wil zeggen, niet van tevoren geen opslag reserveren. U wordt gefactureerd alleen voor de opslag die u verbruikt.

Zie voor meer informatie de [pagina met prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) en [informatie over uw factuur voor Azure Cosmos DB](understand-your-bill.md).

Het prijsmodel in Azure Cosmos DB is consistent voor alle API's. Zie voor meer informatie, [hoe Azure Cosmos-DB prijsmodel is kostenefficiënt voor klanten](total-cost-ownership.md). Er is een minimale doorvoer nodig is om te controleren of de SLA's op een database of een container en u kunt vergroten of verkleinen van de ingerichte doorvoer door $6 voor elke 100 RU/s.

De minimale prijs voor zowel de database en de doorvoer op basis van een container is momenteel $ 24 uur per dag/maand (Zie de [pagina met prijzen van Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) voor de meest recente informatie. Als uw werkbelasting gebruikmaakt van meerdere containers, kan deze kosten worden geoptimaliseerd met behulp van de database niveau doorvoer omdat database niveau doorvoer kunt u een onbeperkt aantal containers in een database delen van de doorvoer van de containers. De volgende tabel geeft een overzicht van de ingerichte doorvoer en de kosten voor de verschillende entiteiten:

|**Entiteit**  | **Minimaal doorvoer & kosten** |**De stappen van de schaal en kosten** |**Inrichting van bereik** |
|---------|---------|---------|-------|
|Database    | 400 RU/s ($ 24 uur per dag/maand)    | 100 RU/s ($6/ maand)   |Doorvoer is gereserveerd voor de database en wordt gedeeld door containers in de database |
|Container     | 400 RU/s ($ 24 uur per dag/maand)    | 100 RU/s ($6/ maand)  |Doorvoer is gereserveerd voor een specifieke container |

Zoals weergegeven in de vorige tabel, is de minimale doorvoer in Azure Cosmos DB wordt gestart voor een prijs van $ 24 uur per dag/maand. Als u met de minimale doorvoer en de schaal begint van na verloop van tijd om uw productieworkloads te ondersteunen, worden de kosten voor de soepel, stijging van de stappen van $6/ maand. Het prijsmodel in Azure Cosmos DB is elastisch en er is een goede toename of afname in de prijs als u omhoog of omlaag schalen.

## <a name="try-azure-cosmos-db-for-free"></a>Probeer Azure Cosmos DB gratis uit 

Azure Cosmos DB biedt verschillende opties voor ontwikkelaars voor het gratis. Het gaat om deze opties:

* **Gratis Azure-account**: Azure biedt een [gratis laag](https://azure.microsoft.com/free/) waarmee u $200 aan Azure-tegoed voor de eerste 30 dagen en een beperkte hoeveelheid van gratis services voor 12 maanden. Zie [Gratis Azure-account](../billing/billing-avoid-charges-free-account.md) voor meer informatie. Azure Cosmos DB is een onderdeel van de gratis Azure-account. Specifiek voor Azure Cosmos DB biedt dit gratis account 5 GB opslagruimte en 400 ru's van de ingerichte doorvoer voor het hele jaar. 

* **Azure Cosmos DB gratis uitproberen**: Azure Cosmos DB biedt een beperkte ervaring met behulp van Azure Cosmos DB uitproberen voor gratis accounts. U kunt maken van een Azure Cosmos DB-account, -database en verzamelingen maken en uitvoeren van een voorbeeldtoepassing met behulp van de snelstartgidsen en zelfstudies. U kunt de voorbeeldtoepassing uitvoeren zonder dat u zich abonneert op een Azure-account of met behulp van uw creditcard. [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/) gedurende één maand, Azure Cosmos DB biedt de mogelijkheid om het vernieuwen van uw account een willekeurig aantal keren.

* **Azure Cosmos DB-emulator**: Azure Cosmos DB-emulator biedt een lokale omgeving waarin de Azure Cosmos DB-service voor ontwikkelingsdoeleinden worden geëmuleerd. Emulator wordt aangeboden zonder kosten en met hoge kwaliteit naar de cloudservice. Met behulp van Azure Cosmos DB-emulator gebruikt, kunt u ontwikkelen en testen van uw toepassingen lokaal op, zonder dat het maken van een Azure-abonnement of er kosten in rekening. U kunt uw toepassingen ontwikkelen met behulp van de emulator lokaal voordat u doorgaat naar de productie. Nadat u tevreden met de functionaliteit van de toepassing meer veiligheid tegen de emulator bent, kunt u overstappen naar de Azure Cosmos DB-account in de cloud en aanzienlijk besparen op kosten. Zie voor meer informatie over emulator [met behulp van Azure Cosmos DB voor ontwikkelen en testen](local-emulator.md) artikel voor meer informatie.

## <a name="pricing-with-reserved-capacity"></a>Prijzen met gereserveerde capaciteit

Azure Cosmos DB [gereserveerde capaciteit](cosmos-db-reserved-capacity.md) helpt u geld besparen door vooraf betalen voor Azure Cosmos DB-resources voor één jaar of drie jaar. U kunt aanzienlijk verminderen de kosten met een eenjarige of driejarige vooraf toezeggingen en opslaan tussen 20 en 65% kortingen in vergelijking met de reguliere prijzen. Azure Cosmos DB capaciteit kunt die u kosten verlagen door vooraf betalen voor de ingerichte doorvoer (RU/s) voor een periode van één of drie jaar gereserveerd en u krijgt een korting op de ingerichte doorvoer. 

Gereserveerde capaciteit biedt een korting van facturering en heeft geen invloed op de runtimestatus van uw Azure Cosmos DB-resources. Gereserveerde capaciteit kan consistent worden voor alle API's, inclusief MongoDB, Cassandra, SQL, Gremlin, en Azure-tabellen en alle regio's over de hele wereld. U kunt meer informatie over gereserveerde capaciteit in [Vooruitbetalen voor Azure Cosmos DB-resources met gereserveerde capaciteit](cosmos-db-reserved-capacity.md) artikel en koop gereserveerde capaciteit van de [Azure-portal](https://portal.azure.com/).

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over het optimaliseren van de kosten voor uw Azure Cosmos DB-resources in de volgende artikelen:

* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [informatie over uw factuur voor Azure Cosmos DB](understand-your-bill.md)
* Meer informatie over [doorvoer kosten optimaliseren](optimize-cost-throughput.md)
* Meer informatie over [opslagkosten te optimaliseren](optimize-cost-storage.md)
* Meer informatie over [optimaliseren van de kosten van lees- en schrijfbewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [de kosten van query's optimaliseren](optimize-cost-queries.md)
* Meer informatie over [optimaliseren van de kosten van de Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md)
* Meer informatie over [Azure Cosmos DB gereserveerde capaciteit](cosmos-db-reserved-capacity.md)
* Meer informatie over [Azure Cosmos DB-Emulator](local-emulator.md)
