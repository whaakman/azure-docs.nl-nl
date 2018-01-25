---
title: Azure Portal gebruiken voor het maken van een Data Factory-pijplijn | Microsoft Docs
description: "Deze zelfstudie bevat stapsgewijze instructies voor het gebruik van een Azure-portal om een data factory te maken met een pijplijn. In dit voorbeeld gebruikt de pijplijn de kopieeractiviteit om gegevens vanuit een Azure-blobopslag naar een Azure SQL-database te kopiëren. "
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 424a5ec49018e969edbf90c374a9da7e1d22395d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Gegevens met Azure Data Factory kopiëren van Azure Blob Storage naar SQL Database
In deze zelfstudie maakt u een data factory met behulp van de Azure Data Factory-gebruikersinterface. Met de pijplijn in deze data factory worden gegevens gekopieerd van Azure Blob Storage naar Azure SQL Database. Het configuratiepatroon in deze zelfstudie geldt voor het kopiëren van een gegevensarchief op basis van bestanden naar een relationeel gegevensarchief. Zie de tabel [Ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensarchieven die worden ondersteund als bron en als sink.

> [!NOTE]
> - Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.
>
> - Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Pijplijn met een kopieeractiviteit aanmaken.
> * De uitvoering van de pijplijn testen
> * De pijplijn handmatig activeren
> * De pijplijn activeren volgens een schema
> * De uitvoering van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten
* **Azure-abonnement**. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* **Een Azure Storage-account**. U gebruikt de blob-opslag als **bron**-gegevensopslag. Als u geen Azure-opslagaccount hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) om een account te maken.
* **Azure SQL-database**. U gebruikt de database als **sink**-gegevensopslag. Als u geen Azure SQL-database hebt, raadpleegt u het artikel [Een Azure SQL-database maken](../sql-database/sql-database-get-started-portal.md) om een database te maken.

### <a name="create-a-blob-and-a-sql-table"></a>Een blob en een SQL-tabel maken

Voer nu de volgende stappen uit om Azure Blob Storage en Azure SQL Database voor te bereiden voor gebruik in deze zelfstudie:

#### <a name="create-a-source-blob"></a>Een bron-blob maken

1. Start Kladblok. Kopieer de volgende tekst en sla deze op uw schijf op in het bestand **emp.txt**.

    ```
    John,Doe
    Jane,Doe
    ```

2. Maak in de Azure-blobopslag een container met de naam **adftutorial**. Maak een map aan met de naam **invoer** in deze container. Vervolgens kunt u het bestand **emp.txt** uploaden naar de map **invoer**. Gebruik de Azure-portal of hulpprogramma's zoals [Azure Storage Explorer](http://storageexplorer.com/) voor deze taken.

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

2. Geef Azure-services toegang tot de SQL-server. Zorg ervoor dat **Toegang tot Azure-services toestaan** op **AAN** staat voor uw Azure SQL-server, zodat de Data Factory-service gegevens kan wegschrijven naar uw Azure SQL-server. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

    1. Klik op de hub **Meer services** aan de linkerkant en klik op **SQL-servers**.
    2. Selecteer uw server en klik op **Firewall** onder **INSTELLINGEN**.
    3. Klik op de pagina **Firewallinstellingen** op **AAN** bij **Toegang tot Azure-services toestaan**.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
In deze stap maakt u een data factory en start u de Azure Data Factory-gebruikersinterface om een pijplijn te maken in de data factory. 

1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
     ![De pagina Nieuwe data factory](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![De pagina Nieuwe data factory](./media/tutorial-copy-data-portal/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
        Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.      
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/tutorial-copy-data-portal/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
   ![Startpagina van de gegevensfactory](./media/tutorial-copy-data-portal/data-factory-home-page.png)
10. Klik op de tegel **Author & Monitor** om de gebruikersinterface (UI) van Azure Data Factory te openen in een afzonderlijk tabblad.

## <a name="create-a-pipeline"></a>Een pijplijn maken
In deze stap maakt u een pijplijn met een Kopieeractiviteit in de data factory. In dit voorbeeld kopieert de Kopieeractiviteit gegevens vanuit Azure Blob Storage naar Azure SQL Database. In de [Quickstart](quickstart-create-data-factory-portal.md) hebt u een pijplijn gemaakt met de volgende stappen:

1. Maak een gekoppelde service. 
2. Maak invoer- en uitvoergegevenssets.
3. Maak daarna een pijplijn.

In deze zelfstudie begint u met het maken van de pijplijn en gekoppelde services en gegevenssets wanneer u ze voor het configureren van de pijplijn nodig hebt. 

1. Klik op de pagina Aan de slag op de tegel **Pijplijn maken**. 

   ![Pijplijntegel maken](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
3. In het venster **Eigenschappen** wijzigt u de **naam** van de pijplijn in **CopyPipeline**.

    ![Naam pijplijn](./media/tutorial-copy-data-portal/pipeline-name.png)
4. Vouw in de **Activiteiten**-werkset de categorie **Gegevensstroom** uit. Gebruik vervolgens slepen-en-neerzetten om de **Kopieeractiviteit** uit de werkset te verplaatsen naar het ontwerpoppervlak voor pijplijnen. 

    ![De kopieeractiviteit slepen en neerzetten](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
5. Op het tabblad **Algemeen** van het venster **Eigenschappen** geeft u **CopyFromBlobToSql** op als de naam van de activiteit.

    ![Naam van de activiteit](./media/tutorial-copy-data-portal/activity-name.png)
6. Schakel over naar het tabblad **Bron**. Klik op **+ Nieuw** om een brongegevensset te maken. 

    ![Menu Nieuwe brongegevensset](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
7. Selecteer in het venster **Nieuwe gegevensset** de optie **Azure Blob Storage** en klik op **Voltooien**. De brongegevens bevinden zich in een Azure blobopslag, daarom selecteert u Azure Blob Storage voor de brongegevensset. 

    ![Azure Blob Storage selecteren](./media/tutorial-copy-data-portal/select-azure-storage.png)
8. U ziet een nieuw **tabblad** geopend in de toepassing met de titel **AzureBlob1**.

    ![Azure Blob1-tabblad ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
9. Geef in het tabblad **Algemeen** in het venster **Eigenschappen** onderaan **SourceBlobDataset** op als **naam**.

    ![Naam van de gegevensset](./media/tutorial-copy-data-portal/dataset-name.png)
10. Ga naar het tabblad **Verbinding** in het venster Eigenschappen.   

    ![Het tabblad Verbinding](./media/tutorial-copy-data-portal/source-dataset-connection-tab.png)
11. Klik op **+ Nieuw** naast het tekstvak **Gekoppelde service**. Een gekoppelde service verbindt een gegevensopslag of een rekenservice aan de data factory. In dit geval maakt u een Azure Storage-gekoppelde service om uw Azure Storage-account te koppelen aan de data factory. De gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-services gebruikt om tijdens runtime een verbinding met uw blobopslag tot stand te brengen. De gegevensset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens. 

    ![Knop Nieuwe gekoppelde service](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
12. Voer in het venster **Nieuwe gekoppelde service** de volgende stappen uit: 

    1. Voer **AzureStorageLinkedService** in als **Naam**. 
    2. Selecteer uw Azure Storage-account als de **Naam van het opslagaccount**.
    3. Als u de verbinding met het Azure Storage-account wilt testen, klikt u op **Verbinding testen**.  
    4. Klik op **Opslaan** om de gekoppelde service op te slaan.

        ![Nieuwe Azure Storage-gekoppelde service](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
13. Klik op de knop **Bladeren** voor het **Bestandspad**.  

    ![Knop Bladeren voor het bestand](./media/tutorial-copy-data-portal/file-browse-button.png)
14. Navigeer naar de map **adftutorial/input**, selecteer het bestand **emp.txt** en klik op **Voltooien**. U kunt ook kunt dubbelklikken op emp.txt. 

    ![Invoerbestand selecteren](./media/tutorial-copy-data-portal/select-input-file.png)
15. Controleer of de **Bestandsindeling** is ingesteld op **Tekstindeling** en **kolomscheidingsteken** is ingesteld op **Komma(`,`)**. Als het bronbestand gebruikmaakt van andere rij- en kolomscheidingstekens, klikt u op **Tekstindeling detecteren** voor het veld **Bestandsindeling**. Het hulpprogramma Copy Data detecteert de bestandsindeling en scheidingstekens automatisch voor u. U kunt deze waarden nog steeds overschrijven. U kunt gegevens op deze pagina bekijken door te klikken op **Voorbeeld van gegevens**.

    ![De tekstopmaak detecteren](./media/tutorial-copy-data-portal/detect-text-format.png)
17. Ga in het venster Eigenschappen naar het tabblad **Schema** en klik op **Schema importeren**. U ziet dat de toepassing twee kolommen detecteert in het bronbestand. U kunt hier het schema importeren, zodat u kolommen uit de gegevensopslag van de bron aan het sink-gegevensarchief kunt toewijzen. Als u geen kolommen hoeft toe te wijzen, kunt u deze stap overslaan. Importeer het schema voor deze tutorial.

    ![Het schema van de gegevensbron detecteren](./media/tutorial-copy-data-portal/detect-source-schema.png)  
19. Ga naar het **tabblad met de pijplijn** of klik op de pijplijn in de **structuurweergave** aan de linkerkant.  

    ![Tabblad Pijplijn](./media/tutorial-copy-data-portal/pipeline-tab.png)
20. Bevestig in het venster Eigenschappen dat de **SourceBlobDataset** voor het veld Brongegevensset is geselecteerd. U kunt gegevens op deze pagina bekijken door te klikken op **Voorbeeld van gegevens**. 
    
    ![Brongegevensset](./media/tutorial-copy-data-portal/source-dataset-selected.png)
21. Ga naar het tabblad **Sink** en klik op **Nieuw** om een sink-gegevensset aan te maken. 

    ![Menu Nieuwe sink-gegevensset](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
22. Selecteer in het venster **Nieuwe gegevensset** de optie **Azure SQL-database** en klik op **Voltooien**. In deze zelfstudie kopieert u gegevens naar een Azure SQL-database. 

    ![Azure SQL-database selecteren](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
23. Verander in het tabblad **Algemeen** van het venster Eigenschappen de naam in **OutputSqlDataset**. 
    
    ![Naam Uitvoergegevensset](./media/tutorial-copy-data-portal/output-dataset-name.png)
24. Ga naar het tabblad **Verbinding** en klik op **Nieuw** voor de **Gekoppelde service**. Een gegevensset moet worden gekoppeld aan een gekoppelde service. De gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-service gebruikt om tijdens runtime een verbinding met uw Azure SQL database tot stand te brengen. De dataset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens. 
    
    ![Knop Nieuwe gekoppelde service](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
25. Voer in het venster **Nieuwe gekoppelde service** de volgende stappen uit: 

    1. Voer **AzureSqlDatabaseLinkedService** in bij **Naam**. 
    2. Selecteer uw Azure SQL-server bij **Servernaam**.
    4. Selecteer uw Azure SQL-database bij **Databasenaam**. 
    5. Voer de gebruikersnaam in bij **Gebruikersnaam**. 
    6. Voer het wachtwoord voor de gebruiker in bij **Wachtwoord**. 
    7. Als u de verbinding wilt testen, klikt u op **Verbinding testen**.
    8. Klik op **Opslaan** om de gekoppelde service op te slaan. 
    
        ![Een nieuwe gekoppelde Azure SQL Database-service](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

26. Selecteer **[dbo].[emp]** als **Tabel**. 

    ![Emp-tabel selecteren](./media/tutorial-copy-data-portal/select-emp-table.png)
27. Schakel over naar het tabblad **Schema** en klik op Schema importeren. 

    ![Doelschema importeren](./media/tutorial-copy-data-portal/import-destination-schema.png)
28. Selecteer de kolom **ID** en klik op **Verwijderen**. De kolom ID is een identiteitskolom in de SQL-database, zodat de kopieeractiviteit geen gegevens hoeft in te voegen in deze kolom.

    ![De kolom ID verwijderen](./media/tutorial-copy-data-portal/delete-id-column.png)
30. Schakel over naar het tabblad met de **pijplijn**, en controleer of **OutputSqlDataset** is geselecteerd voor **Sink-gegevensset**.

    ![Tabblad Pijplijn](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
31. Ga in het venster Eigenschappen naar het tabblad **Toewijzing** en klik op **Schema’s importeren**. U ziet dat de eerste en tweede kolom in het bronbestand zijn toegewezen aan de velden **FirstName** en **LastName** in de SQL-database.

    ![Schema's toewijzen](./media/tutorial-copy-data-portal/map-schemas.png)
33. Valideer eerst de pijplijn door te klikken op de knop **Valideren** op de werkbalk. Klik op de **pijl-rechts** om het validatievenster te sluiten.

    ![Validatie-uitvoer voor pijplijn](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
34. Klik op de knop **Code** in de rechterhoek. U ziet de JSON-code die is gekoppeld aan de pijplijn. 

    ![Knop Code](./media/tutorial-copy-data-portal/code-button.png)
35. De JSON-code lijkt op het volgende codefragment:  

    ```json
    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToSql",
                    "type": "Copy",
                    "dependsOn": [],
                    "policy": {
                        "timeout": "7.00:00:00",
                        "retry": 0,
                        "retryIntervalInSeconds": 20
                    },
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "SqlSink",
                            "writeBatchSize": 10000
                        },
                        "enableStaging": false,
                        "parallelCopies": 0,
                        "cloudDataMovementUnits": 0,
                        "translator": {
                            "type": "TabularTranslator",
                            "columnMappings": "Prop_0: FirstName, Prop_1: LastName"
                        }
                    },
                    "inputs": [
                        {
                            "referenceName": "SourceBlobDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "OutputSqlDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ]
                }
            ]
        }
    }
    ```

## <a name="test-run-the-pipeline"></a>De uitvoering van de pijplijn testen
U kunt het uitvoeren van een pijplijn testen voordat u artefacten (gekoppelde services, gegevenssets en pijplijn) publiceert naar Data Factory (of) uw eigen VSTS GIT-opslagplaats. 

1. Klik in de werkbalk voor de pijplijn op **Testuitvoering**. De status van de pijplijnuitvoering wordt weergegeven op het tabblad **Uitvoer** van het venster onderaan. 

    ![Knop Testuitvoering](./media/tutorial-copy-data-portal/test-run-output.png)
2. Controleer of de gegevens uit het bronbestand zijn ingevoegd in de doel-SQL-database. 

    ![SQL-uitvoer verifiëren](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. Klik hiervoor in het linkerdeelvenster op **Publiceren**. In deze stap publiceert u entiteiten (gekoppelde services, gegevenssets en pijplijn) die u eerder hebt gemaakt met Azure Data Factory.

    ![De knop Publiceren](./media/tutorial-copy-data-portal/publish-button.png)
4. Wacht tot u het bericht **Gepubliceerd** ziet. Om meldingsberichten te zien, klikt u op het tabblad **Meldingen weergeven** op de linkerzijbalk. Sluit het meldingenvenster door op **X** te klikken.

    ![Meldingen tonen](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Codeopslagplaats configureren
U kunt de code die is gekoppeld aan uw data factory-artefacten naar een codeopslagplaats voor Visual Studio Team System (VSTS) publiceren. In deze stap maakt u de codeopslagplaats. 

Als u niet wilt werken met de VSTS-codeopslagplaats, kunt u deze stap overslaan en naar Data Factory blijven publiceren, net als bij de vorige stap. 

1. Klik op **Data Factory** in de linkerhoek (of) de pijl-omlaag ernaast en klik op **Codeopslagplaats configureren**. 

    ![Knop Code](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. Op de pagina **Instellingen opslagplaats** voert u de volgende stappen uit: 
    1. Selecteer **Visual Studio Team Services Git** voor het veld **Type opslagplaats**.
    2. Selecteer uw VSTS-account voor het veld **Visual Studio Team Services-account**.
    3. Selecteer een project in uw account VSTS voor de **Projectnaam**.
    4. Voer **Zelfstudie2** in om de **naam van de Git-opslagplaats** te koppelen aan uw data factory. 
    5. Bevestig dat de optie **Bestaande resources van Data Factory importeren naar opslagplaats** is geselecteerd. 
    6. Klik op **Opslaan** om de instellingen op te slaan. 

        ![Instellingen van de opslagplaats](./media/tutorial-copy-data-portal/repository-settings.png)
3. Bevestig dat **VSTS GIT** is geselecteerd voor de opslagplaats.

    ![VSTS GIT geselecteerd](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. In een afzonderlijk tabblad in de webbrowser, gaat u naar de **Tutorial2**-opslagplaats. U ziet hier twee vertakkingen: **master** en **adf_publish**.

    ![Master- en adf_publish-vertakkingen](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Controleer of de **JSON-bestanden** voor de Data Factory-entiteiten in de **master**-vertakking zijn.

    ![Bestanden in de master-vertakking](./media/tutorial-copy-data-portal/master-branch-files.png)
6. Controleer of de **JSON-bestanden** zich nog niet in de **adf_publish**-vertakking bevinden. 

    ![Bestanden in de vertakking adf_publish](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. Voeg een **beschrijving** voor de **pijplijn** toe en klik op **Opslaan** op de werkbalk. 

    ![Beschrijving voor de pijplijn toevoegen](./media/tutorial-copy-data-portal/pipeline-description.png)
8. U ziet nu een **vertakking** met uw gebruikersnaam in de opslagplaats **Zelfstudie2**. De wijziging is in uw eigen vertakking, niet in de master-vertakking. U kunt alleen entiteiten van de master-vertakking publiceren.

    ![Uw vertakking](./media/tutorial-copy-data-portal/your-branch.png)
9. Beweeg over de knop **Synchroniseren** (klik nog niet), selecteer de optie **Wijzigingen doorvoeren** en klik op de knop **Synchroniseren** om uw wijzigingen met de **master**-vertakking te synchroniseren. 

    ![Uw wijzigingen doorvoeren en synchroniseren](./media/tutorial-copy-data-portal/commit-and-sync.png)
9. Volg in het venster Veranderingen synchroniseren de volgende acties uit: 

    1. Bevestig dat **CopyPipeline** wordt weergegeven in de lijst met bijgewerkte pijplijnen.
    2. Bevestig dat **Wijzigingen publiceren na synchronisatie** is geselecteerd. Als u deze optie uitschakelt, synchroniseert u alleen de wijzigingen in uw vertakking met de master-vertakking, maar u publiceert ze niet naar de Data Factory-service. U kunt ze later publiceren met behulp van de knop **Publiceren**. Als u deze optie inschakelt, worden de wijzigingen eerst met de master gesynchroniseerd en vervolgens worden zij gepubliceerd naar de Data Factory-service.
    3. Klik op **Synchroniseren**. 

    ![Het venster Wijzigingen synchroniseren](./media/tutorial-copy-data-portal/sync-your-changes.png)
10. Nu ziet u de bestanden in de **adf_publish**-vertakking van de opslagplaats **Zelfstudie2**. U kunt ook het Azure Resource Manager-sjabloon voor uw Data Factory-oplossing in deze vertakking vinden.  

    ![Bestanden in de vertakking adf_publish](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>De pijplijn handmatig activeren
In deze stap moet u handmatig de pijplijn activeren, die u in de vorige stap heeft gepubliceerd. 

1. Klik in de werkbalk op **Activeren** en klik op **Nu activeren**. 

    ![Menu Nu activeren](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Ga naar het tabblad **Controleren** aan de linkerkant. U ziet een pijplijn die wordt geactiveerd door een handmatige trigger. U kunt de koppelingen in de Acties-kolom gebruiken om details van de activiteiten te bekijken en de pijplijn opnieuw uit te voeren.

    ![De uitvoering van de pijplijn controleren](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. Klik op de koppeling **Uitvoering van activiteiten weergeven** in de kolom **Acties** om de uitvoering van activiteiten weer te geven die zijn gekoppeld aan de pijplijnuitvoering. Omdat er in dit voorbeeld slechts één activiteit in de pijplijn is, ziet u slechts één vermelding in de lijst. Klik op de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Als u wilt terugkeren naar de **Pijplijnuitvoeringen**, klikt u op de koppeling **Pijplijnen** bovenaan. Als u de lijst wilt vernieuwen, klikt u op **Vernieuwen**.

    ![Uitvoeringen van activiteit bekijken](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Controleer of er twee extra rijen zijn toegevoegd aan de **emp**-tabel in de Azure SQL-database. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>De pijplijn activeren volgens een schema
In dit schema maakt u een planning-trigger voor de pijplijn. De trigger voert de pijplijn uit met de opgegeven planning (elk uur, dagelijks, enz.). In dit voorbeeld stelt u de trigger in om elke minuut tot en met de opgegeven einddatum/-tijd te worden uitgevoerd. 

1. Ga naar het tabblad **Bewerken** aan de linkerkant. 

    ![Tabblad Bewerken](./media/tutorial-copy-data-portal/edit-tab.png)
2. Klik op **Trigger**, en selecteer **Nieuw/bewerken**. Als de pijplijn niet actief is, schakel ernaar over. 

    ![Menu Trigger nieuw/bewerken](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. Klik op de pagina **Triggers toevoegen** op **Trigger kiezen...** , en klik op **+ Nieuw**. 

    ![Triggers toevoegen - nieuwe trigger](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. Voer in het venster **Nieuwe trigger** de volgende stappen uit: 

    1. Stel de **naam** in op **RunEveryMinute**.
    2. Selecteer **Op datum** voor **Einde**. 
    3. Klik op de vervolgkeuzelijst voor **Eindigt op**.
    4. Selecteer de **huidige dag**. De dag is standaard ingesteld op de volgende dag. 
    5. Update het gedeelte **minuten** naar een paar minuten na de huidige datum/tijd. De trigger wordt pas geactiveerd nadat u de wijzigingen publiceert. Als u het daarom slechts enkele minuten uit elkaar instelt en u deze dan niet publiceert, ziet u geen triggeruitvoering.  
    6. Klik op **Toepassen**. 

        ![Trigger-eigenschappen instellen](./media/tutorial-copy-data-portal/set-trigger-properties.png)
    7. Controleer de optie **Geactiveerd**. U kunt deze deactiveren en deze later activeren met behulp van dit selectievakje.
    8. Klik op **Volgende**.

        ![Trigger geactiveerd - volgende](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Er zijn kosten gekoppeld aan elke uitvoering van de pijplijn. Stel daarom op de juiste wijze de einddatum in. 
6. Lees de waarschuwing op de pagina **Triggeruitvoeringsparameters** en klik op **Voltooien**. De pijplijn in dit voorbeeld gebruikt geen parameters. 

    ![Pijplijnparameters](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
7. Klik op **Publiceren** voor het publiceren van de wijzigingen naar de opslagplaats. De trigger wordt niet daadwerkelijk geactiveerd totdat de publicatie is uitgevoerd. 

    ![Trigger publiceren](./media/tutorial-copy-data-portal/publish-trigger.png) 
8. Schakel over naar het tabblad **Controleren** aan de linkerkant om de geactiveerde pijplijnuitvoeringen te zien. 

    ![Geactiveerde pijplijnuitvoeringen](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
9. Als u wilt overschakelen van de weergave van pijplijnuitvoeringen naar de weergave van triggeruitvoeringen, klik dan op Pijplijnuitvoeringen en selecteer Triggeruitvoeringen.
    
    ![Menu Triggeruitvoeringen](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
10. U ziet de triggeruitvoeringen in een lijst. 

    ![Lijst Triggeruitvoeringen](./media/tutorial-copy-data-portal/trigger-runs-list.png)
11. Controleer of er twee rijen per minuut (voor elke pijplijnuitvoering) tot de opgegeven eindtijd in de **emp**-tabel worden ingevoegd. 

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Een data factory maken.
> * Pijplijn met een kopieeractiviteit aanmaken.
> * De uitvoering van de pijplijn testen
> * De pijplijn handmatig activeren
> * De pijplijn activeren volgens een schema
> * De uitvoering van de pijplijn en van de activiteit controleren.


Ga verder met de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
>[On-premises gegevens kopiëren naar de cloud](tutorial-hybrid-copy-portal.md)
