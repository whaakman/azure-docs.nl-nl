---
title: Het verwijderen van een HDInsight-cluster - Azure
description: Informatie over de verschillende manieren waarop u dat kunt u een HDInsight-cluster verwijderen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 4df4fa29722dd3ad33cf1ce123877f04f9f4b4c1
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58360385"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>Verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of de klassieke Azure-CLI

De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. In dit document leert u hoe u verwijdert een cluster met de [Azure-portal](https://portal.azure.com), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/), en de klassieke Azure-CLI.

> [!IMPORTANT]  
> Verwijderen van een HDInsight-cluster verwijdert niet de Azure Storage-accounts of Data Lake-opslag die is gekoppeld aan het cluster. U kunt gegevens die zijn opgeslagen in deze services in de toekomst opnieuw gebruiken.

## <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en selecteert u uw HDInsight-cluster. Als uw HDInsight-cluster niet aan het dashboard vastgemaakt is, kunt u door het zoekveld de naam voor het zoeken.
   
    ![Portal zoeken](./media/hdinsight-delete-cluster/navbar.png)

2. Selecteer in de clusterinstellingen, de **verwijderen** pictogram. Wanneer u hierom wordt gevraagd, selecteert u **Ja** om het cluster te verwijderen.
   
    ![pictogram verwijderen](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Gebruik de volgende opdracht om het cluster te verwijderen uit een PowerShell-prompt:

    Remove-AzHDInsightCluster -ClusterName CLUSTERNAME

Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

## <a name="azure-classic-cli"></a>CLI van Azure Classic

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Gebruik de volgende om het cluster te verwijderen vanaf de opdrachtprompt:

    azure hdinsight cluster delete CLUSTERNAME

Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.
