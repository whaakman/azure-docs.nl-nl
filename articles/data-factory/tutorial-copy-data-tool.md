---
title: Gegevens kopiëren met behulp van het hulpprogramma Azure Copy Data | Microsoft Docs
description: Maak een Azure-data factory aan en gebruik vervolgens het hulpprogramma Copy Data om gegevens uit Azure Blob Storage te kopiëren naar een SQL Database.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: d2f1d089c6a08a1dc90f82fd9d1c3cb2b6f6dc0a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-the-copy-data-tool"></a>Gegevens kopiëren van Azure Blob Storage naar een SQL-database met behulp van het hulpprogramma Copy Data
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Versie 1: algemeen verkrijgbaar](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](tutorial-copy-data-tool.md)

In deze zelfstudie gebruikt u Azure Portal om een gegevensfactory te maken. Vervolgens gebruikt u het hulpprogramma Copy Data om een pijplijn te maken waarmee gegevens uit Azure Blob Storage worden gekopieerd naar een SQL-database. 

> [!NOTE]
> Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.
>
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van Data Factory gebruikt, die algemeen beschikbaar is, raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken.
> * De uitvoering van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.
* **Azure-opslagaccount**: Blob Storage als het _brongegevensarchief_ gebruiken. Als u geen Azure-opslagaccount hebt, raadpleegt u de instructies in [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* **Azure SQL Database**: een SQL-database als de _sink-gegevensopslag_ gebruiken. Als u geen SQL-database hebt, raadpleegt u de instructies in [Een SQL Database maken](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Een blob en een SQL-tabel maken

Bereid uw Blob-opslag en de SQL-database voor voor gebruik tijdens de zelfstudie, door de volgende stappen uit te voeren.

#### <a name="create-a-source-blob"></a>Een bron-blob maken

1. Start **Kladblok**. Kopieer de volgende tekst en sla deze op schijf op in een bestand met de naam**inputEmp.txt**:

    ```
    John|Doe
    Jane|Doe
    ```

2. Maak een container met de naam **adfv2tutorial** en upload het bestand inputEmp.txt naar de container. U kunt verschillende hulpprogramma's gebruiken om deze taken uit te voeren, zoals [Azure Storage Explorer](http://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Een SQL-sink-tabel maken

1. Gebruik het volgende SQL-script om een tabel met de naam **dbo.emp** te maken in uw SQL-database:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Geef Azure-services toegang tot de SQL-server. Controleer of de instelling **Toegang tot Azure-services toestaan** is ingeschakeld voor de server waarop SQL Server wordt uitgevoerd. Met behulp van deze instelling kan Data Factory gegevens naar uw SQL Server-exemplaar schrijven. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

    a. Selecteer aan de linkerkant **Meer services** en selecteer vervolgens **SQL-servers**.

    b. Selecteer uw server en selecteer vervolgens **Instellingen** > **Firewall**.

    c. Stel op de pagina **Firewallinstellingen** de optie **Toegang tot Azure-services toestaan** in op **AAN**.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het linkermenu **+ Nieuw** > **Gegevens en analyses** > **Data Factory**: 
   
   ![Nieuwe data factory maken](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in bij **Naam**. 
      
     ![Nieuwe data factory](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de data factory moet _wereldwijd uniek_ zijn. Mogelijk wordt het volgende foutbericht weergegeven:
   
   ![Foutbericht nieuwe data factory](./media/tutorial-copy-data-tool/name-not-available-error.png)

   Als u een foutbericht ontvangt dat betrekking heeft op de waarde die bij de naam is ingevuld, voert u een andere naam in voor de data factory. Gebruik bijvoorbeeld de naam _**uwnaam**_**ADFTutorialDataFactory**. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
3. Selecteer het Azure-**abonnement** waarin u de nieuwe data factory wilt maken. 
4. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
    a. Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst.

    b. Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 
         
    Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.

5. Selecteer bij **Versie** de optie **V2 (preview-versie)** als de versie.
6. Selecteer bij **Locatie** de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven (bijvoorbeeld Azure Storage en SQL Database) en -berekeningen (bijvoorbeeld Azure HDInsight) die door uw data factory worden gebruikt, kunnen zich in andere locaties of regio's bevinden.
7. Selecteer **Vastmaken aan dashboard**. 
8. Selecteer **Maken**.
9. Op het dashboard wordt op de tegel **Data Factory implementeren** de processtatus weergegeven.

    ![Tegel Data Factory implementeren](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Nadat de data factory is gemaakt, wordt de startpagina **Data Factory** weergegeven.
   
    ![Startpagina van de gegevensfactory](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Selecteer de tegel **Author & Monitor** om de gebruikersinterface (UI) van Azure Data Factory te openen in een afzonderlijk tabblad. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Het hulpprogramma Copy Data gebruiken om een pijplijn te maken

1. Selecteer op de pagina **Aan de slag** de tegel **Copy Data** om het hulpprogramma Copy Data te starten. 

   ![De tegel Copy Data-hulpprogramma](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Op de pagina **Eigenschappen** bij **Taaknaam**, voert u **CopyFromBlobToSqlPipeline** in. Selecteer vervolgens **Volgende**. De gebruikersinterface van Data Factory maakt een pijplijn met de opgegeven taaknaam. 

    ![De pagina Eigenschappen](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Selecteer op de pagina **Brongegevensarchief** de optie **Azure Blob Storage** en selecteer vervolgens **Volgende**. De brongegevens bevinden zich in Blob Storage. 

    ![De pagina Brongegevensarchief](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. Voer op de pagina **Azure Blob-opslagaccount opgeven** de volgende stappen uit:

    a. Voer bij **Verbindingsnaam** **AzureStorageLinkedService** in.

    b. Selecteer uw opslagaccountnaam uit de vervolgkeuzelijst **Opslagaccountnaam**.

    c. Selecteer **Volgende**. 

    ![Geef het opslagaccount op.](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

    Een gekoppelde service verbindt een gegevensopslag of een rekenservice aan de data factory. In dit geval maakt u een opslaggekoppelde service om uw opslagaccount te koppelen aan de gegevensopslag. De gekoppelde service beschikt over de verbindingsgegevens die Data Factory gebruikt om tijdens runtime een verbinding met Blob Storage tot stand te brengen. De gegevensset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens. 

5. Voer op de pagina **Het invoerbestand of de invoermap kiezen** de volgende stappen uit:
    
    a. Blader naar de map **adfv2tutorial/input**.

    b. Selecteer het bestand **inputEmp.txt**.

    c. Selecteer **Kiezen**. U kunt ook kunt dubbelklikken op het bestand **inputEmp.txt**.

    d. Selecteer **Volgende**. 

    ![Kies het invoerbestand of invoermap.](./media/tutorial-copy-data-tool/choose-input-file-folder.png)

6. Op de pagina **Instellingen bestandsindelingen** ziet u dat het hulpprogramma automatisch de scheidingstekens voor kolommen en rijen detecteert. Selecteer **Volgende**. U kunt ook een voorbeeld van gegevens en het schema van de ingevoerde gegevens op deze pagina bekijken. 

    ![Pagina Instellingen bestandsindelingen](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. Op de pagina **Doelgegevensarchief** selecteert u **Azure SQL Database** en selecteert u vervolgens **Volgende**.

    ![Doelgegevensarchief](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. Voer op de pagina **Azure SQL-database opgeven** de volgende stappen uit: 

    a. Voer bij **Verbindingsnaam** **AzureSqlDatabaseLinkedService** in.

    b. Selecteer bij **Servernaam** uw SQL Server-exemplaar.

    c. Selecteer uw SQL-database bij **Databasenaam**.

    d. Voer bij **Gebruikersnaam** de naam van de gebruiker in.

    e. Voer bij **Wachtwoord** het wachtwoord voor de gebruiker in.

    f. Selecteer **Volgende**. 

    ![Geef de SQL-database op.](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

    Een gegevensset moet worden gekoppeld aan een gekoppelde service. De gekoppelde service beschikt over de verbindingsreeks die door Data Factory wordt gebruikt om tijdens runtime een verbinding met de SQL-database tot stand te brengen. De dataset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens.

9. Selecteer op de pagina **Tabeltoewijzing** de tabel **[dbo].[emp]** en selecteer vervolgens **Volgende**. 

    ![Tabeltoewijzing](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. Op de pagina **Schematoewijzing** ziet u dat de eerste en tweede kolom in het invoerbestand zijn toegewezen aan de kolommen **FirstName** en **LastName** van de tabel **emp**.

    ![De pagina Schematoewijzing](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. Selecteer op de pagina **Instellingen** de optie **Volgende**. 

    ![De pagina Instellingen](./media/tutorial-copy-data-tool/settings-page.png)
12. Bekijk op de **Overzichtspagina** de waarden voor alle instellingen en selecteer vervolgens **Volgende**.

    ![Overzichtspagina](./media/tutorial-copy-data-tool/summary-page.png)
13. Selecteer op de pagina **Implementatie** de optie **Controleren** om de pijplijn of taak te controleren.

    ![De pagina Implementatie](./media/tutorial-copy-data-tool/deployment-page.png)
14. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De kolom **Acties** bevat onder andere koppelingen om details van de uitvoering van activiteiten te bekijken en de pijplijn opnieuw uit te voeren. Selecteer **Vernieuwen** om de lijst te vernieuwen. 

    ![Pijplijnuitvoeringen controleren](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. Uitvoeringen van de activiteit die aan de pijplijn zijn gekoppeld, kunt u bekijken door de koppeling **Uitvoeringen van activiteit weergeven** in de kolom **Acties** te selecteren. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Selecteer de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Als u wilt terugkeren naar de weergave**Pijplijnuitvoeringen**, klikt u op de koppeling **Pijplijnen** bovenaan. Selecteer **Vernieuwen** om de weergave te vernieuwen. 

    ![Uitvoering van activiteiten controleren](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Selecteer het tabblad **Bewerken** aan de linkerkant om over te schakelen naar de bewerkingsmodus. U kunt de gekoppelde services, gegevenssets en pijplijnen die zijn gemaakt met het hulpprogramma, bijwerken met behulp van de editor. Selecteer **Code** om de JSON-code voor de entiteit weer te geven, die momenteel in de editor is geopend. Bekijk [de Azure Portal-versie van deze tutorial](tutorial-copy-data-portal.md) voor details over het bewerken van entiteiten in de Data Factory-UI.

    ![Tabblad Editor](./media/tutorial-copy-data-tool/edit-tab.png)
17. Controleer of de gegevens zijn opgenomen in de tabel **emp** in uw SQL-database.

    ![SQL-uitvoer verifiëren](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kopieert de pijplijn gegevens vanuit Blob Storage naar een SQL-database. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken.
> * De uitvoering van de pijplijn en van de activiteit controleren.

Ga verder met de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
>[On-premises gegevens kopiëren naar de cloud](tutorial-hybrid-copy-data-tool.md)
