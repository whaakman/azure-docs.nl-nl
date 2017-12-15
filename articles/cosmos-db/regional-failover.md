---
title: Regionale failover in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe handmatige en automatische failover werkt met Azure Cosmos DB.
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 446e2580-ff49-4485-8e53-ae34e08d997f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3a8b32440ce3ec6cd2da7aaccf218a94e0ee3e77
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Automatische regionale failover voor bedrijfscontinuïteit in Azure Cosmos-DB
Azure Cosmos DB vereenvoudigt de algemene distributie van gegevens door het aanbieden van volledig worden beheerd, [meerdere landen/regio database accounts](distribute-data-globally.md) die wissen balans vinden tussen de consistentie, beschikbaarheid en prestaties, met bijbehorende garanties bieden. Cosmos DB accounts bieden hoge beschikbaarheid, één cijfer ms latenties, [goed gedefinieerde consistentieniveaus](consistency-levels.md), transparante regionale failover met multihoming-API's en de mogelijkheid om te schalen doorvoer en opslag overal ter wereld. 

Cosmos DB ondersteunt zowel expliciete en failovers aangestuurd beleid waarmee u kunt om het systeemgedrag van de end-to-end-in geval van fouten. In dit artikel kijken we:

* Hoe werken handmatige failover in Cosmos-database?
* Hoe automatische failovers werk in Cosmos-DB en wat er gebeurt wanneer een data center gaan omlaag?
* Hoe kunt u een handmatige failover in toepassingsarchitecturen?

U kunt ook meer informatie over regionale failovers in deze Azure vrijdag video met Scott Hanselman en Principal Engineering Manager Karthik Raman.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

## <a id="ConfigureMultiRegionApplications"></a>Meerdere landen/regio-toepassingen configureren
Voordat we Duik in de failover-modi, kijken we hoe u een toepassing te profiteren van beschikbaarheid in meerdere regio's en netwerkfouten met betrekking tot regionale failovers kunt configureren.

* Implementeer eerst uw toepassing in meerdere regio 's
* Om ervoor te zorgen lage latentie toegang vanaf elke regio uw toepassing wordt geïmplementeerd, configureert u de bijbehorende [lijst met aanbevolen regio's](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) voor elke regio via een van de ondersteunde SDK's.

Het volgende fragment toont hoe u een toepassing met meerdere regio initialiseren. Hier, kan de accountnaam Azure Cosmos DB `contoso.documents.azure.com` is geconfigureerd met twee gebieden - VS-West en Noord-Europa. 

* De toepassing wordt geïmplementeerd in de regio VS-West is (bijvoorbeeld Azure App Services met) 
* Met geconfigureerd `West US` leest als de eerste voorkeursregio voor lage latentie
* Met geconfigureerd `North Europe` als de tweede voorkeursregio (voor hoge beschikbaarheid tijdens regionale fouten)

Deze configuratie ziet het volgende fragment in de SQL-API:

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
    "memf7qfF89n6KL9vcb7rIQl6tfgZsRt5gY5dh3BIjesarJanYIcg2Edn9uPOUIVwgkAugOb2zUdCR2h0PTtMrA==",
    usConnectionPolicy);
```

De toepassing ook geïmplementeerd in de regio Noord-Europa met de volgorde van voorkeur gebieden omgekeerd. Dat wil zeggen, is de regio Noord-Europa eerst opgegeven voor leesbewerkingen lage latentie. Vervolgens, de regio VS-West is opgegeven als de tweede voorkeursregio voor hoge beschikbaarheid tijdens regionale fouten.

De volgende Architectuurdiagram toont een implementatie voor meerdere landen/regio waar Cosmos-database en de toepassing beschikbaar zijn in vier Azure geografische regio's zijn geconfigureerd.  

![Globaal gedistribueerde toepassingsimplementatie met Azure Cosmos-DB](./media/regional-failover/app-deployment.png)

Nu gaan we kijken hoe regionale fouten via automatische failovers worden verwerkt in de database van de Cosmos-service. 

## <a id="AutomaticFailovers"></a>Automatische Failovers
In het zeldzame geval van een Azure regionale uitval of datacentrum Cosmos DB automatisch geactiveerd failovers van alle Cosmos-DB-accounts met een aanwezigheid in de desbetreffende regio. 

**Wat gebeurt er als een lezen regio een storing heeft?**

Cosmos DB accounts met een regio lezen in een van de betrokken regio's worden automatisch losgekoppeld van hun regio schrijven en gemarkeerd als offline. De Cosmos-DB SDK's implementeren een regionale discovery-protocol waarmee ze automatisch worden gedetecteerd wanneer een regio beschikbaar is en omleiding aanroepen naar de volgende beschikbare regio in de lijst voorkeursregio lezen. Als geen van de regio's in de lijst voorkeursregio beschikbaar is, terugvallen gesprekken automatisch op het huidige schrijven gebied. Er zijn geen wijzigingen vereist zijn in uw toepassingscode voor het afhandelen van regionale failover. Tijdens dit proces gehele blijven consistentie wordt gegarandeerd worden herkend door het Cosmos-DB.

![Lees regio fouten in Azure Cosmos-DB](./media/regional-failover/read-region-failures.png)

Wanneer de desbetreffende regio vanuit de onderbreking herstelt, worden alle betrokken Cosmos-DB-accounts in de regio automatisch hersteld door de service. Cosmos DB-accounts die een lees regio in de desbetreffende regio had wordt vervolgens automatisch synchroniseren met de huidige schrijven gebied en online inschakelen. De Cosmos-DB SDK's detecteren van de beschikbaarheid van de nieuwe regio en beoordelen of de regio als de huidige lezen regio op basis van de lijst voorkeursregio die zijn geconfigureerd door de toepassing moet worden geselecteerd. Opeenvolgende leesbewerkingen worden omgeleid naar de herstelde regio, zonder eventuele wijzigingen in uw toepassingscode.

**Wat gebeurt er als een regio schrijven een storing heeft?**

Als de betrokken regio de huidige schrijven regio is en automatische failover voor de Azure DB die Cosmos-account is ingeschakeld, kan de regio is automatisch gemarkeerd als offline. Vervolgens wordt een andere regio gepromoveerd als schrijven regio voor de betrokken Azure DB die Cosmos-account. U kunt automatische failover en volledig beheren de volgorde van de selectie regio voor uw Azure Cosmos DB accounts via Azure portal of [programmatisch](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Failover prioriteiten voor het Azure Cosmos-DB](./media/regional-failover/failover-priorities.png)

Tijdens automatische failovers Azure Cosmos DB automatisch gekozen voor de volgende regio voor een bepaald Azure DB die Cosmos-account op basis van de volgorde van de opgegeven prioriteit schrijven. Toepassingen kunnen gebruikmaken van de eigenschap WriteEndpoint van DocumentClient klasse voor het detecteren van de wijziging in de regio schrijven.

![Schrijffouten regio in Azure Cosmos-DB](./media/regional-failover/write-region-failures.png)

Wanneer de desbetreffende regio vanuit de onderbreking herstelt, worden alle betrokken Cosmos-DB-accounts in de regio automatisch hersteld door de service. 

* Gegevens aanwezig zijn in de vorige schrijven regio die niet is gerepliceerd om te lezen regio's tijdens de onderbreking is gepubliceerd als een conflict feed. Toepassingen kunnen lezen van de feed conflict, Verhelp de conflicten op basis van specifieke toepassingslogica en schrijft de bijgewerkte gegevens naar de Azure DB die Cosmos-account naar gelang van toepassing. 
* De vorige schrijven regio is opnieuw gemaakt als een lezen regio en weer online automatisch. 
* Kunt u lezen regio die is weer online automatisch als de regio schrijven door het uitvoeren van een handmatige failover via de Azure-portal opnieuw configureren of [programmatisch](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

Het volgende codefragment ziet u hoe conflicten verwerken nadat de betrokken regio vanuit de onderbreking herstelt.

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

Naast automatische failovers, kan de huidige regio schrijven van een opgegeven Cosmos-DB-account handmatig dynamisch worden gewijzigd op een van de bestaande lezen gebieden. Handmatige failover kan worden gestart via de Azure-portal of [programmatisch](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Handmatige failover zorg **nul gegevensverlies** en **nul beschikbaarheid** verlies en probleemloos overdracht schrijven status van de oude regio schrijven naar het nieuwe bericht voor de opgegeven Cosmos-DB-account. Net als in automatische failover de Cosmos-SDK voor Documentdb automatisch schrijven regio wijzigingen verwerkt tijdens handmatige failovers en zorgt ervoor dat oproepen automatisch omgeleid naar de nieuwe schrijven regio. Er zijn geen wijzigingen code of configuratie vereist zijn in uw toepassing voor het beheren van failovers. 

![Handmatige failover in Azure Cosmos-DB](./media/regional-failover/manual-failovers.png)

Enkele veelvoorkomende scenario's waarbij handmatige failover kan nuttig zijn, zijn:

**Ga als volgt het model klok**: als uw toepassingen voorspelbare verkeerspatronen op basis van de tijd van de dag hebt, kunt u periodiek de status schrijven wijzigen in de meeste actieve geografische regio op basis van tijd van de dag.

**Service-update**: bepaalde implementatie globaal gedistribueerde toepassing dient u mogelijk verkeer omleiden in andere regio via het traffic manager tijdens de geplande service-update. Nu de implementatie van deze toepassing kunt handmatige failover gebruiken om de status van het schrijven naar de regio waar er is het verstandig om actieve verkeer tijdens het venster van de service-update.

**Bedrijfscontinuïteit en herstel na noodgevallen (BCDR) en High Availability and Disaster Recovery (HADR) zoomt**: bedrijfstoepassingen in de meeste zakelijke continuïteit tests opnemen als onderdeel van het proces voor ontwikkeling en release. BCDR- en HADR testen is vaak een belangrijke stap in de naleving certificeringen en aansprakelijke servicebeschikbaarheid in het geval van regionale uitval. U kunt de gereedheid van de BCDR van uw toepassingen die gebruikmaken van de Cosmos-database voor opslag door de activering van een handmatige failover van de Cosmos-DB-account en/of toevoegen en verwijderen van een regio dynamisch testen.

In dit artikel bekeken we hoe handmatige en automatische failovers werk in Cosmos-database en hoe u uw Cosmos-DB-accounts en -toepassingen wereldwijd beschikbaar kunt configureren. U kunt met behulp van de globale Replicatieondersteuning Cosmos-database, end-to-end-latentie verbeteren en ervoor te zorgen dat ze maximaal beschikbaar, zelfs in het geval van storingen regio. 

## <a id="NextSteps"></a>Volgende stappen
* Meer informatie over hoe Cosmos DB ondersteunt [globale distributie](distribute-data-globally.md)
* Meer informatie over [globale consistentie met Azure Cosmos-DB](consistency-levels.md)
* Ontwikkelen met meerdere regio's met behulp van Azure Cosmos DB [SQL-API](tutorial-global-distribution-sql-api.md)
* Informatie over het bouwen [meerdere landen/regio writer architecturen](multi-region-writers.md) met Azure Cosmos-DB

