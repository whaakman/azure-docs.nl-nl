---
title: 'Azure Toolkit voor IntelliJ: fouten opsporen in Spark scala-toepassingen op afstand via SSH | Microsoft Docs'
description: Stapsgewijze instructies voor het gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ opsporen toepassingen op afstand op HDInsight-clusters via SSH
keywords: foutopsporing op afstand intellij, externe foutopsporing intellij, ssh, intellij, hdinsight, fouten opsporen in intellij, foutopsporing
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: jejiang
ms.openlocfilehash: cebbe2a0e28d49c93d0ebf12cc04b3d201dcec97
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Fouten opsporen in Spark scala-toepassingen op een HDInsight-cluster in Azure werkset voor IntelliJ via SSH

Dit artikel bevat stapsgewijze instructies over het gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ opsporen toepassingen op afstand op een HDInsight-cluster. Als u wilt opsporen in uw project, u kunt ook bekijken de [fouten opsporen in HDInsight Spark-toepassingen met Azure Toolkit voor IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.

**Vereisten**

* **HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ**. Dit hulpprogramma maakt deel uit van Azure Toolkit voor IntelliJ. Zie voor meer informatie [Azure Toolkit installeren voor IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).
* **Azure Toolkit voor IntelliJ**. Gebruik deze toolkit Spark om toepassingen te maken voor een HDInsight-cluster. Voor meer informatie, volg de instructies in [gebruik Azure Toolkit voor IntelliJ Spark om toepassingen te maken voor een HDInsight-cluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **HDInsight SSH-service met gebruikersnaam en wachtwoord management**. Zie voor meer informatie [verbinding maken met HDInsight (Hadoop) met behulp van SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) en [SSH gebruiken voor toegang tot de Ambari-tunneling web-UI, JobHistory, NameNode, Oozie en andere web-UI](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Een Spark Scala-toepassing maken en te configureren voor foutopsporing op afstand

1. Start IntelliJ IDEA en maak vervolgens een project. In de **nieuw Project** dialoogvenster de volgende handelingen uit:

   a. Selecteer **HDInsight**. 

   b. Selecteer een Java of Scala sjabloon op basis van uw voorkeur. Selecteer een van de volgende opties:

      - **Spark in HDInsight (Scala)**

      - **Spark in HDInsight (Java)**

      - **Spark in HDInsight-Cluster uitvoeren voorbeeld (Scala)**

      In dit voorbeeld wordt een **Spark in HDInsight-Cluster uitvoeren voorbeeld (Scala)** sjabloon.

   c. In de **Build hulpprogramma** , selecteert u een van de volgende, afhankelijk van uw behoeften:

      - **Maven**, voor ondersteuning van de wizard Scala project maken

      -  **SBT**, voor het beheren van de afhankelijkheden en bouwen voor het project Scala 

      ![Een debug-project maken](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. Selecteer **volgende**.     
 
3. In de volgende **nieuw Project** venster de volgende handelingen uit:

   ![Selecteer de Spark SDK](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Voer een naam van het project en de projectlocatie.

   b. In de **Project SDK** vervolgkeuzelijst, selecteer **Java 1.8** voor **2.x Spark** cluster of selecteer **Java 1.7** voor **Spark 1.x** cluster.

   c. In de **Spark versie** vervolgkeuzelijst, de wizard Scala project maken voor Spark SDK en Scala SDK de juiste versie kan worden geïntegreerd. Als de spark-cluster versie ouder dan 2.0 is, selecteert u **Spark 1.x**. Selecteer anders **2.x Spark.** In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)**.

   d. Selecteer **Voltooien**.

4. Selecteer **src** > **belangrijkste** > **scala** uw code in het project te openen. In dit voorbeeld wordt de **SparkCore_wasbloTest** script.

5. Voor toegang tot de **configuraties bewerken** menu, selecteer het pictogram in de rechterbovenhoek. U kunt vanuit dit menu maken of bewerken van de configuraties voor foutopsporing op afstand.

   ![Configuraties bewerken](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png) 

6. In de **uitvoeren/Debug configuraties** dialoogvenster Selecteer het plusteken (**+**). Selecteer vervolgens de **Submit Spark Job** optie.

   ![Nieuwe configuratie toevoegen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)
7. Geef gegevens voor **naam**, **Spark-cluster**, en **Main klassenaam**. Selecteer vervolgens **geavanceerde configuratie**. 

   ![Configuraties voor foutopsporing uitvoeren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

8. In de **Spark verzending van geavanceerde configuratie** dialoogvenster, **foutopsporing op afstand inschakelen Spark**. Geef de SSH-gebruikersnaam en een wachtwoord invoeren of een bestand met een persoonlijke sleutel gebruiken. Selecteer om de configuratie op te slaan, **OK**.

   ![Spark foutopsporing op afstand inschakelen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

9. De configuratie wordt nu opgeslagen met de naam die u hebt opgegeven. Selecteer de om configuratiedetails te bekijken, de naam van de configuratie. Als u wilt wijzigen, selecteert u **configuraties bewerken**. 

10. Nadat u de configuratie-instellingen hebt voltooid, kunt u het project uitvoeren op de externe cluster of foutopsporing op afstand uitvoeren.

## <a name="learn-how-to-perform-remote-debugging"></a>Leer hoe u foutopsporing op afstand
### <a name="scenario-1-perform-remote-run"></a>Scenario 1: Extern uitvoeren uitvoeren

In deze sectie wordt beschreven hoe u fouten opsporen in stuurprogramma's en Executor.

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. Dingen instellen en selecteer vervolgens de **Debug** pictogram.

   ![Selecteer het pictogram voor foutopsporing](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

2. Wanneer de programma-uitvoering is het laatste punt bereikt, ziet u een **stuurprogramma** tabblad en de twee **Executor** tabbladen de **foutopsporingsprogramma** deelvenster. Selecteer de **hervatten programma** pictogram om door te gaan met het uitvoeren van de code, die vervolgens het volgende onderbrekingspunt bereikt en richt zich op de bijbehorende **Executor** tabblad. U kunt de logboeken worden uitgevoerd op de bijbehorende bekijken **Console** tabblad.

   ![Tabblad foutopsporing](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a>Scenario 2: Voer foutopsporing op afstand en het verhelpen van problemen
In deze sectie wordt beschreven hoe u de variabele waarde dynamisch bijwerken met behulp van de foutopsporing mogelijkheid IntelliJ voor een eenvoudige oplossing. In het volgende codevoorbeeld is een uitzondering opgetreden, omdat het doelbestand al bestaat.
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a>Foutopsporing op afstand en de fout is hersteld uit te voeren
1. Twee dingen instellen en selecteer vervolgens de **Debug** pictogram om de externe foutopsporing proces te starten.

2. Hiermee stopt u de code op het eerste belangrijk punt en de parameter en de variabele gegevens worden weergegeven in de **variabelen** deelvenster. 

3. Selecteer de **hervatten programma** pictogram om door te gaan. De code reageert op het tweede punt. De uitzondering is opgetreden, zoals verwacht.

  ![Genereert fout](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png) 

4. Selecteer de **hervatten programma** pictogram opnieuw. De **HDInsight Spark verzending** venster een fout met 'job run is mislukt' weergegeven.

  ![Verzending van de fout](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png) 

5. Schakel wilt de variabele waarde dynamisch bijwerken met behulp van de mogelijkheid foutopsporing IntelliJ **Debug** opnieuw. De **variabelen** deelvenster opnieuw wordt weergegeven. 

6. Het doel met de rechtermuisknop op de **Debug** tabblad en selecteer vervolgens **waarde instellen**. Voer vervolgens een nieuwe waarde voor de variabele. Selecteer vervolgens **Enter** om op te slaan van de waarde. 

  ![Waarde instellen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value.png) 

7. Selecteer de **hervatten programma** pictogram om door te gaan het programma uit te voeren. Dit moment is geen uitzondering opgetreden. U kunt zien dat het project met succes wordt uitgevoerd zonder eventuele uitzonderingen.

  ![Fouten opsporen in zonder uitzondering](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Volgende stappen
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demo
* Maak Scala project (video): [Spark Scala-toepassingen maken](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Foutopsporing op afstand (video): [gebruik Azure Toolkit voor IntelliJ fouten opsporen in Spark scala-toepassingen op afstand op een HDInsight-cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight analyseren gebouwtemperatuur met behulp van HVAC-gegevens gebruiken](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Spark in HDInsight voor het bouwen van realtime streamingtoepassingen gebruiken](apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Gebruik van Azure Toolkit voor IntelliJ Spark-toepassingen voor een HDInsight-cluster maken](apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
