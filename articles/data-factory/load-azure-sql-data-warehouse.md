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
ms.openlocfilehash: 36e24da50386d1abc441e2beb09f570a9612a346
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-using-azure-data-factory"></a>Gegevens laden in Azure SQL Data Warehouse met behulp van Azure Data Factory

[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is een cloud-gebaseerde, uitbreidbare database geschikt is voor het verwerken van grote hoeveelheden relationele en niet-relationele gegevens.  Gebaseerd op de uitgebreide parallelle verwerkingsarchitectuur (MPP) en is SQL Data Warehouse geoptimaliseerd voor enterprise datawarehouse-werkbelastingen.  Elasticiteit cloud biedt de flexibiliteit om te schalen van opslag en onafhankelijk berekenen.

Aan de slag met Azure SQL Data Warehouse is nu eenvoudiger dan ooit met **Azure Data Factory**.  Azure Data Factory is een volledig beheerde cloud-gebaseerde integration-gegevensservice kan worden gebruikt voor het vullen van een SQL Data Warehouse met de gegevens uit uw bestaande systeem en bespaart u kostbare tijd bij het evalueren van SQL Data Warehouse en het bouwen van uw analyses oplossingen. Hier volgen de belangrijkste voordelen van het laden van gegevens in Azure SQL Data Warehouse met behulp van Azure Data Factory:

* **Eenvoudig instellen van**: 5 stap intuïtieve wizard geen scripts nodig.
* **Uitgebreide ondersteuning voor gegevensopslag**: ingebouwde ondersteuning voor een groot aantal on-premises en cloud-gebaseerde gegevensarchieven gedetailleerd overzicht in [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.
* **Beveiligd en compatibel**: gegevens worden overgedragen via HTTPS of ExpressRoute en global service aanwezigheid zorgt ervoor dat uw gegevens de geografische grens nooit verlaten
* **Ongeëvenaarde prestaties door middel van PolyBase**: met behulp van Polybase is de meest efficiënte manier om gegevens te verplaatsen naar Azure SQL Data Warehouse. De fasering blob-functie kunt u hoge belasting snelheden van alle soorten gegevensarchieven naast Azure Blob-opslag- en Data Lake Store, die ondersteuning biedt voor de Polybase standaard bereiken. Meer informatie over de details van [kopiëren van de prestaties van de activiteit](copy-activity-performance.md).

Dit artikel laat zien hoe u gegevens kopiëren van Data Factory-hulpprogramma voor **gegevens uit Azure SQL Database in Azure SQL Data Warehouse laden**. U kunt de stappen vergelijkbaar voor het kopiëren van gegevens van andere typen gegevensarchieven.

> [!NOTE]
>  Raadpleeg voor algemene informatie over de mogelijkheden van Data Factory bij het kopiëren van gegevens naar/van Azure SQL Data Warehouse [kopiëren van gegevens of naar Azure SQL Data Warehouse met behulp van Azure Data Factory](connector-azure-sql-data-warehouse.md) artikel.
>
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* **Azure SQL Data Warehouse**. Dit datawarehouse bevat de uit de SQL Database gekopieerde gegevens. Als u geen Azure SQL Data Warehouse hebt, raadpleegt u het artikel [Create a SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md) (Een Azure SQL-database maken) voor de stappen om er een te maken.
* **Azure SQL-database**. Deze zelfstudie kopieert gegevens van een Azure SQL Database met voorbeeldgegevens van Adventure Works LT, kunt u een volgende maken de [maken van een Azure SQL database](../sql-database/sql-database-get-started-portal.md) artikel. 
* **Een Azure Storage-account**. Azure Storage wordt gebruikt als **fasering** blob in de kopieerbewerking bulksgewijs. Als u geen Azure-opslagaccount hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) om een account te maken.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Klik op **NIEUW** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. In de **nieuwe gegevensfactory** pagina, de waarden opgeven, zoals wordt weergegeven in de volgende schermafbeelding:
      
     ![Nieuwe data factory-pagina](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
   * **Name.** Voer een globale unieke naam voor de data factory. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialDataFactory) en probeert u het opnieuw. Zie [Data Factory - naamgevingsregels](naming-rules.md) artikel voor naamgevingsregels voor Data Factory-artefacten.
  
            `Data factory name "LoadSQLDWDemo" is not available`

    * **Abonnement.** Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
    * **Resourcegroep.** Selecteer een bestaande resourcegroep in de vervolgkeuzelijst, of selecteer **nieuw** optie en typ de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Versie.** Selecteer **V2 (Preview)**.
    * **Locatie.** Selecteer de locatie voor de data factory. Alleen ondersteunde locaties worden weergegeven in de vervolgkeuzelijst. De gegevens worden opgeslagen die door data factory wordt gebruikt (Azure Storage, Azure SQL Database, etc.) kunnen zich in andere locaties/regio's. 

3. Klik op **Create**.
4. Nadat het maken voltooid, gaat u naar uw gegevensfactory is en u ziet de **Data Factory** pagina zoals in de afbeelding. Klik op **auteur & Monitor** tegel om de toepassing van de integratie gegevens op een afzonderlijke tabblad te starten.
   
   ![Startpagina van de gegevensfactory](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

## <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse

1. Klik in de pagina aan de slag, **gegevens kopiëren** tegel starten van het hulpprogramma voor kopiëren. 

   ![Kopiëren van gegevens hulpprogramma tegel](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. In de **eigenschappen** pagina opgeven van het hulpprogramma gegevens kopiëren **CopyFromSQLToSQLDW** voor de **taaknaam**, en klik op **volgende**. 

    ![Pagina met hulpprogramma gegevenseigenschappen kopiëren](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. In de **brongegevensarchief** pagina **Azure SQL Database**, en klik op **volgende**.

    ![Bronpagina data store](./media/load-azure-sql-data-warehouse/specify-source.png)
4. In de **opgeven van de Azure SQL database** pagina, voert u de volgende stappen uit: 
    1. Selecteer uw Azure SQL-server voor de **servernaam**.
    2. Selecteer uw Azure SQL database voor de **databasenaam**.
    3. Geef de naam van de gebruiker voor **gebruikersnaam**.
    4. Geef het wachtwoord van de gebruiker voor **wachtwoord**.
    5. Klik op **Volgende**. 

        ![Azure SQL-database opgeven](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. In de **Selecteer tabellen waarvan de gegevens kopiëren of gebruik een aangepaste query** pagina, filteren op de tabellen geven **SalesLT** in het invoervak, controleert u de **(Selecteer alle)** selectievakje alle tabellen selecteren en klik vervolgens op **volgende**. 

    ![Invoerbestand of de invoermap kiezen](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. In de **doelgegevensopslagplaats** pagina **Azure SQL Data Warehouse**, en klik op **volgende**. 

    ![Doelpagina data store](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. In de **opgeven van de Azure SQL Data Warehouse** pagina, voert u de volgende stappen uit: 

    1. Selecteer uw Azure SQL-server voor de **servernaam**.
    2. Selecteer uw Azure SQL Data Warehouse voor de **databasenaam**.
    3. Geef de naam van de gebruiker voor **gebruikersnaam**.
    4. Geef het wachtwoord van de gebruiker voor **wachtwoord**.
    5. Klik op **Volgende**. 

        ![Geef de Azure SQL datawarehouse](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. In de **tabeltoewijzing** pagina, controleren en klikt u op **volgende**. Een intelligente tabeltoewijzing wordt weergegeven dat op basis van tabelnamen doeltabellen maps bron. Als de tabel bestaat niet in maakt de bestemming Azure Data Factory standaard een met dezelfde naam. U kunt ook om toe te wijzen aan een bestaande tabel. 

    > [!NOTE]
    > Maken van de tabel automatisch voor SQL Data Warehouse sink is van toepassing wanneer SQL Server of Azure SQL Database bron. Als u gegevens uit andere brongegevensarchief kopiëren, moet op vooraf maakt u het schema in Azure SQL Data Warehouse sink voordat de gegevens opnieuw te kopiëren.

    ![Tabel toewijzingspagina](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. In de **schematoewijzing** pagina, controleren en klikt u op **volgende**. Intelligent toewijzing is gebaseerd op de kolomnaam. Als u laten Data Factory automatisch maken van de tabellen wilt, worden de juiste conversie van type kan gebeuren als die nodig zijn om op te lossen de incompatibiliteit tussen bron- en doelserver winkels. Als er een conversie van het type niet-ondersteunde gegevens tussen de bron- en doelserver kolom, ziet u een foutbericht weergegeven naast de bijbehorende tabel.

    ![Pagina schema-toewijzing](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. In de **instellingen** pagina, selecteert u een Azure Storage in de **opslagaccountnaam** vervolgkeuzelijst. Het wordt gebruikt voor het Faseren van de gegevens voordat deze wordt geladen in SQL Data Warehouse met PolyBase. Nadat de kopie is voltooid, wordt de tijdelijke gegevens in de opslag automatisch opgeschoond. 

    Schakel onder 'Geavanceerd' optie "standaard gebruiken".

    ![Pagina met instellingen](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. In de **samenvatting** pagina, controleert u de instellingen en klik op **volgende**.

    ![Overzichtspagina](./media/load-azure-sql-data-warehouse/summary-page.png)
13. In de **implementatie pagina**, klikt u op **Monitor** voor het bewaken van de pijplijn (taak).

    ![Implementatie-pagina](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. U ziet dat de **Monitor** tabblad aan de linkerkant wordt automatisch geselecteerd. U ziet de koppelingen naar details uitvoering van activiteiten bekijken en opnieuw uit te voeren van de pijplijn in de **acties** kolom. 

    ![De pijplijn bewaken wordt uitgevoerd](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Als u wilt weergeven die zijn gekoppeld aan de pijplijn uitvoeren activiteiten bij uitvoering **weergave activiteit wordt uitgevoerd** koppelen de **acties** kolom. Er zijn 10 kopie activiteiten in de pijplijn, elk één tabel van de gegevens gekopieerd. Overschakelen naar de pijplijn uitgevoerd weergeven, klikt u op **pijplijnen** koppeling aan de bovenkant. Klik op **vernieuwen** de lijst te vernieuwen. 

    ![Monitor activiteit wordt uitgevoerd](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. U kunt meer details van de uitvoering van de kopieeractiviteit van elke, controleren door te klikken op de **Details** koppeling onder **acties** in activiteit controle weergeven. Hier ziet u informatie zoals de hoeveelheid gegevens gekopieerd van bron naar sink, doorvoer, stappen het doorloopt met bijbehorende duur en configuraties gebruikt.

    ![Monitoractiviteit details uitvoering](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over Azure SQL Data Warehouse-ondersteuning: 

> [!div class="nextstepaction"]
>[Azure SQL Data Warehouse-connector](connector-azure-sql-data-warehouse.md)