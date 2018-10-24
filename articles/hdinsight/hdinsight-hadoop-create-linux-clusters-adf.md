---
title: 'Zelfstudie: On-demand Hadoop-clusters maken in Azure HDInsight met Data Factory '
description: Informatie over het maken van on-demand Hadoop-clusters in HDInsight met behulp van Azure Data Factory.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: jasonh
ms.openlocfilehash: 9e21c49e7a472830c19f61388739c35aac17c3ff
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955606"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Zelfstudie: On-demand Hadoop-clusters in HDInsight met behulp van Azure Data Factory maken
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In dit artikel leert u over het maken van een Hadoop-cluster op aanvraag in Azure HDInsight met behulp van Azure Data Factory. Vervolgens gebruikt u gegevenspijplijnen in Azure Data Factory Hive-taken uitvoeren en verwijderen van het cluster. Aan het einde van deze zelfstudie leert u hoe u voor het operationeel maken van een big data-taak uitgevoerd waar het cluster te maken, taak uitvoeren en verwijderen van de cluster worden uitgevoerd volgens een schema.

Deze zelfstudie bestaat uit de volgende taken: 

> [!div class="checklist"]
> * Een Azure-opslagaccount maken
> * Inzicht in Azure Data Factory-activiteit
> * Een data factory maken met Azure portal
> * Gekoppelde services maken
> * Een pijplijn maken
> * Een pijplijn activeren
> * Een pijplijn bewaken
> * De uitvoer controleren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

- Azure PowerShell. Zie voor instructies [installeren en configureren van Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

- Een Azure Active Directory-service-principal. Als u de service-principal hebt gemaakt, moet u om op te halen de **toepassings-ID** en **verificatiesleutel** met behulp van de instructies in het gekoppelde artikel. U hebt deze waarden later in deze zelfstudie nodig. Controleer ook of de service-principal is lid van de *Inzender* rol van het abonnement of de resourcegroep waarin het cluster is gemaakt. Zie voor instructies voor het ophalen van de vereiste waarden en de juiste rollen toewijzen [maken van een Azure Active Directory service-principal](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

In deze sectie maakt u een opslagaccount dat wordt gebruikt als de standaardopslag voor het maken van on-demand HDInsight-cluster. Dit opslagaccount bevat ook het voorbeeld HiveQL-script (**hivescript.hql**) die u gebruikt voor het simuleren van een voorbeeld-Hive-taak die wordt uitgevoerd op het cluster.

Deze sectie wordt een Azure PowerShell-script voor het maken van de storage-account en kopiëren via de vereiste bestanden in de storage-account. De Azure PowerShell-voorbeeldscript in deze sectie worden de volgende taken uitgevoerd:

1. Logboeken in Azure.
1. Hiermee maakt u een Azure-resourcegroep.
1. Hiermee maakt u een Azure-opslagaccount.
1. Hiermee maakt u een Blob-container in de storage-account
1. Kopieert het voorbeeld HiveQL-script (**hivescript.hql**) de Blob-container. Het script is beschikbaar op [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). Het voorbeeldscript is al beschikbaar in een andere openbare Blob-container. Het onderstaande PowerShell-script maakt een kopie van deze bestanden in de Azure Storage-account die wordt gemaakt.


**Een storage-account maken en kopieer de bestanden met behulp van Azure PowerShell:**
> [!IMPORTANT]
> Geef namen voor de Azure-resourcegroep en de Azure storage-account dat door het script wordt gemaakt.
> Noteer **groepsnaam voor accountresources**, **opslagaccountnaam**, en **opslagaccountsleutel** output door het script. U moet deze in de volgende sectie.

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

**Om te controleren of het opslagaccount is gemaakt**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **resourcegroepen** in het linkerdeelvenster.
1. Dubbelklik op de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Als er te veel resourcegroepen die worden vermeld, gebruikt u het filter.
1. Op de **Resources** tegel, ziet u een resource in de lijst, tenzij u de resourcegroep met andere projecten delen. Deze resource is de storage-account met de naam die u eerder hebt opgegeven. Selecteer de naam van het opslagaccount.
1. Selecteer de **Blobs** tegels.
1. Selecteer de **adfgetstarted** container. Ziet u een map met de naam **hivescripts**.
1. Open de map en zorg ervoor dat deze de voorbeeld-scriptbestand bevat **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Inzicht in de Azure Data Factory-activiteit

[Azure Data Factory](../data-factory/introduction.md) wordt georganiseerd en de verplaatsing en transformatie van gegevens worden geautomatiseerd. Azure Data Factory kunt maken van een HDInsight Hadoop-cluster just-in-time voor het verwerken van een segment invoergegevens en verwijderen van het cluster wanneer de verwerking voltooid is. 

In Azure Data Factory hebben een data factory een of meer pijplijnen. Een pijplijn heeft één of meer activiteiten. Er zijn twee soorten activiteiten:

- [Activiteiten voor gegevensverplaatsing](../data-factory/copy-activity-overview.md) -gebruik van activiteiten voor gegevensverplaatsing om gegevens te verplaatsen van een brongegevensarchief naar een doelgegevensarchief.
- [Activiteiten voor gegevenstransformatie](../data-factory/transform-data.md). Kunt u activiteiten voor gegevenstransformatie gegevens transformeren en verwerken. HDInsight Hive-activiteit is een van de activiteiten voor gegevenstransformatie ondersteund door Data Factory. U de Hive-transformatie-activiteit gebruiken in deze zelfstudie.

In dit artikel configureert u de Hive-activiteit voor het maken van een on-demand HDInsight Hadoop-cluster. Wanneer de activiteit wordt uitgevoerd om gegevens te verwerken, is dit wat er gebeurt:

1. Een HDInsight Hadoop-cluster wordt automatisch gemaakt voor u just-in-time voor het verwerken van het segment. 

1. De ingevoerde gegevens worden verwerkt door een HiveQL-script uitgevoerd op het cluster. In deze zelfstudie worden de volgende acties uitgevoerd door het HiveQL-script dat is gekoppeld aan het hive-activiteit:
    
    - Maakt gebruik van de bestaande tabel (*hivesampletable*) te maken van een andere tabel **HiveSampleOut**.
    - Vult de **HiveSampleOut** tabel met alleen bepaalde kolommen uit de oorspronkelijke *hivesampletable*.
    
1. Het HDInsight Hadoop-cluster wordt verwijderd nadat de verwerking voltooid is en het cluster niet actief voor de geconfigureerde hoeveelheid tijd (timeToLive-instelling is). Als het volgende gegevenssegment voor verwerking met deze timeToLive niet-actieve tijd beschikbaar is, wordt hetzelfde cluster wordt gebruikt voor het verwerken van het segment.  

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer in de Azure portal, **een resource maken** > **gegevens en analyses** > **Data Factory**.

    ![Azure Data Factory in de portal](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Data Factory in de portal")

1. Typ of Selecteer de waarden, zoals wordt weergegeven in de volgende schermafbeelding:

    ![Azure Data Factory maken met Azure portal](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "maken Azure Data Factory met behulp van Azure portal")

    Typ of selecteer de volgende waarden:
    
    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Naam** |  Voer een naam voor de data factory. Deze naam moet wereldwijd uniek zijn.|
    |**Abonnement**     |  Selecteer uw Azure-abonnement. |
    |**Resourcegroep**     | Selecteer **gebruik bestaande** en selecteer vervolgens de resourcegroep die u hebt gemaakt met de PowerShell-script. |
    |**Versie**     | Selecteer **V2 (Preview)** |
    |**Locatie**     | De locatie is automatisch ingesteld op de locatie die u hebt opgegeven tijdens het maken van de resourcegroep eerder. Voor deze zelfstudie, de locatie is ingesteld op **VS-Oost 2**. |
    

1. Selecteer **vastmaken aan dashboard**, en selecteer vervolgens **maken**. U ziet een nieuwe tegel met de mededeling dat **de implementatie wordt verzonden**. Het maken van een data factory kan duren voordat tussen 2 tot 4 minuten.

    ![Voortgang van de implementatie sjabloon](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "voortgang van de sjabloon voor implementatie") 
 
1. Nadat de gegevensfactory is gemaakt, de portal ziet u het overzicht voor de data factory.

    ![Overzicht van Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Data Factory-overzicht")

1. Selecteer **Author & Monitor** om te starten van de Azure Data Factory voor ontwerp en controle van de portal.

## <a name="create-linked-services"></a>Gekoppelde services maken

In deze sectie maakt maken u twee gekoppelde services in uw data factory.

- Een **gekoppelde Azure Storage-service** waarmee een Azure-opslagaccount wordt gekoppeld aan de gegevensfactory. Deze opslag wordt gebruikt voor het HDInsight-cluster op aanvraag. Het bevat ook het Hive-script dat wordt uitgevoerd op het cluster.
- Een **gekoppelde HDInsight-service op aanvraag**. Azure Data Factory wordt automatisch een HDInsight-cluster maakt en het Hive-script wordt uitgevoerd. Het HDInsight-cluster wordt vervolgens verwijderd als het cluster gedurende een vooraf geconfigureerde tijd inactief is geweest.

###  <a name="create-an-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken

1. In het linkerdeelvenster van de **aan de slag** weergeeft, schakelt de **bewerken** pictogram.

    ![Maak een gekoppelde Azure Data Factory-service](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "een gekoppelde Azure Data Factory-service maken")

1. Selecteer **verbindingen** in de linkerbenedenhoek van het venster en selecteer vervolgens **+ nieuw**.

    ![Verbindingen maken in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "verbindingen in Azure Data Factory maken")

1. In de **nieuwe gekoppelde Service** in het dialoogvenster, selecteer **Azure Blob Storage** en selecteer vervolgens **doorgaan**.

    ![Gekoppelde maken Azure Storage-service voor Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "maken Azure Storage gekoppelde service voor Data Factory")

1. Geef een naam op voor de gekoppelde storage-service, selecteert u de Azure Storage-account dat u hebt gemaakt als onderdeel van het PowerShell-script en selecteer vervolgens **voltooien**.

    ![Geef de naam op voor Azure Storage gekoppelde service](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "Geef de naam op voor Azure Storage gekoppelde service")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Een gekoppelde HDInsight-service op aanvraag maken

1. Selecteer nogmaals de knop **+ Nieuw** om een andere gekoppelde service te maken.

1. Selecteer in het venster **Nieuwe gekoppelde service** **Compute** > **Azure HDInsight** en selecteer vervolgens **Doorgaan**.

    ![Create HDInsight gekoppelde service voor Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "HDInsight maken gekoppelde service voor Azure Data Factory")

1. In de **nieuwe gekoppelde Service** venster, geef de vereiste waarden.

    ![Geef waarden voor HDInsight gekoppelde service](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "Geef waarden voor HDInsight gekoppelde service")

    Voer de volgende waarden en laat de overige als standaard.

    | Eigenschap | Beschrijving |
    | --- | --- |
    | Naam | Voer een naam voor de gekoppelde HDInsight-service |
    | Type | Selecteer **On-demand HDInsight** |
    | Een gekoppelde Azure Storage-service | Selecteer de gekoppelde Storage-service die u eerder hebt gemaakt. |
    | Clustertype | Selecteer **hadoop** |
    | Time To Live | Geef de duur die u het HDInsight-cluster wilt moet beschikbaar zijn voordat het wordt automatisch verwijderd.|
    | Service-principal-id | Geef de toepassings-ID van de service-principal voor Azure Active Directory die u hebt gemaakt als onderdeel van de vereisten |
    | Sleutel van service-principal | Geef de verificatiesleutel voor de Azure Active Directory service-principal |
    | Het voorvoegsel van cluster | Geef een waarde die wordt voorafgegaan aan de clustertypen die zijn gemaakt door de data factory |
    | Resourcegroep | Selecteer de resourcegroep die u hebt gemaakt als onderdeel van het PowerShell-script dat u eerder hebt gebruikt| 
    | Cluster SSH-gebruikersnaam | Een SSH-gebruikersnaam invoeren |
    | Cluster SSH-wachtwoord | Geef een wachtwoord op voor de SSH-gebruiker |

    Selecteer **Voltooien**.

## <a name="create-a-pipeline"></a>Een pijplijn maken

1. Selecteer de knop **+** (plus) en selecteer vervolgens **Pijplijn**.

    ![Een pijplijn maakt in Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "een pijplijn maakt in Azure Data Factory")

1. In de **activiteiten** werkset Vouw **HDInsight**, en sleep de **Hive** activiteit naar het ontwerpoppervlak voor pijplijnen. In de **algemene** tabblad, Geef een naam op voor de activiteit.

    ![Activiteiten toevoegen aan de Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "activiteiten toevoegen aan de Data Factory-pijplijn")

1. Zorg ervoor dat u hebt de Hive-activiteit die is geselecteerd, selecteer de **HDI-Cluster** tabblad, en van de **gekoppelde Service HDInsight** vervolgkeuzelijst, selecteert u de gekoppelde service die u eerder hebt gemaakt voor HDInsight.

    ![Geef HDInsight-clusterdetails op voor de pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "bieden HDInsight-clusterdetails voor de pijplijn")

1. Selecteer de **Script** tabblad en voer de volgende stappen uit:
    
    1. Voor **Script gekoppelde Service**, selecteer **HDIStorageLinkedService**. Deze waarde is de gekoppelde storage-service die u eerder hebt gemaakt.
    
    1. Voor **bestandspad**, selecteer **Browse Storage** en navigeer naar de locatie waar de voorbeeld-Hive-script beschikbaar is. Als u eerder hebt uitgevoerd van het PowerShell-script, deze locatie moet zijn `adfgetstarted/hivescripts/hivescript.hql`.
    
        ![Geef details van de Hive-script voor de pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "bieden Hive-script details voor de pijplijn")
    
    1. Onder **Geavanceerd** > **Parameters**, selecteer **automatisch ingevuld uit het script**. Deze optie ziet er uit voor de parameters waarvoor waarden tijdens runtime in de Hive-script. Het script dat u gebruikt (**hivescript.hql**) heeft een **uitvoer** parameter. Geef de waarde in de indeling `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` om te verwijzen naar een bestaande map op uw Azure-opslag. Het pad is hoofdlettergevoelig. Dit is het pad waar u de uitvoer van het script wordt opgeslagen.
    
        ![Geef parameters op voor de Hive-script](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "parameters opgeven voor het Hive-script")

1. Selecteer **valideren** voor het valideren van de pijplijn. Selecteer de **>>** (pijl-rechts) om het validatievenster te sluiten.

    ![Valideren van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "valideren van de Azure Data Factory-pijplijn")

1. Selecteer ten slotte **Alles publiceren** de artefacten publiceren naar Azure Data Factory.

    ![Publiceren van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "publiceren van de Azure Data Factory-pijplijn")

## <a name="trigger-a-pipeline"></a>Een pijplijn activeren

1. Selecteer in de werkbalk op het ontwerpoppervlak voor pijplijnen **Trigger** > **nu activeren**.

    ![De Azure Data Factory-pijplijn activeren](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "de Azure Data Factory-pijplijn activeren")

1. Selecteer **voltooien** in het pop-zijbalk.

## <a name="monitor-a-pipeline"></a>Een pijplijn bewaken

1. Ga naar het tabblad **Controleren** aan de linkerkant. U ziet een pijplijn die worden uitgevoerd in de lijst **Pipeline Runs**. U ziet de status van de uitvoering onder de **Status** kolom.

    ![Bewaken van de Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "bewaken van de Azure Data Factory-pijplijn")

1. Selecteer **Vernieuwen** om de status te vernieuwen.

1. U kunt ook selecteren de **uitvoeringen van activiteit weergeven** pictogram om te zien van de activiteit die wordt uitgevoerd die is gekoppeld aan de pijplijn. In de onderstaande schermafbeelding ziet u slechts één activiteit die wordt uitgevoerd, omdat er slechts één activiteit in de pijplijn die u hebt gemaakt. Als u wilt overschakelen naar de vorige weergave, selecteert u **pijplijnen** boven aan de pagina.

    ![De Azure Data Factory pipeline-activiteit controleren](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "bewaken van de Azure Data Factory pipeline-activiteit")


## <a name="verify-the-output"></a>De uitvoer controleren

1. Om te controleren of de uitvoer, in de Azure-portal gaat u naar het opslagaccount dat u voor deze zelfstudie gebruikt. U ziet de volgende mappen of containers:

    - U ziet een **adfgerstarted/outputfolder** die de uitvoer van de Hive-script is uitgevoerd als onderdeel van de pijplijn bevat.

    - U ziet een **adfhdidatafactory -\<gekoppeld-service-name >-\<tijdstempel >** container. Deze container is de standaardlocatie voor de opslag van het HDInsight-cluster dat is gemaakt als onderdeel van de pijplijnuitvoering.

    - U ziet een **adfjobs** container met de Azure Data Factory-taak zich aanmeldt.  

        ![Controleer of de uitvoer van Azure Data Factory-pijplijn](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "de uitvoer van Azure Data Factory-pijplijn controleren")


## <a name="clean-up-the-tutorial"></a>De zelfstudie opschonen

Met de on-deman maken van HDInsight-cluster, hoeft u niet expliciet verwijderen van het HDInsight-cluster. Het cluster is verwijderd op basis van de configuratie die u hebt opgegeven tijdens het maken van de pijplijn. Zelfs nadat het cluster is verwijderd, blijven de storage-accounts die zijn gekoppeld aan het cluster echter bestaan. Dit gedrag is inherent aan het ontwerp, zodat u uw gegevens kunt behouden. Als u niet behouden van de gegevens wilt, kunt u het opslagaccount dat u hebt gemaakt verwijderen.

U kunt ook de hele resourcegroep die u hebt gemaakt voor deze zelfstudie verwijderen. Hiermee verwijdert u het opslagaccount en de Azure Data Factory die u hebt gemaakt.

### <a name="delete-the-resource-group"></a>De resourcegroep verwijderen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **resourcegroepen** in het linkerdeelvenster.
1. Selecteer de Resourcegroepnaam die u hebt gemaakt in uw PowerShell-script. Als er te veel resourcegroepen die worden vermeld, gebruikt u het filter. Hiermee opent u de resourcegroep.
1. Op de **Resources** tegel, u moet het standaardaccount voor opslag en de data factory, tenzij u de resourcegroep met andere projecten delen weergegeven.
1. Selecteer **Resourcegroep verwijderen**. In dat geval worden de storage-account en de gegevens die zijn opgeslagen in het opslagaccount verwijderd.

    ![Resourcegroep verwijderen](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "resourcegroep verwijderen")

1. Voer de naam van de resourcegroep om te bevestigen en selecteer vervolgens **verwijderen**.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u on-demand HDInsight-cluster maken en uitvoeren van Hive-taken met Azure Data Factory. Ga naar de volgende artciel voor informatie over het maken van HDInsight-clusters met aangepaste configuratie.

> [!div class="nextstepaction"]
>[Azure HDInsight-clusters maken met aangepaste configuratie](hdinsight-hadoop-provision-linux-clusters.md)


