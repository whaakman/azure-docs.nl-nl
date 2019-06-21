---
title: Het plannen van indexeerfuncties - Azure Search
description: Azure Search-indexeerfuncties naar inhoud indexeren plannen periodiek of op specifieke tijdstippen.
ms.date: 05/31/2019
author: RobDixon22
manager: HeidiSteen
ms.author: v-rodixo
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 4bf931b19b7490a94f30afde49038cdc7573fab3
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67302250"
---
# <a name="how-to-schedule-indexers-for-azure-search"></a>Het plannen van indexeerfuncties voor Azure Search
Een indexeerfunctie normaal gesproken één keer uitgevoerd, onmiddellijk nadat deze is gemaakt. U kunt het opnieuw uitvoeren op aanvraag via de portal, de REST-API of de .NET SDK. U kunt ook een indexeerfunctie periodiek wordt uitgevoerd volgens een planning configureren.

Sommige situaties waarin het plannen van de indexeerfunctie handig is:

* Brongegevens na verloop van tijd wordt gewijzigd en u wilt dat de Azure Search-indexeerfuncties de gewijzigde gegevens automatisch wilt verwerken.
* De index worden ingevuld uit meerdere gegevensbronnen en u wilt controleren of dat de indexeerfuncties uitvoeren op verschillende tijdstippen om te conflicten te verminderen.
* De brongegevens erg groot is en u wilt spreiden van de indexeerfunctie verwerkt na verloop van tijd. Zie voor meer informatie over het indexeren van grote hoeveelheden gegevens, [grote gegevenssets in Azure Search indexeren](search-howto-large-index.md).

De scheduler is een ingebouwde functie van Azure Search. U kunt een externe scheduler niet gebruiken voor het beheren van de search-indexeerfuncties.

## <a name="define-schedule-properties"></a>Schema-eigenschappen definiëren

De planning van een indexeerfunctie heeft twee eigenschappen:
* **Interval**, waarmee de hoeveelheid tijd tussen wordt gedefinieerd geplande uitvoeringen van de indexeerfunctie. Het kleinste interval toegestaan is 5 minuten en 24 uur van de grootste is.
* **Start Time (UTC)** , wat aangeeft dat de eerste keer waarop de indexeerfunctie moet worden uitgevoerd.

U kunt een schema tijdens het maken van de indexeerfunctie, of door het bijwerken van eigenschappen van de indexeerfunctie later opgeven. Indexeerfunctie schema's kunnen worden ingesteld met behulp van de [portal](#portal), wordt de [REST-API](#restApi), of de [.NET SDK](#dotNetSdk).

Slechts één uitvoering van een indexeerfunctie kunt uitvoeren op een tijdstip. Als een indexeerfunctie wordt al uitgevoerd wanneer de volgende uitvoering is gepland, wordt die worden uitgevoerd totdat de volgende tijd geplande uitgesteld.

Laten we eens een voorbeeld waarmee dit meer concrete. Stel dat we bij het configureren van de planning van een indexeerfunctie met een **Interval** van elk uur en een **begintijd** van 1 juni 2019 om 8:00:00 AM UTC. Dit is wat kan gebeuren als er een uitvoerbewerking van de indexeerfunctie langer duurt dan een uur:

* De eerste uitvoering van de indexeerfunctie wordt gestart op of omstreeks 1 juni 2019 om 8:00 uur UTC. Wordt ervan uitgegaan dat deze uitvoering duurt 20 minuten (of elk gewenst moment minder dan 1 uur).
* De tweede uitvoering gestart op of omstreeks 1 juni 2019 9:00 uur UTC. Stel dat deze uitvoering 70 minuten - meer dan een uur duurt – en dat deze kan niet worden voltooid tot en met 10:10:00 UTC.
* De derde uitvoering is gepland om te beginnen bij 10:00 uur UTC, maar op dat moment wordt de vorige uitvoering nog steeds uitgevoerd. Dit geplande uitvoering vervolgens is overgeslagen. De volgende uitvoering van de indexeerfunctie wordt niet gestart tot en met 11:00 uur UTC.

> [!NOTE]
> Als een indexeerfunctie is ingesteld op een bepaalde planning, maar herhaaldelijk mislukt op dezelfde document telkens opnieuw telkens wanneer deze wordt uitgevoerd, begint de indexeerfunctie wordt uitgevoerd in een interval van minder frequent optreden (tot het maximum van ten minste elke 24 uur) totdat het in voortgang aga in.  Als u denkt u alles wat het probleem is veroorzaakt door de indexeerfunctie dat te zijn vastgelopen op een bepaald moment hebt opgelost, kunt u een op aanvraag uitvoeren van de indexeerfunctie uitvoeren en als die is maakt uitgevoerd, de indexeerfunctie wordt geretourneerd naar de schema-interval instellen opnieuw.

<a name="portal"></a>

## <a name="define-a-schedule-in-the-portal"></a>Een schema definiëren in de portal

De wizard gegevens importeren in de portal kunt u het schema voor een indexeerfunctie definiëren bij het maken. De standaardinstelling voor de planning is **per uur**, wat betekent dat de indexeerfunctie wordt eenmaal uitgevoerd nadat deze is gemaakt en wordt daarna opnieuw elk uur uitgevoerd.

U kunt de instelling van het schema te wijzigen **zodra** als u niet wilt dat de indexeerfunctie opnieuw automatisch wordt uitgevoerd of **dagelijkse** om uit te voeren één keer per dag. Stel deze in op **aangepaste** als u wilt een ander interval of een specifieke toekomstige begintijd op te geven.

Als u de planning instelt op **aangepaste**, velden worden weergegeven waarin u kunt opgeven de **Interval** en de **Start Time (UTC)** . De kortste tijd toegestaan-interval 5 minuten en het langste is is 1440 minuten (24 uur).

   ![Planning van de instelling indexeerfunctie in de wizard gegevens importeren](media/search-howto-schedule-indexers/schedule-import-data.png "instelling indexeerfunctie planning in de wizard gegevens importeren")

Nadat een indexeerfunctie is gemaakt, kunt u de schema-instellingen met behulp van de indexeerfunctie bewerken Configuratiescherm kunt wijzigen. De schema-velden zijn hetzelfde als in de wizard gegevens importeren.

   ![Instellen van de planning in Configuratiescherm van indexeerfunctie bewerken](media/search-howto-schedule-indexers/schedule-edit.png "instellen van de planning in Configuratiescherm van indexeerfunctie bewerken")

<a name="restApi"></a>

## <a name="define-a-schedule-using-the-rest-api"></a>Een schema met behulp van de REST-API definiëren

U kunt het schema voor een indexeerfunctie met behulp van de REST-API definiëren. Om dit te doen, bevatten de **planning** eigenschap bij het maken of bijwerken van de indexeerfunctie. In het volgende voorbeeld toont een PUT-aanvraag voor het bijwerken van een bestaande indexeerfunctie:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

De **interval** parameter is vereist. Het interval verwijst naar de tijd tussen het begin van de twee opeenvolgende indexeerfunctie uitvoeringen. De minimaal toegestane interval is 5 minuten. het langste is één dag. Deze moet zijn opgemaakt als een "dayTimeDuration" XSD-waarde (een beperkte subset van een [duur van de ISO 8601](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) waarde). Het patroon hiervoor is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur.

De optionele **startTime** geeft aan wanneer de geplande uitvoeringen moeten beginnen. Als deze wordt weggelaten, wordt de huidige UTC-tijd gebruikt. Deze tijd kan worden in het verleden in dat geval de eerste uitvoering is gepland als wanneer de indexeerfunctie is actief continu vanaf de oorspronkelijke **startTime**.

U kunt ook een indexeerfunctie uitvoeren op aanvraag aan de hand van de aanroep van indexeerfunctie worden uitgevoerd. Zie voor meer informatie over het uitvoeren van Indexeerfuncties en indexeerfunctie planningen instellen [indexeerfunctie uitvoeren](https://docs.microsoft.com/rest/api/searchservice/run-indexer), [ophalen indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/get-indexer), en [Update indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/update-indexer) in de REST API-verwijzing.

<a name="dotNetSdk"></a>

## <a name="define-a-schedule-using-the-net-sdk"></a>Een schema met de .NET SDK definiëren

U kunt het schema voor een indexeerfunctie met behulp van de Azure Search .NET SDK definiëren. Om dit te doen, bevatten de **planning** eigenschap bij het maken of bijwerken van een indexeerfunctie.

De volgende C# voorbeeld maakt u een indexeerfunctie, met behulp van een vooraf gedefinieerde gegevensbron en index, en stelt de planning naar wens eenmaal elke dag vanaf 30 minuten vanaf nu worden uitgevoerd:

```
    Indexer indexer = new Indexer(
        name: "azure-sql-indexer",
        dataSourceName: dataSource.Name,
        targetIndexName: index.Name,
        schedule: new IndexingSchedule(
                        TimeSpan.FromDays(1), 
                        new DateTimeOffset(DateTime.UtcNow.AddMinutes(30))
                    )
        );
    await searchService.Indexers.CreateOrUpdateAsync(indexer);
```
Als de **planning** parameter wordt weggelaten, wordt de indexeerfunctie wordt slechts één keer uitgevoerd onmiddellijk nadat deze is gemaakt.

De **startTime** parameter kan worden ingesteld op een tijdstip in het verleden. In dat geval wordt de eerste uitvoering is gepland als de indexeerfunctie actief is geweest continu sinds de opgegeven **startTime**.

Het schema is gedefinieerd met behulp van de [IndexingSchedule](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingschedule?view=azure-dotnet) klasse. De **IndexingSchedule** constructor vereist een **interval** parameter die is opgegeven met behulp van een **TimeSpan** object. De toegestane intervalwaarde van kleinste is 5 minuten en 24 uur van de grootste is. De tweede **startTime** parameter, die is opgegeven als een **DateTimeOffset** object, is optioneel.

De .NET SDK kunt u bewerkingen voor indexeerfunctie besturingselement met behulp van de [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) klasse en de bijbehorende [indexeerfuncties](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexers) eigenschap, die u implementeert methoden uit de **IIndexersOperations**interface. 

U kunt een indexeerfunctie uitvoeren op aanvraag op elk gewenst moment met behulp van een van de [uitvoeren](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.run), [RunAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.runasync), of [RunWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations.runwithhttpmessagesasync) methoden.

Zie voor meer informatie over het maken, bijwerken en het uitvoeren van indexeerfuncties, [IIindexersOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations?view=azure-dotnet).
