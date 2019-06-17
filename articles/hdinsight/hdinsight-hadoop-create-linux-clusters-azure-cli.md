---
title: Apache Hadoop-clusters met behulp van de Azure CLI - Azure HDInsight maken
description: Informatie over het maken van HDInsight-clusters met behulp van de platformoverschrijdende Azure CLI.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: hrasheed
ms.openlocfilehash: 0a278cd98b0dd6c6d8f0fe9bfee81e5bafd4f543
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65597676"
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Maken van HDInsight-clusters met behulp van de Azure CLI

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

De stappen in dit document overzicht maken van een HDInsight 3.6-cluster met behulp van de Azure CLI.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Azure CLI. Als u de Azure CLI nog niet hebt geïnstalleerd, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) voor stappen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-cluster"></a>Een cluster maken

1. Meld u aan met uw Azure-abonnement. Als u van plan bent te gebruiken van Azure Cloud Shell en vervolgens selecteert u **uitproberen** in de rechterbovenhoek van het codeblok. Anders, voer de volgende opdracht:

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

2. Omgevingsvariabelen instellen. Het gebruik van variabelen in dit artikel is gebaseerd op Bash. Kleine variaties nodig voor andere omgevingen. Zie [az-hdinsight-create](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) voor een volledige lijst van mogelijke parameters voor het maken van clusters.

    |Parameter | Description |
    |---|---|
    |`--size`| Het aantal worker-knooppunten in het cluster. In dit artikel wordt de variabele `clusterSizeInNodes` als de waarde die is doorgegeven aan `--size`. |
    |`--version`| De versie van het HDInsight-cluster. In dit artikel wordt de variabele `clusterVersion` als de waarde die is doorgegeven aan `--version`. Zie ook: [Ondersteunde versies van HDInsight](./hdinsight-component-versioning.md#supported-hdinsight-versions).|
    |`--type`| HDInsight-cluster, zoals het type: hadoop, interactivehive, hbase, kafka, storm, spark, rserver, mlservices.  In dit artikel wordt de variabele `clusterType` als de waarde die is doorgegeven aan `--type`. Zie ook: [Typen en de configuratie van cluster](./hdinsight-hadoop-provision-linux-clusters.md#cluster-types).|
    |`--component-version`|De versies van verschillende Hadoop-onderdelen, in versies spaties gescheiden in ' onderdeel = versie-indeling. In dit artikel wordt de variabele `componentVersion` als de waarde die is doorgegeven aan `--component-version`. Zie ook: [Hadoop-onderdelen](./hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions).|

    Vervang `RESOURCEGROUPNAME`, `LOCATION`, `CLUSTERNAME`, `STORAGEACCOUNTNAME`, en `PASSWORD` met de gewenste waarden. Waarden voor de andere variabelen naar wens wijzigen. Voer vervolgens de CLI-opdrachten.

    ```azurecli-interactive
    export resourceGroupName=RESOURCEGROUPNAME
    export location=LOCATION
    export clusterName=CLUSTERNAME
    export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
    export httpCredential='PASSWORD'
    export sshCredentials='PASSWORD'
    
    export AZURE_STORAGE_CONTAINER=$clusterName
    export clusterSizeInNodes=1
    export clusterVersion=3.6
    export clusterType=hadoop
    export componentVersion=Hadoop=2.7
    ```

3. [Maak de resourcegroep](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) met de onderstaande opdracht:

    ```azurecli-interactive
    az group create \
        --location $location \
        --name $resourceGroupName
    ```

    Voor een lijst van geldige locaties, gebruikt u de `az account list-locations` opdracht in en gebruik een van de locaties van de `name` waarde.

4. [Een Azure storage-account maken](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) met de onderstaande opdracht:

    ```azurecli-interactive
    # Note: kind BlobStorage is not available as the default storage account.
    az storage account create \
        --name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

5. [De primaire sleutel onttrekken aan het Azure storage-account](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest#az-storage-account-keys-list) en op te slaan in een variabele met de onderstaande opdracht:

    ```azurecli-interactive
    export AZURE_STORAGE_KEY=$(az storage account keys list \
        --account-name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

6. [Maken van een Azure storage-container](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) met de onderstaande opdracht:

    ```azurecli-interactive
    az storage container create \
        --name $AZURE_STORAGE_CONTAINER \
        --account-key $AZURE_STORAGE_KEY \
        --account-name $AZURE_STORAGE_ACCOUNT
    ```

7. [Maken van het HDInsight-cluster](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) met de volgende opdracht:

    ```azurecli-interactive
    az hdinsight create \
        --name $clusterName \
        --resource-group $resourceGroupName \
        --type $clusterType \
        --component-version $componentVersion \
        --http-password $httpCredential \
        --http-user admin \
        --location $location \
        --size $clusterSizeInNodes \
        --ssh-password $sshCredentials \
        --ssh-user sshuser \
        --storage-account $AZURE_STORAGE_ACCOUNT \
        --storage-account-key $AZURE_STORAGE_KEY \
        --storage-default-container $AZURE_STORAGE_CONTAINER \
        --version $clusterVersion
    ```

    > [!IMPORTANT]  
    > HDInsight clusters worden geleverd in verschillende typen die overeenkomen met de werkbelasting of technologie die het cluster is afgestemd op. Er is geen ondersteunde methode om een cluster die meerdere typen, zoals Storm en HBase op één cluster combineert te maken.

    Het duurt enkele minuten voor het cluster maken van het proces is voltooid. Meestal ongeveer 15.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u het artikel hebt voltooid, kunt u het cluster verwijderen. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Geef alle of een deel van de volgende opdrachten om resources te verwijderen:

```azurecli-interactive
# Remove cluster
az hdinsight delete \
    --name $clusterName \
    --resource-group $resourceGroupName

# Remove storage container
az storage container delete \
    --account-name $AZURE_STORAGE_ACCOUNT \
    --name $AZURE_STORAGE_CONTAINER

# Remove storage account
az storage account delete \
    --name $AZURE_STORAGE_ACCOUNT \
    --resource-group $resourceGroupName

# Remove resource group
az group delete \
    --name $resourceGroupName
```

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](./hdinsight-hadoop-customize-cluster-linux.md#access-control) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een HDInsight-cluster met behulp van de Azure CLI hebt gemaakt, gebruikt u de volgende voor informatie over het werken met uw cluster:

### <a name="apache-hadoop-clusters"></a>Apache Hadoop-clusters

* [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase-clusters

* [Aan de slag met Apache HBase op HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Java-toepassingen voor Apache HBase op HDInsight ontwikkelen](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm-clusters

* [Java-topologieën ontwikkelen voor Apache Storm op HDInsight](storm/apache-storm-develop-java-topology.md)
* [Python-onderdelen in Apache Storm op HDInsight gebruiken](storm/apache-storm-develop-python-topology.md)
* [Topologieën met Apache Storm op HDInsight implementeren en bewaken](storm/apache-storm-deploy-monitor-topology-linux.md)
