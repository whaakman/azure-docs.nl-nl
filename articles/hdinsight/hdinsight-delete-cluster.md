---
title: Het verwijderen van een HDInsight-cluster - Azure | Microsoft Docs
description: Informatie over de verschillende manieren waarop u een HDInsight-cluster kunt verwijderen.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 04b89b2cceb18a0e3c88d0d1deada1a05b8187f6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI

HDInsight-cluster omdat facturering begint zodra een cluster wordt gemaakt en stopt wanneer het cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. In dit document, informatie over het verwijderen van een cluster met behulp van de Azure-portal, Azure PowerShell en de Azure CLI 1.0.

> [!IMPORTANT]
> Verwijderen van een HDInsight-cluster, wordt de Azure Storage-accounts niet verwijderd of de Data Lake Store die zijn gekoppeld aan het cluster. U kunt gegevens die zijn opgeslagen in deze services in de toekomst opnieuw gebruiken.

## <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en selecteer uw HDInsight-cluster. Als uw HDInsight-cluster niet aan het dashboard vastgemaakt is, kunt u voor het zoeken op naam met het zoekveld.
   
    ![Portal zoeken](./media/hdinsight-delete-cluster/navbar.png)

2. Selecteer in de clusterinstellingen, de **verwijderen** pictogram. Wanneer u wordt gevraagd, selecteert u **Ja** verwijderen van het cluster.
   
    ![Pictogram verwijderen](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Gebruik de volgende opdracht het cluster verwijderen uit een PowerShell-prompt:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

## <a name="azure-cli-10"></a>Azure CLI 1.0

Gebruik de volgende het cluster verwijderen uit de opdrachtprompt:

    azure hdinsight cluster delete CLUSTERNAME

Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

> [!NOTE]
> Azure CLI 2.0 ondersteunt geen verwijderen HDInsight-clusters op dit moment (23 oktober 2017).