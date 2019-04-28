---
title: Het verwijderen van een HDInsight-cluster - Azure
description: Informatie over de verschillende manieren waarop u dat kunt u een HDInsight-cluster verwijderen.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: eca7b4f8bd7e91bc8dcb9bcc49ed3b981010aaee
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097171"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI

De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. In dit document leert u hoe u verwijdert een cluster met de [Azure-portal](https://portal.azure.com), [Az van Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/overview), en de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

> [!IMPORTANT]  
> Verwijderen van een HDInsight-cluster verwijdert niet de Azure Storage-accounts of Data Lake-opslag die is gekoppeld aan het cluster. U kunt gegevens die zijn opgeslagen in deze services in de toekomst opnieuw gebruiken.

## <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. In het menu links, gaat u naar **alle services** > **Analytics** > **HDInsight-clusters** en selecteert u uw cluster.

3. Selecteer in de standaardweergave van de **verwijderen** pictogram. Volg de aanwijzing om uw cluster te verwijderen.
   
    ![pictogram verwijderen](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell-az-module"></a>Azure PowerShell Az-module

Vervang `CLUSTERNAME` met de naam van uw HDInsight-cluster in de onderstaande code. Voer de volgende opdracht om het cluster te verwijderen uit een PowerShell-prompt:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure-CLI

Vervang `CLUSTERNAME` met de naam van uw HDInsight-cluster, en `RESOURCEGROUP` met de naam van de resourcegroep in de onderstaande code.  Voer de volgende als u wilt verwijderen van het cluster via een opdrachtprompt:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```
