---
title: 'Azure Toolkit voor IntelliJ: Spark toepassingen maken voor een HDInsight-cluster | Microsoft Docs'
description: De Azure-werkset voor IntelliJ gebruiken voor het ontwikkelen van Spark scala-toepassingen die zijn geschreven in Scala en deze verzenden naar een HDInsight Spark-cluster.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 19cb8f436fa4d86f323013a5d4b3b50bf6c80a1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a>Gebruik van Azure Toolkit voor IntelliJ Spark-toepassingen voor een HDInsight-cluster maken

Gebruik de Azure-Toolkit voor IntelliJ-invoegtoepassing voor het ontwikkelen van Spark scala-toepassingen die zijn geschreven in Scala en deze vervolgens verzenden naar een HDInsight Spark-cluster rechtstreeks vanuit de IntelliJ integrated development environment (IDE). U kunt de invoegtoepassing gebruiken in een aantal manieren:

* Ontwikkelen en het verzenden van een Scala Spark-toepassing op een HDInsight Spark-cluster.
* Toegang tot de bronnen van uw Azure HDInsight Spark-cluster.
* Ontwikkelen en een Scala Spark-toepassing lokaal uitvoeren.

Voor het maken van uw project, geven de [Spark scala-toepassingen maken met de Azure-Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.

> [!IMPORTANT]
> U kunt deze invoegtoepassing gebruiken voor het maken en verzenden van toepassingen alleen voor een HDInsight Spark-cluster op Linux.
> 

## <a name="prerequisites"></a>Vereisten

- Een Apache Spark-cluster in HDInsight Linux. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
- Oracle Java Development Kit. U kunt installeren vanuit de [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. In dit artikel gebruikt versie 2017.1. U kunt installeren vanuit de [JetBrains website](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Azure Toolkit voor IntelliJ installeren
Zie voor installatie-instructies [Azure Toolkit installeren voor IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="sign-in-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

1. Start de IntelliJ IDE en Azure Explorer te openen. Op de **weergave** selecteert u **hulpprogramma Windows**, en selecteer vervolgens **Azure Explorer**.
       
   ![De Azure Explorer-koppeling](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. Met de rechtermuisknop op de **Azure** knooppunt en selecteer vervolgens **aanmelden**.

3. In de **Azure aanmelden** dialoogvenster, **aanmelden**, en voer uw Azure-referenties.

    ![Het dialoogvenster Azure aanmelden](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. Nadat u bent aangemeld, de **abonnementen Selecteer** in het dialoogvenster geeft een lijst van alle Azure-abonnementen die gekoppeld aan de referenties zijn. Selecteer de **Selecteer** knop.

    ![Het dialoogvenster abonnementen selecteren](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. Op de **Azure Explorer** tabblad uit, vouw **HDInsight** om weer te geven van de HDInsight Spark-clusters die zich in uw abonnement.
   
    ![HDInsight Spark-clusters in Azure Explorer](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. Als u wilt weergeven van de bronnen (bijvoorbeeld opslagaccounts) die gekoppeld aan het cluster zijn, kunt u een naam van de cluster-knooppunt verder uitbreiden.
   
    ![Een uitgebreide naam van de cluster-knooppunt](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Een Spark Scala-toepassing uitvoeren op een HDInsight Spark-cluster

1. Start IntelliJ IDEA en maak vervolgens een project. In de **nieuw Project** dialoogvenster de volgende handelingen uit: 

   a. Selecteer **HDInsight** > **Spark in HDInsight (Scala)**.

   b. In de **Build hulpprogramma** , selecteert u een van de volgende, afhankelijk van uw behoeften:

      * **Maven**, voor ondersteuning van de wizard Scala project maken
      * **SBT**, voor het beheren van de afhankelijkheden en bouwen voor het project Scala

    ![Het dialoogvenster Nieuw Project](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. Selecteer **volgende**.

3. Het maken van het project Scala wizard detecteert automatisch of u de invoegtoepassing Scala hebt geïnstalleerd. Selecteer **installeren**.

   ![Controle van de invoegtoepassing scala](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. Selecteer voor het downloaden van de invoegtoepassing Scala **OK**. Volg de instructies IntelliJ opnieuw opstarten. 

   ![Het dialoogvenster Scala-invoegtoepassing installeren](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. In de **nieuw Project** venster de volgende handelingen uit:  

    ![De Spark SDK selecteren](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Voer een naam en locatie.

   b. In de **Project SDK** vervolgkeuzelijst, selecteer **Java 1.8** voor het Spark-cluster voor 2.x of selecteer **Java 1.7** voor de 1.x Spark-cluster.

   c. In de **Spark versie** vervolgkeuzelijst, wizard van Scala project maken voor Spark SDK en Scala SDK de juiste versie kan worden geïntegreerd. Als de Spark-cluster versie ouder dan 2.0 is, selecteert u **Spark 1.x**. Selecteer anders **Spark2.x**. In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)**.

6. Selecteer **Voltooien**.

7. Het Spark-project wordt automatisch een artefact voor u gemaakt. Als u wilt weergeven van het artefact, het volgende doen:

   a. Op de **bestand** selecteert u **projectstructuur**.

   b. In de **projectstructuur** dialoogvenster, **artefacten** om weer te geven van de standaard-artefacten die is gemaakt. U kunt ook uw eigen artefacten maken door het plusteken (**+**).

      ![Informatie over de artefacten in het dialoogvenster](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. De broncode van uw toepassing toevoegen als volgt:

   a. In Projectverkenner met de rechtermuisknop op **src**, wijs **nieuw**, en selecteer vervolgens **Scala klasse**.
      
      ![Opdrachten voor het maken van een klasse Scala van Projectverkenner](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. In de **nieuwe Scala klasse maken** dialoogvenster vak, Geef een naam op, selecteer **Object** in de **soort** vak en selecteer vervolgens **OK**.
      
      ![Dialoogvenster Nieuwe Scala klasse maken](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. In de **MyClusterApp.scala** bestand, plak de volgende code. De code leest de gegevens van HVAC.csv (beschikbaar op alle HDInsight Spark-clusters), worden de rijen waarvoor slechts één cijfer in de zevende kolom in het CSV-bestand en schrijft de uitvoer naar **/HVACOut** onder de standaardcontainer voor opslag voor het cluster.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. Voer de toepassing op een HDInsight Spark-cluster als volgt:

   a. In Projectverkenner met de rechtermuisknop op de projectnaam en selecteer vervolgens **Spark toepassing verzenden naar HDInsight**.
      
      ![De toepassing Spark verzenden naar HDInsight-opdracht](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. U wordt gevraagd de referenties van uw Azure-abonnement in te voeren. In de **Spark verzending** in het dialoogvenster bieden de volgende waarden en selecteer vervolgens **indienen**.
      
      * Voor **Spark-clusters (alleen voor Linux)**, selecteert u het HDInsight Spark-cluster waarop u wilt dat uw toepassing uit te voeren.

      * Selecteer een artefact van het project IntelliJ of Selecteer een van de vaste schijf.

      * In de **Main klassenaam** Selecteer het weglatingsteken (**...** ), selecteert u de belangrijkste klasse in de broncode van uw toepassing en selecteer vervolgens **OK**.

        ![Het dialoogvenster Main-klasse selecteren](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Omdat de toepassingscode die in dit voorbeeld geen opdrachtregelargumenten vereist of verwijzen naar potten of bestanden, kunt u de resterende selectievakjes leeg laten. Nadat u de informatie te verstrekken, moet de volgende afbeelding eruitzien als in het dialoogvenster.
        
        ![Het dialoogvenster Spark verzending](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. De **Spark verzending** tabblad aan de onderkant van het venster moet worden gestart om de voortgang weer te geven. U kunt ook de toepassing stoppen door het selecteren van de rode knop in de **Spark verzending** venster.
      
      ![Het venster Spark verzending](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      Zie voor meer informatie over het openen van de taakuitvoer van de, het ' toegang en HDInsight Spark-clusters beheren met behulp van Azure Toolkit voor IntelliJ ' verderop in dit artikel.

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Uitvoeren of fouten opsporen in een Spark Scala-toepassing op een HDInsight Spark-cluster
Aangeraden wordt ook een andere manier om de Spark-toepassing aan het cluster in te dienen. U kunt dit doen door het instellen van de parameters in de **uitvoeren/Debug configuraties** IDE. Zie voor meer informatie [op afstand fouten opsporen in Spark scala-toepassingen op een HDInsight-cluster in Azure werkset voor IntelliJ via SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Toegang tot en HDInsight Spark-clusters beheren met behulp van Azure Toolkit voor IntelliJ
U kunt verschillende bewerkingen kunt uitvoeren met behulp van Azure Toolkit voor IntelliJ.

### <a name="access-the-job-view"></a>Toegang tot de taakweergave van de
1. Vouw in Azure Explorer **HDInsight**, vouw de naam van het Spark-cluster en selecteer vervolgens **taken**.  

    ![Taak weergave knooppunt](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. In het rechterdeelvenster de **Spark taakweergave** tabblad geeft alle toepassingen die op het cluster worden uitgevoerd. Selecteer de naam van de toepassing waarvoor u wilt voor meer informatie.

    ![App-details](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. Als u wilt weergeven basisinformatie actieve taak, de muisaanwijzer op de taakgrafiek. Als u wilt weergeven in de grafiek fasen en informatie die elke taak genereert, selecteer een knooppunt in de taakgrafiek.

    ![Fase taakdetails](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Om weer te geven vaak gebruikt u Logboeken, zoals *stuurprogramma Stderr*, *stuurprogramma Stdout*, en *Directory Info*, selecteer de **logboek** tabblad.

    ![Logboekgegevens](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. U kunt ook de geschiedenis Spark gebruikersinterface en de gebruikersinterface van YARN (op het toepassingsniveau) weergeven door een koppeling aan de bovenkant van het venster te selecteren.

### <a name="access-the-spark-history-server"></a>Toegang tot de server van de geschiedenis van Spark
1. Vouw in Azure Explorer **HDInsight**, met de rechtermuisknop op de naam van uw Spark-cluster en selecteer vervolgens **Open Spark geschiedenis UI**. 

2. Wanneer u wordt gevraagd, typt u de beheerdersreferenties van het cluster, dat u hebt opgegeven bij het instellen van het cluster.

3. Op het dashboard van de server in de Spark geschiedenis, kunt u de naam van de toepassing op zoek naar de toepassing dat u zojuist hebt voltooid. In de bovenstaande code stelt u de naam van de toepassing met behulp van `val conf = new SparkConf().setAppName("MyClusterApp")`. De naam van uw toepassing Spark is daarom **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Start de Ambari-portal
1. Vouw in Azure Explorer **HDInsight**, met de rechtermuisknop op de naam van uw Spark-cluster en selecteer vervolgens **beheerportal Open Cluster (Ambari)**. 

2. Wanneer u wordt gevraagd, typt u de beheerdersreferenties voor het cluster. U kunt deze referenties opgegeven tijdens het installatieproces van het cluster.

### <a name="manage-azure-subscriptions"></a>Azure-abonnementen beheren
Azure-Toolkit voor IntelliJ bevat standaard het Spark-clusters van alle uw Azure-abonnementen. Indien nodig, kunt u de abonnementen die u wilt openen. 

1. In Azure Explorer met de rechtermuisknop op de **Azure** de hoofd-knooppunt en selecteer vervolgens **abonnementen beheren**. 

2. Schakel de selectievakjes naast de abonnementen die u niet wilt openen, en selecteer vervolgens in het dialoogvenster **sluiten**. U kunt ook selecteren **Afmelden** als u wilt afmelden bij uw Azure-abonnement.

## <a name="run-a-spark-scala-application-locally"></a>Een Spark Scala-toepassing lokaal uitvoeren
U kunt Azure Toolkit voor IntelliJ gebruiken Spark Scala-toepassingen lokaal uitvoeren op uw werkstation. De toepassingen die normaal gesproken geen toegang nodig tot clusterresources, zoals de storage-containers en u kunt uitvoeren en lokaal te testen.

### <a name="prerequisite"></a>Vereiste
Terwijl u de lokale Spark Scala-toepassingen op een Windows-computer uitvoert, krijgt u mogelijk een uitzondering, zoals wordt beschreven in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). De uitzondering is omdat WinUtils.exe in Windows ontbreekt. 

Deze fout op te lossen [downloaden van het uitvoerbare bestand](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**. Voeg de omgevingsvariabele **HADOOP_HOME**, en stel de waarde van de variabele **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Een lokale Spark Scala-toepassing uitvoeren
1. IntelliJ IDEA Start en een project maken. 

2. In de **nieuw Project** dialoogvenster de volgende handelingen uit:
   
    a. Selecteer **HDInsight** > **Spark in HDInsight lokaal uitvoeren voorbeeld (Scala)**.

    b. In de **Build hulpprogramma** , selecteert u een van de volgende, afhankelijk van uw behoeften:

      * **Maven**, voor ondersteuning van de wizard Scala project maken
      * **SBT**, voor het beheren van de afhankelijkheden en bouwen voor het project Scala

    ![Het dialoogvenster Nieuw Project](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. Selecteer **volgende**.
 
4. In het volgende venster de volgende doen:
   
    a. Voer een naam en locatie.

    b. In de **Project SDK** vervolgkeuzelijst, selecteert u een Java-versie die is hoger dan versie 1.7.

    c. In de **Spark versie** vervolgkeuzelijst, selecteert u de versie van Scala die u wilt gebruiken: Scala 2.11.x voor Spark 2.0 of Scala 2.10.x voor Spark 1.6.

    ![Het dialoogvenster Nieuw Project](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. Selecteer **Voltooien**.

6. De sjabloon wordt toegevoegd een voorbeeldcode (**LogQuery**) onder de **src** map die u lokaal op uw computer uitvoeren kunt.
   
    ![Locatie van LogQuery](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. Met de rechtermuisknop op de **LogQuery** toepassing en selecteer vervolgens **uitvoeren 'LogQuery'**. Op de **uitvoeren** tabblad onderaan, ziet u de volgende uitvoer:
   
   ![Spark toepassing lokale resultaat van uitgevoerde](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Bestaande IntelliJ IDEA toepassingen met Azure Toolkit voor IntelliJ converteren
U kunt de bestaande Spark Scala converteren toepassingen die u in IntelliJ IDEA voor compatibiliteit met Azure Toolkit voor IntelliJ gemaakt. Vervolgens kunt u de invoegtoepassing voor het verzenden van de toepassingen naar een HDInsight Spark-cluster.

1. Voor een bestaande Spark Scala-toepassing die is gemaakt via IntelliJ IDEA, het bijbehorende .iml-bestand te openen.

2. In de hoofdmap van het niveau is een **module** element als volgt:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Bewerk het element om toe te voegen `UniqueKey="HDInsightTool"` zodat de **module** element ziet er als volgt:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Sla de wijzigingen op. Nu moet uw toepassing compatibel zijn met Azure Toolkit voor IntelliJ. U kunt dit testen met de rechtermuisknop op de projectnaam in Projectverkenner. Het pop-upmenu heeft nu de optie **Spark toepassing verzenden naar HDInsight**.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Fout bij lokaal uitvoeren: *gebruik een grotere heapgrootte*
Als u een 32-bits Java SDK tijdens de uitvoering van lokale, kunt u in Spark 1.6, de volgende fouten tegenkomen:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Deze fouten optreden omdat de heapgrootte is niet groot genoeg zijn voor Spark om uit te voeren. Spark vereist ten minste 471 MB. (Zie voor meer informatie [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) Een eenvoudige oplossing is een 64-bits Java SDK gebruiken. U kunt ook de JVM-instellingen in IntelliJ wijzigen door de volgende opties toe te voegen:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Opties toe te voegen aan het vak 'VM options' in IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>Veelgestelde vragen
Als u een toepassing met Azure Data Lake Store, kies **interactief** modus tijdens het proces voor Azure aanmelden. Als u selecteert **automatisch** modus, kunt u een foutmelding krijgt.

![interactief aanmelden](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

Nu we dit heeft opgelost. U kunt een Azure Data Lake-Cluster verzenden van uw toepassing met een methode voor aanmelden.

## <a name="feedback-and-known-issues"></a>Feedback en bekende problemen
Op dit moment wordt bekijken Spark uitvoer rechtstreeks niet ondersteund.

Als u suggesties of feedback hebt, of als er problemen optreden wanneer u deze invoegtoepassing gebruikt, een e-mail sturen naar hdivstool@microsoft.com.

## <a name="seealso"></a>Volgende stappen
* [Overzicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Demo
* Maak Scala project (video): [Spark Scala-toepassingen maken](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Foutopsporing op afstand (video): [gebruik Azure Toolkit voor IntelliJ fouten opsporen in Spark scala-toepassingen op afstand op HDInsight-Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight analyseren gebouwtemperatuur met behulp van HVAC-gegevens gebruiken](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Spark in HDInsight voor het bouwen van realtime streamingtoepassingen gebruiken](hdinsight-apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark-toepassingen op afstand via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](hdinsight-apache-spark-job-debugging.md)

