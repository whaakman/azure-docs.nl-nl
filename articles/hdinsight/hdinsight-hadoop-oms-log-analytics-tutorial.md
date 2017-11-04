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
ms.date: 09/12/2017
ms.author: nitinme
ms.openlocfilehash: dbd3d0ed4337d4fe86465c5c59bf20c0a50a87b4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Azure Log Analytics gebruiken voor het bewaken van HDInsight-clusters (Preview)

In dit artikel leert u hoe u met Azure-logboekanalyse bewaak de bewerkingen van HDInsight Hadoop-clusters.

Log Analytics is een service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) die wordt bewaakt uw cloud en on-premises omgevingen voor het onderhouden van de beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden. 

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/free).

* **Een Azure HDInsight-cluster**. Op dit moment kunt u Azure OMS met de volgende typen van de HDInsight-cluster:
    * Hadoop
    * Spark
    * HBase
    * Storm
    * Kafka
    * Interactieve Hive

    Zie voor instructies over het maken van een HDInsight-cluster [aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).


* **Een werkruimte voor logboekanalyse**. U kunt deze werkruimte beschouwen als een unieke Log Analytics-omgeving met een eigen data-opslagplaats, gegevensbronnen en oplossingen. Hebt u een dergelijke werkruimte al gemaakt die u kunt koppelen aan Azure HDInsight-clusters. Zie voor instructies [maken van een werkruimte voor logboekanalyse](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

## <a name="configure-hdinsight-cluster-to-use-azure-log-analytics"></a>HDInsight-cluster voor het gebruik van Azure-logboekanalyse configureren

In deze sectie configureert u een bestaand HDInsight Hadoop-cluster voor het gebruik van een Azure-logboekanalyse-werkruimte voor het bewaken van taken, logboeken voor foutopsporing, enz.

1. Klik in de Azure-portal in het linkerdeelvenster op **HDInsight-clusters**, en klik vervolgens op de naam van het cluster dat u wilt configureren met Azure-logboekanalyse.

2. Klik op de blade cluster in het linkerdeelvenster **bewaking**.

3. Klik in het rechterdeelvenster **inschakelen**, en selecteer vervolgens een bestaande werkruimte voor logboekanalyse. Klik op **Opslaan**.

    ![Schakel bewaking voor HDInsight-clusters](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "bewaking voor HDInsight-clusters inschakelen")

4. Nadat het cluster is geconfigureerd voor logboekanalyse voor het bewaken van gebruik, ziet u een **Open OMS-Dashboard** optie aan de bovenkant van het tabblad. Klik op de knop.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "Open OMS-dashboard")

5. Voer uw Azure-referenties in als daarom wordt gevraagd. U wordt omgeleid naar het dashboard Microsoft OMS.

    ![Operations Management Suite-portal](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "Operations Management Suite-portal")

## <a name="next-steps"></a>Volgende stappen
* [Oplossingen voor HDInsight-cluster toevoegen aan Log Analytics](hdinsight-hadoop-oms-log-analytics-management-solutions.md)
