---
title: 'Snelstartgids: Table-API met .NET - Azure Cosmos DB'
description: In deze quickstart ziet u hoe u de Table-API in Azure Cosmos DB gebruikt om een toepassing te maken met Azure Portal en .NET
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: sngun
ms.openlocfilehash: 7ca51b176c17f33b4779a0129c5dc57b220c0097
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57877629"
---
# <a name="quickstart-build-a-table-api-app-with-net-sdk-and-azure-cosmos-db"></a>Quickstart: Een tabel met de SDK voor .NET en Azure Cosmos DB API-app bouwen 

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
>  

Deze quickstart laat zien hoe u .NET en de [Table-API](table-introduction.md) van Azure Cosmos DB gebruikt voor het compileren van een app door een voorbeeld uit GitHub te klonen. In deze quickstart ziet u ook hoe u een Azure Cosmos DB-account maakt en hoe u Data Explorer gebruikt om tabellen en entiteiten te maken in Azure Portal op internet.

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Een tabel toevoegen

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

[!INCLUDE [cosmos-db-create-table-add-sample-data](../../includes/cosmos-db-create-table-add-sample-data.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Table-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een opdrachtprompt, maak een nieuwe map met de naam git-samples en sluit vervolgens de opdrachtprompt.

   ```bash
   md "C:\git-samples"
   ```

2. Open een git-terminalvenster, bijvoorbeeld git bash, en gebruik de `cd`-opdracht om naar de nieuwe map te gaan voor het installeren van de voorbeeld-app.

   ```bash
   cd "C:\git-samples"
   ```

3. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

   ```bash
   git clone https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started.git
   ```
   > [!NOTE]
   > De standaard .NET-voorbeeld in dit document beschreven werkt met Azure Cosmos DB Table-API en Azure-tabelopslag. Als u geïnteresseerd bent in het voorbeeld uitvoert die werkt met .NET Framework 4.5, raadpleegt u de [storage-tabel-dotnet-getting-started](https://github.com/Azure-Samples/storage-table-dotnet-getting-started) voorbeeld. 


## <a name="open-the-sample-application-in-visual-studio"></a>De voorbeeldtoepassing openen in Visual Studio

1. In Visual Studio opent u het menu **Bestand**, kiest u **Openen** en vervolgens **Project/oplossing**. 

   ![De oplossing openen](media/create-table-dotnet/azure-cosmosdb-open-solution.png) 

2. Navigeer naar de map waar u de voorbeeldtoepassing hebt gekloond en open het bestand tablestorage.sln.

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan de app communiceren met de gehoste database. 

1. Klik in [Azure Portal](https://portal.azure.com/) op **Verbindingsreeks**. Gebruik de kopieerknop aan de rechterkant van het venster om de **PRIMARY CONNECTION STRING** te kopiëren.

   ![De PRIMARY CONNECTION STRING in het deelvenster Verbindingsreeks weergeven en kopiëren](./media/create-table-dotnet/connection-string.png)

2. Open in Visual Studio, de **Settings.json** bestand. 

3. Plak de **PRIMARY CONNECTION STRING** vanuit de portal in de waarde StorageConnectionString. Plak de tekenreeks tussen de aanhalingstekens.

   ```csharp
   {
      "StorageConnectionString": "<Primary connection string from Azure portal>"
   }
   ```
   Als u de [.NET Framework](https://github.com/Azure-Samples/storage-table-dotnet-getting-started) steekproef, moet u de verbindingsreeks die zich in de bijwerken de **App.config** bestand.

4. Druk op CTRL + S om op te slaan de **Settings.json** bestand.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="build-and-deploy-the-app"></a>De app compileren en implementeren

1. In Visual Studio met de rechtermuisknop op de **CosmosTableSamples** project **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

   ![NuGet-pakketten beheren](media/create-table-dotnet/azure-cosmosdb-manage-nuget.png)

2. In het NuGet **Bladeren** vak Microsoft.Azure.Cosmos.Table. Daarmee wordt de clientbibliotheek Cosmos DB-tabel API gevonden. Houd er rekening mee dat deze bibliotheek momenteel beschikbaar voor .NET Framework en .NET Standard is. 

   > [!NOTE]
   > Als u de voorbeelden die gebruikmaakt van .NET Framework gebruikt, moet u de *Microsoft.Azure.CosmosDB.Table*, NuGet-pakket. Als u van het .NET Framework-voorbeeld met Azure Table Storage gebruikmaakt, naast het Cosmos DB-specifieke NuGet-pakket hebt u ook nodig de *Microsoft.Azure.Storage.Common* NuGet-pakket. 
   
   ![Tabblad NuGet bladeren](media/create-table-dotnet/azure-cosmosdb-nuget-browse.png)

3. Klik op **installeren** voor het installeren van de **Microsoft.Azure.Cosmos.Table** bibliotheek. Hiermee worden het pakket voor de Azure Cosmos DB-tabel API en alle daarvoor vereiste onderdelen geïnstalleerd.

4. Wanneer u de hele app uitvoert, is de voorbeeldgegevens ingevoegd in de Tabelentiteit en aan het einde verwijderd, zodat u alle gegevens ingevoegd als u het volledige voorbeeld uitvoert niet weergegeven. U kunt echter wel sommige onderbrekingspunten tot de gegevens invoegen. Open BasicSamples.cs bestand en met de rechtermuisknop op regel 52, selecteer **onderbrekingspunt**en selecteer vervolgens **Onderbrekingspunt invoegen**. Voeg nog een onderbrekingspunt in op regel 55.

   ![Een onderbrekingspunt toevoegen](media/create-table-dotnet/azure-cosmosdb-breakpoint.png) 

5. Druk op F5 om de toepassing uit te voeren. De naam van de nieuwe tabeldatabase (in dit geval demoa13b1) weergegeven in het consolevenster in Azure Cosmos DB. 
    
   ![Console-uitvoer](media/create-table-dotnet/azure-cosmosdb-console.png)

   Als u het eerste onderbrekingspunt bereikt, gaat u terug naar de Data Explorer in Azure Portal. Klik op de knop **Vernieuwen**, vouw de demo*-tabel uit en klik op **Entiteiten**. Het tabblad **Entiteiten** aan de rechterkant geeft de nieuwe entiteit weer die is toegevoegd voor Walter Harp. U ziet dat het telefoonnummer voor de nieuwe entiteit 425-555-0101 is.

   ![Nieuwe entiteit](media/create-table-dotnet/azure-cosmosdb-entity.png)
    
   Als u een foutbericht dat bestand Settings.json kan niet worden gevonden ontvangt bij het uitvoeren van het project, kunt u deze kunt oplossen door de volgende XML-vermelding toe te voegen aan de instellingen van het project. Klik met de rechtermuisknop op CosmosTableSamples, CosmosTableSamples.csproj bewerken selecteren en toevoegen van de volgende itemGroup: 

   ```csharp
     <ItemGroup>
       <None Update="Settings.json">
         <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
       </None>
     </ItemGroup>
   ```

6. Sluit het tabblad **Entiteiten** in Data Explorer.
    
7. Druk op F5 om de app uit te voeren tot het volgende onderbrekingspunt wordt bereikt. 

    Wanneer u het onderbrekingspunt bereikt, gaat u terug naar Azure Portal en klikt u opnieuw op **Entiteiten** om het tabblad **Entiteiten** te openen. U ziet dat het telefoonnummer is bijgewerkt naar 425-555-0105.

8. Druk op F5 om de app uit te voeren. 
 
   De app voegt entiteiten toe voor gebruik in een geavanceerde voorbeeld-app die momenteel niet door de tabel-API wordt ondersteund. De app verwijdert vervolgens de tabel die door de voorbeeld-app is gemaakt.

9. Druk in het consolevenster op Enter om te stoppen met de uitvoering van de app. 
  

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een tabel gemaakt met de Data Explorer en hebt u een app uitgevoerd.  Nu kunt u een query uitvoeren op uw gegevens met de Table-API.  

> [!div class="nextstepaction"]
> [Tabelgegevens importeren in de Table-API](table-import.md)

