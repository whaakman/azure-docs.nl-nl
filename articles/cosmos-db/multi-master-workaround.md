---
title: Schrijfbewerkingen in meerdere regio's en leesbewerkingen uitvoeren door het recht partitiesleutel kiezen | Microsoft Docs
description: Meer informatie over het ontwerpen van toepassingsarchitecturen met lokale lees- en schrijfbewerkingen in meerdere geografische regio's met Azure Cosmos DB met een partitiesleutel kiezen.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/6/2018
ms.author: rimman
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d38b7cd7d1f28f706e94782602926abc8fc11e3
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054444"
---
# <a name="perform-multi-region-writes-and-reads-by-choosing-the-right-partitioning-key"></a>Schrijfbewerkingen in meerdere regio's en leesbewerkingen uitvoeren door het recht partitiesleutel kiezen

Azure Cosmos DB ondersteunt kant en klaar [globale replicatie](distribute-data-globally.md), waarmee u gegevens in meerdere regio's met lage latentie toegang overal in de werkbelasting distribueren. Dit model wordt doorgaans gebruikt voor werkbelastingen van de uitgever/consumenten waarbij er een schrijver in een enkele geografische regio en wereldwijd gedistribueerde lezers in andere regio (lezen). 

U kunt ook ondersteuning voor Azure Cosmos DB globale replicatie gebruiken voor het bouwen van toepassingen waarin schrijvers en lezers wereldwijd worden gedistribueerd. Dit document bevat een overzicht van een patroon waarmee de verwezenlijking van lokale schrijven en lokale leestoegang voor gedistribueerde schrijvers met Azure Cosmos DB.

## <a id="ExampleScenario"></a>Inhoud publiceren: een voorbeeldscenario
We bekijken een werkelijk scenario te beschrijven hoe u wereldwijd gedistribueerde meerdere-region/meerdere-pre-master lezen-schrijven patronen met Azure Cosmos DB kunt gebruiken. Houd rekening met een content publishing dat is gebouwd met Azure Cosmos DB. Hier volgen enkele vereisten die dit platform moet voldoen aan voor een optimale ervaring voor zowel uitgevers en consumenten.

* Zowel auteurs en abonnees worden verdeeld over de hele wereld 
* Auteurs moeten hun eigen lokale regio (dichtstbijzijnde) (schrijven) artikelen publiceren
* Auteurs hebben lezers /-abonnees hun artikelen die zijn verdeeld over de hele wereld. 
* Abonnees moeten een melding ontvangen wanneer nieuwe artikelen worden gepubliceerd.
* Abonnees moeten in staat om artikelen te lezen uit hun eigen lokale regio zijn. Ze moeten ook worden beoordelingen toevoegen aan deze artikelen. 
* Iedereen met inbegrip van de auteur van de artikelen moet kunnen weergeven, alle beoordelingen die zijn gekoppeld aan artikelen van een lokale regio's. 

Ervan uitgaande dat miljoenen consumenten en uitgevers met miljarden artikelen, snel hebben we pakt u de problemen van schaal, samen met de locatie van de toegang te garanderen. Net als bij de meeste schaalbaarheidsproblemen, ligt de oplossing in een goede strategie voor partitioneren. Vervolgens gaan we kijken hoe u artikelen, controle en meldingen als documenten modelleren, Azure Cosmos DB-accounts configureren en implementeren van een laag voor gegevenstoegang. 

Als u wilt meer informatie over partitionering en partitiesleutels, Zie [partitioneren en schalen in Azure Cosmos DB](partition-data.md).

## <a id="ModelingNotifications"></a>Modellering van meldingen
Meldingen worden gegevens naar een gebruiker specifieke kanalen. De patronen voor databasetoegang voor meldingen documenten zijn daarom altijd in de context van één gebruiker. U zou bijvoorbeeld 'een melding naar een gebruiker boeken' of 'alle meldingen voor een bepaalde gebruiker ophalen'. Ja, de beste keuze van het partitioneren van de sleutel voor dit type zou worden `UserId`.

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // The user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // The partition Key for the resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of the notification. 
        public string ArticleId { get; set; } 
    }

## <a id="ModelingSubscriptions"></a>Modellering van abonnementen
Abonnementen kunnen worden gemaakt voor verschillende criteria, zoals een specifieke categorie van de artikelen van belang zijn, of een bepaalde uitgever. Daarom de `SubscriptionFilter` is een goede keuze voor de partitiesleutel.

    class Subscriptions 
    { 
        // Unique ID for Subscription 
        public string Id { get; set; }

        // Subscription source. Could be Author | Category etc. 
        public string SubscriptionFilter { get; set; }

        // subscribing User. 
        public string UserId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.SubscriptionFilter; 
            } 
        } 
    }

## <a id="ModelingArticles"></a>Artikelen modelleren
Wanneer een artikel wordt geïdentificeerd door meldingen, de volgende query's doorgaans zijn gebaseerd op de `Article.Id`. Kiezen `Article.Id` als partitie bevat de sleutel dus de beste distributie voor het opslaan van de artikelen in een Azure Cosmos DB-verzameling. 

    class Article 
    { 
        // Unique ID for Article 
        public string Id { get; set; }
        
        public string PartitionKey 
        { 
            get 
            { 
                return this.Id; 
            } 
        }
        
        // Author of the article
        public string Author { get; set; }

        // Category/genre of the article
        public string Category { get; set; }

        // Tags associated with the article
        public string[] Tags { get; set; }

        // Title of the article
        public string Title { get; set; }
        
        //... 
    }

## <a id="ModelingReviews"></a>Modellering van beoordelingen
Beoordelingen zijn voornamelijk, artikelen, zoals geschreven en gelezen in de context van het artikel. Kiezen `ArticleId` als een partitie sleutel biedt aanbevolen distributie en efficiënte toegang van revisies die zijn gekoppeld aan het artikel. 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of the review 
        public string ArticleId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.ArticleId; 
            } 
        }
        
        //Reviewer Id 
        public string UserId { get; set; }
        public string ReviewText { get; set; }
        
        public int Rating { get; set; } }
    }

## <a id="DataAccessMethods"></a>Data access layer methoden
Nu we kijken naar de belangrijkste gegevens toegangsmethoden we moet implementeren. Hier volgt een lijst met methoden die de `ContentPublishDatabase` moet:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Azure Cosmos DB-accountconfiguratie
Om te garanderen lokale leest en schrijft, we moet partitioneren alleen gegevens niet op partitie sleutel, maar ook op basis van de geografische toegangspatroon in regio's. Het model is afhankelijk van de met een geo-gerepliceerd Azure Cosmos DB-databaseaccount geldt voor elke regio. Bijvoorbeeld, met twee regio's is hier een instelling voor schrijfbewerkingen in meerdere regio's:

| Accountnaam | Schrijfregio's | Leesregio |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

Het volgende diagram toont hoe de lees- en schrijfbewerkingen worden uitgevoerd in een typische toepassing met deze instellingen:

![Meerdere masters architectuur van Azure Cosmos DB](./media/multi-master-workaround/multi-master.png)

Hier volgt een codefragment met het initialiseren van de clients in een DAL die worden uitgevoerd in de `West US` regio.
    
    ConnectionPolicy writeClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    writeClientPolicy.PreferredLocations.Add(LocationNames.WestUS);
    writeClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient writeClient = new DocumentClient(
        new Uri("https://contentpubdatabase-usa.documents.azure.com"), 
        writeRegionAuthKey,
        writeClientPolicy);

    ConnectionPolicy readClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    readClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
    readClientPolicy.PreferredLocations.Add(LocationNames.WestUS);

    DocumentClient readClient = new DocumentClient(
        new Uri("https://contentpubdatabase-europe.documents.azure.com"),
        readRegionAuthKey,
        readClientPolicy);

Met de voorgaande instellingen, kan de gegevenstoegangslaag doorsturen alle schrijfbewerkingen naar het lokale account op basis van waar deze is geïmplementeerd. Leesbewerkingen worden uitgevoerd door het lezen van beide accounts om op te halen van de overkoepelende weergave van gegevens. Deze aanpak kan worden uitgebreid naar zo veel regio's waar nodig. Dit is bijvoorbeeld een installatie met behulp van drie geografische regio's:

| Accountnaam | Schrijfregio's | Leesregio 1 | Leesregio 2 |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Data access layer-implementatie
Nu gaan we bekijken de uitvoering van de data access layer (DAL) voor een toepassing met twee beschrijfbare regio's. De DAL moet implementeren de volgende stappen uit:

* Meerdere exemplaren maken van `DocumentClient` voor elk account. Met twee regio's, elk DAL-exemplaar heeft een `writeClient` en één `readClient`. 
* Op basis van de geïmplementeerde regio van de toepassing, configureren van de eindpunten voor `writeclient` en `readClient`. Bijvoorbeeld, de DAL worden geïmplementeerd in `West US` maakt gebruik van `contentpubdatabase-usa.documents.azure.com` voor het uitvoeren van schrijfbewerkingen. De DAL geïmplementeerd in `NorthEurope` maakt gebruik van `contentpubdatabase-europ.documents.azure.com` voor schrijfbewerkingen.

Met de voorgaande instellingen, kunnen de data access-methoden worden geïmplementeerd. Doorsturen van bewerkingen het schrijven naar de bijbehorende schrijven `writeClient`.

    public async Task CreateSubscriptionAsync(string userId, string category)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Subscriptions
        {
            UserId = userId,
            SubscriptionFilter = category
        });
    }

    public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Review
        {
            UserId = userId,
            ArticleId = articleId,
            ReviewText = reviewText,
            Rating = rating
        });
    }

Voor het lezen van meldingen en beoordelingen, moet u lezen in beide regio's en union de resultaten zoals wordt weergegeven in het volgende codefragment:

    public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId)
    {
        IDocumentQuery<Notification> writeAccountNotification = (
            from notification in this.writeClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();
        
        IDocumentQuery<Notification> readAccountNotification = (
            from notification in this.readClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        List<Notification> notifications = new List<Notification>();

        while (writeAccountNotification.HasMoreResults || readAccountNotification.HasMoreResults)
        {
            IList<Task<FeedResponse<Notification>>> results = new List<Task<FeedResponse<Notification>>>();

            if (writeAccountNotification.HasMoreResults)
            {
                results.Add(writeAccountNotification.ExecuteNextAsync<Notification>());
            }

            if (readAccountNotification.HasMoreResults)
            {
                results.Add(readAccountNotification.ExecuteNextAsync<Notification>());
            }

            IList<FeedResponse<Notification>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Notification> feed in notificationFeedResult)
            {
                notifications.AddRange(feed);
            }
        }
        return notifications;
    }

    public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId)
    {
        IDocumentQuery<Review> writeAccountReviews = (
            from review in this.writeClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();
        
        IDocumentQuery<Review> readAccountReviews = (
            from review in this.readClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        List<Review> reviews = new List<Review>();
        
        while (writeAccountReviews.HasMoreResults || readAccountReviews.HasMoreResults)
        {
            IList<Task<FeedResponse<Review>>> results = new List<Task<FeedResponse<Review>>>();

            if (writeAccountReviews.HasMoreResults)
            {
                results.Add(writeAccountReviews.ExecuteNextAsync<Review>());
            }

            if (readAccountReviews.HasMoreResults)
            {
                results.Add(readAccountReviews.ExecuteNextAsync<Review>());
            }

            IList<FeedResponse<Review>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Review> feed in notificationFeedResult)
            {
                reviews.AddRange(feed);
            }
        }

        return reviews;
    }

Dus als u kiest een goede te nemen partitionerende sleutel en een statische partitioneren op basis van een account, kunt u bereiken lokale schrijfbewerkingen in meerdere regio's en leesbewerkingen met Azure Cosmos DB.

## <a id="NextSteps"></a>Volgende stappen
In dit artikel wordt beschreven hoe u patronen lezen-schrijven met een wereldwijd gedistribueerde meerdere regio's kunt gebruiken met Azure Cosmos DB met behulp van de inhoud publiceren als een voorbeeldscenario.

* Meer informatie over hoe Azure Cosmos DB ondersteunt [wereldwijde distributie](distribute-data-globally.md)
* Meer informatie over [automatische en handmatige failovers in Azure Cosmos DB](regional-failover.md)
* Meer informatie over [globale consistentie met Azure Cosmos DB](consistency-levels.md)
* Ontwikkelen met meerdere regio's met behulp van de [Azure Cosmos DB - SQL-API](tutorial-global-distribution-sql-api.md)
* Ontwikkelen met meerdere regio's met behulp van de [Azure Cosmos DB - MongoDB-API](tutorial-global-distribution-MongoDB.md)
* Ontwikkelen met meerdere regio's met behulp van de [Azure Cosmos DB - Table-API](tutorial-global-distribution-table.md)
