---
title: Een Apache Spark-cluster maken in Azure HDInsight met Azure CLI
description: Deze quickstart laat zien hoe u Azure CLI gebruiken voor het maken van een Apache Spark-cluster in Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 05/09/2019
ms.author: hrasheed
ms.openlocfilehash: b9478ca8e1b31c1761e063a6789e96043f9a2c68
ms.sourcegitcommit: 9e8dfa1169a55c3c8af93a6c5f4e0dace4de48b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65556830"
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight-with-azure-cli"></a>Een Apache Spark-cluster maken in Azure HDInsight met Azure CLI

In deze snelstartgids leert u hoe u een Apache Spark-cluster maken in Azure HDInsight met behulp van Azure CLI. Apache Spark maakt het mogelijk om snelle gegevensanalyses en clusterberekeningen uit te voeren met behulp van verwerking in het geheugen. De [Azure-opdrachtregelinterface (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) is de ervaring van Microsoft platformoverschrijdende opdrachtregelprogramma voor het beheren van Azure-resources.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Azure CLI. Als u de Azure CLI nog niet hebt geïnstalleerd, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) voor stappen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-an-apache-spark-cluster"></a>Een Apache Spark-cluster maken

1. Aanmelden bij uw Azure-abonnement. Als u van plan bent te gebruiken van Azure Cloud Shell en vervolgens selecteert u **uitproberen** in de rechterbovenhoek van het codeblok. Anders, voer de volgende opdracht:

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

2. Omgevingsvariabelen instellen. Het gebruik van variabelen in deze Quick Start is gebaseerd op Bash. Kleine variaties nodig voor andere omgevingen. Vervangen door RESOURCEGROUPNAME, locatie, CLUSTERNAME, STORAGEACCOUNTNAME en het wachtwoord in het codefragment hieronder de gewenste waarden. Voer vervolgens de CLI-opdrachten om in te stellen de omgevingsvariabelen.

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
    export clusterType=spark
    export componentVersion=Spark=2.3
    ```

3. De resourcegroep maken met de onderstaande opdracht:

    ```azurecli-interactive
    az group create \
        --location $location \
        --name $resourceGroupName
    ```

4. Maak een Azure storage-account met de onderstaande opdracht:

    ```azurecli-interactive
    az storage account create \
        --name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

5. De primaire sleutel onttrekken aan het Azure storage-account en op te slaan in een variabele met de onderstaande opdracht:

    ```azurecli-interactive
    export AZURE_STORAGE_KEY=$(az storage account keys list \
        --account-name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

6. Maak een Azure storage-container met de onderstaande opdracht:

    ```azurecli-interactive
    az storage container create \
        --name $AZURE_STORAGE_CONTAINER \
        --account-key $AZURE_STORAGE_KEY \
        --account-name $AZURE_STORAGE_ACCOUNT
    ```

7. De Apache Spark-cluster maken met de volgende opdracht:

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

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de Quick Start hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

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

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u een Apache Spark-cluster maakt in Azure HDInsight met behulp van Azure CLI.  Ga naar de volgende zelfstudie voor informatie over het gebruik van een HDInsight Spark-cluster om interactieve query's uit te voeren op voorbeeldgegevens.

> [!div class="nextstepaction"]
> [Interactieve query's uitvoeren in Apache Spark](./apache-spark-load-data-run-query.md)
