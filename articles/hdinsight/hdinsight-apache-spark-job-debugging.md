---
title: Fouten opsporen in Apache Spark taken uitgevoerd op Azure HDInsight | Microsoft Docs
description: Gebruikersinterface van YARN, Spark-UI en Spark geschiedenis server bijhouden en foutopsporing van taken die worden uitgevoerd op een Spark-cluster in Azure HDInsight gebruiken
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: bf66757cc9439a969c9f28abc0b95055ff697c3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Fouten opsporen in Apache Spark taken uitgevoerd op Azure HDInsight

In dit artikel leert u hoe bijhouden en fouten opsporen in Spark-taken op via de gebruikersinterface van YARN, Spark-gebruikersinterface en de Server van de geschiedenis van Spark HDInsight-clusters worden uitgevoerd. Voor dit artikel gaat we een Spark-taak met een laptop beschikbaar met het Spark-cluster **Machine learning: Predictive Analytics op voeding inspectie gegevens met behulp van MLLib**. U kunt de onderstaande stappen voor het bijhouden van een toepassing die u verzonden met een andere benadering, bijvoorbeeld **spark indienen**.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende:

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* U moet worden begonnen met de notebook uitgevoerd  **[Machine learning: Predictive Analytics op voeding inspectie gegevens met behulp van MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**. Volg de koppeling voor instructies over het uitvoeren van deze laptop.  

## <a name="track-an-application-in-the-yarn-ui"></a>Een toepassing in de gebruikersinterface van YARN bijhouden
1. Start de gebruikersinterface van YARN. Klik in de cluster-blade op **Cluster-Dashboard**, en klik vervolgens op **YARN**.
   
    ![Gebruikersinterface van YARN starten](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > U kunt ook kunt u ook de gebruikersinterface van YARN van de Ambari UI starten. Start de UI Ambari van de cluster-blade, klikt u op **Cluster-Dashboard**, en klik vervolgens op **HDInsight-Cluster-Dashboard**. Klik op de UI Ambari **YARN**, klikt u op **snelkoppelingen**, klikt u op de actieve resourcemanager en klik vervolgens op **ResourceManager UI**.    
   > 
   > 
2. Omdat u de Jupyter-notebooks met Spark-taak gestart, de toepassing heeft de naam **remotesparkmagics** (dit is de naam op voor alle toepassingen die worden gestart vanuit de laptops). Klik op de toepassings-ID op basis van de naam van de toepassing voor meer informatie over de taak. De toepassingsweergave wordt gestart.
   
    ![Spark toepassings-ID vinden](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    Voor dergelijke toepassingen die worden gestart vanuit de Jupyter-notebooks, is de status altijd **met** voordat u de notebook afgesloten.
3. Uit de toepassingsweergave, kunt u inzoomen verder uitzoeken de containers die zijn gekoppeld aan de toepassing en de logboeken (stdout/stderr). U kunt de Spark-gebruikersinterface ook starten door te klikken op de koppeling die overeenkomt met de **bijhouden URL**, zoals hieronder weergegeven. 
   
    ![Container logboeken downloaden](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Een toepassing in de gebruikersinterface van Spark bijhouden
In de gebruikersinterface Spark, kunt u inzoomen op de Spark-taken die zijn geïnitieerd door de toepassing die u eerder hebt gestart.

1. Start de Spark-gebruikersinterface uit de toepassingsweergave, klikt u op de koppeling tegen de **bijhouden URL**, zoals weergegeven in de bovenstaande schermafbeelding. U kunt alle Spark-taken die worden gestart door de toepassing die wordt uitgevoerd in de Jupyter-notebook kunt zien.
   
    ![Spark-taken weergeven](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. Klik op de **Executor** tabblad verwerking en opslag van gegevens voor elke executor kunt bekijken. U kunt ook de aanroepstack ophalen door te klikken op de **Thread Dump** koppeling.
   
    ![Spark Executor weergeven](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. Klik op de **fasen** tabblad voor een overzicht van de fasen die zijn gekoppeld aan de toepassing.
   
    ![Spark fasen weergeven](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    Elk stadium kan hebben meerdere taken waarvoor u Uitvoeringsstatistieken, zoals weergeven kunt hieronder weergegeven.
   
    ![Spark fasen weergeven](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. U kunt de detailpagina fase DAG visualisatie starten. Vouw de **DAG visualisatie** koppelen aan de bovenkant van de pagina, zoals hieronder wordt weergegeven.
   
    ![Spark fasen DAG visualisatie weergeven](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    DAG of directe Aclyic grafiek vertegenwoordigt de verschillende fasen in de toepassing. Elke blauw vak in de grafiek vertegenwoordigt een Spark-bewerking aangeroepen vanuit de toepassing.
5. Op de pagina fase details kunt u ook de tijdlijnweergave toepassing starten. Vouw de **gebeurtenis tijdlijn** koppelen aan de bovenkant van de pagina, zoals hieronder wordt weergegeven.
   
    ![Spark fasen gebeurtenis tijdlijn weergeven](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Hiermee worden de gebeurtenissen Spark weergegeven in de vorm van een tijdlijn. De tijdlijnweergave is beschikbaar op drie niveaus tussen jobs binnen een taak en binnen een fase. De afbeelding hierboven worden de tijdlijnweergave voor een bepaald stadium.
   
   > [!TIP]
   > Als u selecteert de **zoomen inschakelen** selectievakje, kunt u bladeren naar links en naar rechts tussen de tijdlijnweergave.
   > 
   > 
6. Andere tabbladen in de gebruikersinterface van Spark bevatten nuttige informatie over het Spark-exemplaar.
   
   * Tabblad opslag - als de toepassing een RDDs maakt vindt u informatie over die in het tabblad opslag.
   * Tabblad omgeving - op dit tabblad bevat veel nuttige informatie over uw Spark-exemplaar, zoals de 
     * Versie van scala
     * Gebeurtenislogboek van directory die is gekoppeld aan het cluster
     * Het aantal kernen executor voor de toepassing
     * Enz.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Voor meer informatie over voltooide taken met behulp van de Server van de geschiedenis van Spark
Zodra een taak is voltooid, wordt de informatie over de taak permanent in de Server van de geschiedenis van Spark.

1. Start de Server van de Spark geschiedenis van de cluster-blade, klikt u op **Cluster-Dashboard**, en klik vervolgens op **Spark geschiedenis Server**.
   
    ![Spark geschiedenis Server starten](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > U kunt ook kunt u ook de Spark geschiedenis Server gebruikersinterface van de Ambari UI starten. Start de UI Ambari van de cluster-blade, klikt u op **Cluster-Dashboard**, en klik vervolgens op **HDInsight-Cluster-Dashboard**. Klik op de UI Ambari **Spark**, klikt u op **snelkoppelingen**, en klik vervolgens op **Spark geschiedenis Server UI**.
   > 
   > 
2. Hier ziet u de voltooide toepassingen vermeld. Klik op een toepassings-ID om in te zoomen in een toepassing voor meer informatie.
   
    ![Spark geschiedenis Server starten](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Zie ook
*  [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

### <a name="for-data-analysts"></a>Voor gegevensanalisten

* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Analyse van Application Insights-telemetriegegevens met behulp van Spark in HDInsight](hdinsight-spark-analyze-application-insight-logs.md)
* [Gebruik Caffe op Azure HDInsight Spark voor gedistribueerde grondige learning](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a>Voor Spark-ontwikkelaars

* [Een zelfstandige toepassing maken met behulp van Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](hdinsight-apache-spark-eventhub-streaming.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)


