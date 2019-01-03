---
title: Apache Spark-taken die worden uitgevoerd op Azure HDInsight
description: Gebruikersinterface van YARN, Spark-gebruikersinterface en geschiedenis van de Spark-server bij te houden en fouten opsporen in taken die worden uitgevoerd op een Spark-cluster in Azure HDInsight gebruiken
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: hrasheed
ms.openlocfilehash: 1601663266f59668918e6799b5c4a7ff606431c4
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53600009"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Apache Spark-taken die worden uitgevoerd op Azure HDInsight

In dit artikel leert u hoe u traceren en foutopsporing kunt uitvoeren [Apache Spark](https://spark.apache.org/) taken die worden uitgevoerd op HDInsight-clusters met behulp van de [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) UI, Spark-gebruikersinterface en de Server van de geschiedenis van Spark. Starten van een Spark-taak met behulp van een laptop beschikbaar met het Spark-cluster **Machine learning: Voorspellende analyse van voedsel inspectie gegevens met behulp van MLLib**. U kunt de volgende stappen uit om bij te houden van een toepassing die u wilde verzenden met behulp van een andere benadering, bijvoorbeeld **spark-submit**.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende:

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.
* U moet zijn gestart voor het uitvoeren van de notebook  **[Machine learning: Voorspellende analyse van voedsel inspectie gegevens met behulp van MLLib](apache-spark-machine-learning-mllib-ipython.md)**. Volg de koppeling voor instructies over het uitvoeren van dit notitieblok.  

## <a name="track-an-application-in-the-yarn-ui"></a>Bijhouden van een toepassing in de gebruikersinterface van YARN
1. Start de gebruikersinterface van YARN. Klik op **Yarn** onder **Clusterdashboards**.
   
    ![Gebruikersinterface van YARN starten](./media/apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]  
   > U kunt ook kunt u ook de YARN-gebruikersinterface van de Ambari UI starten. Als u wilt de UI Ambari starten, klikt u op **Ambari home** onder **Clusterdashboards**. Klik op de UI Ambari **YARN**, klikt u op **snelkoppelingen**, klikt u op de actieve Resource Manager en klik vervolgens op **Resource Manager UI**. 

2. Omdat u de Spark-taak met behulp van Jupyter notebooks eerst, de toepassing heeft de naam van de **remotesparkmagics** (dit is de naam op voor alle toepassingen die worden gestart vanuit de notitieblokken). Klik op de toepassings-ID op basis van de naam van de toepassing voor meer informatie over de taak. Hiermee wordt de toepassingsweergave gestart.
   
    ![Spark-toepassing-ID vinden](./media/apache-spark-job-debugging/find-application-id.png)
   
    Voor zulke toepassingen die worden gestart vanuit de Jupyter-notebooks, is de status altijd **met** totdat u de notebook afgesloten.
3. In de toepassingsweergave, kunt u inzoomen verder Ontdek de containers die zijn gekoppeld aan de toepassing en de logboeken (stdout/stderr). U kunt ook de Spark-gebruikersinterface starten door te klikken op de koppeling die overeenkomt met de **bijhouden URL**, zoals hieronder weergegeven. 
   
    ![Containerlogboeken downloaden](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Een toepassing in de Spark-gebruikersinterface bijhouden
In de Spark-gebruikersinterface kunt u inzoomen op de Spark-taken die zijn geïnitieerd door de toepassing die u eerder hebt gestart.

1. Om te starten van de Spark-gebruikersinterface in de toepassingsweergave, klikt u op de koppeling op basis van de **bijhouden URL**, zoals wordt weergegeven in de bovenstaande schermafbeelding. Hier ziet u alle Spark-taken die worden gestart door de toepassing die wordt uitgevoerd in de Jupyter-notebook.
   
    ![Spark-taken weergeven](./media/apache-spark-job-debugging/view-spark-jobs.png)
2. Klik op de **Executor** tabblad voor verwerking en opslag-informatie voor elke executor. U kunt ook de aanroepstack ophalen door te klikken op de **Thread Dump** koppeling.
   
    ![Spark Executor weergeven](./media/apache-spark-job-debugging/view-spark-executors.png)
3. Klik op de **fasen** tabblad om te bekijken van de fasen die zijn gekoppeld aan de toepassing.
   
    ![Spark-fasen weergeven](./media/apache-spark-job-debugging/view-spark-stages.png)
   
    Elke fase van de kan hebben meerdere taken waarvoor u statistieken kan worden uitgevoerd, zoals weergeven kunt hieronder weergegeven.
   
    ![Spark-fasen weergeven](./media/apache-spark-job-debugging/view-spark-stages-details.png) 
4. U kunt de pagina met details van fase, DAG visualisatie starten. Vouw de **DAG visualisatie** koppelen aan de bovenkant van de pagina, zoals hieronder wordt weergegeven.
   
    ![Spark fasen DAG visualisatie weergeven](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    DAG of Direct Aclyic grafiek vertegenwoordigt de verschillende fasen van de toepassing. Elke blauw vak in de grafiek vertegenwoordigt een Spark-bewerking aangeroepen vanuit de toepassing.
5. Vanaf de pagina met details van fase, kunt u ook de tijdlijnweergave toepassing starten. Vouw de **gebeurtenis tijdlijn** koppelen aan de bovenkant van de pagina, zoals hieronder wordt weergegeven.
   
    ![Spark fasen gebeurtenis tijdlijn weergeven](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Hiermee worden de Spark-gebeurtenissen weergegeven in de vorm van een tijdlijn. De tijdlijnweergave is beschikbaar op drie niveaus voor taken, binnen een taak en binnen een fase. De bovenstaande afbeelding bevat de tijdlijnweergave voor een bepaald tijdstip.
   
   > [!TIP]  
   > Als u selecteert de **zoomen inschakelen** selectievakje, kunt u bladeren naar links en rechts in de tijdlijn weergeven.

6. Andere tabbladen in de gebruikersinterface van Spark bevatten nuttige informatie over de Spark-instantie.
   
   * Tabblad van de opslag - als uw toepassing een rdd's maakt, kunt u informatie over die in het tabblad opslag vinden.
   * Tabblad omgeving - dit tabblad bevat een groot aantal nuttige informatie over uw Spark-instantie, zoals de 
     * Scala-versie
     * Gebeurtenislogboek van directory die zijn gekoppeld aan het cluster
     * Aantal kernen executor voor de toepassing
     * Enz.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Meer informatie over voltooide taken met behulp van de Server van de geschiedenis van Spark
Als een taak is voltooid, wordt de informatie over de taak behouden in de Server van de geschiedenis van Spark.

1. Om te starten van de Spark-geschiedenis-Server, op de blade overzicht klikt u op **Spark geschiedenis server** onder **Clusterdashboards**.
   
    ![Spark geschiedenis Server starten](./media/apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]  
   > U kunt ook kunt u ook de Spark-geschiedenis Server gebruikersinterface van de Ambari UI starten. Als u wilt starten, de UI Ambari, op de blade overzicht klikt u op **Ambari home** onder **Clusterdashboards**. Klik op de UI Ambari **Spark**, klikt u op **snelkoppelingen**, en klik vervolgens op **Spark geschiedenis Server gebruikersinterface**.

2. U ziet de voltooide toepassingen die worden vermeld. Klik op een toepassings-ID Inzoomen op een toepassing voor meer informatie.
   
    ![Spark geschiedenis Server starten](./media/apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Zie ook
*  [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
*  [Apache Spark-taken met behulp van uitgebreide Spark geschiedenis-Server](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Voor gegevensanalisten

* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Telemetrie analyse van Application Insights met Apache Spark in HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Caffe gebruiken op Azure HDInsight Spark voor gedistribueerde deep learning](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Voor Spark-ontwikkelaars

* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](apache-spark-livy-rest-interface.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op te sporen Apache Spark-toepassingen op afstand](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)
