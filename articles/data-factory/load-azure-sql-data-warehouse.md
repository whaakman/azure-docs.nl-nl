---
title: Gegevens laden in Azure SQL Data Warehouse met behulp van Azure Data Factory | Microsoft Docs
description: "Gebruik Azure Data Factory om gegevens te kopiëren naar Azure SQL Data Warehouse"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: eec6eeb3419c5f5f4c8d22398051f7cf057ac980
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Gegevens laden in Azure SQL Data Warehouse met behulp van Azure Data Factory

[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is een cloud-gebaseerde, uitbreidbare database die geschikt is voor het verwerken van grote hoeveelheden relationele en niet-relationele gegevens. SQL Data Warehouse is gebaseerd op de architectuur van massively parallelle processing (MPP) die geoptimaliseerd voor enterprise datawarehouse-werkbelastingen. Elasticiteit cloud biedt de flexibiliteit om te schalen van opslag en onafhankelijk berekenen.

Aan de slag met Azure SQL Data Warehouse is nu eenvoudiger dan ooit bij het gebruik van Azure Data Factory. Azure Data Factory is een volledig beheerde gegevens cloud-gebaseerde integration-service. U kunt de service vullen van een SQL Data Warehouse met gegevens van uw bestaande systeem en de tijd besparen bij het bouwen van uw analyseoplossingen.

Azure Data Factory biedt de volgende voordelen voor gegevens in Azure SQL Data Warehouse te laden:

* **Eenvoudig instellen van**: een intuïtieve wizard 5 stap geen scripts nodig.
* **Uitgebreide ondersteuning voor gegevensopslag**: ingebouwde ondersteuning voor een groot aantal on-premises en cloud-gebaseerde gegevensarchieven. Zie voor een gedetailleerde lijst, de tabel van [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
* **Beveiligd en compatibel**: gegevens worden overgedragen via HTTPS of ExpressRoute. De aanwezigheid van global service zorgt ervoor dat uw gegevens nooit de geografische grens verlaten.
* **Ongeëvenaarde prestaties door middel van PolyBase**: Polybase is de meest efficiënte manier om gegevens te verplaatsen naar Azure SQL Data Warehouse. De tijdelijke blob-functie gebruiken om hoge belasting snelheden van alle typen gegevensarchieven, met inbegrip van Azure Blob-opslag- en Data Lake Store realiseren. (Polybase ondersteunt Azure Blob storage en Azure Data Lake Store standaard.) Zie voor meer informatie [kopiëren van de prestaties van de activiteit](copy-activity-performance.md).

Dit artikel laat zien hoe u de gegevens kopiëren van Data Factory-hulpprogramma te _gegevens uit Azure SQL Database in Azure SQL Data Warehouse laden_. U kunt vergelijkbare stappen voor het kopiëren van gegevens van andere typen gegevensarchieven.

> [!NOTE]
> Zie voor meer informatie [kopiëren van gegevens of naar Azure SQL Data Warehouse met behulp van Azure Data Factory](connector-azure-sql-data-warehouse.md).
>
> Dit artikel is van toepassing op versie 2 van Azure Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service is algemeen beschikbaar (GA), Zie [Kopieeractiviteit in Azure Data Factory versie 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Azure SQL datawarehouse: Het datawarehouse bevat de gegevens die via wordt gekopieerd van de SQL-database. Als u een Azure SQL Data Warehouse hebt, raadpleegt u de instructies in [maken van een SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* Azure SQL Database: In deze zelfstudie worden gegevens gekopieerd van een Azure SQL database met voorbeeldgegevens van Adventure Works LT. U kunt een SQL-database maken door de instructies in [maken van een Azure SQL database](../sql-database/sql-database-get-started-portal.md). 
* Azure storage-account: Azure Storage wordt gebruikt als de _fasering_ blob in de kopieerbewerking bulksgewijs. Als u geen Azure storage-account hebt, raadpleegt u de instructies in [een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account).

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer op het menu links **nieuw** > **gegevens en analyse** > **Data Factory**: 
   
   ![Maak een nieuwe gegevensfactory](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. In de **nieuwe gegevensfactory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding:
      
   ![De pagina Nieuwe data factory](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
    * **Naam**: Voer een globaal unieke naam voor uw Azure data factory. Als u de foutmelding "naam gegevensfactory \"LoadSQLDWDemo\" is niet beschikbaar ' Voer een andere naam voor de data factory. Bijvoorbeeld, u kunt de naam van de  _**uwnaam**_**ADFTutorialDataFactory**. Probeer het opnieuw maken van de gegevensfactory. Zie voor de naamgevingsregels voor Data Factory-artefacten, [Data Factory-naamgevingsregels](naming-rules.md).
    * **Abonnement**: Selecteer uw Azure-abonnement in te maken van de gegevensfactory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst, of Selecteer de **nieuw** optie en typ de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Versie**: Selecteer **V2 (Preview)**.
    * **Locatie**: Selecteer de locatie voor de gegevensfactory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevens worden opgeslagen die worden gebruikt door data factory kunnen zich in andere locaties en regio's. Deze gegevensarchieven bevatten Azure Data Lake Store, Azure Storage en Azure SQL Database.

3. Selecteer **Maken**.
4. Nadat het maken voltooid is, gaat u naar uw gegevensfactory. U ziet de **Data Factory** startpagina zoals weergegeven in de volgende afbeelding:
   
   ![Startpagina van de gegevensfactory](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

   Selecteer de **auteur & Monitor** tegel om de toepassing van de integratie gegevens op een afzonderlijke tabblad te starten.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse

1. In de **aan de slag** pagina de **gegevens kopiëren** tegel starten van het hulpprogramma voor kopiëren:

   ![De tegel Copy Data-hulpprogramma](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina **CopyFromSQLToSQLDW** voor de **taaknaam** veld en selecteert u een **volgende**:

    ![De pagina Eigenschappen](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina **Azure SQL Database**, en selecteer **volgende**:

    ![De pagina Brongegevensarchief](./media/load-azure-sql-data-warehouse/specify-source.png)
4. Voer op de pagina **Azure SQL-database opgeven** de volgende stappen uit: 
   1. Selecteer uw Azure SQL-server bij **Servernaam**.
   2. Selecteer uw Azure SQL Database bij **Databasenaam**.
   3. Geef de naam van de gebruiker op bij **Gebruikersnaam**.
   4. Geef het wachtwoord van de gebruiker voor **wachtwoord**.
   5. Selecteer **Volgende**.
   
   ![Azure SQL-database opgeven](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. In de **Selecteer tabellen waarvan de gegevens kopiëren of gebruik een aangepaste query** pagina **SalesLT** voor het filteren van de tabellen. Kies de **(Selecteer alle)** vak voor het gebruik van alle tabellen voor het exemplaar en selecteer vervolgens **volgende**: 

    ![Selecteer de brontabellen](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. In de **doelgegevensopslagplaats** pagina **Azure SQL Data Warehouse**, en selecteer **volgende**:

    ![De pagina Doelgegevensarchief](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. In de **opgeven van de Azure SQL Data Warehouse** pagina, voert u de volgende stappen uit: 

   1. Selecteer uw Azure SQL-server bij **Servernaam**.
   2. Selecteer uw Azure SQL Data Warehouse voor de **databasenaam**.
   3. Geef de naam van de gebruiker op bij **Gebruikersnaam**.
   4. Geef het wachtwoord van de gebruiker voor **wachtwoord**.
   5. Selecteer **Volgende**.
   
   ![Geef de Azure SQL datawarehouse](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. In de **tabeltoewijzing** pagina, Controleer de inhoud en selecteer **volgende**. Een intelligente tabeltoewijzing wordt weergegeven. De brontabellen zijn toegewezen aan de doeltabellen op basis van de tabelnamen van de. Als een brontabel niet bestaat in de doel-, maakt Azure Data Factory een doeltabel met dezelfde naam standaard. U kunt ook een brontabel toewijzen aan een bestaande tabel van de bestemming. 

   > [!NOTE]
   > Maken van de automatische tabel voor de SQL Data Warehouse sink is van toepassing wanneer de bron SQL Server of Azure SQL Database. Als u gegevens uit een andere brongegevensarchief kopiëren, moet u het schema in de Azure SQL Data Warehouse-sink vooraf maken voordat u de gegevens opnieuw te kopiëren.

   ![De pagina Tabeltoewijzing](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. In de **schematoewijzing** pagina, Controleer de inhoud en selecteer **volgende**. De intelligent tabeltoewijzing is gebaseerd op de naam van de kolom. Als u Data Factory automatisch maken van de tabellen toestaat, worden de conversie van type kan optreden wanneer tussen de bron- en doelservers worden opgeslagen zijn. Als er een conversie van het type niet-ondersteunde gegevens tussen de bron- en doelserver kolom, ziet u een foutbericht weergegeven naast de bijbehorende tabel.

    ![De pagina Schematoewijzing](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. In de **instellingen** pagina, selecteert u de Azure storage-account in de **opslagaccountnaam** vervolgkeuzelijst. Het account wordt gebruikt voor het Faseren van de gegevens voordat deze wordt geladen in SQL Data Warehouse met behulp van PolyBase. Nadat de kopie voltooid is, wordt de tijdelijke gegevens in Azure Storage automatisch opgeschoond. Onder **geavanceerde instellingen**, schakel de **Gebruik standaard** optie:

    ![De pagina Instellingen](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. In de **samenvatting** pagina, controleert u de instellingen en selecteer **volgende**:

    ![Overzichtspagina](./media/load-azure-sql-data-warehouse/summary-page.png)
13. In de **implementatie pagina**, selecteer **Monitor** voor het bewaken van de pijplijn (taak):

    ![De pagina Implementatie](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen naar details uitvoering van activiteiten bekijken en naar de pijplijn opnieuw uitvoeren: 

    ![Pijplijnuitvoeringen controleren](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Als u wilt weergeven van activiteiten bij uitvoering die gekoppeld aan de pijplijn uitvoeren zijn, selecteer de **weergave activiteit wordt uitgevoerd** koppelen de **acties** kolom. Er zijn 10 kopie activiteiten in de pijplijn en elke activiteit een gegevenstabel kopieert. Overschakelen naar de pijplijn uitgevoerd weergeven, selecteert u de **pijplijnen** koppeling aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Voor het bewaken van de uitvoering details voor elke activiteit kopiëren, selecteert u de **Details** koppeling onder **acties** in de activiteit controle weergeven. Als de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met een bijbehorende duur, en configuraties gebruikt, kunt u gegevens controleren:

    ![Monitoractiviteit details uitvoering](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over Azure SQL Data Warehouse-ondersteuning: 

> [!div class="nextstepaction"]
>[Azure SQL Data Warehouse-connector](connector-azure-sql-data-warehouse.md)