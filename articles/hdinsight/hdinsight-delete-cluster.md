---
title: Het verwijderen van een HDInsight-cluster - Azure
description: Informatie over de verschillende manieren waarop u dat kunt u een HDInsight-cluster verwijderen.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: jasonh
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 1d376b365d8755cfea8718d6d0a50cfa6008fdc3
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39596015"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI

De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. In dit document leert u hoe u een cluster met behulp van de Azure portal, Azure PowerShell en de Azure CLI 1.0 verwijderen.

> [!IMPORTANT]
> Verwijderen van een HDInsight-cluster verwijdert niet de Azure Storage-accounts of Data Lake Store die zijn gekoppeld aan het cluster. U kunt gegevens die zijn opgeslagen in deze services in de toekomst opnieuw gebruiken.

## <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en selecteert u uw HDInsight-cluster. Als uw HDInsight-cluster niet aan het dashboard vastgemaakt is, kunt u door het zoekveld de naam voor het zoeken.
   
    ![Portal zoeken](./media/hdinsight-delete-cluster/navbar.png)

2. Selecteer in de clusterinstellingen, de **verwijderen** pictogram. Wanneer u hierom wordt gevraagd, selecteert u **Ja** om het cluster te verwijderen.
   
    ![pictogram verwijderen](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Gebruik de volgende opdracht om het cluster te verwijderen uit een PowerShell-prompt:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

## <a name="azure-cli-10"></a>Azure CLI 1.0

Gebruik de volgende om het cluster te verwijderen vanaf de opdrachtprompt:

    azure hdinsight cluster delete CLUSTERNAME

Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

> [!NOTE]
> Azure CLI 2.0 biedt geen ondersteuning voor verwijderen HDInsight-clusters op dit moment (23 oktober 2017).
