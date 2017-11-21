---
title: 'Snelstartgids: Tabel API met behulp van Node.js - Azure Cosmos DB | Microsoft Docs'
description: Deze snelstartgids laat zien hoe Azure Cosmos DB tabel API gebruiken om te maken van een toepassing met de Azure-portal en Node.js
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: arramac
ms.openlocfilehash: 8cf8820ceea19fe8c4926c65d107d4f770f40926
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="quickstart-build-a-table-api-app-with-nodejs-and-azure-cosmos-db"></a>Snelstartgids: Een tabel met Node.js en Azure Cosmos DB API-app bouwen

Deze snelstartgids laat zien hoe u Node.js en de Azure DB die Cosmos [tabel API](table-introduction.md) voor het bouwen van een app door het klonen van een voorbeeld van GitHub. Deze snelstartgids ziet u ook een Cosmos-DB Azure-account maken en hoe Data Explorer gebruiken om te maken van tabellen en entiteiten in de Azure portal op Internet.

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel maken en query document, sleutel/waarde, wide kolom en grafiek databases, die allemaal van de algemene distributie en de mogelijkheden van de horizontale schaal de kern van Azure Cosmos DB profiteren. 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Daarnaast doet u het volgende:

* [Node.js](https://nodejs.org/en/) versie v0.10.29 of hoger
* [Git](http://git-scm.com/)

## <a name="create-a-database-account"></a>Een databaseaccount maken

> [!IMPORTANT] 
> U moet een nieuwe tabel API-account om te werken met de SDK algemeen beschikbaar tabel-API's maken. Tabel gemaakt tijdens de preview-API-accounts worden niet ondersteund door de algemeen beschikbaar SDK's.
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Een tabel toevoegen

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

U kunt nu gegevens aan uw nieuwe tabel toevoegen met behulp van Data Explorer.

1. Vouw in Data Explorer **sample-table** uit, klik op **Entiteiten** en klik vervolgens op **Entiteit toevoegen**.

   ![Nieuwe entiteiten maken in Data Explorer in Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Nu gegevens toevoegen aan de waarde PartitionKey en RowKey waarde vakken en klik op **entiteit toevoegen**.

   ![De partitiesleutel en de rijsleutel instellen voor een nieuwe entiteit](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    U kunt nu meer entiteiten toevoegen aan uw tabel, uw entiteiten bewerken of een query op uw gegevens uitvoeren in Data Explorer. Data Explorer is ook de plek waar u uw doorvoer kunt schalen en opgeslagen procedures, door de gebruiker gedefinieerde functies en triggers aan uw tabel kunt toevoegen.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Table-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een git-terminalvenster zoals git bash, en gebruik de `cd` opdracht om te wijzigen naar een map voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt u een kopie van de voorbeeld-app op uw computer. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-node-getting-started.git
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan uw app kan communiceren met uw gehoste-database. 

1. In de [Azure-portal](http://portal.azure.com/), klikt u op **verbindingsreeks**. 

    ![Bekijken en kopiëren van de vereiste verbindingsinformatie van de in het deelvenster verbindingsreeks](./media/create-table-nodejs/connection-string.png)

2. Kopieer de primaire VERBINDINGSREEKS met behulp van de knop kopiëren aan de rechterkant.

3. Open het bestand app.config en de waarde in de connectionString op regel drie plakken. 

    > [!IMPORTANT]
    > Als uw eindpunt documents.azure.com treedt op wanneer u een preview-account hebt gebruikt, en u wilt maken een [nieuwe tabel API account](#create-a-database-account) werken met de algemeen beschikbaar tabel API SDK.
    >

3. Sla het bestand app.config.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="run-the-app"></a>De app uitvoeren

1. In het terminalvenster git `cd` naar de map voor opslag-tabel-java-aan de slag.

    ```
    cd "C:\git-samples\storage-table-node-getting-started"
    ```

2. Voer de volgende opdracht voor het installeren van de [azure] [knooppunt-uuid] [nconf] en [asynchrone] modules lokaal ook over een vermelding voor ze opslaan in het package.json-bestand

   ```
   npm install azure-storage node-uuid async nconf --save
   ```

2. In de git start terminalvenster, voer de volgende opdrachten om uit te voeren de knooppunt-toepassing.

    ```
    node ./tableSample.js 
    ```

    Het consolevenster weergegeven gegevens in de tabel wordt toegevoegd aan de nieuwe database in de tabel in Azure Cosmos DB.

    U kunt nu teruggaan naar Data Explorer en deze nieuwe gegevens bekijken, wijzigen, een query erop uitvoeren of er iets anders mee doen. 

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een tabel gemaakt met de Data Explorer en hebt u een app uitgevoerd.  Nu kunt u een query uitvoeren op uw gegevens met de Table-API.  

> [!div class="nextstepaction"]
> [Tabelgegevens importeren in de tabel-API](table-import.md)
