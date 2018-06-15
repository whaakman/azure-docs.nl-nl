---
title: Een data factory-pijplijn maken met Azure Portal | Microsoft Docs
description: Deze zelfstudie bevat stapsgewijze instructies voor het maken van een data factory met een pijplijn met behulp van Azure Portal. De pijplijn gebruikt de kopieeractiviteit om gegevens vanuit Azure Blob-opslag naar een SQL database te kopiëren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 34c78a114c1d106c400a94941aa113153383e206
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30173335"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Gegevens kopiëren van Azure Blob-opslag naar een SQL database met Azure Data Factory
In deze zelfstudie maakt u een data factory met behulp van de Azure Data Factory-gebruikersinterface. Met de pijplijn in deze data factory worden gegevens gekopieerd van Azure Blob-opslag naar een SQL database. Het configuratiepatroon in deze zelfstudie geldt voor het kopiëren van een gegevensarchief op basis van bestanden naar een relationeel gegevensarchief. Zie de tabel [Ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensarchieven die worden ondersteund als bron en als sink.

> [!NOTE]
> - Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.
>
> - Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van Data Factory gebruikt, die algemeen beschikbaar is, raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een pijplijn met kopieeractiviteit maken.
> * De uitvoering van de pijplijn testen.
> * De pijplijn handmatig activeren.
> * De pijplijn activeren volgens een schema.
> * De uitvoering van de pijplijn en van de activiteit controleren.

## <a name="prerequisites"></a>Vereisten
* **Azure-abonnement**. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* **Azure-opslagaccount**. U gebruikt de blobopslag als *bron*-gegevensopslag. Als u geen opslagaccount hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) om een account te maken.
* **Azure SQL-database**. U gebruikt de database als *sink*-gegevensopslag. Als u geen SQL database hebt, raadpleegt u het artikel [Een SQL-database maken](../sql-database/sql-database-get-started-portal.md) om een database te maken.

### <a name="create-a-blob-and-a-sql-table"></a>Een blob en een SQL-tabel maken

Voer nu de volgende stappen uit om uw blobopslag en SQL database voor te bereiden voor gebruik in deze zelfstudie.

#### <a name="create-a-source-blob"></a>Een bron-blob maken

1. Start Kladblok. Kopieer de volgende tekst en sla deze op uw schijf op in het bestand **emp.txt**:

    ```
    John,Doe
    Jane,Doe
    ```

2. Maak in de blobopslag een container met de naam **adftutorial**. Maak een map aan met de naam **invoer** in deze container. Vervolgens kunt u het bestand **emp.txt** uploaden naar de map **invoer**. Gebruik de Azure-portal of hulpprogramma's zoals [Azure Storage Explorer](http://storageexplorer.com/) voor deze taken.

#### <a name="create-a-sink-sql-table"></a>Een SQL-sink-tabel maken

1. Gebruik het volgende SQL-script om de tabel **dbo.emp** te maken in uw SQL database:

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

2. Geef Azure-services toegang tot SQL Server. Zorg ervoor dat **Toegang tot Azure-services toestaan** is **ingeschakeld** voor SQL Server, zodat Data Factory gegevens naar SQL Server kan schrijven. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

    a. Selecteer aan de linkerkant **Meer services** > **SQL-servers**.

    b. Selecteer uw server en klik onder **INSTELLINGEN** op **Firewall**.

    c. Op de pagina **Firewallinstellingen** schakelt u **Toegang tot Azure-services toestaan** **in**.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
In deze stap maakt u een data factory en start u de Data Factory-gebruikersinterface om een pijplijn te maken in de data factory. 

1. Open de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Selecteer in het linkermenu **Nieuw** > **Gegevens en analyses** > **Data Factory**. 
  
   ![Nieuwe data factory maken](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
3. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in bij **Naam**. 
      
     ![Nieuwe data factory](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet *wereldwijd uniek* zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie [Data Factory naming rules](naming-rules.md) (Naamgevingsregels Data Factory) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
   ![Foutbericht](./media/tutorial-copy-data-portal/name-not-available-error.png)
4. Selecteer het Azure-**abonnement** waarin u de data factory wilt maken. 
5. Voer een van de volgende stappen uit voor **Resourcegroep**:
     
    a. Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst.

    b. Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in. 
         
    Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie. 
6. Selecteer **V2 (preview-versie)** bij **Versie**.
7. Selecteer onder **Locatie** een locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven (bijvoorbeeld Azure Storage en SQL Database) en berekenservices (bijvoorbeeld Azure HDInsight) die door de data factory worden gebruikt, kunnen zich in andere regio's bevinden.
8. Selecteer **Vastmaken aan dashboard**. 
9. Selecteer **Maken**. 
10. Op het dashboard ziet u de volgende tegel met de status: **Data Factory implementeren**: 

    ![Tegel Data Factory implementeren](media/tutorial-copy-data-portal/deploying-data-factory.png)
11. Na het maken ziet u de pagina **Data factory** zoals hieronder wordt weergegeven.
   
    ![Startpagina van de gegevensfactory](./media/tutorial-copy-data-portal/data-factory-home-page.png)
12. Selecteer de tegel **Maken en controleren** om de Data Factory-gebruikersinterface te openen op een afzonderlijk tabblad.

## <a name="create-a-pipeline"></a>Een pijplijn maken
In deze stap maakt u een pijplijn met kopieeractiviteit in de data factory. De kopieeractiviteit kopieert gegevens van de blobopslag naar SQL Database. In de [Quickstart](quickstart-create-data-factory-portal.md) hebt u een pijplijn gemaakt met de volgende stappen:

1. Maak een gekoppelde service. 
2. Maak invoer- en uitvoergegevenssets.
3. Maak een pijplijn.

In deze zelfstudie begint u met het maken van de pijplijn. Vervolgens maakt u gekoppelde services en gegevenssets wanneer u deze nodig hebt om de pijplijn te configureren. 

1. Selecteer op de pagina **Aan de slag** de optie **Pijplijn maken**. 

   ![Pijplijn maken](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
2. In het venster **Eigenschappen** van de pijplijn geeft u **CopyPipeline** op bij **Naam**.

    ![Naam pijplijn](./media/tutorial-copy-data-portal/pipeline-name.png)
3. Breid in de werkset **Activiteiten** de categorie **Gegevensstroom** uit. Sleep de **kopieeractiviteit** uit de werkset en zet deze neer op het ontwerpoppervlak voor pijplijnen. 

    ![Kopieeractiviteit](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
4. Op het tabblad **Algemeen** van het venster **Eigenschappen** geeft u **CopyFromBlobToSql** op als naam van de activiteit.

    ![Naam van de activiteit](./media/tutorial-copy-data-portal/activity-name.png)
5. Ga naar het tabblad **Bron**. Selecteer **+Nieuw** om een brongegevensset te maken. 

    ![Tabblad Bron](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
6. Selecteer in het venster **Nieuwe gegevensset** de optie **Azure Blob-opslag** en selecteer vervolgens **Voltooien**. De brongegevens bevinden zich in een blobopslag, daarom selecteert u **Azure Blob-opslag** voor de brongegevensset. 

    ![Opslagselectie](./media/tutorial-copy-data-portal/select-azure-storage.png)
7. U ziet een nieuw tabblad dat in de toepassing is geopend met de titel **AzureBlob1**.

    ![Tabblad AzureBlob1 ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
8. Voer op het tabblad **Algemeen** onder in het venster **Eigenschappen** bij **Naam** **SourceBlobDataset** in.

    ![Naam van de gegevensset](./media/tutorial-copy-data-portal/dataset-name.png)
9. Ga naar het tabblad **Verbinding** van het venster **Eigenschappen**. Klik naast het tekstvak **Gekoppelde service** op **+Nieuw**. 

    Een gekoppelde service verbindt een gegevensopslag of een rekenservice aan de data factory. In dit geval maakt u een Azure Storage-gekoppelde service om uw opslagaccount te koppelen aan de gegevensopslag. De gekoppelde service beschikt over de verbindingsgegevens die Data Factory gebruikt om tijdens runtime een verbinding met Blob Storage tot stand te brengen. De gegevensset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens. 

    ![Knop Nieuwe gekoppelde service](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
10. Voer in het venster **Nieuwe gekoppelde service** de volgende stappen uit: 

    a. Voer bij **Naam** **AzureStorageLinkedService** in. 

    b. Selecteer uw opslagaccount bij **Naam van opslagaccount**.

    c. Als u de verbinding met het opslagaccount wilt testen, selecteert u **Verbinding testen**.

    d. Selecteer **Opslaan** om de gekoppelde service op te slaan.

    ![Nieuwe gekoppelde service](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
11. Selecteer naast **Bestandspad** de knop **Bladeren**.

    ![Knop Bladeren voor bestandspad](./media/tutorial-copy-data-portal/file-browse-button.png)
12. Ga naar de map **adftutorial/input**, selecteer het bestand **emp.txt** en klik vervolgens op **Voltooien**. U kunt ook dubbelklikken op **emp.txt**. 

    ![Invoerbestand selecteren](./media/tutorial-copy-data-portal/select-input-file.png)
13. Controleer of **Bestandsindeling** is ingesteld op **Tekstindeling** en dat **Kolomscheidingsteken** is ingesteld op **Komma (`,`)**. Als het bronbestand gebruikmaakt van andere rij- en kolomscheidingstekens, kunt u **Tekstindeling detecteren** selecteren voor **Bestandsindeling**. Het hulpprogramma Copy Data detecteert de bestandsindeling en scheidingstekens automatisch voor u. U kunt deze waarden nog steeds overschrijven. Selecteer **Gegevens vooraf bekijken** om een voorbeeld van de gegevens op deze pagina te bekijken.

    ![De tekstopmaak detecteren](./media/tutorial-copy-data-portal/detect-text-format.png)
14. Ga in het venster **Eigenschappen** naar het tabblad **Schema** en selecteer **Schema importeren**. U ziet dat de toepassing twee kolommen detecteert in het bronbestand. U kunt hier het schema importeren zodat u kolommen uit de gegevensopslag van de bron aan de sink-gegevensopslag kunt toewijzen. Als u geen kolommen hoeft toe te wijzen, kunt u deze stap overslaan. Importeer het schema voor deze tutorial.

    ![Het schema van de gegevensbron detecteren](./media/tutorial-copy-data-portal/detect-source-schema.png)  
15. Ga vervolgens naar het tabblad met de pijplijn of selecteer de pijplijn aan de linkerkant.

    ![Tabblad Pijplijn](./media/tutorial-copy-data-portal/pipeline-tab.png)
16. Controleer in het venster **Eigenschappen** of **SourceBlobDataset** is geselecteerd als **Brongegevensset**. Selecteer **Gegevens vooraf bekijken** om een voorbeeld van de gegevens op deze pagina te bekijken. 
    
    ![Brongegevensset](./media/tutorial-copy-data-portal/source-dataset-selected.png)
17. Ga naar het tabblad **Sink** en selecteer **+Nieuw** om een sink-gegevensset te maken. 

    ![Sink-gegevensset](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
18. Selecteer in het venster **Nieuwe gegevensset** de optie **Azure SQL Database** en selecteer vervolgens **Voltooien**. In deze zelfstudie kopieert u gegevens naar een SQL database. 

    ![SQL database selecteren](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
19. Op het tabblad **Algemeen** van het venster **Eigenschappen** geeft u **OutputSqlDataset** op als **Naam**. 
    
    ![Naam Uitvoergegevensset](./media/tutorial-copy-data-portal/output-dataset-name.png)
20. Ga naar het tabblad **Verbinding** en selecteer naast **Gekoppelde service** de optie **+Nieuw**. Een gegevensset moet worden gekoppeld aan een gekoppelde service. De gekoppelde service beschikt over de verbindingsreeks die door Data Factory wordt gebruikt om tijdens runtime een verbinding met de SQL-database tot stand te brengen. De dataset geeft informatie over de container, map en het bestand (optioneel) met de brongegevens. 
    
    ![Gekoppelde service](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
21. Voer in het venster **Nieuwe gekoppelde service** de volgende stappen uit: 

    a. Geef **AzureSqlDatabaseLinkedService** op als **Naam**.

    b. Selecteer bij **Servernaam** uw SQL Server-exemplaar.

    c. Selecteer uw SQL-database bij **Databasenaam**.

    d. Voer bij **Gebruikersnaam** de naam van de gebruiker in.

    e. Voer bij **Wachtwoord** het wachtwoord voor de gebruiker in.

    f. Als u de verbinding wilt testen, selecteert u **Verbinding testen**.

    g. Selecteer **Opslaan** om de gekoppelde service op te slaan. 
    
    ![Nieuwe gekoppelde service opslaan](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

22. Selecteer bij **Tabel** **[dbo].[emp]**. 

    ![Tabel](./media/tutorial-copy-data-portal/select-emp-table.png)
23. Ga naar het tabblad **Schema** en selecteer **Schema importeren**. 

    ![Schema importeren selecteren](./media/tutorial-copy-data-portal/import-destination-schema.png)
24. Selecteer de kolom **Id** en vervolgens **Verwijderen**. De kolom **Id** is een identiteitskolom in de SQL database, zodat de kopieeractiviteit geen gegevens hoeft in te voegen in deze kolom.

    ![De kolom ID verwijderen](./media/tutorial-copy-data-portal/delete-id-column.png)
25. Ga naar het tabblad met de pijplijn en controleer bij **Sink-gegevensset** of **OutputSqlDataset** is geselecteerd.

    ![Tabblad Pijplijn](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
26. Ga naar het tabblad **Toewijzing** onder in het venster **Eigenschappen** en selecteer **Schema's importeren**. U ziet dat de eerste en tweede kolom in het bronbestand zijn toegewezen aan **FirstName** en **LastName** in de SQL database.

    ![Schema's toewijzen](./media/tutorial-copy-data-portal/map-schemas.png)
27. Selecteer **Valideren** om de pijplijn te valideren. Selecteer rechtsboven de pijl-rechts om het validatievenster te sluiten.

    ![Validatie-uitvoer voor pijplijn](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
28. Selecteer in de rechterbovenhoek **Code**. U ziet de JSON-code die is gekoppeld aan de pijplijn. 

    ![Knop Code](./media/tutorial-copy-data-portal/code-button.png)
29. U ziet JSON-code die lijkt op het volgende codefragment: 

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
U kunt de uitvoering van een pijplijn testen voordat u artefacten (gekoppelde services, gegevenssets en pijplijn) publiceert naar Data Factory of uw eigen Visual Studio Team Services Git-opslagplaats. 

1. Selecteer in de werkbalk **Testuitvoering** om de uitvoering van de pijplijn te testen. De status van de pijplijnuitvoering wordt weergegeven op het tabblad **Uitvoer** onder in het venster. 

    ![Pijplijn testen](./media/tutorial-copy-data-portal/test-run-output.png)
2. Controleer of de gegevens uit het bronbestand zijn ingevoegd in de doel-SQL-database. 

    ![SQL-uitvoer verifiëren](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. Selecteer **Alles publiceren** in het linkerdeelvenster. Met deze actie publiceert u entiteiten (gekoppelde services, gegevenssets en pijplijnen) die u hebt gemaakt met Data Factory.

    ![Publiceren](./media/tutorial-copy-data-portal/publish-button.png)
4. Wacht tot u het bericht **Gepubliceerd** ziet. Als u meldingsberichten wilt zien, selecteert u in de linkerzijbalk **Meldingen weergeven**. Selecteer **Sluiten** als u het meldingenvenster wilt sluiten.

    ![Meldingen tonen](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Codeopslagplaats configureren
U kunt de code die is gekoppeld aan uw data factory-artefacten naar een codeopslagplaats voor Visual Studio Team Services publiceren. In deze stap maakt u de codeopslagplaats.  Zie [Author with VSTS Git integration](author-visually.md#author-with-vsts-git-integration) (Creëren met VSTS Git-integratie) voor meer informatie over creëren met visuals.

Als u niet wilt werken met een codeopslagplaats voor Visual Studio Team Services, kunt u deze stap overslaan. U kunt publiceren naar Data Factory zoals u in de vorige stap hebt gedaan. 

1. Selecteer linksboven **Data Factory** of klik op de pijl-omlaag ernaast en selecteer **Codeopslagplaats configureren**. 

    ![Codeopslagplaats configureren](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. Op de pagina **Instellingen opslagplaats** voert u de volgende stappen uit:

    a. Selecteer bij **Type opslagplaats** de optie **Visual Studio Team Services Git**.

    b. Selecteer bij **Visual Studio Team Services-account** uw Visual Studio Team Services-account.

    c. Selecteer bij **Projectnaam** een project in uw Visual Studio Team Services-account.

    d. Geef **Tutorial2** op als **naam van de Git-opslagplaats** om de Git-opslagplaats te koppelen aan uw data factory.

    e. Controleer of het selectievakje **Bestaande resources van Data Factory importeren naar opslagplaats** is aangevinkt.

    f. Selecteer **Opslaan** om de instellingen op te slaan. 

    ![Instellingen van de opslagplaats](./media/tutorial-copy-data-portal/repository-settings.png)
3. Bevestig dat **VSTS GIT** is geselecteerd voor de opslagplaats.

    ![VSTS GIT selecteren](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. Open op een ander tabblad in de webbrowser de opslagplaats **Tutorial2**. U ziet twee vertakkingen: **adf_publish** en **master**.

    ![Master- en adf_publish-vertakkingen](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Controleer of de JSON-bestanden voor de Data Factory-entiteiten in de **master**-vertakking staan.

    ![Bestanden in de master-vertakking](./media/tutorial-copy-data-portal/master-branch-files.png)
6. Controleer of de JSON-bestanden nog niet in de **adf_publish**-vertakking staan. 

    ![Bestanden in de vertakking adf_publish](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. Voeg bij **Beschrijving** een beschrijving toe voor de pijplijn en selecteer **Opslaan** in de werkbalk. 

    ![Beschrijving van pijplijn](./media/tutorial-copy-data-portal/pipeline-description.png)
8. U ziet nu een vertakking met uw gebruikersnaam in de opslagplaats **Tutorial2**. De wijziging is in uw eigen vertakking, niet in de master-vertakking. U kunt entiteiten alleen publiceren vanuit de master-vertakking.

    ![Uw vertakking](./media/tutorial-copy-data-portal/your-branch.png)
9. Beweeg de muisaanwijzer over de knop **Synchroniseren** (klik er nog niet op), vink het selectievakje **Wijzigingen doorvoeren** aan en selecteer **Synchroniseren** om uw wijzigingen met de master-vertakking te synchroniseren. 

    ![Wijzigingen doorvoeren en synchroniseren](./media/tutorial-copy-data-portal/commit-and-sync.png)
10. Voer in het venster **Wijzigingen synchroniseren** de volgende stappen uit: 

    a. Bevestig dat **CopyPipeline** wordt weergegeven in de lijst met bijgewerkte **pijplijnen**.

    b. Bevestig dat **Wijzigingen publiceren na synchronisatie** is geselecteerd. Als u deze optie uitschakelt, worden de wijzigingen in uw vertakking alleen gesynchroniseerd met de master-vertakking. Ze worden nog niet gepubliceerd naar Data Factory. U kunt ze later publiceren met behulp van de knop **Publiceren**. Als u deze optie echter inschakelt, worden de wijzigingen eerst gesynchroniseerd met de master-vertakking en vervolgens gepubliceerd naar Data Factory.

    c. Selecteer **Synchroniseren**. 

    ![Wijzigingen synchroniseren](./media/tutorial-copy-data-portal/sync-your-changes.png)
11. Nu ziet u de bestanden in de **adf_publish**-vertakking van de opslagplaats **Zelfstudie2**. In deze vertakking vindt u ook de Azure Resource Manager-sjabloon voor uw Data Factory-oplossing. 

    ![Bestandslijst in de vertakking adf_publish](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>De pijplijn handmatig activeren
In deze stap moet u handmatig de pijplijn activeren, die u in de vorige stap heeft gepubliceerd. 

1. Selecteer op de werkbalk de optie **Activeren** en selecteer vervolgens **Nu activeren**. Klik op de pagina **Pijplijnuitvoering** op **Voltooien**.  

    ![Pijplijn activeren](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Ga naar het tabblad **Controleren** aan de linkerkant. U ziet een pijplijn die wordt geactiveerd door een handmatige trigger. U kunt via de links in de kolom **Acties** details van de activiteiten bekijken en de pijplijn opnieuw uitvoeren.

    ![Pijplijnuitvoeringen controleren](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. Selecteer de link **Uitvoeringen van activiteit weergeven** in de kolom **Acties** om de activiteituitvoeringen te zien die zijn gekoppeld aan de pijplijnuitvoering. Omdat er in dit voorbeeld slechts één activiteit in de pijplijn is, ziet u slechts één vermelding in de lijst. Selecteer de koppeling **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Als u wilt terugkeren naar de weergave**Pijplijnuitvoeringen**, selecteert u **Pijplijnen** bovenaan. Selecteer **Vernieuwen** om de weergave te vernieuwen.

    ![Uitvoering van activiteiten controleren](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Controleer of er twee extra rijen zijn toegevoegd aan de **emp**-tabel in de SQL database. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>De pijplijn activeren volgens een schema
In dit schema maakt u een planningstrigger voor de pijplijn. De trigger voert de pijplijn uit volgens de opgegeven planning, bijvoorbeeld elk uur of dagelijks. In dit voorbeeld stelt u de trigger in om elke minuut tot en met de opgegeven einddatum/-tijd te worden uitgevoerd. 

1. Ga naar het tabblad **Bewerken** aan de linkerkant. 

    ![Tabblad Bewerken](./media/tutorial-copy-data-portal/edit-tab.png)
2. Klik op **Trigger** en selecteer **Nieuw/bewerken**. Als de pijplijn niet actief is, gaat u erheen. 

    ![Trigger-optie](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. Selecteer in het venster **Triggers toevoegen** de optie **Trigger kiezen** en selecteer vervolgens **+Nieuw**. 

    ![Knop Nieuw](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. Voer in het venster **Nieuwe trigger** de volgende stappen uit: 

    a. Geef **RunEveryMinute** op bij **Naam**.

    b. Selecteer bij **Einde** de optie **Op datum**.

    c. Selecteer de vervolgkeuzelijst onder **Eindigen op**.

    d. Selecteer de optie **Huidige dag**. De dag is standaard ingesteld op de volgende dag.

    e. Update het gedeelte **minuten** naar een paar minuten na de huidige datum/tijd. De trigger wordt pas geactiveerd nadat u de wijzigingen publiceert. Als u beide slechts enkele minuten uit elkaar instelt en u voor die tijd niet publiceert, ziet u geen triggeruitvoering.

    f. Selecteer **Toepassen**. 

    ![Trigger-eigenschappen](./media/tutorial-copy-data-portal/set-trigger-properties.png)

    g. Selecteer de optie **Geactiveerd**. U kunt deze deactiveren en deze later activeren met behulp van dit selectievakje.

    h. Selecteer **Volgende**.

    ![Knop Geactiveerd](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Er zijn kosten verbonden aan elke pijplijnuitvoering, dus stel een geschikte einddatum in. 
5. Lees de waarschuwing op de pagina **Parameters voor pijplijnuitvoeringen** en selecteer vervolgens **Voltooien**. De pijplijn in dit voorbeeld gebruikt geen parameters. 

    ![Parameters voor pijplijnuitvoeringen](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
6. Selecteer **Synchroniseren** om wijzigingen in uw vertakking te synchroniseren met de master-vertakking. Standaard is **Wijzigingen publiceren na synchronisatie** geselecteerd. Wanneer u **Synchroniseren** selecteert, worden ook de bijgewerkte entiteiten gepubliceerd naar Data Factory vanuit de master-vertakking. De trigger wordt pas geactiveerd als de publicatie is uitgevoerd.

    ![Wijzigingen synchroniseren](./media/tutorial-copy-data-portal/sync-your-changes-with-trigger.png) 
7. Ga naar het tabblad **Controleren** aan de linkerkant om de geactiveerde pijplijnuitvoeringen te bekijken. 

    ![Geactiveerde pijplijnuitvoeringen](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
8. Als u wilt overschakelen van de weergave **Pijplijnuitvoeringen** naar de weergave **Triggeruitvoeringen**, selecteert u **Pijplijnuitvoeringen** en vervolgens **Triggeruitvoeringen**.
    
    ![Triggeruitvoeringen](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
9. U ziet de triggeruitvoeringen in een lijst. 

    ![Lijst Triggeruitvoeringen](./media/tutorial-copy-data-portal/trigger-runs-list.png)
10. Controleer of er twee rijen per minuut (voor elke pijplijnuitvoering) tot de opgegeven eindtijd in de **emp**-tabel worden ingevoegd. 

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een blobopslag. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Een data factory maken.
> * Een pijplijn met kopieeractiviteit maken.
> * De uitvoering van de pijplijn testen.
> * De pijplijn handmatig activeren.
> * De pijplijn activeren volgens een schema.
> * De uitvoering van de pijplijn en van de activiteit controleren.


Ga verder met de volgende zelfstudie als u wilt weten hoe u on-premises gegevens kopieert naar de cloud: 

> [!div class="nextstepaction"]
>[On-premises gegevens kopiëren naar de cloud](tutorial-hybrid-copy-portal.md)
