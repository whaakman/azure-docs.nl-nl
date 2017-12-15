---
title: Meerdere hoofddatabase architecturen met Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het ontwerpen van toepassingsarchitecturen met lokale lees- en schrijfbewerkingen over meerdere geografische regio's met Azure Cosmos DB.
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 706ced74-ea67-45dd-a7de-666c3c893687
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e02b286db42d8a9de8f1df8263f40c3732484038
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a>Meerdere master gerepliceerd globaal database architecturen met Azure Cosmos-DB
Azure Cosmos DB ondersteunt klare [globale replicatie](distribute-data-globally.md), waarmee u gegevens naar meerdere regio's met lage latentie toegang overal in de werklast verdelen. Dit model wordt meestal gebruikt voor publisher/consumer werkbelastingen wanneer er een schrijver in één geografische regio en globaal gedistribueerde lezers in andere regio (lezen). 

U kunt ook Azure Cosmos DB globale Replicatieondersteuning gebruiken om toepassingen waarin schrijvers en lezers worden globaal gedistribueerd te bouwen. Dit document bevat een patroon waarmee lokale schrijf- en lokale leestoegang voor gedistribueerde schrijvers met behulp van Azure DB die Cosmos bereiken.

## <a id="ExampleScenario"></a>Publicatie - een voorbeeldscenario
We bekijken een werkelijk scenario te beschrijven hoe u globaal gedistribueerde meerdere-region/meerdere-pre-master lezen schrijven patronen kunt gebruiken met Azure Cosmos DB. U kunt een content publishing platform gebouwd op Azure Cosmos DB. Hier volgen enkele vereisten waaraan dit platform voor consumenten en uitgevers voor een goede gebruikerservaring moet voldoen.

* Zowel auteurs en abonnees verdeeld over de hele wereld 
* Auteurs moeten (schrijven) artikelen publiceren naar de lokale regio (dichtst)
* Auteurs hebben lezers/abonnees van hun artikelen die zijn verdeeld over de hele wereld. 
* Abonnees moeten een melding krijgen wanneer nieuwe artikelen worden gepubliceerd.
* Abonnees moet leesrechten artikelen hun lokale regio. Ze moeten ook worden kunnen beoordelingen toevoegen aan deze artikelen. 
* Iedereen met inbegrip van de auteur van de artikelen moet kunnen weergeven, alle beoordelingen zijn verbonden met artikelen vanaf een lokale regio. 

Miljoenen consumenten en uitgevers miljarden artikelen, ervan uitgaande dat we hebben snel pakt u de problemen van de schaal samen met garanderen plaats van toegang. Net als bij de meeste schaalbaarheidsproblemen, ligt de oplossing in de strategie voor een goede partitionering. Daarna bekijken we het model artikelen, controleren en meldingen als documenten, Azure DB die Cosmos-accounts configureren en implementeren van een gegevenstoegangslaag. 

Als u weten over partitioneren en partitiesleutels wilt, Zie [partitionerings- en schalen in Azure Cosmos DB](partition-data.md).

## <a id="ModelingNotifications"></a>Modellering meldingen
Meldingen zijn gegevensfeeds specifiek voor een gebruiker. Daarom zijn de toegangspatronen voor meldingen documenten altijd in de context van één gebruiker. U zou bijvoorbeeld 'een melding naar een gebruiker boeken' of 'alle meldingen voor een bepaalde gebruiker ophalen'. Ja, de beste keuze van de partitiesleutel voor dit type zou worden `UserId`.

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

## <a id="ModelingSubscriptions"></a>Modellering abonnementen
Abonnementen kunnen worden gemaakt voor de verschillende criteria zoals een specifieke categorie artikelen interessegebied of een bepaalde uitgever. Daarom de `SubscriptionFilter` een goede keuze voor de partitiesleutel is.

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

## <a id="ModelingArticles"></a>Vormgeven aan artikelen
Zodra een artikel wordt geïdentificeerd door meldingen, de volgende query's doorgaans zijn gebaseerd op de `Article.Id`. Kiezen `Article.Id` als partitie biedt de sleutel dus het beste distributiepunt voor het opslaan van de artikelen in een verzameling Azure Cosmos DB. 

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

## <a id="ModelingReviews"></a>Modellering beoordelingen
Beoordelingen zijn voornamelijk zoals artikelen, geschreven en gelezen in de context van het artikel. Kiezen `ArticleId` een partitie sleutel biedt aanbevolen distributie en efficiënte toegang van revisies die zijn gekoppeld aan het artikel. 

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

## <a id="DataAccessMethods"></a>Methoden voor gegevenstoegang laag
Nu we kijken naar de belangrijkste gegevens toegangsmethoden we moet implementeren. Hier volgt een lijst met methoden die de `ContentPublishDatabase` moet:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Configuratie van Azure DB Cosmos-account
Als u wilt garanderen lokale leest en schrijft, we moet partitie-alleen gegevens niet op partitie sleutel, maar ook op basis van het toegangspatroon geografische in regio's. Het model afhankelijk is van een account Azure Cosmos DB database geo-replicatie voor elke regio. Bijvoorbeeld, met twee gebieden is hier een instelling voor schrijfacties voor meerdere landen/regio:

| Accountnaam | Regio schrijven | Lees de regio |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

Het volgende diagram toont hoe lees- en schrijfbewerkingen worden uitgevoerd in een typische toepassing met deze instellingen:

![Azure meerdere masters Cosmos-DB-architectuur](./media/multi-region-writers/multi-master.png)

Hier volgt een codefragment met het initialiseren van de clients in een DAL uitgevoerd in de `West US` regio.
    
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

Met de voorgaande setup kan de gegevenstoegangslaag doorsturen alle schrijfbewerkingen naar het lokale account op basis van waarop deze wordt geïmplementeerd. Leesbewerkingen worden uitgevoerd door bij het lezen van beide accounts voor de globale weergave van gegevens. Deze methode kan worden uitgebreid met zoveel regio's waar nodig. Dit is bijvoorbeeld een installatie met drie geografische regio's:

| Accountnaam | Regio schrijven | Regio 1 lezen | Regio 2 lezen |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Data access-laag implementatie
Nu we bekijken de uitvoering van de gegevenstoegangslaag (DAL) voor een toepassing met twee beschrijfbare regio's. De DAL moet implementeren de volgende stappen uit:

* Maken van meerdere exemplaren van `DocumentClient` voor elke account. Met twee regio's, elk DAL-exemplaar heeft een `writeClient` en één `readClient`. 
* Op basis van de geïmplementeerde regio van de toepassing, configureren van de eindpunten voor `writeclient` en `readClient`. Bijvoorbeeld, de DAL worden geïmplementeerd in `West US` gebruikt `contentpubdatabase-usa.documents.azure.com` voor het uitvoeren van schrijfbewerkingen. De DAL geïmplementeerd in `NorthEurope` gebruikt `contentpubdatabase-europ.documents.azure.com` voor schrijfbewerkingen.

Met de voorgaande setup kunnen de data access-methoden worden geïmplementeerd. Schrijven operations doorsturen van het schrijven naar de bijbehorende `writeClient`.

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

Voor het lezen van meldingen en beoordelingen, moet u lezen van zowel regio's en union de resultaten zoals weergegeven in het volgende fragment:

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

Dus als u een goede te nemen partitionerende sleutel en statische op basis van een account partitioneren, kunt u bereiken met meerdere regio lokale geschreven en gelezen met behulp van Azure DB die Cosmos.

## <a id="NextSteps"></a>Volgende stappen
In dit artikel wordt beschreven hoe u globaal gedistribueerde meerdere landen/regio lezen-schrijven patronen kunt gebruiken met Azure Cosmos DB met inhoud publiceren als een voorbeeldscenario.

* Meer informatie over hoe Azure Cosmos DB ondersteunt [globale distributie](distribute-data-globally.md)
* Meer informatie over [automatische en handmatige failover in Azure Cosmos-DB](regional-failover.md)
* Meer informatie over [globale consistentie met Azure Cosmos-DB](consistency-levels.md)
* Ontwikkelen met meerdere regio's met behulp van de [Azure DB Cosmos - SQL-API](tutorial-global-distribution-sql-api.md)
* Ontwikkelen met meerdere regio's met behulp van de [Azure DB Cosmos - MongoDB-API](tutorial-global-distribution-MongoDB.md)
* Ontwikkelen met meerdere regio's met behulp van de [Azure DB Cosmos - tabel-API](tutorial-global-distribution-table.md)
