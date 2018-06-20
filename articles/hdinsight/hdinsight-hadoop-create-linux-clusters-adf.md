---
title: 'Zelfstudie: Hadoop-clusters op aanvraag maken in Azure HDInsight met behulp van de Data Factory | Microsoft Docs'
description: Informatie over het maken van op aanvraag Hadoop-clusters in HDInsight met behulp van Azure Data Factory.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: nitinme
ms.openlocfilehash: 9d54d3481176b36a0d13a9b8af2fad03349b81be
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229250"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Zelfstudie: Hadoop-clusters op aanvraag maken in HDInsight met behulp van Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In dit artikel leert u het maken van een Hadoop-cluster op aanvraag in Azure HDInsight met behulp van Azure Data Factory. Vervolgens gebruikt u gegevenspijplijnen in Azure Data Factory voor het uitvoeren van Hive-taken en het cluster verwijdert. Aan het einde van deze zelfstudie leert u hoe aan operationeel maken van een grote taak waar het maken van het cluster, taak uitvoeren en verwijderen van een cluster worden uitgevoerd volgens een schema worden uitgevoerd.

Deze zelfstudie bestaat uit de volgende taken: 

> [!div class="checklist"]
> * Een Azure-opslagaccount maken
> * Azure Data Factory-activiteit van uw begrijpen
> * Maak een gegevensfactory met Azure portal
> * Gekoppelde services maken
> * Een pijplijn maken
> * Een pijplijn activeren
> * Een pijplijn bewaken
> * De uitvoer controleren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Azure PowerShell. Zie voor instructies [installeren en configureren van Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

* Een Azure Active Directory-service-principal. Nadat u de service-principal gemaakt hebt, moet u voor het ophalen van de **toepassings-ID** en **verificatiesleutel** met de instructies in het gekoppelde artikel. Verderop in deze zelfstudie moet u deze waarden. Controleer ook of de service-principal is lid van de *Inzender* rol van het abonnement of de resourcegroep waarin het cluster is gemaakt. Zie voor instructies voor het ophalen van de vereiste waarden en de juiste rollen toewijzen [een Azure Active Directory-service-principal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

In deze sectie maakt u een opslagaccount dat wordt gebruikt als de opslag van de standaard voor het HDInsight-cluster dat u op aanvraag maakt. Dit opslagaccount bevat ook het voorbeeld HiveQL-script (**hivescript.hql**) waarmee u een voorbeeld Hive-taak die wordt uitgevoerd op het cluster te simuleren.

Deze sectie wordt een Azure PowerShell-script gebruikt voor het maken van het opslagaccount en kopieer via de vereiste bestanden in het opslagaccount. De Azure PowerShell-voorbeeldscript in deze sectie voert de volgende taken:

1. Logboeken in naar Azure.
2. Maakt een Azure-resourcegroep.
3. Hiermee maakt u een Azure-opslagaccount.
4. Hiermee maakt u een Blob-container in het opslagaccount
5. Kopieert het voorbeeldscript HiveQL (**hivescript.hql**) de Blob-container. Het script is beschikbaar op [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). Het voorbeeldscript is al beschikbaar in een andere openbare Blob-container. Het onderstaande PowerShell-script maakt een kopie van deze bestanden in de Azure Storage-account die wordt gemaakt.


**Een opslagaccount maken en kopieer de bestanden met Azure PowerShell:**
> [!IMPORTANT]
> Geef namen voor de Azure-resourcegroep en de Azure storage-account die door het script wordt gemaakt.
> Noteer **Resourcegroepnaam**, **opslagaccountnaam**, en **opslagaccountsleutel** output door het script. U moet deze in de volgende sectie.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
Login-AzureRmAccount
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

**Om te controleren of de storage-account maken**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **resourcegroepen** in het linkerdeelvenster.
3. Dubbelklik op de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Het filter gebruiken als er te veel resourcegroepen die worden vermeld.
4. Op de **Resources** tegel, ziet u een resource in de lijst, tenzij u de resourcegroep met andere projecten delen. Deze resource is het opslagaccount met de naam die u eerder hebt opgegeven. Selecteer de naam van het opslagaccount.
5. Selecteer de **Blobs** tegels.
6. Selecteer de **adfgetstarted** container. U ziet een map met de naam **hivescripts**.
7. Open de map en zorg ervoor dat het script voorbeeldbestand bevat **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Inzicht in de Azure Data Factory-activiteit

[Azure Data Factory](../data-factory/introduction.md) ingedeeld en automatiseert de verplaatsing en transformatie van gegevens. Azure Data Factory kunt maken van een HDInsight Hadoop-cluster just-in-time voor het verwerken van een segment invoergegevens en verwijderen van het cluster wanneer het verwerken voltooid is. 

Een gegevensfactory kan één of meer gegevenspijplijnen hebben in Azure Data Factory. Een pijplijn gegevens heeft een of meer activiteiten. Er zijn twee soorten activiteiten:

* [Activiteiten voor gegevensverplaatsing](../data-factory/copy-activity-overview.md) -u activiteiten voor gegevensverplaatsing gebruiken om gegevens te verplaatsen van een brongegevensarchief naar een doelgegevensopslagplaats.
* [Activiteiten voor gegevenstransformatie](../data-factory/transform-data.md). Kunt u activiteiten voor gegevenstransformatie transformatieproces gegevens. HDInsight Hive-activiteit is een van de activiteiten voor gegevenstransformatie ondersteund door Data Factory. U kunt de Hive-transformatie-activiteit gebruiken in deze zelfstudie.

In dit artikel configureert u het Hive-activiteit voor het maken van een on-demand HDInsight Hadoop-cluster. Wanneer de activiteit wordt uitgevoerd om gegevens te verwerken, moet u dit is wat er gebeurt:

1. Een HDInsight Hadoop-cluster wordt automatisch gemaakt voor u just-in-time voor het verwerken van het segment. 

2. De ingevoerde gegevens worden verwerkt door een HiveQL-script uitgevoerd op het cluster. In deze zelfstudie maakt voert het HiveQL-script dat is gekoppeld aan het hive-activiteit de volgende acties:

    * Maakt gebruik van de bestaande tabel (*hivesampletable*) voor het maken van een andere tabel **HiveSampleOut**.
    * Vult de **HiveSampleOut** tabel met alleen specifieke kolommen van de oorspronkelijke *hivesampletable*.

3. De HDInsight Hadoop-cluster wordt verwijderd nadat de verwerking voltooid is en het cluster niet actief voor de geconfigureerde hoeveelheid tijd (timeToLive-instelling is). Als het volgende gegevenssegment voor verwerking met binnen deze timeToLive niet-actieve tijd beschikbaar is, wordt hetzelfde cluster wordt gebruikt voor het verwerken van het segment.  

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer in de Azure-portal **maken van een resource** > **gegevens en analyse** > **Data Factory**.

    ![Azure Data Factory in de portal](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Data Factory in de portal")

2. Typ of Selecteer de waarden, zoals wordt weergegeven in de volgende schermafbeelding:

    ![Azure Data Factory met Azure portal maken](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "maken Azure Data Factory met Azure portal")

    Typ of selecteer de volgende waarden:
    
    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Naam** |  Voer een naam voor de data factory. Deze naam moet uniek zijn.|
    |**Abonnement**     |  Selecteer uw Azure-abonnement. |
    |**Resourcegroep**     | Selecteer **gebruik bestaande** en selecteer vervolgens de resourcegroep hebt gemaakt met behulp van het PowerShell-script. |
    |**Versie**     | Selecteer **V2 (Preview)** |
    |**Locatie**     | De locatie wordt automatisch ingesteld op de locatie die u hebt opgegeven tijdens het maken van de resourcegroep eerder. Voor deze zelfstudie wordt de locatie ingesteld op **VS-Oost 2**. |
    

3. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**. U ziet een nieuwe tegel met de mededeling dat **de implementatie wordt verzonden**. Maken van een gegevensfactory kan duren voordat een willekeurige plaats tussen 2 tot 4 minuten.

    ![Voortgang van de implementatie van sjabloon](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "sjabloon implementatie uitgevoerd") 
 
4. Zodra de gegevensfactory is gemaakt, ziet u de portal het overzicht voor de data factory.

    ![Overzicht van Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Data Factory-overzicht")

5. Selecteer **auteur & Monitor** Azure Data Factory ontwerp en de bewaking van de portal te starten.

## <a name="create-linked-services"></a>Gekoppelde services maken

In deze sectie kunt u twee gekoppelde services schrijven binnen uw gegevensfactory.

- Een **gekoppelde Azure Storage-service** waarmee een Azure-opslagaccount wordt gekoppeld aan de gegevensfactory. Deze opslag wordt gebruikt voor het HDInsight-cluster op aanvraag. Het bevat ook het Hive-script dat wordt uitgevoerd op het cluster.
- Een **gekoppelde HDInsight-service op aanvraag**. Azure Data Factory automatisch wordt gemaakt van een HDInsight-cluster en het Hive-script wordt uitgevoerd. Het HDInsight-cluster wordt vervolgens verwijderd als het cluster gedurende een vooraf geconfigureerde tijd inactief is geweest.

###  <a name="create-an-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken

1. In het linkerdeelvenster van de **aan de slag** pagina de **bewerken** pictogram.

    ![Maak een Azure Data Factory gekoppelde service](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "een Azure Data Factory gekoppelde service maken")

2. Selecteer **verbindingen** van de linkerbenedenhoek van het venster en selecteer vervolgens **+ nieuw**.

    ![Verbindingen maken in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "verbindingen maken in Azure Data Factory")

3. In de **nieuwe gekoppelde Service** dialoogvenster, **Azure Blob Storage** en selecteer vervolgens **doorgaan**.

    ![Maak Azure Storage gekoppelde service voor Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "maken Azure Storage gekoppelde service voor Data Factory")

4. Geef een naam op voor de service-opslag gekoppeld, selecteert u de Azure Storage-account dat u hebt gemaakt als onderdeel van het PowerShell-script en selecteer **voltooien**.

    ![Geef de naam op voor Azure Storage gekoppelde service](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "Geef de naam op voor Azure Storage gekoppelde service")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Een gekoppelde HDInsight-service op aanvraag maken

1. Selecteer nogmaals de knop **+ Nieuw** om een andere gekoppelde service te maken.

2. Selecteer in het venster **Nieuwe gekoppelde service** **Compute** > **Azure HDInsight** en selecteer vervolgens **Doorgaan**.

    ![Maak HDInsight gekoppelde service voor Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "HDInsight maken gekoppelde service voor Azure Data Factory")

3. In de **nieuwe gekoppelde Service** venster de vereiste waarden opgeven.

    ![Geef waarden op voor HDInsight gekoppelde service](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "Geef waarden op voor HDInsight gekoppelde service")

    Voer de volgende waarden en laat de overige als standaard.

    | Eigenschap | Beschrijving |
    | --- | --- |
    | Naam | Voer een naam voor de gekoppelde HDInsight-service |
    | Type | Selecteer **On-demand HDInsight** |
    | Een gekoppelde Azure Storage-service | Selecteer de gekoppelde Storage-service die u eerder hebt gemaakt. |
    | Clustertype | Selecteer **hadoop** |
    | Gaat live over | Geef de duur die u het HDInsight-cluster beschikbaar wilt stellen wilt voordat ze automatisch worden verwijderd.|
    | Service-principal-id | Geef de toepassings-ID van de Azure Active Directory service-principal die u hebt gemaakt als onderdeel van de vereisten |
    | Sleutel voor service-principal | Geef de verificatiesleutel voor de Azure Active Directory service-principal |
    | Voorvoegsel van cluster | Geef een waarde die wordt voorafgegaan voor alle clustertypen die zijn gemaakt door de data factory |
    | Resourcegroep | Selecteer de resourcegroep die u hebt gemaakt als onderdeel van het PowerShell-script dat u eerder hebt gebruikt.| 
    | Cluster SSH-gebruikersnaam | Voer een SSH-gebruikersnaam |
    | Cluster SSH-wachtwoord | Een wachtwoord opgeven voor de SSH-gebruiker |

    Selecteer **Voltooien**.

## <a name="create-a-pipeline"></a>Een pijplijn maken

1. Selecteer de knop **+** (plus) en selecteer vervolgens **Pijplijn**.

    ![Maken van een pijplijn in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "maken van een pijplijn in Azure Data Factory")

2. In de **activiteiten** werkset Vouw **HDInsight**, en sleep de **Hive** activiteit naar het ontwerpoppervlak pijplijn. In de **algemene** tabblad, Geef een naam op voor de activiteit.

    ![Activiteiten toevoegen aan de Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "activiteiten toevoegen aan de Data Factory-pijplijn")

3. Zorg ervoor dat u hebt de Hive-activiteit die is geselecteerd, selecteer de **HDI-Cluster** tabblad en van de **gekoppelde HDInsight-Service** vervolgkeuzelijst, selecteert u de gekoppelde service die u eerder hebt gemaakt voor HDInsight.

    ![HDInsight-cluster informatie opgeven voor de pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "bieden HDInsight-clusterdetails voor de pijplijn")

4. Selecteer de **Script** tabblad en voer de volgende stappen uit:

    a. Voor **Script gekoppelde Service**, selecteer **HDIStorageLinkedService**. Deze waarde is de gekoppelde storage-service die u eerder hebt gemaakt.

    b. Voor **bestandspad**, selecteer **opslag Bladeren** en navigeer naar de locatie waar het Hive-voorbeeldscript beschikbaar is. Als u het PowerShell-script eerder hebt uitgevoerd, deze locatie moet `adfgetstarted/hivescripts/hivescript.hql`.

    ![Hive-script informatie opgeven voor de pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "bieden Hive-scriptdetails voor de pijplijn")

    c. Onder **Geavanceerd** > **Parameters**, selecteer **automatisch doorvoeren van script**. Deze optie wordt gezocht naar eventuele parameters waarvoor waarden tijdens runtime in het Hive-script. Het script dat u gebruikt (**hivescript.hql**) heeft een **uitvoer** parameter. De waarde in de indeling `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` om te verwijzen naar een bestaande map op uw Azure-opslag. Het pad is hoofdlettergevoelig. Dit is het pad waar de uitvoer van het script wordt opgeslagen.

    ![Geef parameters op voor het Hive-script](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "Geef parameters op voor het Hive-script")

5. Selecteer **valideren** valideren van de pijplijn. Selecteer de **>>** (pijl-rechts) om het validatievenster te sluiten.

    ![Valideren van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "valideren van de Azure Data Factory-pijplijn")

5. Tot slot selecteert **Alles publiceren** de artefacten publiceren naar Azure Data Factory.

    ![Publiceren van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "publiceren van de Azure Data Factory-pijplijn")

## <a name="trigger-a-pipeline"></a>Een pijplijn activeren

1. Selecteer in de werkbalk op het ontwerpoppervlak **Trigger** > **Trigger nu**.

    ![Activeren van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "activeren van de Azure Data Factory-pijplijn")

2. Selecteer **voltooien** in het pop-zijmarge.

## <a name="monitor-a-pipeline"></a>Een pijplijn bewaken

1. Ga naar het tabblad **Controleren** aan de linkerkant. U ziet een pijplijn die worden uitgevoerd in de lijst **Pipeline Runs**. U ziet de status van de uitvoeren-onder de **Status** kolom.

    ![Bewaken van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "de Azure Data Factory-pijplijn bewaken")

2. Selecteer **Vernieuwen** om de status te vernieuwen.

3. U kunt ook selecteren de **weergave activiteiten bij uitvoering** pictogram voor een overzicht van de activiteit die wordt uitgevoerd die zijn gekoppeld aan de pijplijn. In de onderstaande schermafbeelding ziet u slechts één activiteit die wordt uitgevoerd, omdat er slechts één activiteit in de pijplijn die u hebt gemaakt. Als u wilt overschakelen naar de vorige weergave, selecteer **pijplijnen** naar de bovenkant van de pagina.

    ![Azure Data Factory-pijplijn activiteitsbewaking](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "de activiteiten van de Azure Data Factory-pijplijn controleren")


## <a name="verify-the-output"></a>De uitvoer controleren

1. Om te controleren of de uitvoer, in de Azure portal gaat u naar het opslagaccount dat u voor deze zelfstudie gebruikt. U ziet de volgende mappen of -containers:

    - U ziet een **adfgerstarted/outputfolder** die de uitvoer van het Hive-script dat werd uitgevoerd als onderdeel van de pijplijn bevat.

    - U ziet een **adfhdidatafactory -\<gekoppeld-service-name >-\<tijdstempel >** container. Deze container is de standaardlocatie voor de opslag van het HDInsight-cluster dat is gemaakt als onderdeel van de pijplijn uitvoeren.

    - U ziet een **adfjobs** logboeken van de container met de Azure Data Factory-taak.  

        ![Controleer of de uitvoer van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Controleer of de uitvoer van de Azure Data Factory-pijplijn")


## <a name="clean-up-the-tutorial"></a>De zelfstudie opschonen

Met de op-deman maken van HDInsight-cluster, hoeft u niet expliciet verwijderen van het HDInsight-cluster. Het cluster is verwijderd op basis van de configuratie die u hebt opgegeven tijdens het maken van de pijplijn. Zelfs nadat het cluster wordt verwijderd, blijven de storage-accounts die zijn gekoppeld aan het cluster echter bestaan. Dit gedrag is inherent aan het ontwerp, zodat u uw gegevens kunt behouden. Als u niet behouden blijven de gegevens wilt, kan u het opslagaccount verwijderen.

U kunt ook de hele resourcegroep die u hebt gemaakt voor deze zelfstudie verwijderen. Hiermee verwijdert u het opslagaccount en de Azure-Gegevensfactory die u hebt gemaakt.

### <a name="delete-the-resource-group"></a>De resourcegroep verwijderen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **resourcegroepen** in het linkerdeelvenster.
3. Selecteer de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Het filter gebruiken als er te veel resourcegroepen die worden vermeld. De resourcegroep wordt geopend.
4. Op de **Resources** tegel u heeft het standaardopslagaccount en de gegevensfactory weergegeven tenzij u de resourcegroep met andere projecten delen.
5. Selecteer **Resourcegroep verwijderen**. In dat geval worden de storage-account en de gegevens die zijn opgeslagen in het opslagaccount verwijderd.

    ![Verwijderen van resourcegroep](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "resourcegroep verwijderen")

6. Voer de naam van de resourcegroep moet bevestigen en selecteer vervolgens **verwijderen**.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u met Azure Data Factory HDInsight-cluster op aanvraag maken en uitvoeren van Hive-taken. Ga naar de volgende artciel voor informatie over het maken van HDInsight-clusters met aangepaste configuratie.

> [!div class="nextstepaction"]
>[Azure HDInsight-clusters maken met aangepaste configuratie](hdinsight-hadoop-provision-linux-clusters.md)


