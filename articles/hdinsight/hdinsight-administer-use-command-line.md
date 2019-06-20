---
title: Azure HDInsight-clusters met behulp van Azure CLI beheren
description: Informatie over het gebruik van de Azure CLI voor het beheren van Azure HDInsight-clusters. Clustertypen bevatten Apache Hadoop, Spark, HBase, Storm, Kafka, Interactive Query en ML-Services.
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tyfox
ms.openlocfilehash: 5ae97b17d06fa0a9934a58ac662ef12116cce4f6
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137407"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Azure HDInsight-clusters met behulp van Azure CLI beheren

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Meer informatie over het gebruik van [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) voor het beheren van Azure HDInsight-clusters. De Azure-opdrachtregelinterface (CLI) is de platformoverschrijdende opdrachtregelervaring voor het beheren van Azure-resources.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Azure CLI. Als u de Azure CLI nog niet hebt geïnstalleerd, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) voor stappen.

* Een Apache Hadoop-cluster in HDInsight. Zie [aan de slag met HDInsight op Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).

## <a name="connect-to-azure"></a>Verbinding maken met Azure

Aanmelden bij uw Azure-abonnement. Als u van plan bent te gebruiken van Azure Cloud Shell en vervolgens selecteert u **uitproberen** in de rechterbovenhoek van het codeblok. Anders, voer de volgende opdracht:

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>Clusters groeperen

Gebruik [az hdinsight lijst](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) aan de lijst met clusters. De onderstaande opdrachten bewerken door te vervangen `RESOURCE_GROUP_NAME` met de naam van de resourcegroep, voert u de opdrachten:

```azurecli-interactive
# List all clusters in the current subscription
az hdinsight list

# List only cluster name and its resource group
az hdinsight list --query "[].{Cluster:name, ResourceGroup:resourceGroup}" --output table

# List all cluster for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME

# List all cluster names for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME --query "[].{clusterName:name}" --output table
```

## <a name="show-cluster"></a>Cluster weergeven

Gebruik [az hdinsight show](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) om informatie voor een opgegeven cluster weer te geven. De onderstaande opdracht bewerken door te vervangen `RESOURCE_GROUP_NAME`, en `CLUSTER_NAME` met de relevante informatie, voert u de opdracht:

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>Clusters verwijderen

Gebruik [az hdinsight verwijderen](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) verwijderen van een cluster opgegeven gebruikmaakt. De onderstaande opdracht bewerken door te vervangen `RESOURCE_GROUP_NAME`, en `CLUSTER_NAME` met de relevante informatie, voert u de opdracht:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

U kunt ook een cluster verwijderen door de resourcegroep waarin het cluster te verwijderen. Opmerking: Hiermee worden alle resources in de groep met inbegrip van het standaardopslagaccount verwijderd.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>Clusters schalen

Gebruik [az hdinsight vergroten of verkleinen](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) formaat van het opgegeven HDInsight-cluster met de opgegeven grootte. De onderstaande opdracht bewerken door te vervangen `RESOURCE_GROUP_NAME`, en `CLUSTER_NAME` met de relevante informatie. Vervang `TARGET_INSTANCE_COUNT` met het gewenste aantal worker-knooppunten voor uw cluster. Zie voor meer informatie over het schalen van clusters [schaal HDInsight-clusters](./hdinsight-scaling-best-practices.md). Voer de opdracht:

```azurecli-interactive
az hdinsight resize --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --target-instance-count TARGET_INSTANCE_COUNT
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u verschillende HDInsight-cluster administratieve taken uit te voeren. Zie de volgende artikelen voor meer informatie:

* [Apache Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md)
* [HDInsight beheren met behulp van Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Aan de slag met Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
