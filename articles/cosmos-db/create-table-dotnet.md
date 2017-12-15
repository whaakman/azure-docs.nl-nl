---
title: 'Quickstart: Table-API met .NET - Azure Cosmos DB | Microsoft Docs'
description: In deze quickstart ziet u hoe u de Table-API in Azure Cosmos DB gebruikt om een toepassing te maken met Azure Portal en .NET
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quickstart connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: e0f0a95ea086e83ef0c46145b33b348071407aa5
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="quickstart-build-a-table-api-app-with-net-and-azure-cosmos-db"></a>Quickstart: Een Table-API compileren met .NET en Azure Cosmos DB 

Deze quickstart laat zien hoe u Java en de [Table-API](table-introduction.md) van Azure Cosmos DB gebruikt voor het compileren van een app door een voorbeeld uit GitHub te klonen. In deze quickstart ziet u ook hoe u een Azure Cosmos DB-account maakt en hoe u Data Explorer gebruikt om tabellen en entiteiten te maken in Azure Portal op internet.

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafen en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

> [!IMPORTANT] 
> U moet een nieuw Table-API-account maken om te kunnen werken met de algemeen beschikbare SDK’s voor de Table-API. Table-API-accounts die zijn gemaakt tijdens de preview worden niet ondersteund door de algemeen beschikbaar SDK's.
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Een tabel toevoegen

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

U kunt nu gegevens aan uw nieuwe tabel toevoegen met behulp van Data Explorer.

1. Vouw in Data Explorer **sample-table** uit, klik op **Entiteiten** en klik vervolgens op **Entiteit toevoegen**.

   ![Nieuwe entiteiten maken in Data Explorer in Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Voeg nu gegevens toe aan de vakken met een waarde voor PartitionKey en RowKey en klik op **Entiteit toevoegen**.

   ![De partitiesleutel en de rijsleutel instellen voor een nieuwe entiteit](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    U kunt nu meer entiteiten toevoegen aan uw tabel, uw entiteiten bewerken of een query op uw gegevens uitvoeren in Data Explorer. Data Explorer is ook de plek waar u uw doorvoer kunt schalen en opgeslagen procedures, door de gebruiker gedefinieerde functies en triggers aan uw tabel kunt toevoegen.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Table-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een git-terminalvenster, bijvoorbeeld git bash, en gebruik de `cd`-opdracht om naar een map te gaan voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Open vervolgens het oplossingenbestand TableStorage in Visual Studio. 

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan de app communiceren met de gehoste database. 

1. Klik in [Azure Portal](http://portal.azure.com/) op **Verbindingsreeks**. 

    Gebruik de kopieerknoppen aan de rechterkant van het scherm om de PRIMARY CONNECTION STRING te kopiëren.

    ![De PRIMARY CONNECTION STRING in het deelvenster Verbindingsreeks weergeven en kopiëren](./media/create-table-dotnet/connection-string.png)

2. In Visual Studio opent u het bestand App.config. 

3. Verwijder de opmerking bij de StorageConnectionString op regel 8 en plaats een opmerking bij de StorageConnectionString op regel 7. Dit omdat deze zelfstudie geen gebruik maakt van de Storage Emulator. Regel 7 en 8 moeten er nu als volgt uitzien:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Plak de PRIMARY CONNECTION STRING vanuit de portal in de waarde StorageConnectionString op regel 8. Plak de tekenreeks tussen de aanhalingstekens. 

    > [!IMPORTANT]
    > Als uw eindpunt documents.azure.com gebruikt, hebt u een preview-account en moet u een [nieuw Table-API-account](#create-a-database-account) maken om te kunnen werken met de algemeen beschikbare SDK voor Table- API. 
    > 

    Regel 8 moet nu ongeveer als volgt uitzien:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. Sla het bestand App.config op.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="build-and-deploy-the-app"></a>De app compileren en implementeren

1. Klik in Visual Studio met de rechtermuisknop op het project **TableStorage** in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

2. Typ in het NuGet-vak **Bladeren** *Microsoft.Azure.CosmosDB.Table*.

3. Installeer vanuit de resultaten de bibliotheek **Microsoft.Azure.CosmosDB.Table**. Nu worden het Azure Cosmos DB Table-API-pakket en alle daarvoor vereiste onderdelen geïnstalleerd.

4. Open BasicSamples.cs en voeg een onderbrekingspunt toe aan regel 30 en 52.

5. Klik op CTRL+F5 om de toepassing te starten.

    In het consolevenster worden de tabelgegevens weergegeven die worden toegevoegd aan de nieuwe tabeldatabase in Azure Cosmos DB. 
    
    Als u een fout over afhankelijkheden krijgt, bekijkt u de [Probleemoplossing](table-sdk-dotnet.md#troubleshooting).

    Als u het eerste onderbrekingspunt bereikt, gaat u terug naar de Data Explorer in Azure Portal en vouwt u de demotabel* uit. Klik vervolgens op **Entiteiten**. Het tabblad **Entiteiten** aan de rechterkant geeft de nieuwe entiteit weer die is toegevoegd. Houd er rekening mee dat het telefoonnummer voor de gebruiker 425-555-0101 is.
    
6. Sluit het tabblad Entiteiten in Data Explorer.
    
7. Voer de app uit tot u het volgende onderbrekingspunt bereikt.

    Wanneer u het onderbrekingspunt bereikt, gaat u terug naar de portal en klikt u opnieuw op Entiteiten om het tabblad Entiteiten te openen. U ziet dat het telefoonnummer is bijgewerkt naar 425-555-0105.

8. Druk in het consolevenster op CTRL + C om te stoppen met de uitvoering van de app. 

    U kunt nu teruggaan naar Data Explorer en de entiteiten toevoegen of wijzigen, en gegevens opvragen.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een tabel gemaakt met de Data Explorer en hebt u een app uitgevoerd.  Nu kunt u een query uitvoeren op uw gegevens met de Table-API.  

> [!div class="nextstepaction"]
> [Tabelgegevens importeren in de Table-API](table-import.md)

