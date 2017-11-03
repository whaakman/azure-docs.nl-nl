---
title: Maken van een Azure Cosmos DB .NET Framework of Core toepassing met behulp van de Graph API | Microsoft Docs
description: Geeft een voorbeeld van .NET Framework/Core code die kunt u verbinding maken met en query uitvoeren op Azure Cosmos-DB
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 10/06/2017
ms.author: denlee
ms.openlocfilehash: 4c90ead99c513a56f8891b889e2c873952a33ec8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-build-a-net-framework-or-core-application-using-the-graph-api"></a>Azure Cosmos DB: Een .NET Framework of Core-toepassing met behulp van de Graph API bouwen

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafen en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u een Azure Cosmos DB-account, een database en een graaf (container) kunt maken met behulp van Azure Portal. Vervolgens ontwikkelt u een console-app die is gebouwd op de [Graph API](graph-sdk-dotnet.md) (preview) en voert u deze uit.  

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

Als u Visual Studio 2017 geïnstalleerd hebt, controleert u of moet worden geïnstalleerd tot [Visual Studio 2017 Update 3](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Een graaf toevoegen

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Graph API-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

Dit voorbeeldproject .NET Core project-indeling gebruikt en is geconfigureerd voor het doel van de volgende frameworks:
 - netcoreapp2.0
 - net461

1. Open een git-terminalvenster zoals git bash en `cd` naar een werkmap.  

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Open vervolgens Visual Studio en open het oplossingenbestand. 

## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand Program.cs en u zult zien dat deze regels code de Azure Cosmos DB-resources maken. 

* De DocumentClient wordt geïnitialiseerd. In het voorbeeld hebben we een API voor het toevoegen van een graafextensie toegevoegd aan de Azure Cosmos DB-client. We werken op een zelfstandige graafclient die is losgekoppeld van de Azure Cosmos DB-client en -resources.

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* Er wordt een nieuwe database gemaakt.

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* Er wordt en nieuwe graaf gemaakt.

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* Een reeks Gremlin-stappen wordt uitgevoerd met behulp van de methode `CreateGremlinQuery`.

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Open het bestand appsettings.json in Visual Studio-2017. 

2. Klik in Azure Portal in uw Azure Cosmos DB-account in het linkernavigatiegedeelte op **Sleutels**. 

    ![Een primaire sleutel bekijken en kopiëren in Azure Portal, op de pagina Sleutels](./media/create-graph-dotnet/keys.png)

3. Kopieer uw **URI** waarde van de portal en de waarde van de sleutel voor eindpunt in appsettings.json maken. U kunt de knop Kopiëren gebruiken zoals weergegeven op de vorige schermafbeelding om de waarde te kopiëren.

    `"endpoint": "https://FILLME.documents.azure.com:443/",`

4. Kopieer vervolgens de waarde van uw **PRIMAIRE SLEUTEL** vanuit de portal en geef deze als authKey-waarde in App.config. Sla daarna uw wijzigingen op. 

    `"authkey": "FILLME"`

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="run-the-console-app"></a>De console-app uitvoeren

Voordat u de toepassing uitvoert, wordt aanbevolen dat u bijwerkt de *Microsoft.Azure.Graphs* pakket naar de nieuwste versie.

1. Klik in Visual Studio met de rechtermuisknop op het project in **GraphGetStarted** in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

2. In de NuGet Package Manager **Updates** tabblad, typt u *Microsoft.Azure.Graphs* en controleer de **bevat voorlopige versie** vak. 

3. Bijwerken van de resultaten van de **Microsoft.Azure.Graphs** bibliotheek naar de nieuwste versie van het pakket. Hiermee installeert u het pakket met de bibliotheek met graafextensies van Azure Cosmos DB en alle afhankelijkheden.

    Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.

4. Klik op CTRL+F5 om de toepassing te starten.

   In het consolevenster worden de hoekpunten en randen weergegeven die aan de graaf worden toegevoegd. Zodra het script is voltooid, drukt u tweemaal op ENTER om het consolevenster te sluiten.

## <a name="browse-using-the-data-explorer"></a>Bladeren met Data Explorer

U kunt nu teruggaan naar Data Explorer in Azure Portal en door uw nieuwe graafgegevens bladeren en er query’s op uitvoeren.

1. De nieuwe database wordt in Data Explorer weergegeven in het deelvenster Grafieken. Vouw **graphdb**, **graphcollz** uit en klik vervolgens op **Grafiek**.

2. Klik op de knop **Filter toepassen** om de standaardquery te gebruiken om alle verticies in de grafiek weer te geven. De gegevens die worden gegenereerd door de voorbeeldapp worden weergegeven in het deelvenster Grafen.

    U kunt op de grafiek in- en uitzoomen, u kunt u de grafiek uitvouwen voor meer ruimte, extra verticies toevoegen en verticies verplaatsen in de weergaveruimte.

    ![De grafiek weergeven in Data Explorer in Azure Portal](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal: 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een graaf gemaakt met Data Explorer en hebt u een app uitgevoerd. U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)

