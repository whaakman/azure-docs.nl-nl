---
title: Regionale failover in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe handmatige en automatische failover werkt met Azure Cosmos DB.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: govindk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 697be3a1eb07b2f2650f3dd94fd835b9431aec6b
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056826"
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Automatische regionale failover voor zakelijke continuïteit in Azure Cosmos DB
Azure Cosmos DB vereenvoudigt de wereldwijde distributie van gegevens door het aanbieden van volledig beheerde, [databaseaccounts voor meerdere regio's](distribute-data-globally.md) die duidelijk zorgen voor een balans tussen consistentie, beschikbaarheid en prestaties, allemaal met het bijbehorende bieden hebt u gegarandeerd. Cosmos DB-accounts bieden hoge beschikbaarheid, één cijfer ms aan latenties, [goed gedefinieerde consistentieniveaus](consistency-levels.md), transparante regionale failover met multihoming-API's en de mogelijkheid om doorvoer en opslag voor elastisch te schalen de hele wereld. 

Cosmos DB ondersteunt zowel expliciete en failovers gestuurde beleid waarmee u kunt om te bepalen het systeemgedrag van de end-to-end-in geval van problemen. In dit artikel wordt behandeld:

* Hoe werken handmatige failovers in Cosmos DB?
* Hoe automatische failovers werk in Cosmos DB en wat er gebeurt wanneer een data center gaan omlaag?
* Hoe kunt u handmatige failovers in toepassingsarchitecturen gebruiken?

U kunt ook meer informatie over regionale failovers in deze video van Azure Cosmos DB Program Manager Andrew Liu, die laat zien van de functies van de wereldwijde distributie met inbegrip van regionale failover.

>[!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

## <a id="ConfigureMultiRegionApplications"></a>Toepassingen voor meerdere regio's configureren
Voordat we Duik in failover-modi, kijken we hoe u een toepassing om te profiteren van de beschikbaarheid in meerdere regio's en tolerantie bij storingen van regionale failovers apparaten kunt configureren.

* Implementeer eerst uw toepassing in meerdere regio 's
* Om ervoor te zorgen toegang met lage latentie van elke regio's uw toepassing is geïmplementeerd, configureert u de bijbehorende [lijst met aanbevolen regio's](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) voor elke regio via een van de ondersteunde SDK's.

Het volgende codefragment laat zien hoe een toepassing met meerdere regio's worden geïnitialiseerd. Hier, de Azure Cosmos DB-account `contoso.documents.azure.com` is geconfigureerd met twee regio's: VS-West en Noord-Europa. 

* De toepassing wordt geïmplementeerd in de regio VS-West (met behulp van Azure App Services bijvoorbeeld) 
* Geconfigureerde met `West US` gelezen als de eerste voorkeursregio voor lage latentie
* Geconfigureerde met `North Europe` als de tweede voorkeursregio (voor hoge beschikbaarheid tijdens storingen in andere)

In de SQL-API, is deze configuratie ziet eruit zoals in het volgende codefragment:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "<Fill your Cosmos DB account's AuthorizationKey>",
    usConnectionPolicy);
```

De toepassing is ook geïmplementeerd in de regio Noord-Europa met de volgorde van voorkeur regio's ongedaan gemaakt. Dat wil zeggen, is de regio Noord-Europa eerst opgegeven voor leesbewerkingen met lage latentie. Vervolgens, de regio VS-West is opgegeven als de tweede voorkeursregio voor hoge beschikbaarheid tijdens regionale storingen.

De volgende Architectuurdiagram ziet u de implementatie van een toepassing in meerdere regio's waar Cosmos DB en de toepassing beschikbaar zijn in vier Azure geografische regio's zijn geconfigureerd.  

![Wereldwijd gedistribueerde toepassingsimplementatie met Azure Cosmos DB](./media/regional-failover/app-deployment.png)

Nu gaan we kijken hoe de regionale storingen via automatische failovers worden verwerkt in de Cosmos DB-service. 

## <a id="AutomaticFailovers"></a>Automatische failover
In het zeldzame geval van een Azure regionale stroomstoring of een storing in het datacenter, Cosmos DB automatisch wordt geactiveerd failovers van alle Cosmos DB-accounts met een aanwezigheid in de betreffende regio. 

**Wat gebeurt er als een leesregio een storing heeft?**

Cosmos DB-accounts met een leesgebied in een van de betreffende regio's worden automatisch losgekoppeld van de schrijfregio en is gemarkeerd als offline. De Cosmos DB SDK's implementeert een regionale discovery-protocol waarmee ze niet automatisch detecteren wanneer een regio beschikbaar is en omleidings-aanroepen naar de volgende beschikbare regio in de lijst met voorkeursregio lezen. Als geen van de regio's in de lijst met voorkeursregio beschikbaar is, terugvallen gesprekken automatisch op de huidige schrijfregio. Er zijn geen wijzigingen zijn in de code van uw toepassing vereist voor het afhandelen van regionale failovers. Tijdens dit hele proces blijven de garanties voor consistentie worden herkend door Cosmos DB.

![Lezen regio fouten in Azure Cosmos DB](./media/regional-failover/read-region-failures.png)

Wanneer de betreffende regio is hersteld van de serviceonderbreking, worden alle betrokken Cosmos DB-accounts in de regio automatisch hersteld door de service. Cosmos DB-accounts die een leesregio als in de betreffende regio wordt vervolgens automatisch worden gesynchroniseerd met de huidige schrijfregio en online inschakelen. De Cosmos DB SDK's de beschikbaarheid van de nieuwe regio detecteren en evalueren of de regio moet worden geselecteerd als de huidige leesregio die op basis van de regiolijst van de gewenste geconfigureerd door de toepassing. Opeenvolgende leesbewerkingen worden omgeleid naar de herstelde regio zonder wijzigingen in uw toepassingscode.

**Wat gebeurt er als een regio voor schrijven een storing is?**

Als het betrokken gebied de huidige schrijfregio is en automatische failover voor de Azure Cosmos DB-account is ingeschakeld, kan de regio is automatisch gemarkeerd als offline. Vervolgens wordt een andere regio gepromoveerd als de schrijfregio voor de betrokken Azure Cosmos DB-account. U kunt automatische failover inschakelen en volledige controle over de volgorde van de selectie van de regio voor uw Azure Cosmos DB-accounts via Azure portal of [programmatisch](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Failover-prioriteiten voor Azure Cosmos DB](./media/regional-failover/failover-priorities.png)

Tijdens de automatische failovers kiest Azure Cosmos DB automatisch de volgende schrijfregio's voor een bepaald Azure Cosmos DB-account op basis van de volgorde van de opgegeven prioriteit. Toepassingen kunnen de WriteEndpoint-eigenschap van klasse DocumentClient gebruiken voor het detecteren van de wijziging in de schrijfregio.

![Regio-schrijffouten in Azure Cosmos DB](./media/regional-failover/write-region-failures.png)

Wanneer de betreffende regio is hersteld van de serviceonderbreking, worden alle betrokken Cosmos DB-accounts in de regio automatisch hersteld door de service. 

* Gegevens aanwezig zijn in de vorige schrijfregio die niet is gerepliceerd als u wilt lezen regio's tijdens de storing is gepubliceerd als een conflict feed. Toepassingen kunnen de feed conflict lezen, de conflicten op basis van specifieke logica van toepassingen en de bijgewerkte gegevens teruggeschreven naar de Azure Cosmos DB-account waar nodig. 
* De vorige schrijfregio wordt opnieuw gemaakt als een leesregio en automatisch weer online gebracht. 
* U kunt de configuratie van leesregio die is weer online automatisch als de schrijfregio door het uitvoeren van een handmatige failover via Azure portal of [programmatisch](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

Het volgende codefragment laat zien hoe conflicten verwerken nadat de betreffende regio van de serviceonderbreking herstelt.

```cs
string conflictsFeedContinuationToken = null;
do
{
    FeedResponse<Conflict> conflictsFeed = client.ReadConflictFeedAsync(collectionLink,
        new FeedOptions { RequestContinuation = conflictsFeedContinuationToken }).Result;

    foreach (Conflict conflict in conflictsFeed)
    {
        Document doc = conflict.GetResource<Document>();
        Console.WriteLine("Conflict record ResourceId = {0} ResourceType= {1}", conflict.ResourceId, conflict.ResourceType);

        // Perform application specific logic to process the conflict record / resource
    }

    conflictsFeedContinuationToken = conflictsFeed.ResponseContinuation;
} while (conflictsFeedContinuationToken != null);
```

## <a id="ManualFailovers"></a>Handmatige failover

Naast de automatische failover, kunt de huidige schrijfregio van een bepaald Cosmos DB-account handmatig dynamisch worden gewijzigd op een van de bestaande leesregio's. Handmatige failover kan worden gestart via Azure portal of [programmatisch](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Handmatige failover garanderen **nul gegevensverlies** en **nul beschikbaarheid** verlies en zonder problemen de overdracht schrijven status van de oude regio schrijven naar de nieuwe vertakking voor het opgegeven Cosmos DB-account. Net als in automatische failover, de Cosmos DB SDK automatisch schrijven regio wijzigingen tijdens handmatige failovers worden verwerkt en zorgt ervoor dat de aanroepen automatisch worden omgeleid naar de nieuwe schrijfregio. Er zijn geen code of configuratiewijzigingen zijn vereist in uw toepassing voor het beheren van failovers. 

![Handmatige failover in Azure Cosmos DB](./media/regional-failover/manual-failovers.png)

Enkele van de veelvoorkomende scenario's waarbij handmatige failover kan nuttig zijn, zijn:

**Ga als volgt het model klok**: als uw toepassingen voorspelbare verkeerspatronen op basis van de tijd van de dag hebt, kunt u de status schrijven periodiek wijzigen naar de meest actieve geografische regio op basis van tijd van de dag.

**Service-update**: bepaalde implementatie van de wereldwijd gedistribueerde toepassing hebben mogelijk betrekking op het omleiden van verkeer naar andere regio via traffic manager tijdens de geplande service-update. Nu de implementatie van deze toepassing kunt handmatige failover gebruiken om de status van het schrijven naar de regio waar er is nog actief verkeer tijdens het venster van de service-update.

**Zakelijke continuïteit en herstel na noodgevallen (BCDR) en hoge beschikbaarheid en Disaster Recovery (HADR) oefeningen**: de meeste zakelijke toepassingen business continuity tests opnemen als onderdeel van hun ontwikkelings- en release-proces. BCDR en HADR testen is vaak een belangrijke stap in de nalevingscertificeringen en aansprakelijke servicebeschikbaarheid in het geval van regionale storingen. U kunt de BCDR-gereedheid van uw toepassingen die gebruikmaken van Cosmos DB voor de opslag door het activeren van een handmatige failover van uw Cosmos DB-account en/of toevoegen en verwijderen van een regio dynamisch kunt testen.

In dit artikel wordt gecontroleerd hoe handmatige en automatische failovers werk in Cosmos DB, en hoe u uw Cosmos DB-accounts en toepassingen zijn wereldwijd beschikbaar kunt configureren. U kunt met behulp van Cosmos DB globale Replicatieondersteuning, end-to-end-latentie verbeteren en ervoor te zorgen dat ze zeer beschikbaar, zelfs in het geval van fouten in de regio zijn. 

## <a id="NextSteps"></a>De volgende stappen
* Meer informatie over hoe Cosmos DB ondersteunt [wereldwijde distributie](distribute-data-globally.md)
* Meer informatie over [globale consistentie met Azure Cosmos DB](consistency-levels.md)
* Ontwikkelen met meerdere regio's met Azure Cosmos DB [SQL-API](tutorial-global-distribution-sql-api.md)
* Meer informatie over het bouwen van [architecturen voor meerdere regio's schrijver](multi-region-writers.md) met Azure Cosmos DB

