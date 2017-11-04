---
title: Op aanvraag met behulp van de Data Factory - Azure HDInsight Hadoop-clusters maken | Microsoft Docs
description: Informatie over het maken van op aanvraag Hadoop-clusters in HDInsight met behulp van Azure Data Factory.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: b9b73f6691af957e42236ef9a223411a0296f96f
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Hadoop-clusters op aanvraag maken in HDInsight met behulp van Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

[Azure Data Factory](../data-factory/introduction.md) is een cloud-gebaseerde gegevens integration-service die ingedeeld en automatiseert de verplaatsing en transformatie van gegevens. Deze kunt maken van een HDInsight Hadoop-cluster just-in-time voor het verwerken van een segment invoergegevens en verwijderen van het cluster wanneer het verwerken voltooid is. Enkele van de voordelen van het gebruik van een on-demand HDInsight Hadoop-cluster zijn:

- U alleen betalen voor de taak tijd wordt uitgevoerd op de HDInsight Hadoop-cluster (plus een korte configureerbare niet-actieve tijd). De facturering voor HDInsight-clusters worden pro rato per minuut, of u ze worden gebruikt of niet. Wanneer u een gekoppelde HDInsight-service op aanvraag in de Data Factory gebruikt, kan de clusters op aanvraag worden gemaakt. En de clusters worden automatisch verwijderd wanneer de taken zijn voltooid. Daarom betaalt u alleen voor de taak met de tijd en de korte niet-actieve tijd (time to live-instelling).
- U kunt een werkstroom met behulp van een Data Factory-pijplijn maken. U kunt bijvoorbeeld de pijplijn gegevens kopiëren van een lokale SQL Server naar een Azure blob storage, de gegevens worden verwerkt door het uitvoeren van een Hive-script en Pig-script op een on-demand HDInsight Hadoop-cluster hebben. Kopieer vervolgens de resultaatgegevens naar een Azure SQL Data Warehouse voor BI-toepassingen om te gebruiken.
- U kunt plannen dat de werkstroom periodiek wordt uitgevoerd (elk uur, dagelijks, wekelijks, maandelijks, enzovoort).

Een gegevensfactory kan één of meer gegevenspijplijnen hebben in Azure Data Factory. Een pijplijn gegevens heeft een of meer activiteiten. Er zijn twee soorten activiteiten: [activiteiten voor gegevensverplaatsing](../data-factory/copy-activity-overview.md) en [activiteiten voor gegevenstransformatie](../data-factory/transform-data.md). Activiteiten voor gegevensverplaatsing (momenteel alleen Kopieeractiviteit) kunt u gegevens uit een gegevensopslag bron verplaatsen naar een doelgegevensopslagplaats. Kunt u activiteiten voor gegevenstransformatie transformatieproces gegevens. HDInsight Hive-activiteit is een van de activiteiten voor gegevenstransformatie ondersteund door Data Factory. U kunt de Hive-transformatie-activiteit gebruiken in deze zelfstudie.

U kunt een hive-activiteit voor het gebruik van uw eigen HDInsight Hadoop-cluster of een on-demand HDInsight Hadoop-cluster configureren. In deze zelfstudie wordt de Hive-activiteit in de data factory-pijplijn geconfigureerd voor gebruik van een HDInsight-cluster op aanvraag. Wanneer de activiteit wordt uitgevoerd voor het verwerken van een gegevenssegment, dus hier wat er gebeurt:

1. Een HDInsight Hadoop-cluster wordt automatisch gemaakt voor u just-in-time voor het verwerken van het segment.  
2. De ingevoerde gegevens worden verwerkt door een HiveQL-script uitgevoerd op het cluster.
3. De HDInsight Hadoop-cluster wordt verwijderd nadat de verwerking voltooid is en het cluster niet actief voor de geconfigureerde hoeveelheid tijd (timeToLive-instelling is). Als het volgende gegevenssegment voor verwerking met binnen deze timeToLive niet-actieve tijd beschikbaar is, wordt hetzelfde cluster wordt gebruikt voor het verwerken van het segment.  

In deze zelfstudie maakt voert het HiveQL-script dat is gekoppeld aan het hive-activiteit de volgende acties:

1. Maakt een externe tabel die verwijst naar de onbewerkte web logboekgegevens opgeslagen in een Azure-blobopslag.
2. De onbewerkte gegevens partitioneert op jaar en maand.
3. De gepartitioneerde-gegevens opslaat in Azure blob storage.

In deze zelfstudie maakt het HiveQL-script dat is gekoppeld aan het hive-activiteit een externe tabel die verwijst naar de onbewerkte web logboekgegevens opgeslagen in de Azure Blob Storage. Hier ziet u de voorbeeldrijen per maand in het invoerbestand.

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Het HiveQL-script partities van de onbewerkte gegevens op jaar en maand. Drie uitvoermappen op basis van de vorige invoer wordt gemaakt. Elke map bevat een bestand met vermeldingen van elke maand.

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

Zie voor een lijst van activiteiten voor gegevenstransformatie Data Factory naast het Hive-activiteit [transformeren en analyseren met Azure Data Factory](../data-factory/transform-data.md).

> [!NOTE]
> U kunt op dit moment alleen HDInsight-cluster versie 3.2 maken uit Azure Data Factory.

## <a name="prerequisites"></a>Vereisten
Voordat u de instructies in dit artikel, hebt u de volgende items:

* [Azure-abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a>Voorbereiden van de storage-account
In dit scenario kunt u maximaal drie storage-accounts:

- storage-standaardaccount voor het HDInsight-cluster
- Storage-account voor de invoergegevens
- Storage-account voor de uitvoergegevens

Om te vereenvoudigen de zelfstudie, kunt u één opslagaccount voor de drie doeleinden worden gebruikt. De Azure PowerShell-voorbeeldscript vinden in deze sectie voert de volgende taken:

1. Aanmelden bij Azure.
2. Maak een Azure-resourcegroep.
3. Maak een Azure Storage-account.
4. Een Blob-container in het opslagaccount maken
5. Kopieer de volgende twee bestanden naar de Blob-container:

   * Invoergegevens bestand: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)
   * HiveQL-script: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)

     Beide bestanden worden opgeslagen in een openbare Blob-container.


**Voor het voorbereiden van de opslag en kopieer de bestanden met Azure PowerShell:**
> [!IMPORTANT]
> Geef namen voor de Azure-resourcegroep en de Azure storage-account die door het script wordt gemaakt.
> Noteer **Resourcegroepnaam**, **opslagaccountnaam**, en **opslagaccountsleutel** output door het script. U moet deze in de volgende sectie.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

Als u hulp nodig bij het PowerShell-script, Zie [Azure PowerShell gebruiken met Azure Storage](../storage/common/storage-powershell-guide-full.md). Als u Azure CLI gebruiken in plaats daarvan, Zie de [bijlage](#appendix) sectie voor het script voor Azure CLI.

**Het opslagaccount en de inhoud te onderzoeken**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **resourcegroepen** in het linkerdeelvenster.
3. Dubbelklik op de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Het filter gebruiken als er te veel resourcegroepen die worden vermeld.
4. Op de **Resources** tegel, wordt er een resource in de lijst, tenzij u de resourcegroep met andere projecten delen. Deze resource is het opslagaccount met de naam die u eerder hebt opgegeven. Klik op de naam van het opslagaccount.
5. Klik op de **Blobs** tegels.
6. Klik op de **adfgetstarted** container. U ziet twee mappen: **inputdata** en **script**.
7. Open de map en controleert u de bestanden in de mappen. De invoergegevens bevat het bestand input.log met invoergegevens en de scriptmap bevat het bestand HiveQL-script.

## <a name="create-a-data-factory-using-resource-manager-template"></a>Maak een gegevensfactory met Resource Manager-sjabloon
Met de storage-account, de invoergegevens en het HiveQL-script dat is voorbereid, bent u klaar voor het maken van een Azure data factory. Er zijn verschillende methoden voor het maken van de gegevensfactory. In deze zelfstudie maakt maken u een gegevensfactory door het implementeren van een Azure Resource Manager-sjabloon met de Azure portal. U kunt ook een Resource Manager-sjabloon implementeren met behulp van [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) en [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template). Zie voor andere data factory-aanmaakmethoden [zelfstudie: uw eerste gegevensfactory bouwen](../data-factory/quickstart-create-data-factory-dot-net.md).

1. Klik op de volgende afbeelding om u aan te melden bij Azure en de Resource Manager-sjabloon in Azure Portal te openen. De sjabloon bevindt zich op https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json. Zie de [Data Factory-entiteiten in de sjabloon](#data-factory-entities-in-the-template) sectie voor gedetailleerde informatie over entiteiten gedefinieerd in de sjabloon. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Selecteer **gebruik bestaande** optie voor de **resourcegroep** instelling en selecteer de naam van de resourcegroep die u hebt gemaakt in de vorige stap (met behulp van PowerShell-script).
3. Voer een naam voor de gegevensfactory (**Data Factory Name**). Deze naam moet uniek zijn.
4. Voer de **opslagaccountnaam** en **opslagaccountsleutel** u in de vorige stap hebt genoteerd.
5. Selecteer **ik ga akkoord met de voorwaarden en bepalingen** hierboven vermeld wordt na het lezen van via **voorwaarden en bepalingen**.
6. Selecteer **vastmaken aan dashboard** optie.
6. Klik op **aankoop/maken**. U ziet een tegel op het Dashboard aangeroepen **implementatie van sjabloonimplementatie**. Wacht totdat de **resourcegroep** blade voor de resourcegroep wordt geopend. U kunt ook klikken op de tegel met de titel als de naam van uw resources te openen van de blade met resourcegroepen.
6. Klik op de tegel om te openen van de resourcegroep als de resourcegroepblade nog niet is geopend. U ziet nu één meer data factory resource vermeld naast de opslagbronnen-account.
7. Klik op de naam van uw gegevensfactory (waarde als u hebt opgegeven voor de **Data Factory Name** parameter).
8. Klik in de Data Factory-blade op de **Diagram** tegel. Het diagram ziet u een activiteit met een invoergegevensset en een uitvoergegevensset:

    ![Azure Data Factory HDInsight op aanvraag Hive-activiteit pipeline-diagram](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    De namen zijn gedefinieerd in het Resource Manager-sjabloon.
9. Dubbelklik op **AzureBlobOutput**.
10. Op de **onlangs bijgewerkt segmenten**, ziet u een segment. Als de status **Bezig**, wacht u totdat deze is gewijzigd in **gereed**. Het duurt meestal over **20 minuten** om een HDInsight-cluster te maken.

### <a name="check-the-data-factory-output"></a>Controleer de uitvoer van de data factory

1. Gebruik dezelfde procedure in de laatste sessie om te controleren van de containers van de container adfgetstarted. Er zijn twee nieuwe containers naast **adfgetsarted**:

   * Een container met de naam die voldoet aan het patroon: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`. Deze container is de standaardcontainer voor het HDInsight-cluster.
   * adfjobs: deze container is de container voor de logboeken van de ADF-taak.

     De data factory-uitvoer wordt opgeslagen in **afgetstarted** zoals u in de Resource Manager-sjabloon hebt geconfigureerd.
2. Klik op **adfgetstarted**.
3. Dubbelklik op **partitioneddata**. U ziet een **jaar = 2014** map omdat alle weblogboeken datum in het jaar 2014.

    ![Azure Data Factory HDInsight op aanvraag Hive pijplijn uitvoer van activiteit](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    Als u de lijst detailanalyse, ziet u drie mappen voor januari, februari en maart. En er is een logboekbestand voor elke maand.

    ![Azure Data Factory HDInsight op aanvraag Hive pijplijn uitvoer van activiteit](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-the-template"></a>Data Factory-entiteiten in de sjabloon
Hier ziet u hoe de op het hoogste niveau Resource Manager-sjabloon voor een data factory eruit:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a>Een gegevensfactory definiëren
U definieert een gegevensfactory in de Resource Manager-sjabloon zoals in het volgende voorbeeld wordt weergegeven:  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
De dataFactoryName is de naam van de gegevensfactory die u opgeeft wanneer u de sjabloon implementeert. Gegevensfactory is momenteel alleen ondersteund in de regio's VS-Oost, VS-West en Noord-Europa.

### <a name="defining-entities-within-the-data-factory"></a>Entiteiten in de gegevensfactory definiëren
De volgende Data Factory-entiteiten worden in de JSON-sjabloon gedefinieerd:

* [Een gekoppelde Azure Storage-service](#azure-storage-linked-service)
* [Een gekoppelde HDInsight-service op aanvraag](#hdinsight-on-demand-linked-service)
* [De Azure Blob-invoergegevensset](#azure-blob-input-dataset)
* [De Azure Blob-uitvoergegevensset](#azure-blob-output-dataset)
* [De gegevenspijplijn met een kopieerbewerking](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
De gekoppelde Azure Storage-service koppelt uw Azure-opslagaccount aan de gegevensfactory. In deze zelfstudie wordt hetzelfde opslagaccount gebruikt als de storage-standaardaccount HDInsight, invoergegevens-opslag- en uitvoer gegevensopslag. Daarom definieert u slechts één Azure Storage service gekoppelde. In de definitie van de gekoppelde service geeft u de naam en sleutel van uw Azure storage-account. Zie [Een gekoppelde Azure Storage-service](../data-factory/connector-azure-blob-storage.md) voor meer informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van een gekoppelde Azure Storage-service.

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
De tekenreeks **connectionString** maakt gebruik van de parameters storageAccountName en storageAccountKey. U kunt waarden voor deze parameters opgeven tijdens de implementatie van de sjabloon.  

#### <a name="hdinsight-on-demand-linked-service"></a>Een gekoppelde HDInsight-service op aanvraag
In de definitie op aanvraag een gekoppelde HDInsight-service geeft u waarden voor de configuratieparameters die worden gebruikt door de Data Factory-service voor het maken van een HDInsight Hadoop-cluster tijdens runtime. Zie het artikel [Compute linked services (Gekoppelde services verwerken)](../data-factory/compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van een gekoppelde HDInsight-service op aanvraag.  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
Houd rekening met de volgende punten:

* De Data Factory maakt een **op basis van Linux** HDInsight-cluster voor u.
* De HDInsight Hadoop-cluster wordt gemaakt in dezelfde regio bevinden als het opslagaccount.
* U ziet de *timeToLive* instelling. De gegevensfactory wordt het cluster automatisch verwijderd nadat het cluster is inactiviteit gedurende 30 minuten.
* Het HDInsight-cluster maakt een **standaardcontainer** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight verwijdert deze container niet wanneer het cluster wordt verwijderd. Dit gedrag is standaard. Met een gekoppelde on-demand HDInsight-service wordt er steeds een HDInsight-cluster gemaakt wanneer er een segment moet worden verwerkt, tenzij er een bestaand livecluster is (**timeToLive**). Het cluster wordt verwijderd wanneer het verwerken is voltooid.

Zie [Gekoppelde on-demand HDInsight-service](../data-factory/compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie.

> [!IMPORTANT]
> Naarmate er meer segmenten worden verwerkt, verschijnen er meer containers in uw Azure-blobopslag. Als u deze niet nodig hebt voor het oplossen van problemen met taken, kunt u ze verwijderen om de opslagkosten te verlagen. De namen van deze containers worden als volgt opgebouwd: adf**naamvanuwgegevensfactory**-**naamvangekoppeldeservice**-datum-/tijdstempel. Gebruik hulpprogramma's zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) om containers in uw Azure-blobopslag te verwijderen.

#### <a name="azure-blob-input-dataset"></a>Azure Blob-invoergegevensset
In de definitie van de invoergegevensset geeft u de namen van de blob-container, map en -bestand dat de invoergegevens bevat. Zie [Eigenschappen van de Azure Blob-gegevensset](../data-factory/connector-azure-blob-storage.md) voor meer informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van een Azure Blob-gegevensset.

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}

```

U ziet de volgende specifieke instellingen in de JSON-definitie:

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a>Azure Blob-uitvoergegevensset
In de definitie van de uitvoer-gegevensset geeft u de namen van blob-container en map die uitvoergegevens bevat. Zie [Eigenschappen van de Azure Blob-gegevensset](../data-factory/connector-azure-blob-storage.md) voor meer informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van een Azure Blob-gegevensset.  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

FolderPath geeft het pad naar de map die uitvoergegevens bevat:

```json
"folderPath": "adfgetstarted/partitioneddata",
```

De [gegevensset beschikbaarheid](../data-factory/concepts-datasets-linked-services.md) instelling is als volgt:

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

In Azure Data Factory-uitvoer gegevensset beschikbaarheid stations de pijplijn. In dit voorbeeld wordt maandelijks op de laatste dag van de maand (EndOfInterval) het segment geproduceerd. 

#### <a name="data-pipeline"></a>Gegevenspijplijn
Definieert u een pijplijn waarmee gegevens worden omgezet door het Hive-script uitvoeren op een on-demand Azure HDInsight cluster. Zie [JSON-bestand voor een pijplijn](../data-factory/concepts-pipelines-activities.md) voor beschrijvingen van JSON-elementen die worden gebruikt voor het definiëren van een pijplijn in dit voorbeeld.

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

De pijplijn bevat één activiteit, HDInsightHive-activiteit. Als zowel begin- en einddatum in januari 2016, gegevens zijn voor slechts één maand (een segment) wordt verwerkt. Beide *start* en *end* hebben van de activiteit een datum in het verleden, zodat de Data Factory gegevens voor de maand onmiddellijk verwerkt. Als het end in de toekomst is, wordt een ander segment in de data factory maakt wanneer dat nodig is. Zie voor meer informatie [Data Factory plannen en uitvoeren](../data-factory/v1/data-factory-scheduling-and-execution.md).

## <a name="clean-up-the-tutorial"></a>De zelfstudie opschonen

### <a name="delete-the-blob-containers-created-by-on-demand-hdinsight-cluster"></a>De blob-containers die is gemaakt door on-demand HDInsight-cluster verwijderen
Met de gekoppelde HDInsight-service op aanvraag wordt een HDInsight-cluster gemaakt telkens wanneer een segment moet worden verwerkt, tenzij er een bestaand livecluster (timeToLive); en het cluster wordt verwijderd wanneer het verwerken is voltooid. Voor elk cluster maakt Azure Data Factory een blob-container in Azure blob storage gebruikt als het standaardaccount voor stroage voor het cluster. Hoewel de HDInsight-cluster wordt verwijderd, worden de standaard blob storage-container en het bijbehorende opslagaccount niet verwijderd. Dit gedrag is standaard. Naarmate er meer segmenten worden verwerkt, verschijnen er meer containers in uw Azure-blobopslag. Als u deze niet nodig hebt voor het oplossen van problemen met taken, kunt u ze verwijderen om de opslagkosten te verlagen. De namen van deze containers volgen een patroon: `adfyourdatafactoryname-linkedservicename-datetimestamp`.

Verwijder de **adfjobs** en **adfyourdatafactoryname-linkedservicename-datum** mappen. De container adfjobs bevat taaklogboeken uit Azure Data Factory.

### <a name="delete-the-resource-group"></a>De resourcegroep verwijderen
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) wordt gebruikt om te implementeren, beheren en bewaken van uw oplossing als een groep.  Een resourcegroep verwijdert, worden de onderdelen binnen de groep.  

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **resourcegroepen** in het linkerdeelvenster.
3. Klik op de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Het filter gebruiken als er te veel resourcegroepen die worden vermeld. De resourcegroep in een nieuwe blade geopend.
4. Op de **Resources** tegel u heeft het standaardopslagaccount en de gegevensfactory weergegeven tenzij u de resourcegroep met andere projecten delen.
5. Klik op **verwijderen** boven aan de blade. In dat geval worden de storage-account en de gegevens die zijn opgeslagen in het opslagaccount verwijderd.
6. Voer de naam van de resourcegroep verwijderen bevestigen en klik vervolgens op **verwijderen**.

Als u niet dat het storage-account wordt verwijderd wilt wanneer u de resourcegroep verwijdert, kunt u de volgende architectuur door te scheiden van de bedrijfsgegevens van het standaardopslagaccount. In dit geval hebt u een resourcegroep voor het opslagaccount met de zakelijke gegevens en de andere resourcegroep voor het standaardopslagaccount voor HDInsight-service en de data factory gekoppeld. Wanneer u de tweede resourcegroep verwijdert, heeft deze geen gevolgen voor de zakelijke gegevens storage-account. Dit doet u als volgt:

* Voeg het volgende toe aan de site op het hoogste resourcegroep samen met de Microsoft.DataFactory/datafactories resource in het Resource Manager-sjabloon. Hiermee maakt u een opslagaccount:

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* Een nieuw gekoppelde service-punt aan het nieuwe opslagaccount toevoegen:

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* De service van HDInsight ondemand gekoppeld met een extra dependsOn en een additionalLinkedServiceNames configureren:

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Azure Data Factory gebruiken voor het maken van HDInsight-cluster op aanvraag voor het verwerken van Hive-taken. Voor meer informatie:

* [Hadoop-zelfstudie: aan de slag met Hadoop op basis van Linux in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Hadoop op basis van Linux-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight-documentatie](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Data factory-documentatie](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a>Bijlage

### <a name="azure-cli-script"></a>Azure CLI-script
U kunt Azure CLI gebruiken in plaats van de zelfstudie wilt met behulp van Azure PowerShell. Voor het gebruik van Azure CLI, moet u eerst Azure CLI installeren volgens de volgende instructies:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-to-prepare-the-storage-and-copy-the-files"></a>Azure CLI gebruiken voor het voorbereiden van de opslag en kopieer de bestanden

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

De containernaam van de is *adfgetstarted*. Houd het omdat deze. Anders moet u de sjabloon van de Resource Manager bijwerken. Als u hulp nodig bij dit script CLI, Zie [met de Azure CLI met Azure Storage](../storage/common/storage-azure-cli.md).
