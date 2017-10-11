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
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 65dac529df15d2dd43eec17673d82a2832f7692e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI

HDInsight-cluster omdat facturering begint zodra een cluster wordt gemaakt en stopt wanneer het cluster wordt verwijderd. Facturering is pro rato per minuut, dus u uw cluster altijd verwijderen moet wanneer deze niet langer in gebruik. In dit document, informatie over het verwijderen van een cluster met behulp van de Azure-portal, Azure PowerShell en de Azure CLI 1.0.

> [!IMPORTANT]
> Verwijderen van een HDInsight-cluster, wordt de Azure Storage-accounts niet verwijderd of de Data Lake Store die zijn gekoppeld aan het cluster. U kunt gegevens die zijn opgeslagen in deze services in de toekomst opnieuw gebruiken.

## <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en selecteer uw HDInsight-cluster. Als uw HDInsight-cluster niet aan het dashboard vastgemaakt is, kunt u voor het zoeken op naam met het zoekveld.
   
    ![Portal zoeken](./media/hdinsight-delete-cluster/navbar.png)

2. Zodra de blade wordt geopend voor het cluster, selecteert u de **verwijderen** pictogram. Wanneer u wordt gevraagd, selecteert u **Ja** verwijderen van het cluster.
   
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
> Azure CLI 2.0 ondersteunt geen verwijderen HDInsight-clusters op dit moment (31 juli 2017).