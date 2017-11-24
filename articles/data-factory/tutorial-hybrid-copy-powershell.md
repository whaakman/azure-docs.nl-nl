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
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: afd7735712d03110a67509a7e94d336219a65b34
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="tutorial-copy-data-from-on-premises-sql-server-to-azure-blob-storage"></a>Zelfstudie: gegevens van een on-premises SQL-server naar Azure Blob Storage kopiëren
In deze zelfstudie gebruikt u Azure PowerShell om een Data Factory-pijplijn te maken waarmee gegevens worden gekopieerd van een on-premises SQL Server-database naar een Azure Blob-opslag. U maakt en gebruikt een zelf-hostende IR (Integration Runtime) van Azure Data Factory. Deze zorgt voor een integratie van on-premises gegevensopslagexemplaren en cloudgegevensopslag.  Zie [Quickstarts](quickstart-create-data-factory-dot-net.md) voor meer informatie over het gebruik van andere hulpprogramma's/SDK's voor het maken van een gegevensfactory.

Dit artikel is geen gedetailleerde introductie tot de Data Factory-service. Zie [Inleiding tot Azure Data Factory](introduction.md) voor een inleiding tot Azure Data Factory-service. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken.
> * Een zelf-hostende Integration Runtime maken.
> * Gekoppelde services maken voor SQL Server en Azure Storage. 
> * Gegevenssets maken voor SQL Server en Azure Blob.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Een pijplijnuitvoering starten.
> * De pijplijnuitvoering controleert.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

### <a name="sql-server-2014-or-2016"></a>SQL Server 2014 of 2016. 
In deze zelfstudie gebruikt u een on-premises SQL Server-database als een **brongegevensopslag**. Maak een tabel met de naam **emp** in uw SQL Server-database en voeg een aantal voorbeeldgegevens toe aan de tabel.

1. Start **SQL Server Management Studio**. Als u SQL Server 2016 gebruikt, moet u SQL Server Management Studio mogelijk afzonderlijk installeren via het [Downloadcentrum](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms). 
2. Maak verbinding met SQL Server met behulp van uw referenties. 
3. Maak een voorbeelddatabase. Klik in de structuurweergave met de rechtermuisknop op **Databases** en klik op **Nieuwe database**. Voer in het venster **Nieuwe database** een **naam** in voor de database en klik op **OK**. 
4. Voer het volgende script uit op de database; de **emp** tabel wordt gemaakt. In de structuurweergave klikt u met de rechtermuisknop op de **database** die u hebt gemaakt en klikt u op **Nieuwe query**. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Voer de volgende opdrachten uit op de database waarbij een aantal voorbeeldgegevens worden ingevoegd in de tabel:

    ```sql
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="azure-storage-account"></a>Azure-opslagaccount
In deze zelfstudie gaat u een algemeen Azure Storage-account (en dan met name een Blob Storage) gebruiken als een **doel/sink**-gegevensopslag. Zie het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen Azure Storage-account hebt voor algemene doeleinden en er een wilt maken.

#### <a name="get-storage-account-name-and-account-key"></a>De naam en sleutel van een opslagaccount ophalen
In deze QuickStart gaat u de naam en sleutel van uw Azure Storage-accountnaam gebruiken. De volgende procedure bevat stappen waarmee u de naam en sleutel van uw opslagaccount kunt ophalen. 

1. Open een browser en navigeer naar [Azure Portal](https://portal.azure.com). Meld u aan met uw Azure-gebruikersnaam en -wachtwoord. 
2. Klik op **Meer services >** in het menu links, filter op het trefwoord **Opslag** en selecteer **Opslagaccounts**.

    ![Zoeken naar een opslagaccount](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. Filter in de lijst met opslagaccounts op uw opslagaccount (indien nodig) en selecteer vervolgens **uw opslagaccount**. 
4. Selecteer op de pagina **Opslagaccount** de optie **Toegangssleutels** in het menu.

    ![De naam en sleutel van het opslagaccount ophalen](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. Kopieer de waarden voor de velden **Opslagaccountnaam** en **key1** naar het klembord. Plak deze in Kladblok of een andere editor en sla ze op. U gaat in de zelfstudie de naam van het opslagaccount en de sleutel gebruiken. 

#### <a name="create-the-adftutorial-container"></a>De container adftutorial maken 
In deze sectie maakt u in uw Azure Blob-opslag een blobcontainer met de naam adftutorial. 

1. Installeer [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) als deze nog niet op uw computer is geïnstalleerd. 
2. Start **Microsoft Azure Storage Explorer** op uw computer.   
3. Selecteer in het venster **Verbinding maken met Azure Storage** de optie **Een opslagaccountnaam en -sleutel gebruiken** en klik op **Volgende**. Als het venster **Verbinding maken met Azure Storage** niet wordt weergegeven, klikt u in de structuurweergave met de rechtermuisknop op **Opslagaccounts** en klikt u op **Verbinding maken met Azure Storage**. 

    ![Verbinding maken met Azure Storage](media/tutorial-hybrid-copy-powershell/storage-explorer-connect-azure-storage.png)
4. Plak in het venster **Toevoegen met naam en sleutel** de **accountnaam** en de **accountsleutel** die u tijdens de vorige stap hebt opgeslagen. Klik op **Volgende**. 
5. In het venster **Samenvatting verbinding** klikt u op **Verbinding maken**.
6. Controleer of uw opslagaccount voorkomt in de structuurweergave onder **(Lokaal en Toegevoegd)** -> **Opslagaccounts**. 
7. Vouw **Blob-containers** uit en controleer of er geen blobcontainer met de naam **adftutorial** bestaat. Als deze al bestaat, slaat u de volgende stappen voor het maken van de container over. 
8. Klik met de rechtermuisknop op **Blob-containers** en selecteer **Blob-container maken**.

    ![Blob-container maken](media/tutorial-hybrid-copy-powershell/stroage-explorer-create-blob-container-menu.png)
9. Voer **adftutorial** in als naam en druk op **ENTER**. 
10. Controleer of de container **adftutorial** in de structuurweergave is geselecteerd. In Data Factory wordt automatisch in deze container de uitvoermap gemaakt, zodat u er zelf geen hoeft te maken. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Azure PowerShell installeren
Installeer de nieuwste versie van Azure PowerShell als u deze niet al op uw computer hebt. 

1. Navigeer in uw webbrowser naar de pagina [Azure SDK-downloads en SDK’s](https://azure.microsoft.com/downloads/). 
2. Klik op **Windows installeren** in de sectie **Opdrachtregelprogramma's** -> **PowerShell**. 
3. Voer het **MSI**-bestand uit om Azure PowerShell te installeren. 

Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor gedetailleerde instructies. 

#### <a name="log-in-to-azure-powershell"></a>Aanmelden bij Azure PowerShell
Start **PowerShell** op uw computer. Houd Azure PowerShell geopend tot het einde van deze QuickStart. Als u het programma sluit en opnieuw opent, moet u de opdrachten opnieuw uitvoeren.

1. Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Als u meerdere Azure-abonnementen hebt, voert u de volgende opdracht uit om alle abonnementen voor dit account weer te geven:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Voer de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **SubscriptionId** door de id van uw Azure-abonnement:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Een data factory maken

1. Definieer een variabele voor de naam van de resourcegroep die u later gaat gebruiken in PowerShell-opdrachten. Kopieer de tekst van de volgende opdracht naar PowerShell, geef tussen dubbele aanhalingstekens een naam op voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en voer de opdracht uit. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Definieer een variabele voor de naam van de data factory die u later kunt gebruiken in PowerShell-opdrachten. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Definieer een variabele voor de locatie van de data factory: 

    ```powershell
    $location = "East US"
    ```
4. Voer de volgende opdracht uit om de resourcegroep te maken: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$resourceGroupName`-variabele en probeer het opnieuw. Als u de resourcegroep met anderen wilt delen, gaat u verder met de volgende stap.  
5. Voer de volgende cmdlet **Set AzureRmDataFactoryV2** uit om de data factory te maken: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Als u Data Factory-exemplaren wilt maken, moet u **bijdrager** of **beheerder** zijn van het Azure-abonnement.
* Momenteel kunt u met Data Factory versie 2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

## <a name="create-a-self-hosted-ir"></a>Een zelf-hostende IR maken

In deze sectie kunt u een zelf-hostende Integration Runtime maken en deze koppelen aan een on-premises knooppunt (machine).

1. Maak een variabele voor de naam van een Integration Runtime. 

    ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
    ```
1. Een zelf-hostende Integration Runtime maken. Gebruik een unieke naam voor het geval er al een Integration Runtime met dezelfde naam bestaat.

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

3. Voer de volgende opdracht uit om verificatiesleutels op te halen voor het registreren van de zelf-hostende Integration Runtime met Data Factory-service in de cloud. Kopieer een van de sleutels voor het registreren van de zelf-hostende Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

## <a name="install-integration-runtime"></a>Integration Runtime installeren
1. [Download](https://www.microsoft.com/download/details.aspx?id=39717) de zelf-hostende Integration Runtime op een lokale Windows-computer en voer de installatie uit. 
2. Klik bij **Welkom bij de installatiewizard van Microsoft Integration Runtime** op **Volgende**.  
3. Ga op de pagina **Gebruiksrechtovereenkomst** akkoord met de voorwaarden en de gebruiksrechtovereenkomst en klik op **Volgende**. 
4. Klik op de pagina **Doelmap** op **Volgende**. 
5. Klik bij **Gereed om Microsoft Integration Runtime te installeren** op **Installeren**. 
6. Als er een waarschuwingsbericht wordt weergegeven met de melding of de computer moet worden geconfigureerd om in de slaapstand of sluimerstand over te gaan als deze niet in gebruik is, klikt u op **OK**. 
7. Klik op de pagina **De installatiewizard voor Microsoft Integration Runtime is voltooid**  op **Voltooien**.
8. Plak de sleutel die u in de vorige sectie hebt opgeslagen op de pagina **Integration Runtime (zelf-hostend) registreren** en klik op **Registreren**. 

   ![Integration Runtime registreren](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)
2. U ziet het volgende bericht wanneer de zelf-hostende Integration Runtime is geregistreerd:

   ![Registratie is voltooid](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

3. Klik op de pagina **Nieuw knooppunt voor Integration Runtime (zelf-hostend)** op **Volgende**. 

    ![Pagina Nieuw knooppunt voor Integration Runtime](media/tutorial-hybrid-copy-powershell/new-integration-runtime-node-page.png)
4. Bij **Intranetcommunicatiekanaal** klikt u op **Overslaan**. U kunt een TLS/SSL-certificaat selecteren voor het beveiligen van de communicatie tussen knooppunten in een Integration Runtime-omgeving met meerdere knooppunten. 

    ![Pagina Intranetcommunicatiekanaal](media/tutorial-hybrid-copy-powershell/intranet-communication-channel-page.png)
5. Op de pagina **Integration Runtime (zelf-hostend) registeren** klikt u op **Configuration Manager starten**. 
6. Wanneer het knooppunt is verbonden met de cloudservice, ziet u de volgende pagina:

   ![Knooppunt is verbonden](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Gekoppelde services maken

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Een gekoppelde Azure Storage-service maken (bestemming/sink)

1. Maak een JSON-bestand met de naam **AzureStorageLinkedService.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud: Maak de map ADFv2Tutorial als deze nog niet bestaat.  

    > [!IMPORTANT]
    > Vervang &lt;accountName&gt; en &lt;accountKey&gt; door de naam en sleutel van uw Azure Storage-account voordat u het bestand opslaat.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```
2. Schakel in **Azure PowerShell** over naar de map **ADFv2Tutorial**.

   Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureStorageLinkedService** te maken. De cmdlets die worden gebruikt in deze zelfstudie, nemen de waarden voor de parameters **ResourceGroupName** en **DataFactoryName**. U kunt ook het **DataFactory**-object doorgeven dat met de cdmlet Set-AzureRmDataFactoryV2 is geretourneerd. U hoeft dan niet telkens wanneer u een cmdlet uitvoert ResourceGroupName en DataFactoryName op te geven.

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

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Een gekoppelde SQL Server-service (bron) maken en versleutelen

1. Maak een JSON-bestand met de naam **SqlServerLinkedService.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud: vervang voordat u het bestand opslaat **&lt;servername>**, **&lt;databasename>**, **&lt;username>**, **&lt;servername>**, en **&lt;password>** door de waarden van uw SQL-server. 

    > [!IMPORTANT]
    > Vervang de **naam>** van de **&lt;Integration** **Runtime** door de naam van uw Integration Runtime.

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
2. Voer **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** uit en geef de bovenstaande JSON-nettolading door om de gevoelige gegevens uit de JSON-nettolading te versleutelen in de on-premises Integration Runtime. Deze versleuteling zorgt ervoor dat de referenties zijn versleuteld met behulp van DPAPI (Data Protection Application Programming Interface). De versleutelde referenties worden lokaal op het zelf-hostende Integration Runtime-knooppunt (lokale computer) opgeslagen. De uitvoernettolading kan worden omgeleid naar een ander JSON-bestand (in dit geval encryptedLinkedService.json). Dit bestand bevat de versleutelde referenties.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Voer de volgende opdracht uit door het JSON-bestand uit de vorige stap te gebruiken om de **SqlServerLinkedService** te maken:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u invoer- en uitvoergegevenssets die invoer- en uitvoergegevenssets voor de kopieerbewerking vertegenwoordigen (on-premises SQL Server-database => Azure Blob-opslag).

### <a name="create-a-dataset-for-source-sql-database"></a>Een gegevensset maken voor bron SQL-Database

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

1. Maak een JSON-bestand met de naam **AzureBlobDataset.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud:

    > [!IMPORTANT]
    > In deze voorbeeldcode wordt ervan uitgegaan dat u een container hebt met de naam **adftutorial** in de Azure Blob-opslag.

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

## <a name="create-pipelines"></a>Pijplijnen maken

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>De pijplijn SqlServerToBlobPipeline maken

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



## <a name="start-and-monitor-a-pipeline-run"></a>Een pijplijnuitvoering starten en controleren

1. Start een pijplijnuitvoering voor de pijplijn SQLServerToBlobPipeline en leg de id voor de pijplijnuitvoering vast voor toekomstige controle.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Voer het volgende script uit om continu de uitvoeringsstatus van de pijplijn **SQLServerToBlobPipeline** te controleren, en druk het eindresultaat af.

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

3. U kunt de run-id van de pijplijn **SQLServerToBlobPipeline** ophalen en vervolgens het gedetailleerde uitvoeringsresultaat van de activiteit controleren.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Hier volgt een voorbeeld van de voorbeelduitvoering:

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
De uitvoermap `fromonprem` wordt automatisch door de pijplijn gemaakt in de blobcontainer `adftutorial`. Controleer of u het bestand **dbo.emp.txt** in de uitvoermap ziet. Gebruik [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) om te controleren of de uitvoer is gemaakt. 

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een data factory maken.
> * Een zelf-hostende Integration Runtime maken.
> * Gekoppelde services maken voor SQL Server en Azure Storage. 
> * Gegevenssets maken voor SQL Server en Azure Blob.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Een pijplijnuitvoering starten.
> * De pijplijnuitvoering controleert.

Zie [Ondersteunde gegevensopslagexemplaren](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensopslagexemplaren die worden ondersteund door Azure Data Factory.

Ga door naar de volgende zelfstudie voor informatie over het bulksgewijs kopiëren van gegevens uit een bron naar een bestemming:

> [!div class="nextstepaction"]
>[Gegevens bulksgewijs kopiëren](tutorial-bulk-copy.md)
