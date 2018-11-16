---
title: Log Analytics gebruiken voor Azure HDInsight-clusters controleren
description: Informatie over het gebruik van Azure Log Analytics voor het bewaken van taken die worden uitgevoerd in een HDInsight-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 23c8098927a051da48c7da2bac0c96173da1d2f2
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713296"
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Azure Log Analytics gebruiken voor het HDInsight-clusters controleren

Meer informatie over het inschakelen van Azure Log Analytics voor het bewaken van bewerkingen voor Hadoop-cluster in HDInsight en het toevoegen van een Hdinsight voor controle.

[Log Analytics](../log-analytics/log-analytics-overview.md) is een service in Azure Monitor die uw cloud en on-premises omgevingen voor het onderhouden van hun beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Een Log Analytics-werkruimte**. U kunt deze werkruimte zien als een unieke omgeving van Log Analytics met een eigen gegevensopslagplaats, gegevensbronnen en oplossingen. Zie voor instructies [een Log Analytics-werkruimte maken](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

* **Een Azure HDInsight-cluster**. U kunt op dit moment Log Analytics gebruiken met de volgende typen van de HDInsight-cluster:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Zie voor instructies over het maken van een HDInsight-cluster [aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

> [!NOTE]
> Het verdient aanbeveling om zowel het HDInsight-cluster en de Log Analytics-werkruimte in dezelfde regio voor betere prestaties. Azure Log Analytics is niet beschikbaar in alle Azure-regio's.

## <a name="enable-log-analytics-by-using-the-portal"></a>Log Analytics met behulp van de portal inschakelen

In deze sectie configureert u een bestaand HDInsight Hadoop-cluster voor het gebruik van een Azure Log Analytics-werkruimte voor het bewaken van taken, logboeken voor foutopsporing, enz.

1. Open een HDInsight-cluster in Azure portal.
2. Selecteer in het linkerdeelvenster **bewaking**.
3. Selecteer in het rechter deelvenster **inschakelen**, selecteer een bestaande Log Analytics-werkruimte en selecteer vervolgens **opslaan**.

    ![Schakel de bewaking voor HDInsight-clusters](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Schakel bewaking voor HDInsight-clusters")

    Het duurt een paar minuten om op te slaan van de instelling.

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Log Analytics inschakelen met behulp van Azure PowerShell

Log Analytics met Azure PowerShell, kunt u inschakelen. De cmdlet is:

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
      [-Name] <CLUSTER NAME>
      [-WorkspaceId] <LOG ANALYTICS WORKSPACE NAME>
      [-PrimaryKey] <LOG ANALYTICS WORKSPACE PRIMARY KEY>
      [-ResourceGroupName] <RESOURCE GROUIP NAME>
```

Zie [inschakelen AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0).

Als u wilt uitschakelen, is de cmdlet:

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
       [-Name] <CLUSTER NAME>
       [-ResourceGroupName] <RESOURCE GROUP NAME>
```

Zie [uitschakelen AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0).

## <a name="install-hdinsight-cluster-management-solutions"></a>Installeren van HDInsight-cluster-beheeroplossingen

HDInsight biedt van de cluster-specifieke beheeroplossingen die u voor Azure Log Analytics toevoegen kunt. [Beheeroplossingen](../log-analytics/log-analytics-add-solutions.md) functionaliteit toevoegen aan Log Analytics, aanvullende gegevens en analysehulpprogramma's bieden. Deze oplossingen belangrijke prestatiegegevens verzamelen van uw HDInsight-clusters en over de hulpprogramma's om te zoeken naar de metrische gegevens. Deze oplossingen bieden ook visualisaties en dashboards voor de meeste clustertypen in HDInsight wordt ondersteund. Met behulp van de metrische gegevens die u verzamelt van de oplossing, kunt u aangepaste regels voor bewaking en waarschuwingen kunt maken.

Dit zijn de beschikbare HDInsight-oplossingen:

* HDInsight Hadoop-bewaking
* HDInsight HBase-bewaking
* HDInsight Interactive Query-bewaking
* HDInsight Kafka-bewaking
* HDInsight Spark-bewaking
* HDInsight Storm-bewaking

Zie voor instructies voor het installeren van een oplossing voor [oplossingen in Azure](../azure-monitor/insights/solutions.md#install-a-management-solution). Als u wilt experimenteren, door een oplossing voor HDInsight Hadoop Monotiring te installeren. Wanneer dit is voltooid, ziet u een **HDInsightHadoop** tegel vermeld onder **samenvatting**. Selecteer de **HDInsightHadoop** tegel. De oplossing HDInsightHadoop ziet eruit zoals:

![Bewakingsweergave oplossing voor HDInsight](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Omdat het cluster een nieuwe cluster is, weergeven niet alle activiteiten in het rapport.

## <a name="next-steps"></a>Volgende stappen

* [Query uitvoeren op Azure Log Analytics voor het bewaken van HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-use-queries.md)
