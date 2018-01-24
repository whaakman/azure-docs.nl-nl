---
title: Verplaatsen van gegevens - Data Management Gateway | Microsoft Docs
description: Stel een gegevensgateway in om gegevens te verplaatsen tussen on-premises en de cloud. Gebruik Data Management Gateway in Azure Data Factory om uw gegevens te verplaatsen.
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: e326a512bdb9c30068845b51e86795a410e6c9b3
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Gegevens verplaatsen tussen lokale bronnen en de cloud met Data Management Gateway
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [kopiëren van gegevens tussen on-premises en in de cloud met behulp van de Data Factory versie 2](../tutorial-hybrid-copy-powershell.md).

Dit artikel bevat een overzicht van de gegevensintegratie tussen on-premises gegevensopslagexemplaren en cloud gegevensarchieven gebruik Data Factory. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel en andere data factory core concepten artikelen: [gegevenssets](data-factory-create-datasets.md) en [pijplijnen](data-factory-create-pipelines.md).

## <a name="data-management-gateway"></a>Gegevensbeheergateway
U moet de Data Management Gateway installeren op uw on-premises machine zwevend gegevens uit een on-premises gegevensopslag inschakelen. De gateway kan worden geïnstalleerd op dezelfde computer als het gegevensarchief of op een andere computer, zolang de gateway verbinding met de gegevensopslag maken kan.

> [!IMPORTANT]
> Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over Data Management Gateway. 

De volgende procedure laat zien hoe een gegevensfactory maken met een pijplijn die de gegevens vanuit een on-premises verplaatst u **SQL Server** database naar een Azure blob storage. Als onderdeel van de procedure installeert en configureert u de gegevensbeheergateway op uw computer.

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Overzicht: kopieer on-premises gegevens in de cloud
In dit scenario moet u de volgende stappen uitvoeren: 

1. Een data factory maken.
2. Maak een data management gateway. 
3. Gekoppelde services voor de bron- en sink gegevensarchieven maken.
4. Maak gegevenssets vertegenwoordigen de invoer-en uitvoergegevens.
5. Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.

## <a name="prerequisites-for-the-tutorial"></a>Vereisten voor de zelfstudie
Voordat u deze procedure begint, hebt u de volgende vereisten:

* **Azure-abonnement**.  Als u geen abonnement hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie de [gratis proefversie](http://azure.microsoft.com/pricing/free-trial/) artikel voor meer informatie.
* **Azure Storage-Account**. Gebruik van de blob storage als een **bestemming/sink** gegevens opslaan in deze zelfstudie. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) voor de stappen voor het maken van een account.
* **SQL Server**. In deze zelfstudie gebruikt u een on-premises SQL Server-database als een **brongegevensopslag**. 

## <a name="create-data-factory"></a>Een gegevensfactory maken
In deze stap maakt u de Azure portal gebruiken voor het maken van een Azure Data Factory-exemplaar met de naam **ADFTutorialOnPremDF**.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **+ nieuw**, klikt u op **Intelligence en analyse**, en klik op **Data Factory**.

   ![Nieuw -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. In de **nieuwe gegevensfactory** pagina **ADFTutorialOnPremDF** voor de naam.

    ![Toevoegen aan Startboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u de foutmelding: **Data factory-naam 'ADFTutorialOnPremDF' is niet beschikbaar**, wijzig de naam van de gegevensfactory (bijvoorbeeld yournameADFTutorialOnPremDF) en probeert u het opnieuw. Gebruik deze naam in plaats van ADFTutorialOnPremDF tijdens het uitvoeren van de resterende stappen in deze zelfstudie.
   >
   > De naam van de gegevensfactory wordt in de toekomst mogelijk geregistreerd als **DNS**-naam en wordt daarmee ook voor iedereen zichtbaar.
   >
   >
4. Selecteer het **Azure-abonnement** waarvoor u de gegevensfactory wilt maken.
5. Selecteer een bestaande **resourcegroep** of maak een resourcegroep. Voor de zelfstudie maakt u een resourcegroep met de naam: **ADFTutorialResourceGroup**.
6. Klik op **maken** op de **nieuwe gegevensfactory** pagina.

   > [!IMPORTANT]
   > Als u Data Factory-exemplaren wilt maken, moet u lid zijn van de rol [Inzender Data Factory](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) op abonnements-/resourcegroepsniveau.
   >
   >
7. Nadat het maken voltooid is, ziet u de **Data Factory** pagina zoals in de volgende afbeelding:

   ![Data Factory-startpagina](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Gateway maken
1. In de **Data Factory** pagina, klikt u op **auteur en implementeren van** tegel starten de **Editor** voor de data factory.

    ![Tegel Ontwerpen en implementeren](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. In de Data Factory-Editor, klikt u op **... Meer** op de werkbalk en klikt u vervolgens op **nieuwe gegevensgateway**. U kunt ook u kunt met de rechtermuisknop op **gegevensgateways** in de structuurweergave en klik op **nieuwe gegevensgateway**.

   ![Nieuwe gegevensgateway op werkbalk](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. In de **maken** pagina **adftutorialgateway** voor de **naam**, en klik op **OK**.     

    ![Pagina Gateway maken](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > In dit scenario maakt u de logische gateway met slechts één knooppunt (lokale Windows-machine). U kunt een data management gateway uitbreiden door meerdere lokale computers koppelen aan de gateway. U kunt schalen omhoog door het aantal data movement taken die kunnen tegelijkertijd worden uitgevoerd op een knooppunt te verhogen. Deze functie is ook beschikbaar voor een logische gateway met één knooppunt. Zie [schaal data management gateway in Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikel voor meer informatie.  
4. In de **configureren** pagina, klikt u op **rechtstreeks op deze computer installeren**. Deze actie downloadt van het installatiepakket voor de gateway, wordt geïnstalleerd, configureert en registreert de gateway op de computer.  

   > [!NOTE]
   > Internet Explorer of een webbrowser die Microsoft ClickOnce compatibel gebruiken.
   >
   > Als u met Chrome werkt, gaat u naar de [Chrome Web Store](https://chrome.google.com/webstore/), zoekt u op het trefwoord 'ClickOnce', en kiest en installeert u een van de ClickOnce-extensies.
   >
   > Doe hetzelfde voor Firefox (install-invoegtoepassing). Klik op **Menu openen** op de werkbalk (**drie horizontale lijnen** in de rechterbovenhoek), klikt u op **invoegtoepassingen**, zoeken met het sleutelwoord 'ClickOnce', kies een van de ClickOnce-extensies en te installeren.    
   >
   >

    ![Gateway - pagina configureren](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Op deze manier is de eenvoudigste manier (met één muisklik) te downloaden, installeren, configureren en registreren van de gateway in één enkele stap. U ziet de **Microsoft Data Management Gateway Configuration Manager** toepassing op uw computer is geïnstalleerd. U kunt ook het uitvoerbare bestand vinden **ConfigManager.exe** in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.

    U kunt ook downloaden en gateway handmatig installeren met behulp van de koppelingen op deze pagina en registreren met behulp van de sleutel die wordt weergegeven in de **nieuwe sleutel** in het tekstvak.

    Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor de details over de gateway.

   > [!NOTE]
   > U moet een beheerder op de lokale computer installeren en configureren van de Data Management Gateway is. U kunt extra gebruikers toevoegen aan de **Data Management Gateway-gebruikers** lokale Windows-groep. De leden van deze groep kunnen het Data Management Gateway Configuration Manager-hulpprogramma gebruiken om de gateway te configureren.
   >
   >
5. Wacht een paar minuten of wacht tot u de volgende melding:

    ![Gateway-installatie is geslaagd](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. Start **Data Management Gateway Configuration Manager** toepassing op uw computer. In de **Search** venster, type **Data Management Gateway** voor toegang tot dit hulpprogramma. U kunt ook het uitvoerbare bestand vinden **ConfigManager.exe** in de map: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

    ![Gateway Configuration Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. Controleer of u `adftutorialgateway is connected to the cloud service` bericht. De statusbalk in het onderste worden **verbonden met de cloudservice** samen met een **groen vinkje**.

    Op de **Start** tabblad kunt u ook de volgende bewerkingen uitvoeren:

   * **Registreren** een gateway met een sleutel van de Azure-portal met behulp van de knop registreren.
   * **Stop** de Data Management Gateway Host Service uitgevoerd op uw computer met de gateway.
   * **Updates plannen** moet worden geïnstalleerd op een bepaald tijdstip van de dag.
   * Wanneer de gateway is **laatst bijgewerkt**.
   * Geef de tijd waarop een update voor de gateway kan worden geïnstalleerd.
8. Overschakelen naar de **instellingen** tabblad. Het certificaat dat is opgegeven in de **certificaat** worden gebruikt om de referenties voor het lokale gegevensarchief die u in de portal opgeeft versleutelen/ontsleutelen. (optioneel) Klik op **wijziging** voor gebruik in plaats hiervan uw eigen certificaat. De gateway maakt standaard gebruik van het certificaat dat automatisch wordt gegenereerd door de Data Factory-service.

    ![De configuratie van de gateway-certificaat](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    U kunt ook de volgende acties uitvoeren op de **instellingen** tabblad:

   * Weergeven of Exporteer het certificaat wordt gebruikt door de gateway.
   * Wijzig de HTTPS-eindpunt dat is gebruikt door de gateway.    
   * Stel een HTTP-proxy moet worden gebruikt door de gateway.     
9. (optioneel) Overschakelen naar de **Diagnostics** tabblad, Controleer de **uitgebreide logboekregistratie inschakelen** optie als u uitgebreide logboekregistratie die u gebruiken kunt voor het oplossen van problemen met de gateway in te schakelen. Gegevens in het logboek vindt u in **logboeken** onder **logboeken toepassingen en Services** -> **Data Management Gateway** knooppunt.

    ![Tabblad Diagnostische gegevens](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    U kunt ook uitvoeren met de volgende acties in de **Diagnostics** tabblad:

   * Gebruik **testverbinding** sectie met een lokale gegevensbron met behulp van de gateway.
   * Klik op **logboeken bekijken** om te zien van het logboek Data Management Gateway in een venster Logboeken.
   * Klik op **logboeken verzenden** met Logboeken van de afgelopen zeven dagen een zip-bestand uploaden naar Microsoft te vergemakkelijken oplossen van problemen met uw.
10. Op de **Diagnostics** tabblad, in de **testverbinding** sectie **SqlServer** voor het type van het gegevensarchief, voer de naam van de databaseserver, de naam van de database verificatietype opgeven, gebruikersnaam en wachtwoord invoeren en op **testen** om te controleren of de gateway verbinding met de database maken kan.
11. Ga naar de webbrowser en in de **Azure-portal**, klikt u op **OK** op de **configureren** pagina en klik vervolgens op de **nieuwe gegevensgateway** pagina.
12. U ziet **adftutorialgateway** onder **gegevensgateways** in de structuurweergave links.  Als u erop klikt, ziet u de bijbehorende JSON.

## <a name="create-linked-services"></a>Gekoppelde services maken
In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService** en **SqlServerLinkedService**. De **SqlServerLinkedService** koppelingen van een lokale SQL Server-database en de **AzureStorageLinkedService** gekoppelde service een Azure blob-archief is gekoppeld aan de gegevensfactory. U maakt een pijplijn verderop in dit scenario waarmee gegevens worden gekopieerd van de lokale SQL Server-database naar de Azure blob-store.

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Een gekoppelde service toevoegen aan een lokale SQL Server-database.
1. In de **Data Factory-Editor**, klikt u op **nieuwe gegevensopslag** op de werkbalk en selecteer **SQL Server**.

   ![Nieuwe SQL-Server gekoppeld-service](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. In de **JSON-editor** aan de rechterkant, komen de volgende stappen uit:

   1. Voor de **gatewayName**, geef **adftutorialgateway**.    
   2. In de **connectionString**, moet u de volgende stappen uitvoeren:    

      1. Voor **servername**, voer de naam van de server die als host fungeert voor de SQL Server-database.
      2. Voor **databasename**, voer de naam van de database.
      3. Klik op **versleutelen** op de werkbalk. Ziet u de toepassing Referentiebeheer.

         ![Referenties Manager-toepassing](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. In de **instelling referenties** in het dialoogvenster verificatietype, gebruikersnaam en wachtwoord opgeven en klik op **OK**. Als de verbinding geslaagd is, de versleutelde referenties worden opgeslagen in de JSON en het dialoogvenster wordt gesloten.
      5. Het tabblad leeg browser dat in het dialoogvenster gestart als deze optie wordt niet automatisch gesloten sluiten en terug te gaan naar het tabblad met de Azure-portal.

         Op de gatewaycomputer deze referenties zijn **versleutelde** met behulp van een Data Factory-service-certificaat. Als u wilt het certificaat dat is gekoppeld aan de Data Management Gateway in plaats daarvan gebruiken, raadpleegt u [referenties veilig instellen](#set-credentials-and-security).    
   3. Klik op **implementeren** gekoppelde service op de opdrachtbalk voor het implementeren van de SQL-Server. Hier ziet u de gekoppelde service in de structuurweergave.

      ![De service SQL Server is gekoppeld in de structuurweergave](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Een gekoppelde service voor een Azure storage-account toevoegen
1. In de **Data Factory-Editor**, klikt u op **nieuwe gegevensopslag** op de opdrachtbalk klikken en op **Azure storage**.
2. Voer de naam van uw Azure storage-account voor de **accountnaam**.
3. Voer de sleutel voor uw Azure storage-account voor de **accountsleutel**.
4. Klik op **implementeren** voor het implementeren van de **AzureStorageLinkedService**.

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u invoer- en uitvoergegevenssets die invoer- en uitvoergegevenssets voor de kopieerbewerking vertegenwoordigen (on-premises SQL Server-database => Azure Blob-opslag). Voordat u de gegevenssets maakt, voert u de volgende stappen uit (gedetailleerde stappen vindt u na de lijst):

* Maak een tabel met de naam **emp** in de SQL Server-database die u als gekoppelde service hebt toegevoegd aan de gegevensfactory, en voer een aantal voorbeeldvermeldingen toe aan de tabel.
* Maak een blobcontainer met de naam **adftutorial** in het Azure Blob-opslagaccount dat u als gekoppelde service hebt toegevoegd aan de gegevensfactory.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>On-premises SQL Server voorbereiden voor de zelfstudie
1. Gebruik in de database die u hebt opgegeven voor de met de on-premises SQL-server gekoppelde service, (**SqlServerLinkedService**) het volgende SQL-script om de tabel **emp** te maken in de database.

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Voer een aantal voorbeelden in de tabel in:

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a>Invoergegevensset maken

1. Klik in de **Data Factory-editor** op **... Meer**, klikt u op **nieuwe gegevensset** op de opdrachtbalk klikken en op **SQL Server-tabel**.
2. Vervang de JSON in het rechterdeelvenster met de volgende tekst:

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   Houd rekening met de volgende punten:

   * **type** is ingesteld op **SqlServerTable**.
   * **tableName** is ingesteld op **emp**.
   * **linkedServiceName** is ingesteld op **SqlServerLinkedService** (u hebt deze gekoppelde service gemaakt eerder in dit scenario.).
   * Voor een invoergegevensset die niet worden gegenereerd door een andere pijplijn in Azure Data Factory, moet u instellen **externe** naar **true**. Hiermee wordt aangegeven dat de invoergegevens buiten de Azure Data Factory-service wordt geproduceerd. U kunt optioneel opgeven de beleidsregels van elke externe gegevens met behulp van de **externalData** -element in de **beleid** sectie.    

   Zie [gegevens verplaatsen naar/van de SQL Server](data-factory-sqlserver-connector.md) voor meer informatie over de JSON-eigenschappen.
3. Klik op **implementeren** op de opdrachtbalk om de gegevensset te implementeren.  

### <a name="create-output-dataset"></a>Uitvoergegevensset maken

1. In de **Data Factory-Editor**, klikt u op **nieuwe gegevensset** op de opdrachtbalk klikken en op **Azure Blob storage**.
2. Vervang de JSON in het rechterdeelvenster met de volgende tekst:

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   Houd rekening met de volgende punten:

   * **type** is ingesteld op **AzureBlob**.
   * **linkedServiceName** is ingesteld op **AzureStorageLinkedService** (u hebt deze gekoppelde service gemaakt in stap 2).
   * **folderPath** is ingesteld op **adftutorial/outfromonpremdf** waar outfromonpremdf is de map in de container adftutorial. Maak de **adftutorial** container als deze niet al bestaat.
   * De **beschikbaarheid** wordt ingesteld op **elk uur** (de **frequentie** wordt ingesteld op **elk uur** en het **interval** wordt ingesteld op **1**).  De Data Factory-service een gegevenssegment uitvoer genereert elk uur in de **emp** tabel in de Azure SQL Database.

   Als u geen opgeeft een **fileName** voor een **uitvoertabel**, de bestanden in die worden gegenereerd de **folderPath** zijn met de naam in de volgende indeling: Data.<Guid>. txt (bijvoorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   Om in te stellen **folderPath** en **fileName** dynamisch op basis van de **SliceStart** tijd, gebruikt u de eigenschap partitionedBy. In het volgende voorbeeld worden voor folderPath Year, Month en Day gebruikt van de SliceStart-waarde (tijd waarop is begonnen met het verwerken van het segment). Voor fileName wordt gebruikgemaakt van Hour van de SliceStart-waarde. Als er bijvoorbeeld een segment wordt geproduceerd voor 2014-10-20T08:00:00, wordt folderName ingesteld op wikidatagateway/wikisampledataout/2014/10/20 en wordt fileName ingesteld op 08.csv.

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    Zie [gegevens verplaatsen naar/van Azure Blob Storage](data-factory-azure-blob-connector.md) voor meer informatie over de JSON-eigenschappen.
3. Klik op **implementeren** op de opdrachtbalk om de gegevensset te implementeren. Controleer of u zowel de gegevenssets in de structuurweergave.  

## <a name="create-pipeline"></a>Pijplijn maken
In deze stap maakt u een **pijplijn** met een **Kopieeractiviteit** die gebruikmaakt van **EmpOnPremSQLTable** als invoer en **OutputBlobTable** als uitvoer.

1. Klik in Data Factory-Editor **... Meer** en vervolgens op **Nieuwe pijplijn**.
2. Vervang de JSON in het rechterdeelvenster met de volgende tekst:    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server to blob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
               }
             },
             "Policy": {
               "concurrency": 1,
               "executionPriorityOrder": "NewestFirst",
               "style": "StartOfInterval",
               "retry": 0,
               "timeout": "01:00:00"
             }
           }
         ],
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > Vervang de waarde van de eigenschap **start** door de huidige dag en de waarde **end** door de volgende dag.
   >
   >

   Houd rekening met de volgende punten:

   * In het gedeelte activiteiten is alleen activiteit waarvan **type** is ingesteld op **kopie**.
   * **Invoer** voor de activiteit is ingesteld op **EmpOnPremSQLTable** en **uitvoer** voor de activiteit is ingesteld op **OutputBlobTable**.
   * In de **typeProperties** sectie **SqlSource** is opgegeven als de **gegevensbrontype** en ** BlobSink ** is opgegeven als de **sink-type**.
   * SQL-query `select * from emp` is opgegeven voor de **sqlReaderQuery** eigenschap van **SqlSource**.

   Zowel de begin- als einddatum en -tijd moeten de [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601) hebben. Bijvoorbeeld: 2014-10-14T16:32:41Z. De **eindtijd** is optioneel, maar we gebruiken hem in deze zelfstudie.

   Als u geen waarde opgeeft voor de eigenschap **end**, wordt automatisch **start + 48 uur** gebruikt. Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9/9/9999** op als waarde voor de eigenschap **end**.

   U definieert de tijdsduur waarin de gegevenssegmenten worden verwerkt op basis van de **beschikbaarheid** eigenschappen die zijn gedefinieerd voor elke Azure Data Factory-gegevensset.

   In het bovenstaande voorbeeld zijn er 24 gegevenssegmenten omdat er elk uur één gegevenssegment wordt gemaakt.        
3. Klik op **implementeren** op de opdrachtbalk om de gegevensset te implementeren (tabel is een rechthoekige gegevensset). Bevestig dat de pijplijn wordt weergegeven in de structuurweergave onder **pijplijnen** knooppunt.  
4. Klik nu op **X** twee keer te sluiten van de pagina terugkeren naar de **Data Factory** pagina voor de **ADFTutorialOnPremDF**.

**Gefeliciteerd!** U hebt een Azure-gegevensfactory, gekoppelde services, gegevenssets en een pijplijn gemaakt en u hebt ingesteld voor de pijplijn.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>De gegevensfactory weergeven in een diagramweergave
1. In de **Azure-portal**, klikt u op **Diagram** tegel op de startpagina van de **ADFTutorialOnPremDF** gegevensfactory. :

    ![Diagram koppeling](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. U ziet een diagram dat lijkt op de volgende afbeelding:

    ![Diagramweergave](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    U kunt inzoomen, uitzoomen, zoomen naar 100%, zoomen past, automatisch plaats pijplijnen en gegevenssets en afkomst weergeven (upstream- en downstreamitems van items worden gemarkeerd geselecteerde).  U kunt dubbelklikken op een object (invoer/uitvoer gegevensset of pijplijn) Controleer de eigenschappen voor.

## <a name="monitor-pipeline"></a>De pijplijn bewaken
In deze stap gebruikt u Azure Portal om te controleren wat er gebeurt in een Azure data factory. U kunt ook PowerShell-cmdlets gebruiken om gegevenssets en pijplijnen te bewaken. Zie voor meer informatie over het bewaken van [pijplijnen bewaken en beheren](data-factory-monitor-manage-pipelines.md).

1. Dubbelklik in het diagram op **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable segmenten](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. U ziet dat de gegevenssegmenten up allemaal **gereed** status omdat de pipeline-duur (tijd met de eindtijd) in het verleden. Het is ook omdat u de gegevens in de SQL Server-database hebt geplaatst en er voortdurend. Bevestig dat er geen segmenten weergegeven in de **probleemsegmenten** sectie onderaan. Als u wilt weergeven van alle segmenten **meer** onder aan de lijst met segmenten.
3. Nu In de **gegevenssets** pagina, klikt u op **OutputBlobTable**.

    ![OputputBlobTable segmenten](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. Klik op een gegevenssegment uit de lijst en u ziet de **gegevenssegment** pagina. U ziet dat de activiteit voor het segment wordt uitgevoerd. Er is slechts één activiteit die meestal worden uitgevoerd.  

    ![Blade gegevenssegment](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Als het segment niet de status **Gereed** heeft, kunt u de upstreamsegmenten bekijken die niet de status Gereed hebben en die voorkomen dat de huidige status wordt uitgevoerd. U ziet deze segmenten in de lijst **Upstreamsegmenten die niet gereed zijn**.
5. Klik op de **activiteit die wordt uitgevoerd** uit de lijst onderaan om te zien **details uitvoering van activiteit**.

   ![De pagina Details uitvoering van activiteit](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   Hier ziet u informatie zoals de doorvoer, de duur en de gateway die wordt gebruikt voor de gegevensoverdracht.
6. Klik op **X** alle pagina's totdat u sluiten
7. teruggaan naar de startpagina van de **ADFTutorialOnPremDF**.
8. (optioneel) Klik op **pijplijnen**, klikt u op **ADFTutorialOnPremDF**, en detailanalyse van invoertabellen (**verbruikt**) of uitvoergegevenssets (**geproduceerde**).
9. Gebruik hulpprogramma's zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) om te controleren of een blob of het bestand is gemaakt voor elk uur.

   ![Azure Opslagverkenner](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor de details over de Data Management Gateway.
* Zie [gegevens kopiëren van Azure-Blob naar Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor meer informatie over het gebruik van de Kopieeractiviteit om gegevens te verplaatsen van een brongegevensarchief naar een gegevensopslag sink.
