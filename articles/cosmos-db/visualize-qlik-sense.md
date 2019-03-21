---
title: Maak Qlik Sense verbinding met Azure Cosmos DB en Visualiseer uw gegevens
description: In dit artikel wordt beschreven hoe u verbinding maken met Azure Cosmos DB Qlik Sense en Visualiseer uw gegevens.
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 10/22/2018
ms.reviewer: sngun
ms.openlocfilehash: 4532962b6fd9f40fad625ab000116e5a617682e5
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258769"
---
# <a name="connect-qlik-sense-to-azure-cosmos-db-and-visualize-your-data"></a>Maak Qlik Sense verbinding met Azure Cosmos DB en Visualiseer uw gegevens

Qlik Sense is een hulpmiddel voor gegevensvisualisatie waarin gegevens uit verschillende bronnen worden gecombineerd in één weergave. Qlik Sense indexeert elke mogelijke relatie in uw gegevens zodat u inzicht in de gegevens krijgt. U kunt Azure Cosmos DB-gegevens visualiseren met behulp van Qlik Sense. In dit artikel wordt beschreven hoe u verbinding maken met Azure Cosmos DB Qlik Sense en Visualiseer uw gegevens. 

> [!NOTE]
> Qlik Sense verbinden met Azure Cosmos DB wordt momenteel ondersteund voor SQL-API en Azure Cosmos DB-API voor MongoDB-accounts.

U kunt Qlik Sense verbinding maken met Azure Cosmos DB met:

* Cosmos DB SQL API met behulp van de ODBC-connector.

* Azure Cosmos-DB-API voor MongoDB met behulp van de connector Qlik Sense MongoDB (momenteel in Preview-versie).

* Azure Cosmos DB API voor MongoDB en SQL-API met behulp van REST-API-connector in Qlik Sense.

* Cosmos DB Mongo DB API met behulp van de connector gRPC voor Qlik Core.
Dit artikel beschrijft de details van het verbinding maken met de Cosmos DB SQL API via de ODBC-connector.

Dit artikel beschrijft de details van het verbinding maken met de Cosmos DB SQL API via de ODBC-connector.

## <a name="prerequisites"></a>Vereisten

Voordat u de instructies in dit artikel te volgen, moet u dat u de volgende resources gereed hebben:

* Download de [Qlik Sense Desktop](https://www.qlik.com/us/try-or-buy/download-qlik-sense) of Qlik Sense in Azure instellen [installeren van de marketplace-item Qlik Sense](https://azuremarketplace.microsoft.com/marketplace/apps/qlik.qlik-sense).

* Download de [videogame gegevens](https://www.kaggle.com/gregorut/videogamesales), deze voorbeeldgegevens is CSV-indeling. U deze gegevens opslaan in een Cosmos DB-account en visualiseren in Qlik Sense.

* Een Azure Cosmos DB SQL API-account maken met behulp van de stappen in [maken van een account](create-sql-api-dotnet.md#create-account) gedeelte van het quickstart-artikel.

* [Een database en een verzameling maken](create-sql-api-dotnet.md#create-collection-database) – u kunt gebruiken de doorvoercapaciteit van de verzameling ingesteld op 1000 RU/s. 

* Het voorbeeld videogame verkoop gegevens laden in uw Cosmos DB-account. U kunt de gegevens importeren met behulp van Azure Cosmos DB-hulpprogramma voor migratie van gegevens, kunt u een [opeenvolgende](import-data.md#SQLSeqTarget) of een [bulkimport](import-data.md#SQLBulkTarget) van gegevens. Het duurt ongeveer 3 tot 5 minuten om de gegevens te importeren naar het Cosmos DB-account.

* Downloaden, installeren en configureren van het ODBC-stuurprogramma via de stappen in de [verbinding maken met Cosmos DB met ODBC-stuurprogramma](odbc-driver.md) artikel. De gegevens videogame is een eenvoudige set gegevens en u hoeft te bewerken van het schema, gebruikt het standaardschema voor de verzameling-toewijzing.

## <a name="connect-qlik-sense-to-cosmos-db"></a>Maak Qlik Sense verbinding met Cosmos DB

1. Open Qlik Sense en selecteer **nieuwe app maken**. Geef een naam op voor uw app en selecteer **maken**.

   ![Een nieuwe Qlik Sense-app maken](./media/visualize-qlik-sense/create-new-qlik-sense-app.png)

2. Nadat de nieuwe app is gemaakt, selecteert u **app openen** en kies **toevoegen van gegevens uit bestanden en andere bronnen**. 

3. Selecteer in de gegevensbronnen, **ODBC** om de nieuwe verbinding instellen-venster te openen. 

4. Schakel over naar **gebruiker DSN** en kies de ODBC-verbinding die u eerder hebt gemaakt. Geef een naam op voor de verbinding en selecteer **maken**. 

   ![Een nieuwe verbinding maken](./media/visualize-qlik-sense/create-new-connection.png)

5. Nadat u verbinding maakt, kiest u de database, verzameling waar de gegevens videogame zich bevindt en vervolgens het voorbeeldvenster.

   ![Kies de database en verzameling](./media/visualize-qlik-sense/choose-database-and-collection.png) 

6. Selecteert u vervolgens **gegevens toevoegen** om de gegevens naar Qlik Sense te laden. U kunt na het laden van gegevens naar Qlik Sense inzichten genereren, en analyses uitvoeren op de gegevens. U kunt de inzichten gebruiken of bouw uw eigen app de verkoop videogames verkennen. De volgende afbeelding ziet u 

   ![Gegevens visualiseren](./media/visualize-qlik-sense/visualize-data.png)

### <a name="limitations-when-connecting-with-odbc"></a>Beperkingen bij het maken van een verbinding met ODBC 

Cosmos DB is een gedistribueerde database zonder schema met stuurprogramma's die zijn gemodelleerd rond de behoeften van ontwikkelaars. Het ODBC-stuurprogramma vereist een database met het schema voor het afleiden van kolommen, de gegevenstypen en andere eigenschappen. De normale SQL-query of de DML-syntaxis met de relationele functionaliteit is niet van toepassing op Cosmos DB SQL API omdat SQL-API niet ANSI SQL. De SQL-instructies die is uitgegeven door het ODBC-stuurprogramma zijn vanwege reden worden vertaald in Cosmos DB-specifieke SQL-syntaxis die geen equivalent voor alle constructies. Om te voorkomen dat deze problemen vertaling, moet u een schema toepassen bij het instellen van de ODBC-verbinding. De [verbinding maken met ODBC-stuurprogramma](odbc-driver.md) artikel vindt u suggesties en methoden om te helpen u bij het configureren van het schema. Zorg ervoor dat u deze toewijzing voor elke databaseverzameling binnen de Cosmos DB-account maken.

## <a name="next-steps"></a>Volgende stappen

Als u een andere visualisatiehulpprogramma, zoals Power BI gebruikt, u verbinding mee kan maken met behulp van de instructies in het volgende document:

* [Cosmos DB-gegevens visualiseren met behulp van de Power BI-connector](powerbi-visualize.md)
