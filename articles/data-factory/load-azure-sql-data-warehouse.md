---
title: Gegevens laden in Azure SQL Data Warehouse met behulp van Azure Data Factory | Microsoft Docs
description: Azure Data Factory gebruiken om gegevens te kopiëren in Azure SQL Data Warehouse
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: jingwang
ms.openlocfilehash: 6a7e0a27d3cda4193a04467d541f851a9e57fa46
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446712"
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Gegevens laden in Azure SQL Data Warehouse met behulp van Azure Data Factory

[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is een cloud-gebaseerde, scale-out-database die geschikt zijn van de verwerking van grote hoeveelheden gegevens, zowel relationele als niet-relationele is. SQL Data Warehouse is gebaseerd op de architectuur met krachtige parallelle verwerking (MPP) die geoptimaliseerd voor werkbelastingen van datawarehouses enterprise. Cloudflexibiliteit biedt de flexibiliteit om te schalen storage en compute-onafhankelijk van elkaar.

Aan de slag met Azure SQL Data Warehouse is nu eenvoudiger dan ooit bij het gebruik van Azure Data Factory. Azure Data Factory is een volledig beheerde cloud-gebaseerde service voor gegevensintegratie. U kunt de service gebruiken voor het vullen van een SQL Data Warehouse met gegevens uit uw bestaande systeem en bespaar tijd bij het bouwen van uw analyseoplossingen.

Azure Data Factory biedt de volgende voordelen voor het laden van gegevens in Azure SQL Data Warehouse:

* **Eenvoudig**: Een intuïtieve 5-stap-wizard met geen scripts vereist.
* **Uitgebreide gegevens store ondersteunen**: Ingebouwde ondersteuning voor een uitgebreide set on-premises en cloud-gebaseerde gegevensarchieven. Zie de tabel van voor een gedetailleerde lijst [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
* **Beveiligd en compatibel**: Gegevens worden overgedragen via HTTPS of ExpressRoute. De aanwezigheid van de algemene service zorgt ervoor dat uw gegevens nooit de geografische grens verlaat.
* **Ongeëvenaarde prestaties met behulp van PolyBase**: Polybase is de meest efficiënte manier om gegevens te verplaatsen naar Azure SQL Data Warehouse. De staging blob-functie gebruiken om een halen van hoge belasting van alle soorten gegevensarchieven, met inbegrip van Azure Blob-opslag en Data Lake Store. (Polybase Azure Blob storage en Azure Data Lake Store biedt standaard ondersteuning voor.) Zie voor meer informatie, [prestaties van de Kopieeractiviteit](copy-activity-performance.md).

Dit artikel laat u het gebruik van het hulpprogramma Data Factory kopiëren van gegevens naar _gegevens uit Azure SQL Database laden in Azure SQL Data Warehouse_. U kunt uitvoeren van gelijksoortige stappen als u wilt kopiëren van gegevens van andere soorten gegevensarchieven.

> [!NOTE]
> Zie voor meer informatie, [gegevens kopiëren naar of van Azure SQL Data Warehouse met behulp van Azure Data Factory](connector-azure-sql-data-warehouse.md).

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Azure SQL Data Warehouse: Het datawarehouse bevat de gegevens die uit de SQL-database gekopieerd. Als u een Azure SQL Data Warehouse hebt, raadpleegt u de instructies in [maken van een SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* Azure SQL Database: In deze zelfstudie worden gegevens gekopieerd van een Azure SQL-database met Adventure Works LT-voorbeeldgegevens. U kunt een SQL-database maken door de instructies in [maken van een Azure SQL database](../sql-database/sql-database-get-started-portal.md). 
* Azure Storage-account: Azure Storage wordt gebruikt als de _staging_ blob in de bulksgewijze kopieerbewerking. Als u geen Azure-opslagaccount hebt, raadpleegt u de instructies in [Een opslagaccount maken](../storage/common/storage-quickstart-create-account.md).

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het menu links **een resource maken** > **gegevens en analyses** > **Data Factory**: 
   
   ![Selectie van Data Factory in het deelvenster Nieuw](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. In de **nieuwe data factory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding:
      
   ![De pagina Nieuwe data factory](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
    * **Naam**: Voer een unieke naam op voor uw Azure-gegevensfactory. Als u de foutmelding ' naam Data factory \"LoadSQLDWDemo\" is niet beschikbaar, "Voer een andere naam voor de data factory. Bijvoorbeeld, kunt u de naam van de  _**uwnaam**_**ADFTutorialDataFactory**. Probeer het opnieuw maken van de data factory. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement waarin u wilt maken van de data factory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst of Selecteer de **nieuw** optie en voer de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Version**: Selecteer **V2**.
    * **Locatie**: Selecteer de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven die worden gebruikt door data factory, kunnen zich in andere locaties of regio's. Deze gegevensarchieven bevatten Azure Data Lake Store, Azure Storage en Azure SQL Database.

3. Selecteer **Maken**.
4. Na het aanmaken is voltooid, gaat u naar uw data factory. U ziet de **Data Factory** startpagina zoals wordt weergegeven in de volgende afbeelding:
   
   ![Startpagina van de gegevensfactory](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

   Selecteer de **Author & Monitor** tegel om te starten van de toepassing voor gegevensintegratie in een afzonderlijk tabblad.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse

1. In de **aan de slag** weergeeft, schakelt de **Copy Data** tegel om het hulpprogramma Copy Data starten:

   ![De tegel Copy Data-hulpprogramma](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
1. In de **eigenschappen** pagina **CopyFromSQLToSQLDW** voor de **taaknaam** veld en selecteer **volgende**:

    ![De pagina Eigenschappen](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)

1. In de **brongegevensarchief** pagina, de volgende stappen uit:

    a. Klik op **+ nieuwe verbinding maken**:

    ![De pagina Brongegevensarchief](./media/load-azure-sql-data-warehouse/new-source-linked-service.png)

    b. Selecteer **Azure SQL Database** uit de galerie, en selecteer **doorgaan**. U kunt 'SQL' typt in het zoekvak om te filteren van de connectors.

    ![Azure SQL DB selecteren](./media/load-azure-sql-data-warehouse/select-azure-sql-db-source.png)

    c. In de **nieuwe gekoppelde Service** pagina, selecteer uw servernaam en databasenaam in de vervolgkeuzelijst en geef de gebruikersnaam en wachtwoord. Klik op **verbinding testen** voor het valideren van de instellingen, selecteer **voltooien**.
   
    ![Azure SQL DB configureren](./media/load-azure-sql-data-warehouse/configure-azure-sql-db.png)

    d. Selecteer de zojuist gemaakte gekoppelde service als bron. Klik vervolgens op **Volgende**.

    ![Aan de bron gekoppelde service selecteren](./media/load-azure-sql-data-warehouse/select-source-linked-service.png)

1. In de **tabellen selecteren waaruit de gegevens kopiëren of een aangepaste query gebruiken** pagina **SalesLT** voor het filteren van de tabellen. Kies de **(Alles selecteren)** vak voor het gebruik van alle tabellen voor het kopiëren en selecteer vervolgens **volgende**: 

    ![Selecteer de brontabellen](./media/load-azure-sql-data-warehouse/select-source-tables.png)

1. In de **doelgegevensarchief** pagina, de volgende stappen uit:

    a. Klik op **+ Nieuwe verbinding maken** om een verbinding toe te voegen

    ![Sink-pagina brongegevensarchief](./media/load-azure-sql-data-warehouse/new-sink-linked-service.png)

    b. Selecteer **Azure SQL Data Warehouse** uit de galerie, en selecteer **volgende**.

    ![Selecteer Azure SQL DW](./media/load-azure-sql-data-warehouse/select-azure-sql-dw-sink.png)

    c. In de **nieuwe gekoppelde Service** pagina, selecteer uw servernaam en databasenaam in de vervolgkeuzelijst en geef de gebruikersnaam en wachtwoord. Klik op **verbinding testen** voor het valideren van de instellingen, selecteer **voltooien**.
   
    ![Configureren van Azure SQL DW](./media/load-azure-sql-data-warehouse/configure-azure-sql-dw.png)

    d. Selecteer de zojuist gemaakte gekoppelde service als sink. Klik vervolgens op **Volgende**.

    ![Aan sink gekoppelde service selecteren](./media/load-azure-sql-data-warehouse/select-sink-linked-service.png)

1. In de **tabeltoewijzing** pagina, Controleer de inhoud en selecteer **volgende**. Een intelligente tabeltoewijzing wordt weergegeven. De brontabellen zijn naar de doeltabellen op basis van de tabelnamen van de toegewezen. Als een brontabel niet in het doel bestaat, maakt Azure Data Factory een doeltabel met dezelfde naam standaard. U kunt ook een brontabel toewijzen aan een bestaande tabel van de bestemming. 

   > [!NOTE]
   > Automatische tabel maken voor de SQL Data Warehouse sink is van toepassing wanneer de bron SQL Server of Azure SQL Database. Als u gegevens van een andere bron-gegevensopslag kopieert, moet u het schema in de Azure SQL Data Warehouse-sink vooraf maken alvorens het kopiëren van gegevens.

   ![De pagina Tabeltoewijzing](./media/load-azure-sql-data-warehouse/table-mapping.png)

1. In de **schematoewijzing** pagina, Controleer de inhoud en selecteer **volgende**. De intelligente tabeltoewijzing is gebaseerd op de naam van de kolom. Als u Data Factory automatisch de tabellen te maken, worden de gegevensconversie van het type kan optreden wanneer er compatibiliteitsproblemen tussen de bron en bestemming zijn. Als er een niet-ondersteunde typeconversie tussen de kolom bron en bestemming, ziet u een foutbericht weergegeven naast de bijbehorende tabel.

    ![De pagina Schematoewijzing](./media/load-azure-sql-data-warehouse/schema-mapping.png)

1. In de **instellingen** pagina, de volgende stappen uit:

    a. In **instellingen voor fasering** sectie, klikt u op **+ nieuw** naar de nieuwe een tijdelijke opslag. De opslag wordt gebruikt voor het Faseren van de gegevens voordat deze wordt geladen in SQL Data Warehouse met behulp van PolyBase. Nadat de kopie voltooid is, wordt de tussentijdse gegevens in Azure Storage automatisch opgeschoond. 

    ![Fasering configureren](./media/load-azure-sql-data-warehouse/configure-staging.png)

    b. In de **nieuwe gekoppelde Service** pagina, selecteert u uw storage-account en selecteer **voltooien**.
   
    ![Azure Storage configureren](./media/load-azure-sql-data-warehouse/configure-blob-storage.png)

    c. In de **geavanceerde instellingen** sectie, schakelt u de **gebruik het standaardtype** optie en selecteer vervolgens **volgende**.

    ![PolyBase configureren](./media/load-azure-sql-data-warehouse/configure-polybase.png)

1. In de **samenvatting** pagina, Controleer de instellingen en selecteer **volgende**:

    ![Overzichtspagina](./media/load-azure-sql-data-warehouse/summary-page.png)
1. In de **implementatiepagina**, selecteer **Monitor** voor het bewaken van de pijplijn of taak:

    ![De pagina Implementatie](./media/load-azure-sql-data-warehouse/deployment-page.png)
1. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen om details uitvoering van activiteit weer te geven en de pijplijn opnieuw uitvoeren: 

    ![Pijplijnuitvoeringen controleren](./media/load-azure-sql-data-warehouse/pipeline-monitoring.png)
1. Als u uitvoeringen van activiteit die gekoppeld aan de pijplijnuitvoering zijn, selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. Overschakelen naar de vorige weergave, selecteert u de **pijplijnen** koppelen aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/load-azure-sql-data-warehouse/activity-monitoring.png)

1. Voor het controleren van de uitvoering van details voor elke activiteit kopiëren, selecteert u de **Details** koppeling onder **acties** in de activiteit controle weergeven. U kunt details controleren zoals de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met de bijbehorende tijdsduur en configuraties gebruikt:

    ![Details uitvoering van activiteit controleren](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over ondersteuning voor Azure SQL Data Warehouse: 

> [!div class="nextstepaction"]
>[Azure SQL Data Warehouse-connector](connector-azure-sql-data-warehouse.md)
