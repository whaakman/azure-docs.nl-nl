---
title: "Gegevens kopiëren met behulp van het hulpprogramma Azure Copy Data | Microsoft Docs"
description: "Maak een Azure-data factory aan en gebruik vervolgens het hulpprogramma Copy Data om gegevens uit Azure Blob Storage te kopiëren naar een Azure SQL Database."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: a8c7035ebf462bb168147e9d8eb60742333ce8b8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-copy-data-tool"></a>Gegevens met het hulpprogramma Copy Data kopiëren van Azure Blob naar SQL Database
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](tutorial-copy-data-tool.md)

In deze tutorial gebruikt u Azure Portal om een data factory te maken. Vervolgens gebruikt u het hulpprogramma Copy Data om een pijplijn te maken waarmee gegevens uit een map in een Azure Blob-opslag worden gekopieerd naar een Azure SQL-database. 

> [!NOTE]
> - Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.
>
> - Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken
> * De uitvoering van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* **Een Azure Storage-account**. U gebruikt de blob-opslag als **bron**-gegevensopslag. Als u geen Azure-opslagaccount hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) om een account te maken.
* **Azure SQL-database**. U gebruikt de database als **sink**-gegevensopslag. Als u geen Azure SQL-database hebt, raadpleegt u het artikel [Een Azure SQL-database maken](../sql-database/sql-database-get-started-portal.md) om een database te maken.

### <a name="create-a-blob-and-a-sql-table"></a>Een blob en een SQL-tabel maken

Voer nu de volgende stappen uit om Azure Blob Storage en Azure SQL Database voor te bereiden voor gebruik in deze zelfstudie:

#### <a name="create-a-source-blob"></a>Een bron-blob maken

1. Start Kladblok. Kopieer de volgende tekst en sla deze op schijf op in het bestand **inputEmp.txt**.

    ```
    John|Doe
    Jane|Doe
    ```

2. Gebruik hulpprogramma's zoals [Azure Opslagverkenner](http://storageexplorer.com/) om de container **adfv2tutorial** te maken en om het bestand **inputEmp.txt** te uploaden naar de container.

#### <a name="create-a-sink-sql-table"></a>Een SQL-sink-tabel maken

1. Gebruik het volgende SQL-script om de tabel **dbo.emp** te maken in de Azure SQL-database.

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

2. Geef Azure-services toegang tot de SQL-server. Zorg ervoor dat **Toegang tot Azure-services** aanstaat voor uw Azure SQL-server, zodat de Data Factory-service gegevens kunnen worden weggeschreven naar uw Azure SQL-server. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

    1. Klik op de hub **Meer services** aan de linkerkant en klik op **SQL-servers**.
    2. Selecteer uw server en klik op **Firewall** onder **INSTELLINGEN**.
    3. Klik op de pagina **Firewallinstellingen** op **AAN** bij **Toegang tot Azure-services toestaan**.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
     ![Pagina nieuwe data factory](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![De pagina Nieuwe data factory](./media/tutorial-copy-data-tool/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensopslag (Azure Storage, Azure SQL Database, enz.) en berekeningen (HDInsight, enz.) die worden gebruikt in Data Factory, kunnen zich op andere locaties/regio’s bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-copy-data-tool/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-copy-data-tool/data-factory-home-page.png)
10. Klik op de tegel **Author & Monitor** om de gebruikersinterface (UI) van Azure Data Factory te openen in een afzonderlijk tabblad. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Het hulpprogramma Copy Data gebruiken om een pijplijn te maken

1. Klik op de pagina Aan de slag op de tegel **Copy Data** om het hulpprogramma Copy Data op te starten. 

   ![De tegel Copy Data-hulpprogramma](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Geef op de pagina **Eigenschappen** van het hulpprogramma Copy Data **CopyFromBlobToSqlPipeline** op als de **Taaknaam** en klik op **Volgende**. Met het hulpprogramma Copy Data wordt een pijplijn gemaakt met de taaknaam die u opgeeft in dit veld. 

    ![Copy Data-hulpprogramma - pagina Eigenschappen](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Selecteer op de pagina **Brongegevensarchief** de optie **Azure Blob Storage** en klik op **Volgende**. De brongegevens bevinden zich in een Azure-blobopslag. 

    ![De pagina Brongegevensarchief](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. Voer op de pagina **Azure Blob-opslagaccount opgeven** de volgende stappen uit: 
    1. Voer **AzureStorageLinkedService** in als **verbindingsnaam**.
    2. Selecteer uw **opslagaccountnaam** uit de vervolgkeuzelijst.
    3. Klik op **Volgende**. 

        ![Blob-opslagaccount opgeven](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

        Een gekoppelde service verbindt een gegevensopslag of een rekenservice aan de data factory. In dit geval maakt u een Azure Storage-gekoppelde service om uw Azure Storage-account te koppelen aan de data factory. De gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-services gebruikt om tijdens runtime een verbinding met uw blobopslag tot stand te brengen. De gegevensset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens. 
5. Voer op de pagina **Het invoerbestand of de invoermap kiezen** de volgende stappen uit:
    
    1. Navigeer naar de map **adfv2tutorial/input**.
    2. Selecteer **inputEmp.txt**
    3. Klik op **Kiezen**. U kunt ook kunt dubbelklikken op **inputEmp.txt**. 
    4. Klik op **Volgende**. 

    ![Het invoerbestand of de invoermap kiezen](./media/tutorial-copy-data-tool/choose-input-file-folder.png)
6. Op de pagina **Instellingen bestandsindelingen**, ziet u ziet dat het hulpprogramma automatisch de scheidingstekens voor kolommen en rijen detecteert. Hier klikt u op **Volgende**. U kunt ook een voorbeeld van gegevens en het schema van de ingevoerde gegevens op deze pagina bekijken. 

    ![De pagina Instellingen bestandsindelingen](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. Op de pagina **Doelgegevensarchief** selecteert u **Azure SQL Database** en klikt u op **Volgende**. 

    ![De pagina Doelgegevensarchief](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. Voer op de pagina **Azure SQL-database opgeven** de volgende stappen uit: 

    1. Voer **AzureSqlDatabaseLinkedService** in bij **Verbindingsnaam**. 
    2. Selecteer uw Azure SQL-server bij **Servernaam**.
    3. Selecteer uw Azure SQL Database bij **Databasenaam**.
    4. Geef de naam van de gebruiker op bij **Gebruikersnaam**.
    5. Geef het wachtwoord van de gebruiker op bij **Wachtwoord**.
    6. Klik op **Volgende**. 

        ![Azure SQL-database opgeven](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

        Een gegevensset moet worden gekoppeld aan een gekoppelde service. De gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-service gebruikt om tijdens runtime een verbinding met uw Azure SQL database tot stand te brengen. De dataset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens.
9.  Selecteer op de pagina **Tabeltoewijzing** **[dbo].[ emp]**, en klik op **Volgende**. 

    ![De pagina Tabeltoewijzing](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. Op de pagina **Schematoewijzing** ziet u dat de eerste en tweede kolom in het invoerbestand zijn toegewezen aan de kolommen **FirstName** en **LastName** van de tabel **emp**. 

    ![De pagina Schematoewijzing](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. Klik op de pagina **Instellingen** op **Volgende**. 

    ![De pagina Instellingen](./media/tutorial-copy-data-tool/settings-page.png)
12. Bekijk op de **Overzichtspagina** de waarden voor alle instellingen en klik op **Volgende**.

    ![Overzichtspagina](./media/tutorial-copy-data-tool/summary-page.png)
13. Klik op de pagina **Implementatie** op **Controleren** om de pijplijn of taak die u hebt gemaakt, te controleren.

    ![De pagina Implementatie](./media/tutorial-copy-data-tool/deployment-page.png)
14. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. U ziet de koppelingen om details van de uitvoering van activiteiten te bekijken en de pijplijn opnieuw uit te voeren in de kolom **Acties**. Klik op **Vernieuwen** om de lijst te vernieuwen. 

    ![Pijplijnuitvoeringen controleren](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. Klik op de koppeling **Uitvoering van activiteiten weergeven** in de kolom **Acties** om de uitvoering van activiteiten weer te geven die zijn gekoppeld aan de pijplijnuitvoering. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Klik op de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Als u wilt terugkeren naar de vorige weergave, klikt u op de koppeling **Pipelines** bovenaan. Als u de lijst wilt vernieuwen, klikt u op **Vernieuwen**. 

    ![Uitvoering van activiteiten controleren](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Klik op het tabblad **Bewerken** aan de linkerkant om over te schakelen naar de bewerkingsmodus. U kunt de gekoppelde services, gegevenssets en pijplijnen die zijn gemaakt met het hulpprogramma, bijwerken met behulp van de editor. Klik op **Code** om de JSON-code weer te geven die is gekoppeld aan de entiteit die in de editor is geopend. Bekijk [de Azure Portal-versie van deze tutorial](tutorial-copy-data-portal.md) voor details over het bewerken van entiteiten in de Data Factory-UI.

    ![Tabblad Editor](./media/tutorial-copy-data-tool/edit-tab.png)
17. Controleer of dat de gegevens worden opgenomen in de tabel **emp** in uw Azure SQL-database. 

    ![SQL-uitvoer verifiëren](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kopieert de pijplijn gegevens vanuit een Azure-blobopslag naar een Azure SQL-database. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Een data factory maken.
> * Het hulpprogramma Copy Data gebruiken om een pijplijn te maken
> * De uitvoering van de pijplijn en van de activiteit controleren.

Ga verder met de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
>[On-premises gegevens kopiëren naar de cloud](tutorial-hybrid-copy-data-tool.md)