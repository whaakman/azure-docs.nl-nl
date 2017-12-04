---
title: "Gegevens van SQL Server naar Blob Storage kopiëren met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over het kopiëren van gegevens uit een on-premises gegevensopslag naar Azure-cloud met behulp van de zelf-hostende Integration Runtime in Azure Data Factory."
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
ms.date: 11/16/2017
ms.author: jingwang
ms.openlocfilehash: ca8e664ff1fd509d0461b6d167f28743d2e1e69c
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="tutorial-copy-data-from-on-premises-sql-server-to-azure-blob-storage"></a>Zelfstudie: gegevens van een on-premises SQL-server naar Azure Blob Storage kopiëren
In deze zelfstudie gebruikt u Azure PowerShell om een Data Factory-pijplijn te maken waarmee gegevens worden gekopieerd van een on-premises SQL Server-database naar een Azure Blob-opslag. U gaat een zelf-hostende Integration Runtime maken en gebruiken. Deze verplaatst gegevens van on-premises gegevensarchieven en gegevensarchieven in de cloud en omgekeerd. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Dit artikel is geen gedetailleerde introductie tot de Data Factory-service. Zie [Inleiding tot Azure Data Factory](introduction.md) voor een inleiding tot Azure Data Factory-service. 

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een zelf-hostende Integration Runtime maken.
> * Gekoppelde services maken voor SQL Server en Azure Storage. 
> * Gegevenssets maken voor SQL Server en Azure Blob.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Start een pijplijnuitvoering.
> * De pijplijnuitvoering controleert.

## <a name="prerequisites"></a>Vereisten
### <a name="azure-subscription"></a>Azure-abonnement
Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

### <a name="azure-roles"></a>Azure-rollen
Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, lid zijn van de rollen **Inzender** of **Eigenaar**, of moet dit een **beheerder** van het Azure-abonnement zijn. Klik in Azure Portal op uw **gebruikersnaam** in de rechterbovenhoek en selecteer **Machtigingen** om de machtigingen weer te geven die u in het abonnement hebt. Als u toegang tot meerdere abonnementen hebt, moet u het juiste abonnement selecteren. Zie het artikel [Rollen toevoegen](../billing/billing-add-change-azure-subscription-administrator.md) voor voorbeelden van instructies voor het toevoegen van een gebruiker aan een rol.

### <a name="sql-server-201420162017"></a>SQL Server 2014/2016/2017
In deze zelfstudie gebruikt u een on-premises SQL Server-database als een **brongegevensopslag**. De pijplijn in de data factory die u in deze zelfstudie gaat maken, kopieert gegevens van deze on-premises SQL Server-database (bron) naar een Azure Blob-opslag (sink). Maak een tabel met de naam **emp** in uw SQL Server-database en voeg een aantal voorbeeldgegevens toe aan de tabel. 

1. Start **SQL Server Management Studio** op uw computer. Als u SQL Server Management Studio niet op uw computer hebt geïnstalleerd, installeert u dit via het [Downloadcentrum](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms). 
2. Maak verbinding met SQL Server met behulp van uw referenties. 
3. Maak een voorbeelddatabase. Klik in de structuurweergave met de rechtermuisknop op **Databases** en klik op **Nieuwe database**. Voer in het venster **Nieuwe database** een **naam** in voor de database en klik op **OK**. 
4. Voer het volgende queryscript uit voor de database. Hiermee wordt de **emp**-tabel gemaakt en worden enkele voorbeeldgegevens ingevoegd in deze tabel. In de structuurweergave klikt u met de rechtermuisknop op de **database** die u hebt gemaakt en klikt u op **Nieuwe query**. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )

    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    GO
    ```

### <a name="azure-storage-account"></a>Azure-opslagaccount
In deze zelfstudie gaat u een algemeen Azure Storage-account (en dan met name een Blob Storage) gebruiken als een **doel/sink**-gegevensopslag. Zie het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen Azure Storage-account hebt voor algemene doeleinden en er een wilt maken. De pijplijn in de data factory die u in deze zelfstudie gaat maken, kopieert gegevens van de on-premises SQL Server-database (bron) naar deze Azure Blob-opslag (sink). 

#### <a name="get-storage-account-name-and-account-key"></a>De naam en sleutel van een opslagaccount ophalen
In deze QuickStart gaat u de naam en sleutel van uw Azure Storage-account gebruiken. De volgende procedure bevat stappen waarmee u de naam en sleutel van uw opslagaccount kunt ophalen. 

1. Open een browser en navigeer naar [Azure Portal](https://portal.azure.com). Meld u aan met uw Azure-gebruikersnaam en -wachtwoord. 
2. Klik op **Meer services >** in het menu links, filter op het trefwoord **Opslag** en selecteer **Opslagaccounts**.

    ![Zoeken naar een opslagaccount](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. Filter in de lijst met opslagaccounts op uw opslagaccount (indien nodig) en selecteer vervolgens **uw opslagaccount**. 
4. Selecteer op de pagina **Opslagaccount** de optie **Toegangssleutels** in het menu.

    ![De naam en sleutel van het opslagaccount ophalen](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. Kopieer de waarden voor de velden **Opslagaccountnaam** en **key1** naar het klembord. Plak deze in Kladblok of een andere editor en sla ze op. U gaat in de zelfstudie de naam van het opslagaccount en de sleutel gebruiken. 

#### <a name="create-the-adftutorial-container"></a>De container adftutorial maken 
In deze sectie maakt u in uw Azure Blob Storage een blobcontainer met de naam **adftutorial**. 

1. Schakel op de pagina **Opslagaccount** over naar **Overzicht** en klik vervolgens op **Blobs**. 

    ![De optie Blobs selecteren](media/tutorial-hybrid-copy-powershell/select-blobs.png)
1. Klik op de pagina **Blob service** in de werkbalk op **+ Container**. 

    ![Knop Container toevoegen](media/tutorial-hybrid-copy-powershell/add-container-button.png)
3. Voer in het dialoogvenster **Nieuwe container** als naam **adftutorial** in en klik op **OK**. 

    ![Naam van container invoeren](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)
4. Klik op **adftutorial** in de lijst met containers.  

    ![De container selecteren](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)
5. Houd de pagina met de **container** voor **adftutorial** geopend. U gaat hiermee aan het einde van deze zelfstudie de uitvoer controleren. In Data Factory wordt automatisch in deze container de uitvoermap gemaakt, zodat u er zelf geen hoeft te maken.

    ![Pagina Container](media/tutorial-hybrid-copy-powershell/container-page.png)

### <a name="windows-powershell"></a>Windows Powershell

#### <a name="install-powershell"></a>PowerShell installeren
Installeer de nieuwste versie van PowerShell als deze niet al op de computer staat. 

1. Navigeer in uw webbrowser naar de pagina [Azure SDK-downloads en SDK’s](https://azure.microsoft.com/downloads/). 
2. Klik op **Windows installeren** in de sectie **Opdrachtregelprogramma's** -> **PowerShell**. 
3. Voer het **MSI**-bestand uit om Azure PowerShell te installeren. 

Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor gedetailleerde instructies. 

#### <a name="log-in-to-powershell"></a>Aanmelden bij PowerShell

1. Start **PowerShell** op uw computer. Houd het PowerShell-venster geopend tot het einde van deze QuickStart. Als u het programma sluit en opnieuw opent, moet u deze opdrachten opnieuw uitvoeren.

    ![PowerShell starten](media/tutorial-hybrid-copy-powershell/search-powershell.png)
1. Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Als u meerdere Azure-abonnementen hebt, voert u de volgende opdracht uit om alle abonnementen voor dit account weer te geven:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Als u meerdere Azure-abonnementen hebt, voert u de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **SubscriptionId** door de id van uw Azure-abonnement:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Definieer een variabele voor de naam van de resourcegroep die u later gaat gebruiken in PowerShell-opdrachten. Kopieer de tekst van de volgende opdracht naar PowerShell, geef tussen dubbele aanhalingstekens een naam op voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en voer de opdracht uit. Bijvoorbeeld: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup"
    ```
2. Voer de volgende opdracht uit om de resourcegroep te maken: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$resourceGroupName`-variabele en voer de opdracht opnieuw uit.
3. Definieer een variabele voor de naam van de data factory die u later kunt gebruiken in PowerShell-opdrachten. Namen moeten beginnen met een letter of cijfer en mogen alleen letters, cijfers en streepjes (-) bevatten.

    > [!IMPORTANT]
    >  Werk de naam van de data factory zodanig bij dat deze uniek is. Bijvoorbeeld: ADFTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFTutorialFactory"
    ```
1. Definieer een variabele voor de locatie van de data factory: 

    ```powershell
    $location = "East US"
    ```  
5. Voer de volgende cmdlet **Set AzureRmDataFactoryV2** uit om de data factory te maken: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

    ```
    The specified Data Factory name 'ADFv2TutorialDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, lid zijn van de rollen **Inzender** of **Eigenaar**, of moet dit een **beheerder** van het Azure-abonnement zijn.
* Momenteel kunt u met Data Factory versie 2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

## <a name="create-a-self-hosted-ir"></a>Een zelf-hostende IR maken

In deze sectie kunt u een zelf-hostende Integration Runtime maken en deze koppelen aan een on-premises computer met de SQL Server database. De zelf-hostende Integration Runtime is het onderdeel waarmee gegevens worden gekopieerd van SQL Server op uw computer naar Azure Blob Storage. 

1. Maak een variabele voor de naam van een Integration Runtime. Gebruik een unieke naam en noteer deze. U gaat deze verderop in de zelfstudie gebruiken. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Een zelf-hostende Integration Runtime maken. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Voer de volgende opdracht uit om de status van de gemaakte zelf-hostende Integration Runtime op te halen.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Voer de volgende opdracht uit om **verificatiesleutels** op te halen voor het registreren van de zelf-hostende Integration Runtime met Data Factory-service in de cloud. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Kopieer een van de sleutels (zonder de dubbele aanhalingstekens) om de zelf-hostende Integration Runtime te registeren die u tijdens de volgende stap op uw computer gaat installeren.  

## <a name="install-integration-runtime"></a>Integration Runtime installeren
1. [Download](https://www.microsoft.com/download/details.aspx?id=39717) de zelf-hostende Integration Runtime op een lokale Windows-computer en voer de installatie uit. 
2. Klik bij **Welkom bij de installatiewizard van Microsoft Integration Runtime** op **Volgende**.  
3. Ga op de pagina **Gebruiksrechtovereenkomst** akkoord met de voorwaarden en de gebruiksrechtovereenkomst en klik op **Volgende**. 
4. Klik op de pagina **Doelmap** op **Volgende**. 
5. Klik bij **Gereed om Microsoft Integration Runtime te installeren** op **Installeren**. 
6. Als er een waarschuwingsbericht wordt weergegeven met de melding of de computer moet worden geconfigureerd om in de slaapstand of sluimerstand over te gaan als deze niet in gebruik is, klikt u op **OK**. 
7. Als het venster **Energiebeheer** wordt weergegeven, sluit u dit en schakelt u over naar het instellingenvenster. 
8. Klik op de pagina **De installatiewizard voor Microsoft Integration Runtime is voltooid**  op **Voltooien**.
9. Plak de sleutel die u in de vorige sectie hebt opgeslagen op de pagina **Integration Runtime (zelf-hostend) registreren** en klik op **Registreren**. 

   ![Integratieruntime registreren](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)
2. U ziet het volgende bericht wanneer de zelf-hostende Integration Runtime is geregistreerd:

   ![Registratie is voltooid](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

3. Klik op de pagina **Nieuw knooppunt voor Integration Runtime (zelf-hostend)** op **Volgende**. 

    ![Pagina Nieuw knooppunt voor Integration Runtime](media/tutorial-hybrid-copy-powershell/new-integration-runtime-node-page.png)
4. Bij **Intranetcommunicatiekanaal** klikt u op **Overslaan**. U kunt een TLS/SSL-certificaat selecteren voor het beveiligen van de communicatie tussen knooppunten in een Integration Runtime-omgeving met meerdere knooppunten. 

    ![Pagina Intranetcommunicatiekanaal](media/tutorial-hybrid-copy-powershell/intranet-communication-channel-page.png)
5. Op de pagina **Integration Runtime (zelf-hostend) registeren** klikt u op **Configuration Manager starten**. 
6. Wanneer het knooppunt is verbonden met de cloudservice, ziet u de volgende pagina:

   ![Knooppunt is verbonden](media/tutorial-hybrid-copy-powershell/node-is-connected.png)
7. Test nu de verbinding met uw SQL Server-database.

    ![Tabblad Diagnostische gegevens](media/tutorial-hybrid-copy-powershell/config-manager-diagnostics-tab.png)   

    - Schakel in het venster **Configuratiebeheer** over naar het tabblad **Diagnostische gegevens**.
    - Selecteer **SqlServer** als het **type gegevensbron**.
    - Voer de naam van de **server** in.
    - Voer de naam van de **database** in. 
    - Selecteer de **verificatiemethode**. 
    - Voer de **gebruikersnaam** in. 
    - Voer het **wachtwoord** voor de gebruikersnaam in.
    - Klik op **Test** om te controleren of Integration Runtime verbinding kan maken met de SQL Server. U ziet een groen vinkje als het gelukt is om verbinding te maken. Anders wordt er een foutbericht weergegeven. Los eventuele problemen op en zorg ervoor dat de Integration Runtime verbinding met uw SQL Server kan maken.
    - Noteer deze waarden (verificatietype, server, database, gebruiker, wachtwoord). U hebt deze waarden verderop in deze zelfstudie nodig. 
    
      
## <a name="create-linked-services"></a>Gekoppelde services maken
Maak gekoppelde services in een data factory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze zelfstudie gaat u uw Azure Storage-account en de on-premises SQL Server aan de gegevensopslag koppelen. De gekoppelde services beschikken over de verbindingsgegevens die de Data Factory-service tijdens runtime gebruikt om er een verbinding mee tot stand te brengen. 

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Een gekoppelde Azure Storage-service maken (bestemming/sink)
Tijdens deze stap koppelt u uw Azure Storage-account aan de data factory.

1. Maak een JSON-bestand met de naam **AzureStorageLinkedService.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud: Maak de map ADFv2Tutorial als deze nog niet bestaat.  

    > [!IMPORTANT]
    > Vervang &lt;accountName&gt; en &lt;accountKey&gt; door de naam en sleutel van uw **Azure Storage-account** voordat u het bestand opslaat. U hebt deze waarden genoteerd als onderdeel van de [vereisten](#get-storage-account-name-and-account-key).

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>;EndpointSuffix=core.windows.net"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

    Als u Kladblok gebruikt, selecteert u **Alle bestanden** voor het veld **Opslaan als** in het dialoogvenster **Opslaan als**. Als u dat niet doet, wordt mogelijk de extensie `.txt` toegevoegd aan het bestand. Bijvoorbeeld `AzureStorageLinkedService.json.txt`. Als u het bestand in Verkenner maakt voordat u het opent in Kladblok, ziet u de extensie `.txt` mogelijk niet omdat de optie **Extensies voor bekende bestandstypen verbergen** standaard is ingeschakeld. Verwijder de extensie `.txt` voordat u doorgaat met de volgende stap. 
2. Schakel in **Azure PowerShell** over naar de map **C:\ADFv2Tutorial**.

   Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureStorageLinkedService** te maken. 

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Hier volgt een voorbeeld van uitvoer:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

    Als er een foutmelding wordt geretourneerd waarin staat dat het bestand niet is gevonden, voert u de `dir`-opdracht uit om te bevestigen dat het bestand bestaat. Als de bestandsnaam de extensie `.txt` heeft (bijvoorbeeld AzureStorageLinkedService.json.txt), verwijdert u deze en voert u vervolgens de PowerShell-opdracht opnieuw uit. 

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Een gekoppelde SQL Server-service (bron) maken en versleutelen
In deze stap gaat u uw on-premises SQL Server aan de data factory koppelen.

1. Maak een JSON-bestand met de naam **SqlServerLinkedService.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud: Selecteer de juiste sectie op basis van de **verificatie** die u gebruikt om verbinding te maken met SQL Server.  

    > [!IMPORTANT]
    > Selecteer de juiste sectie op basis van de **verificatie** die u gebruikt om verbinding te maken met SQL Server.

    **Als u gebruikmaakt van SQL-verificatie (sa), moet u de volgende JSON-definitie kopiëren:**

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```    
    **Als u gebruikmaakt van Windows-verificatie, moet u de volgende JSON-definitie kopiëren:**

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<server>;Database=<database>;Integrated Security=True"
                },
                "userName": "<user> or <domain>\\<user>",
                "password": {
                    "type": "SecureString",
                    "value": "<password>"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }    
    ```
    > [!IMPORTANT]
    > - Selecteer de juiste sectie op basis van de **verificatie** die u gebruikt om verbinding te maken met SQL Server.
    > - Vervang de **naam>** van de **&lt;Integration** **Runtime** door de naam van uw Integration Runtime.
    > - Vervang **&lt;servername>**, **&lt;databasename>**, **&lt;username**> en **&lt;password>** door de waarden van uw SQL Server voordat u het bestand opslaat.
    > - Als u een slash wilt gebruiken (`\`) in de naam van uw gebruikersaccount of server, moet u het escapeteken (`\`) gebruiken. Bijvoorbeeld `mydomain\\myuser`. 

2. Voor het versleutelen van de gevoelige gegevens (gebruikersnaam, wachtwoord enzovoort), moet u de cmdlet **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** uitvoeren. Deze versleuteling zorgt ervoor dat de referenties zijn versleuteld met behulp van DPAPI (Data Protection Application Programming Interface). De versleutelde referenties worden lokaal op het zelf-hostende Integration Runtime-knooppunt (lokale computer) opgeslagen. De uitvoernettolading kan worden omgeleid naar een ander JSON-bestand (in dit geval encryptedLinkedService.json). Dit bestand bevat de versleutelde referenties.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Voer de volgende opdracht uit. Hiermee wordt de **EncryptedSqlServerLinkedService** gemaakt:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u invoer- en uitvoergegevenssets die invoer- en uitvoergegevenssets voor de kopieerbewerking vertegenwoordigen (on-premises SQL Server-database => Azure Blob-opslag).

### <a name="create-a-dataset-for-source-sql-database"></a>Een gegevensset maken voor bron SQL-Database
Tijdens deze stap definieert u een gegevensset die gegevens in de SQL Server-database vertegenwoordigt. De gegevensset is van het type **SqlServerTable**. Deze gegevensset verwijst naar de **gekoppelde SQL Server-service** die u in de vorige stap hebt gemaakt. De gekoppelde service beschikt over de **verbindingsgegevens** die de Data Factory-service gebruikt om tijdens runtime een verbinding met uw SQL Server tot stand te brengen. Deze gegevensset bepaalt welke **SQL-tabel** in de database de gegevens bevat. In deze zelfstudie bevat de tabel `emp` de brongegevens. 

1. Maak een JSON-bestand met de naam **SqlServerDataset.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. De gegevensset **SqlServerDataset** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Dataset** uit.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Een gegevensset maken voor Azure Blob-sinkopslag
Tijdens deze stap gaat u een gegevensset definiëren die gegevens vertegenwoordigt die moeten worden gekopieerd naar de Azure Blob Storage. De gegevensset is van het type **AzureBlob**. Deze gegevensset verwijst naar de **gekoppelde Azure Storage-service** die u eerder in deze zelfstudie hebt gemaakt. De gekoppelde service beschikt over de **verbindingsgegevens** die de Data Factory-service tijdens runtime gebruikt om verbinding met uw Azure Storage-account te maken. Dit **gegevensset** duidt de **map** in de Azure-opslag aan waarnaar de gegevens van de SQL Server-database worden gekopieerd. In deze zelfstudie is de map `adftutorial/fromonprem` waar `adftutorial` de blobcontainer is en `fromonprem` map. 

1. Maak een JSON-bestand met de naam **AzureBlobDataset.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud:

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. De gegevensset **AzureBlobDataset** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Dataset** uit.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>Een pijplijn maken
Tijdens deze stap maakt u een pijplijn met een kopieeractiviteit. Tijdens de kopieeractiviteit wordt de **SqlServerDataset** als invoergegevensset en **AzureBlobDataset** als de uitvoergegevensset gebruikt. Het brontype is ingesteld op **SqlSource** en het sinktype is ingesteld op **BlobSink**.

1. Maak een JSON-bestand met de naam **SqlServerToBlobPipeline.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. De pijplijn **SQLServerToBlobPipeline** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Pipeline** uit.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```


## <a name="create-a-pipeline-run"></a>Een pijplijnuitvoering maken
Start een pijplijnuitvoering voor de pijplijn SQLServerToBlobPipeline en leg de id voor de pijplijnuitvoering vast voor toekomstige controle.

```powershell
$runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
```

## <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

1. Voer het volgende script uit om continu de uitvoeringsstatus van de pijplijn **SQLServerToBlobPipeline** te controleren, en druk het eindresultaat af. Kopieer/plak het volgende script in het PowerShell-venster en druk op ENTER.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Hier volgt een voorbeeld van de voorbeelduitvoer:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. U kunt de run-id van de pijplijn **SQLServerToBlobPipeline** ophalen en vervolgens het gedetailleerde uitvoeringsresultaat van de activiteit controleren met de volgende opdracht: 

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Hier volgt een voorbeeld van de voorbeelduitvoer:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 3,
      "throughput": 0.01171875,
      "errors": [],
      "effectiveIntegrationRuntime": "MyIntegrationRuntime",
      "billedDuration": 3
    }
    ```

## <a name="verify-the-output"></a>De uitvoer controleren
De uitvoermap `fromonprem` wordt automatisch door de pijplijn gemaakt in de blobcontainer `adftutorial`. Controleer of u het bestand **dbo.emp.txt** in de uitvoermap ziet. 

1. Klik in Azure Portal op de pagina met de **adftutorial**-container op **Vernieuwen** om de uitvoermap weer te geven.

    ![uitvoermap gemaakt](media/tutorial-hybrid-copy-powershell/fromonprem-folder.png)
2. Klik op `fromonprem` in de lijst met mappen. 
3. Controleer of u een bestand met de naam `dbo.emp.txt` ziet.

    ![uitvoerbestand](media/tutorial-hybrid-copy-powershell/fromonprem-file.png)


## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een data factory maken.
> * Een zelf-hostende Integration Runtime maken.
> * Gekoppelde services maken voor SQL Server en Azure Storage. 
> * Gegevenssets maken voor SQL Server en Azure Blob.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Start een pijplijnuitvoering.
> * De pijplijnuitvoering controleert.

Zie [Ondersteunde gegevensopslagexemplaren](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensopslagexemplaren die worden ondersteund door Azure Data Factory.

Ga door naar de volgende zelfstudie voor informatie over het bulksgewijs kopiëren van gegevens uit een bron naar een bestemming:

> [!div class="nextstepaction"]
>[Gegevens bulksgewijs kopiëren](tutorial-bulk-copy.md)
