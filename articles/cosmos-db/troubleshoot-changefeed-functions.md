---
title: Problemen vaststellen en oplossen van problemen bij het gebruik van Azure Cosmos DB-Trigger in Azure Functions
description: Veelvoorkomende problemen en tijdelijke oplossingen diagnostische stappen bij het gebruik van de Azure Cosmos DB-Trigger met Azure Functions
author: ealsur
ms.service: cosmos-db
ms.date: 04/16/2019
ms.author: maquaran
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: e8f0b9c8bf1bfb846f13306f58bcb1721ed6b422
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510526"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-trigger-in-azure-functions"></a>Problemen vaststellen en oplossen van problemen bij het gebruik van Azure Cosmos DB-Trigger in Azure Functions

In dit artikel bevat informatie over veelvoorkomende problemen en tijdelijke oplossingen diagnostische stappen wanneer u de [Azure Cosmos DB-Trigger](change-feed-functions.md) met Azure Functions.

## <a name="dependencies"></a>Afhankelijkheden

De Azure Cosmos DB-Trigger en bindingen afhankelijk van de extensiepakketten via de basis Azure Functions-runtime. Bewaar deze pakketten bijgewerkt, omdat ze mogelijk oplossingen en nieuwe functies waarmee alle mogelijke problemen die kunnen optreden kunnen bevatten:

* Zie voor Azure Functions V2 [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB).
* Zie voor Azure Functions V1 [Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

In dit artikel wordt altijd verwijzen naar Azure Functions V2 wanneer de runtime wordt vermeld, tenzij expliciet worden opgegeven.

## <a name="consume-the-azure-cosmos-db-sdk-independently"></a>De Azure Cosmos DB SDK onafhankelijk gebruiken

De belangrijkste functionaliteit van het pakket met de uitbreiding is om ondersteuning te bieden voor de Azure Cosmos DB-trigger en bindingen. Dit omvat ook de [Azure Cosmos DB .NET SDK](sql-api-sdk-dotnet-core.md), dit is handig als u werken met Azure Cosmos DB via een programma wilt zonder gebruik te maken van de trigger en bindingen.

Als wilt gebruiken de Azure Cosmos DB SDK, zorg ervoor dat u een andere NuGet-pakketverwijzing niet aan uw project toevoegen. In plaats daarvan **laat de SDK-verwijzing oplossen via de Azure Functions-uitbreidingspakket**. De Azure Cosmos DB SDK los van de trigger en bindingen gebruiken

Bovendien, als u handmatig het maken van uw eigen exemplaar van de [Azure Cosmos DB SDK-client](./sql-api-sdk-dotnet-core.md), u moet volgen het patroon dat u hoeft slechts één exemplaar van de client [met behulp van een Singleton-patroon benadering](../azure-functions/manage-connections.md#documentclient-code-example-c) . Dit proces zal voorkomen dat de mogelijke socket problemen in uw activiteiten.

## <a name="common-scenarios-and-workarounds"></a>Algemene scenario's en oplossingen

### <a name="azure-function-fails-with-error-message-collection-doesnt-exist"></a>Azure functie mislukt met fout bericht verzameling bestaat niet

Azure-functie is mislukt met foutbericht "een van beide de bronverzameling 'verzameling-naam' (in database 'database-naam') of de leaseverzameling 'verzameling2-naam' (in database 'database2-naam') bestaat niet. Beide verzamelingen moeten bestaan voordat de listener wordt gestart. Stel 'CreateLeaseCollectionIfNotExists' op 'true' voor het automatisch maken van de leaseverzameling "

Dit betekent dat een of beide van de Azure Cosmos-containers die vereist zijn voor de trigger werkt niet bestaan of niet bereikbaar zijn voor de Azure-functie zijn. **De fout zich laat u weten welke Azure-Cosmos-database en containers is de trigger op zoek naar** op basis van uw configuratie.

1. Controleer of de `ConnectionStringSetting` kenmerk en dat deze **verwijst naar een instelling die deel uitmaakt van uw Azure-functie-App**. De waarde van dit kenmerk mag niet de Connection String zelf, maar de naam van de configuratie-instelling.
2. Controleer de `databaseName` en `collectionName` aanwezig zijn in uw Azure Cosmos-account. Als u automatische waarde vervanging (met behulp van `%settingName%` patronen), Controleer of de naam van de instelling bestaat in uw Azure-functie-App.
3. Als u geen opgeeft een `LeaseCollectionName/leaseCollectionName`, de standaardwaarde is 'leases'. Controleer of deze container bestaat. Eventueel kunt u instellen de `CreateLeaseCollectionIfNotExists` kenmerk in de Trigger op `true` deze automatisch te maken.
4. Controleer of uw [firewallconfiguratie voor Azure Cosmos-account](how-to-configure-firewall.md) om te zien om te zien dat het niet is het niet wordt geblokkeerd door de Azure-functie.

### <a name="azure-function-fails-to-start-with-shared-throughput-collection-should-have-a-partition-key"></a>Azure-functie mislukt te beginnen met "gedeelde doorvoer verzameling moet een partitiesleutel"

De vorige versies van de Azure Cosmos DB-extensie niet ondersteunt met behulp van een lease-container die is gemaakt binnen een [gedeelde doorvoer database](./set-throughput.md#set-throughput-on-a-database). U lost dit probleem, werken de [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) uitbreiding van de meest recente versie.

### <a name="azure-function-fails-to-start-with-the-lease-collection-if-partitioned-must-have-partition-key-equal-to-id"></a>Azure-functie mislukt te beginnen met "de leaseverzameling als gepartitioneerd, moet hebben gelijk zijn aan id partitiesleutel."

Deze fout betekent dat uw huidige lease-container is gepartitioneerd, maar het pad van de partitie niet is `/id`. U lost dit probleem, moet u opnieuw maken van de lease-container met `/id` als de partitiesleutel.

### <a name="you-see-a-value-cannot-be-null-parameter-name-o-in-your-azure-functions-logs-when-you-try-to-run-the-trigger"></a>U ziet een 'waarde mag niet null zijn. Parameternaam: o ' in de logboeken van uw Azure Functions wanneer u probeert om uit te voeren van de Trigger

Dit probleem treedt op als u de Azure-portal gebruikt en u probeert te selecteren de **uitvoeren** knop op het scherm bij de inspectie van een Azure-functie die de trigger gebruikt. De trigger is niet vereist voor uitvoeren om te starten, selecteert u deze automatisch gestart wanneer de Azure-functie wordt geïmplementeerd. Als u wilt controleren van de Azure-functie logboekstream in Azure portal, net gaat u naar de bewaakte container en enkele nieuwe items in te voegen, wordt automatisch ziet u de Trigger wordt uitgevoerd.

### <a name="my-changes-take-too-long-be-received"></a>Mijn wijzigingen duurt te lang worden ontvangen

In dit scenario kan verschillende oorzaken hebben, en ze allemaal moeten worden gecontroleerd:

1. Is uw Azure-functie geïmplementeerd in dezelfde regio als uw Azure Cosmos-account? Voor optimale netwerklatentie, moeten zowel de Azure-functie als uw Azure Cosmos-account in dezelfde Azure-regio worden geplaatst.
2. De wijzigingen die plaatsvinden in uw Azure Cosmos-container continue of er kunnen sporadisch?
Als dat het laatste geval is, kan er enige vertraging tussen de wijzigingen worden opgeslagen en de Azure-functie die ze ophalen. Dit komt doordat intern, wanneer de trigger wordt gecontroleerd op wijzigingen in uw Azure Cosmos-container en wordt geen in behandeling gevonden om te lezen, er wordt naar de slaapstand gedurende een configureerbare tijd (standaard 5 seconden) voordat het controleren op nieuwe wijzigingen (om te voorkomen dat hoge RU-verbruik). U kunt configureren dat dit moment overschakelen naar de slaapstand via de `FeedPollDelay/feedPollDelay` instellen in de [configuratie](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) van de trigger (verwachting is de waarde in milliseconden).
3. Uw Azure Cosmos-container wellicht [rate-limited](./request-units.md).
4. U kunt de `PreferredLocations` kenmerk in de trigger om op te geven van een door komma's gescheiden lijst van Azure-regio's voor het definiëren van een aangepaste verbinding van de gewenste volgorde.

### <a name="some-changes-are-missing-in-my-trigger"></a>Er ontbreken enkele wijzigingen in mijn Trigger

Als u dat sommige van de wijzigingen die hebben plaatsgevonden in uw Azure Cosmos-container niet worden opgehaald door de Azure-functie hebt gevonden, is er een eerste onderzoek stap die moet worden uitgevoerd.

Wanneer uw Azure-functie ontvangt de wijzigingen, het vaak ze worden verwerkt, en kan eventueel, wordt het resultaat verzenden naar een andere bestemming. Wanneer u ontbrekende wijzigingen onderzoekt, zorg ervoor dat u **meting welke wijzigingen worden ontvangen op het punt** (wanneer de Azure Function begint), niet op de doelcomputer.

Als sommige wijzigingen op de doelcomputer ontbreken, kan dit betekenen dat is een fout heeft voorgedaan tijdens de uitvoering van een Azure-functie nadat de wijzigingen zijn ontvangen.

In dit scenario, de beste loop van de actie is het toevoegen van `try/catch blocks` in uw code en binnen de lussen die kunnen worden verwerkt de wijzigingen, voor het detecteren van een storing voor een bepaalde subset met objecten en deze dienovereenkomstig te verwerken (verzend dit naar een andere opslag voor verdere Probeer het opnieuw of analyse). 

> [!NOTE]
> De Azure Cosmos DB-Trigger wordt niet standaard opnieuw een batch van wijzigingen Als er een onverwerkte uitzondering opgetreden tijdens de uitvoering van uw code is. Dit betekent dat de reden dat de wijzigingen niet op de bestemming ontvangen is omdat dat u niet kunnen ze verwerken.

Als u merkt dat sommige wijzigingen zijn niet helemaal ontvangen door de trigger, de meest voorkomende scenario is dat er wordt **die wordt uitgevoerd met een andere Azure-functie**. Dit kan een andere Azure-functie geïmplementeerd in Azure of een Azure-functie lokaal op worden uitgevoerd van een ontwikkelaar van de computer die is **exact dezelfde configuratie** (hetzelfde bewaakt en containers van de lease), en het stelen van deze Azure-functie een een subset van de te uw Azure-functie verwachten voor het verwerken van wijzigingen.

Bovendien kan worden gevalideerd op het scenario, als u weet hoeveel exemplaren van de Azure Function-App die u hebt uitgevoerd. Als u uw container leases inspecteren en het aantal items van de lease binnen de afzonderlijke waarden van telt de `Owner` eigenschap erin moet gelijk zijn aan het aantal exemplaren van uw functie-App. Als er meer eigenaren dan de bekende Azure Function-App-instanties, betekent dit dat deze extra eigenaren van een 'stelen' van de wijzigingen zijn.

Een eenvoudige manier om tijdelijke oplossing deze situatie is om toe te passen een `LeaseCollectionPrefix/leaseCollectionPrefix` naar de functie met een nieuwe/andere waarde of u kunt ook testen met een nieuwe container van de leases.

## <a name="next-steps"></a>Volgende stappen

* [Schakel de bewaking voor uw Azure-functies](../azure-functions/functions-monitoring.md)
* [Azure Cosmos DB .NET SDK oplossen](./troubleshoot-dot-net-sdk.md)