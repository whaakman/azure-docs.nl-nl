---
title: "Gegevens laden in Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: Deze zelfstudie gegevens laadt in Azure SQL Data Warehouse met behulp van Azure Data Factory en maakt gebruik van een SQL Server-database als de gegevensbron.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 6399f1a3390119685c1c9fd7332937e0cdb6f9ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Gegevens laden in SQL Data Warehouse met Data Factory

U kunt Azure Data Factory om gegevens te laden in Azure SQL Data Warehouse uit een van de [ondersteunde gegevensarchieven bron](../data-factory/copy-activity-overview.md). U kunt bijvoorbeeld gegevens uit een Azure SQL database of een Oracle-database laden in een SQL datawarehouse met behulp van de Data Factory. Zelfstudie in dit artikel wordt beschreven hoe u gegevens uit een on-premises SQL Server database laden in een SQL datawarehouse.

**Tijd schatting**: in deze zelfstudie duurt ongeveer 10-15 minuten worden voltooid als aan de vereisten is voldaan.

## <a name="prerequisites"></a>Vereisten

- U moet een **SQL Server-database** bij tabellen met de gegevens moeten worden gekopieerd naar de SQL datawarehouse.  

- U moet een online **SQL Data Warehouse**. Als u nog geen datawarehouse, Ontdek hoe u [maken van een Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).

- U moet een **Azure Storage-Account**. Als u nog geen opslagaccount, Ontdek hoe u [een opslagaccount maken](../storage/common/storage-create-storage-account.md). Zoek het storage-account en het datawarehouse in dezelfde Azure-regio voor de beste prestaties.

## <a name="configure-a-data-factory"></a>Een gegevensfactory configureren
1. Meld u aan bij [Azure Portal][].
2. Zoek uw datawarehouse en klikt u op om deze te openen.
3. Klik in de hoofdblade op **gegevens laden** > **Azure Data Factory**.

    ![Gegevens laden wizard starten](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. Als u een gegevensfactory niet in uw Azure-abonnement hebt, ziet u een **New Data Factory** dialoogvenster in een afzonderlijke tabblad van de browser. Vul de aangevraagde informatie en klik op **maken**. Nadat de gegevensfactory is gemaakt, de **New Data Factory** dialoogvenster wordt gesloten en u ziet de **Selecteer Data Factory** in het dialoogvenster.

    Als u een of meer gegevensfactory's al in de Azure-abonnement hebt, ziet u de **Selecteer Data Factory** in het dialoogvenster. In dit dialoogvenster, u kunt selecteren van een bestaande gegevensfactory of klik op **maken nieuwe gegevensfactory** naar een nieuwe maken.

    ![Data Factory configureren](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. In de **Selecteer Data Factory** in het dialoogvenster de **gegevens laden** optie is standaard geselecteerd. Klik op **volgende** om te beginnen met het maken van een taak bij het laden van gegevens.

## <a name="configure-the-data-factory-properties"></a>De data factory-eigenschappen configureren
Nu dat u een gegevensfactory gemaakt hebt, wordt de volgende stap is om de gegevens bij het laden van schema te configureren.

1. Voor **taaknaam**, voer **DWLoadData fromSQLServer**.
2. De standaard **eenmaal nu uitvoeren** optie, klikt u op **volgende**.

    ![Configureer load planning](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-the-source-data-store-and-gateway"></a>De gegevensopslag van de bron en de gateway configureren
U kunt nu de Data Factory over de lokale SQL Server-database van waaruit u gegevens wilt laden.

1. Kies **SQL Server** uit de ondersteunde brongegevens catalogus opslaan en klik op **volgende**.

    ![SQL Server-bron kiezen](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. Een **opgeven van de lokale SQL Server-database** dialoogvenster wordt weergegeven. De eerste **verbindingsnaam** veld wordt automatisch ingevuld. Het tweede veld vraagt om de naam van de **Gateway**. Als u een bestaande gegevensfactory waarop al een gateway gebruikt, kunt u de gateway hergebruiken door deze te selecteren in de lijst vervolgkeuzelijst. Klik op de **Gateway maken** koppeling voor het maken van een Data Management Gateway.  

    > [!NOTE]
    > Als de brongegevens opslaat on-premises of in een virtuele machine van Azure IaaS een Data Management Gateway is vereist. Een gateway heeft een 1-1-relatie met een data factory. Kan niet worden gebruikt uit een andere data factory, maar deze kan worden gebruikt door meerdere gegevens bij het laden van taken met in de dezelfde gegevensfactory. Een gateway kan worden gebruikt voor het verbinding maken met meerdere gegevensarchieven bij het uitvoeren van taken laden van gegevens.
    >
    > Zie voor gedetailleerde informatie over de gateway [Data Management Gateway](../data-factory/v1/data-factory-data-management-gateway.md) artikel.

3. Een **Gateway maken** dialoogvenster wordt weergegeven. Voer voor de naam, **GatewayForDWLoading**, en klik op **maken**.

4. Een **Gateway configureren** dialoogvenster wordt weergegeven. Klik op **snelle installatie op deze computer starten** automatisch downloaden, installeren en registreren van Data Management Gateway op de huidige computer. De voortgang wordt weergegeven in een pop-upvenster. Als de computer kan geen verbinding met de gegevensopslag maken, kunt u handmatig [download en installeer de gateway](https://www.microsoft.com/download/details.aspx?id=39717) op een computer die verbinding met de gegevens maken kan opslaan en vervolgens de sleutel gebruiken om u te registreren.
    > [!NOTE]
    > De snelle installatie werkt systeemeigen met Microsoft Edge en Internet Explorer. Als u Google Chrome gebruikt, moet u eerst de ClickOnce-extensie van de Chrome web store installeren.

    ![Snelle installatie starten](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. Wacht tot de gateway-installatie te voltooien. Zodra de gateway is geregistreerd en online is, wordt het pop-upvenster wordt gesloten en de nieuwe gateway wordt weergegeven in het veld met de gateway. Vervolgens vullen in de overige velden als volgt vereist, klikt u op **volgende**.
    - **Servernaam**: naam van de lokale SQL Server.
    - **Databasenaam**: SQL Server-database.
    - **Versleuteling van referenties**: Gebruik de standaardwaarde 'door webbrowser".
    - **Verificatietype**: Kies het type verificatie dat u gebruikt.
    - **Gebruikersnaam** en **wachtwoord**: Voer de gebruikersnaam en wachtwoord voor een gebruiker die is gemachtigd om de gegevens te kopiëren.

    ![Snelle installatie starten](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. De volgende stap is het kiezen van de tabellen van waaruit de gegevens te kopiëren. U kunt de tabellen met trefwoorden filteren. En u kunt het schema voor gegevens en de tabel in het onderste deelvenster bekijken. Nadat u hebt geselecteerd, klikt u op **volgende**.

    ![Selecteer tabellen](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a>Configureren van de bestemming, uw SQL Data Warehouse

U kunt Data Factory nu informatie over de bestemming.

1. De verbindingsinformatie voor SQL Data Warehouse wordt automatisch ingevuld. Voer het wachtwoord voor de gebruikersnaam. en klik op **volgende**.

    ![Doel configureren](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. Een intelligente tabeltoewijzing wordt weergegeven dat op basis van tabelnamen doeltabellen maps bron. Als de tabel bestaat niet in de doel-, standaard wordt ADF maken met dezelfde naam (dit geldt voor SQL Server of Azure SQL Database als bron). U kunt ook om toe te wijzen aan een bestaande tabel. Controleer en klik op **volgende**.

    ![Tabellen toewijzen](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. Bekijk de schematoewijzing en zoekt u fout- of waarschuwingsberichten. Intelligent toewijzing is gebaseerd op de kolomnaam. Als er een conversie van het type niet-ondersteunde gegevens tussen de bron- en doelserver kolom, ziet u een foutbericht weergegeven naast de bijbehorende tabel. Als u laten Data Factory automatisch maken van de tabellen wilt, worden de juiste conversie van type kan gebeuren als die nodig zijn om op te lossen de incompatibiliteit tussen bron- en doelserver winkels.

    ![Schema toewijzen](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. Klik op **Volgende**.

## <a name="configure-the-performance-settings"></a>De prestatie-instellingen configureren
In de configuraties van de prestaties, configureert u een Azure storage-account gebruikt voor het Faseren van de gegevens voordat deze wordt geladen in SQL Data Warehouse met behulp van sommen [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly). Nadat de kopie is voltooid, wordt de tijdelijke gegevens in de opslag automatisch opgeschoond.

Selecteer een bestaand Azure Storage-account en klikt u op **volgende**.

![Fasering blob configureren](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-the-pipeline"></a>Samenvattingsinformatie controleren en implementeer de pijplijn

Controleer de configuratie en klik op **voltooien** knop om de pijplijn te implementeren.

![Gegevensfactory implementeren](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a>Monitor gegevens voortgang van het laden

U ziet de voortgang van de implementatie en resultaten in de **implementatie** pagina.

1. Als de implementatie is voltooid, klikt u op de koppeling met de tekst **Klik hier om de pijplijn bewaken kopiëren** voor het bewaken van de voortgang van het laden van gegevens.

    ![De pijplijn bewaken](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. Het zojuist gemaakte **DWLoadData fromSQLServer** gegevens laden van de pijplijn wordt automatisch geselecteerd in de links **Resource Explorer**.

    ![Pijplijn voor weergeven](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. Klik in de pijplijn in het middelste deelvenster de gedetailleerde status voor elke tabel die is toegewezen aan een activiteit.

    ![Activiteit van de tabel weergeven](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. Verder op in een activiteit en ziet u de gegevens bij het laden van gegevens in het rechterpaneel met inbegrip van de gegevensgrootte, rijen, doorvoer, enzovoort.

    ![Details van activiteit tabel weergeven](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. Start deze bewakingsweergave hoger, gaat u naar uw SQL Data Warehouse, klikt u op **gegevens laden > Azure Data Factory**, selecteer uw gegevensfactory en kiezen **bewaken bestaande taken laden**.

## <a name="next-steps"></a>Volgende stappen

Zie voor het migreren van de database met SQL Data Warehouse, [Migratieoverzicht](sql-data-warehouse-overview-migrate.md).

Zie voor meer informatie over Azure Data Factory en de mogelijkheden van de verplaatsing van gegevens, de volgende artikelen:

- [Inleiding tot Azure Data Factory](../data-factory/introduction.md)
- [Verplaatsen van gegevens met behulp van kopieerbewerking](../data-factory/copy-activity-overview.md)
- [Move data to and from Azure SQL Data Warehouse using Azure Data Factory](../data-factory/connector-azure-sql-data-warehouse.md)

Als u wilt uw gegevens in SQL Data Warehouse verkennen, Zie de volgende artikelen:

- [Verbinding maken met SQL datawarehouse met Visual Studio en SSDT](sql-data-warehouse-query-visual-studio.md)
- [Visuele gegevens met Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).

<!-- Azure references -->
[Azure Portal]: https://portal.azure.com
