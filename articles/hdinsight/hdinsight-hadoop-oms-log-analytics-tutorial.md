---
title: Log Analytics gebruiken voor het bewaken van Azure HDInsight-clusters | Microsoft Docs
description: Informatie over het gebruiken van Azure-logboekanalyse voor het bewaken van taken die worden uitgevoerd in een HDInsight-cluster.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: nitinme
ms.openlocfilehash: ca2cf642cfff2961dcb0dd18f0e712f61d6915c2
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Azure Log Analytics gebruiken voor het bewaken van HDInsight-clusters

Informatie over het gebruiken van Azure-logboekanalyse voor het bewaken van bewerkingen voor Hadoop-cluster in HDInsight.

[Meld u Analytics](../log-analytics/log-analytics-overview.md) is een service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) die wordt bewaakt uw cloud en on-premises omgevingen voor het onderhouden van de beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden. 

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/free).

* **Een Azure HDInsight-cluster**. Op dit moment kunt u Azure Operations Management Suite met de volgende typen van de HDInsight-cluster:

    * Hadoop
    * HBase
    * Interactive Query
    * Kafka
    * Spark
    * Storm

    Zie voor instructies over het maken van een HDInsight-cluster, [aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Een werkruimte voor logboekanalyse**. U kunt deze werkruimte beschouwen als een unieke Log Analytics-omgeving met een eigen data-opslagplaats, gegevensbronnen en oplossingen. Hebt u een dergelijke werkruimte al gemaakt die u kunt koppelen aan Azure HDInsight-clusters. Zie voor instructies [maken van een werkruimte voor logboekanalyse](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

## <a name="enable-log-analytics-by-using-the-portal"></a>Log Analytics inschakelen met behulp van de portal

In deze sectie configureert u een bestaand HDInsight Hadoop-cluster voor het gebruik van een Azure-logboekanalyse-werkruimte voor het bewaken van taken, logboeken voor foutopsporing, enz.

1. Open een HDInsight-cluster in de Azure portal.
2. Klik in het linkerdeelvenster op **bewaking**.
3. Klik in het rechterdeelvenster **inschakelen**, selecteer een bestaande werkruimte voor logboekanalyse en klik vervolgens op **opslaan**.

    ![Schakel bewaking voor HDInsight-clusters](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "bewaking voor HDInsight-clusters inschakelen")

    Het duurt enkele minuten om op te slaan van de instelling.  Als deze is voltooid, ziet u een **Open OMS-Dashboard** knop bovenaan. 

    ![Open Operations Management Suite-dashboard](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "Open OMS-dashboard")

5. Klik op **Open OMS dashboard**.
6. Voer uw Azure-referenties in als daarom wordt gevraagd.

    ![Operations Management Suite-portal](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "Operations Management Suite-portal")

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Log Analytics inschakelen met behulp van Azure PowerShell

Log Analytics met Azure PowerShell, kunt u inschakelen. De cmdlet is:

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
```

Zie [inschakelen AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0).

Als u wilt uitschakelen, is de cmdlet 

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
```

Zie [uitschakelen AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0).


## <a name="next-steps"></a>Volgende stappen
* [Oplossingen voor HDInsight-cluster toevoegen aan Log Analytics](hdinsight-hadoop-oms-log-analytics-management-solutions.md)
