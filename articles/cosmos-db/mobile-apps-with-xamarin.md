---
title: Maken van mobiele toepassingen met Xamarin en Azure Cosmos DB | Microsoft Docs
description: Een zelfstudie waarmee u een Xamarin iOS, Android of formulieren maakt met behulp van Azure Cosmos DB een toepassing. Azure Cosmos DB is een snelle, schaal wereld, cloud-database voor mobiele apps.
services: cosmos-db
documentationcenter: .net
author: arramac
manager: monicar
editor: 
ms.assetid: ff97881a-b41a-499d-b7ab-4f394df0e153
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/15/2017
ms.author: arramac
ms.openlocfilehash: 34952fb1cbe5577fa00ed7799d51ba46e7173d7e
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="build-mobile-applications-with-xamarin-and-azure-cosmos-db"></a>Mobiele toepassingen met Xamarin en Azure Cosmos DB maken

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

De meeste mobiele apps moeten voor het opslaan van gegevens in de cloud en Azure Cosmos DB is een cloud-database voor mobiele apps. Heeft alles wat die een mobiele ontwikkelaar nodig heeft. Er is een volledig beheerde database als een service die wordt geschaald op aanvraag. Deze kan laat uw gegevens voor uw toepassing transparant, waar uw gebruikers overal ter wereld zich bevinden. Met behulp van de [Azure Cosmos DB .NET Core SDK](sql-api-sdk-dotnet-core.md), kunt u mobiele apps om te communiceren rechtstreeks met Azure Cosmos DB, zonder de middelste laag Xamarin inschakelen.

Dit artikel bevat een zelfstudie voor het bouwen van mobiele apps met Xamarin en Azure Cosmos DB. U vindt de volledige broncode voor de zelfstudie op [Xamarin en Azure Cosmos DB op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin), met inbegrip van het beheren van gebruikers en machtigingen.

## <a name="azure-cosmos-db-capabilities-for-mobile-apps"></a>Mogelijkheden voor mobiele apps van Azure DB Cosmos
Azure Cosmos DB biedt de volgende belangrijke mogelijkheden voor ontwikkelaars van mobiele app:

![Mogelijkheden voor mobiele apps van Azure DB Cosmos](media/mobile-apps-with-xamarin/documentdb-for-mobile.png)

* Uitgebreide query's over schemaloos gegevens. Azure Cosmos DB slaat gegevens als schemaloos JSON-documenten in heterogene verzamelingen. Dit biedt [snel en uitgebreide query's](sql-api-sql-query.md) zonder te hoeven maken over de schema's of indexen.
* De doorvoer van snelle. Het duurt slechts enkele milliseconden te lezen en schrijven van documenten met Azure Cosmos DB. Ontwikkelaars kunnen de doorvoer die ze nodig hebben, geef en Azure Cosmos DB zich houdt aan deze met SLA van 99,99% beschikbaarheid voor alle één regio en alle meerdere landen/regio-accounts met beperkte consistentie en 99,999% lezen beschikbaarheid voor alle accounts voor meerdere landen/regio-database .
* Oneindig schaal. Uw Azure Cosmos DB verzamelingen [groeien wanneer uw app groeit](partition-data.md). U kunt beginnen met kleine gegevenssets en de doorvoer van honderden aanvragen per seconde. Uw verzamelingen kunnen worden uitgebreid voor petabytes aan gegevens en willekeurig grote doorvoer met honderden miljoenen aanvragen per seconde.
* Globaal gedistribueerd. Gebruikers van mobiele app zijn onderweg, vaak over de hele wereld. Azure Cosmos-database is een [globaal gedistribueerde database](distribute-data-globally.md). Klik op de kaart om uw gegevens toegankelijk is voor uw gebruikers.
* Ingebouwde uitgebreide autorisatie. Met Azure Cosmos DB, kunt u eenvoudig populaire patronen zoals implementeren [gegevens per gebruiker](https://aka.ms/documentdb-xamarin-todouser) of gegevens, zonder complexe aangepaste autorisatie-code voor meerdere gebruikers worden gedeeld.
* Georuimtelijke query's. Veel mobiele apps bieden vandaag geo-contextafhankelijke ervaringen. Met uitstekende ondersteuning voor [georuimtelijke typen](geospatial.md), Azure Cosmos DB maakt het creëren van deze ervaringen eenvoudig om uit te voeren.
* Binaire bijlagen. Uw appgegevens bevatten vaak binaire blobs. Systeemeigen ondersteuning voor bijlagen vergemakkelijkt het gebruiken van Azure DB die Cosmos als één loket voor uw app-gegevens.

## <a name="azure-cosmos-db-and-xamarin-tutorial"></a>Azure DB Cosmos en Xamarin-zelfstudie
De volgende zelfstudie laat zien hoe een mobiele toepassing bouwen met Xamarin en Azure Cosmos DB. U vindt de volledige broncode voor de zelfstudie op [Xamarin en Azure Cosmos DB op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).

### <a name="get-started"></a>Aan de slag
Het is gemakkelijk aan de slag met Azure Cosmos DB. Ga naar de Azure-portal en maak een nieuwe Azure DB die Cosmos-account. Klik op de **snel starten** tabblad. De lijst-voorbeeld voor Xamarin Forms taak al met uw Azure DB die Cosmos-account verbonden is downloaden. 

![Azure Cosmos DB snel starten voor mobiele apps](media/mobile-apps-with-xamarin/cosmos-db-quickstart.png)

Of als u een bestaande Xamarin-app hebt, kunt u toevoegen de [Azure Cosmos DB NuGet-pakket](sql-api-sdk-dotnet-core.md). Azure Cosmos DB ondersteunt Xamarin.IOS, Xamarin.Android, en Xamarin Forms gedeelde bibliotheken.

### <a name="work-with-data"></a>Werken met gegevens
De records van uw gegevens worden opgeslagen in Azure Cosmos DB als schemaloos JSON-documenten in heterogene verzamelingen. U kunt documenten met verschillende structuren opslaan in dezelfde verzameling:

```cs
    var result = await client.CreateDocumentAsync(collectionLink, todoItem);
```

U kunt geïntegreerde language-query's in uw Xamarin-projecten gebruiken via schemaloos gegevens:

```cs
    var query = await client.CreateDocumentQuery<ToDoItem>(collectionLink)
                    .Where(todoItem => todoItem.Complete == false)
                    .AsDocumentQuery();

    Items = new List<TodoItem>();
    while (query.HasMoreResults) {
        Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
    }
```
### <a name="add-users"></a>Gebruikers toevoegen
Net als veel gestart voorbeelden ophalen, verifieert het Azure Cosmos DB voorbeeld die u hebt gedownload met de service met behulp van een hardcoded hoofdsleutel in de app-code. Deze standaard is niet verstandig voor een app die u van plan bent om uit te voeren overal, behalve op uw lokale emulator. Als een onbevoegde gebruiker die is verkregen van de hoofdsleutel, kan de gegevens over uw Azure DB die Cosmos-account worden geschonden. In plaats daarvan wilt u uw app voor toegang tot alleen de records voor de aangemelde gebruiker. Azure Cosmos DB kunnen ontwikkelaars verleent u lees toepassing of de machtiging lezen/schrijven naar een verzameling, een verzameling van documenten die zijn gegroepeerd op een partitiesleutel, of een bepaald document. 

Volg deze stappen voor het wijzigen van de taak lijst app naar een lijst met meerdere gebruikers taak app: 

  1. Aanmelding toevoegen aan uw app via Facebook, Active Directory of een andere provider.

  2. Maken van een verzameling Azure Cosmos DB UserItems met **/userId** als de partitiesleutel. De partitiesleutel voor uw verzameling opgeven, kunt Azure Cosmos DB schalen oneindig als het nummer van de gebruikers van uw app groeit, terwijl u snel query's bieden.

  3. Azure Cosmos DB Resource Token Broker toevoegen. Deze eenvoudige Web-API verifieert gebruikers en problemen met tijdelijke tokens voor aangemelde gebruikers met alleen toegang tot de documenten in hun partitie. In dit voorbeeld wordt Resource Token Broker gehost in App Service.

  4. Wijzigen van de app voor de verificatie bij Resource Token Broker met Facebook en aanvragen van de resource-tokens voor de Facebook aangemelde gebruikers. U kunt vervolgens toegang tot hun gegevens in de verzameling UserItems.  

U vindt een steekproef van de volledige code van dit patroon op [Resource Token Broker op GitHub](http://aka.ms/documentdb-xamarin-todouser). Dit diagram ziet u de oplossing:

![Azure DB Cosmos-broker voor gebruikers en machtigingen](media/mobile-apps-with-xamarin/documentdb-resource-token-broker.png)

Als u twee gebruikers toegang hebben tot de dezelfde takenlijst wilt, kunt u extra machtigingen toevoegen aan het toegangstoken in Resource Token Broker.

### <a name="scale-on-demand"></a>Schalen op aanvraag
Azure Cosmos-database is een beheerde database als een service. Wanneer uw gebruikersgroep groeit, hoeft u niet te hoeven maken over het inrichten van virtuele machines of kernen verhogen. U vertelt Azure Cosmos DB hoeft u uw app moet het aantal bewerkingen per seconde (doorvoer). U kunt opgeven dat de doorvoer via de **Scale** tabblad met behulp van een meting van doorvoer aangeroepen (RUs) Aanvraageenheden per seconde. Bijvoorbeeld, een leesbewerking op een document van 1 KB vereist 1 RU. U kunt ook waarschuwingen toevoegen de **doorvoer** metrische gegevens om te controleren van de groei van verkeer en de doorvoer fire waarschuwingen via programmacode wijzigen.

![Azure DB Cosmos scale doorvoer op aanvraag](media/mobile-apps-with-xamarin/cosmos-db-xamarin-scale.png)

### <a name="go-planet-scale"></a>Ga wereld schaal
Als uw app toeneemt populariteit, kunt u gebruikers toegang overal ter wereld. Of misschien wilt u voor onvoorziene gebeurtenissen worden voorbereid. Ga naar de Azure-portal en open uw Azure DB die Cosmos-account. Klik op de kaart om uw gegevens continu repliceren naar een willekeurig aantal regio's over de hele wereld. Hierdoor kunt u uw gegevens beschikbaar waar uw gebruikers zijn. U kunt ook de failover-beleid worden voorbereid voor diverse risico's toevoegen.

![Azure DB Cosmos schaal tussen de geografische regio 's](media/mobile-apps-with-xamarin/cosmos-db-xamarin-replicate.png)

Gefeliciteerd! U de oplossing hebt voltooid en een mobiele App met Xamarin en Azure Cosmos DB. Uitvoeren van gelijksoortige stappen Cordova-apps bouwen met behulp van de Azure Cosmos DB JavaScript SDK en een systeemeigen iOS/Android-apps met behulp van Azure Cosmos DB REST API's.

## <a name="next-steps"></a>Volgende stappen
* Bekijk de broncode voor [Xamarin en Azure Cosmos DB op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin).
* Download de [Azure Cosmos DB .NET Core SDK](sql-api-sdk-dotnet-core.md).
* Meer voorbeelden van code voor vinden [.NET-toepassingen](sql-api-dotnet-samples.md).
* Meer informatie over [Azure Cosmos DB uitgebreide mogelijkheden query](sql-api-sql-query.md).
* Meer informatie over [georuimtelijke ondersteuning in Azure Cosmos DB](geospatial.md).



