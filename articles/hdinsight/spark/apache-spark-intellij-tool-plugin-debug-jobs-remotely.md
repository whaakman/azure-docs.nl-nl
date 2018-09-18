---
title: 'Azure Toolkit voor IntelliJ: foutopsporing op afstand in HDInsight Spark-toepassingen '
description: Informatie over hoe HDInsight-hulpprogramma's gebruiken in Azure Toolkit voor IntelliJ op afstand fouten opsporen in Spark-toepassingen die worden uitgevoerd op HDInsight-clusters via VPN-verbinding.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.openlocfilehash: 1a7fcc11bf1b98d5feaeae07264e556acce2b14b
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736070"
---
# <a name="use-azure-toolkit-for-intellij-to-debug-spark-applications-remotely-in-hdinsight-through-vpn"></a>Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand in HDInsight via VPN-verbinding

U wordt aangeraden opsporen van fouten in spark-toepassingen op afstand via SSH. Zie voor instructies [op afstand fouten opsporen in Spark-toepassingen op een HDInsight-cluster met de Azure Toolkit voor IntelliJ via SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

In dit artikel biedt stapsgewijze instructies over het gebruik van de HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ indienen van een Spark-taak op een HDInsight Spark-cluster en op afstand foutopsporing op uw desktopcomputer. Als u wilt deze taken hebt voltooid, moet u de volgende geavanceerde stappen uitvoeren:

1. Maak een site-naar-site of een punt-naar-site virtueel Azure-netwerk. De stappen in dit document wordt ervan uitgegaan dat u een site-naar-site-netwerk.
1. Een Spark-cluster in HDInsight die deel uitmaakt van de site-naar-site virtueel netwerk maken.
1. Controleer de verbinding tussen het hoofdknooppunt van het cluster en het bureaublad.
1. Een Scala-toepassing maken in IntelliJ IDEA, en deze vervolgens configureren voor foutopsporing op afstand.
1. Uitvoeren en fouten opsporen in de toepassing.

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie voor meer informatie, [een gratis proefversie van Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een Apache Spark-cluster in HDInsight**. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.
* **Oracle Java development kit**. U kunt installeren vanuit de [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* **IntelliJ IDEA**. In dit artikel wordt versie 2017.1. U kunt installeren vanuit de [JetBrains website](https://www.jetbrains.com/idea/download/).
* **HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ**. HDInsight-hulpprogramma's voor IntelliJ zijn beschikbaar als onderdeel van de Azure Toolkit voor IntelliJ. Zie voor instructies over het installeren van de Azure Toolkit [Installeer Azure Toolkit voor IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation).
* **Aanmelden bij uw Azure-abonnement van IntelliJ IDEA**. Volg de instructies in [Azure-Toolkit voor IntelliJ Spark om toepassingen te maken voor een HDInsight-cluster](apache-spark-intellij-tool-plugin.md).
* **Tijdelijke oplossing uitzondering**. Tijdens het uitvoeren van de Spark Scala-toepassing voor foutopsporing op afstand op een Windows-computer, krijgt u mogelijk een uitzondering. Deze uitzondering wordt uitgelegd in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) en treedt op vanwege een ontbrekend bestand WinUtils.exe in Windows. U kunt deze fout omzeilen, moet u [downloaden van het uitvoerbare bestand](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**. Voeg een **HADOOP_HOME** omgevingsvariabele, en stel de waarde van de variabele **C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>Stap 1: Een Azure-netwerk maken
Volg de instructies in de volgende koppelingen voor het maken van een Azure-netwerk en controleer vervolgens of de connectiviteit tussen uw computer en het virtuele netwerk:

* [Een VNet met een site-naar-site VPN-verbinding met Azure portal maken](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Maak een VNet met een site-naar-site VPN-verbinding met behulp van PowerShell](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Een punt-naar-site-verbinding met een virtueel netwerk met behulp van PowerShell configureren](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>Stap 2: Een HDInsight Spark-cluster maken
Het is raadzaam dat u ook een Apache Spark-cluster maken in Azure HDInsight die deel uitmaakt van het Azure-netwerk dat u hebt gemaakt. Gebruik de informatie die beschikbaar zijn in [clusters in HDInsight op basis van Linux maken](../hdinsight-hadoop-provision-linux-clusters.md). Selecteer het Azure-netwerk dat u in de vorige stap hebt gemaakt als onderdeel van de optionele configuratie.

## <a name="step-3-verify-the-connectivity-between-the-cluster-head-node-and-your-desktop"></a>Stap 3: Controleer de verbinding tussen het hoofdknooppunt van het cluster en uw bureaublad
1. Ophalen van het IP-adres van het hoofdknooppunt. Open de Ambari UI voor het cluster. Selecteer in de cluster-blade **Dashboard**.

    ![Dashboard selecteren in Ambari](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/launch-ambari-ui.png)
1. Selecteer in de UI Ambari **Hosts**.

    ![Hosts selecteren in Ambari](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/ambari-hosts.png)
1. U ziet een lijst van hoofdknooppunten, werkknooppunten en zookeeper-knooppunten. De hoofdknooppunten hebben een **hn*** voorvoegsel. Selecteer het eerste hoofdknooppunt.

    ![Het hoofdknooppunt in Ambari zoeken](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/cluster-headnodes.png)
1. Uit de **samenvatting** deelvenster onderaan de pagina die wordt geopend, Kopieer de **IP-adres** van het hoofdknooppunt en de **hostnaam**.

    ![Het IP-adres vinden in Ambari](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/headnode-ip-address.png)
1. Voeg het IP-adres en de hostnaam van het hoofdknooppunt de **hosts** bestand op de computer waarop u wilt uitvoeren en op afstand fouten opsporen in de Spark-taak. Hiermee kunt u om te communiceren met het hoofdknooppunt met behulp van het IP-adres, evenals de hostnaam.

   a. Open een Kladblok-bestand met verhoogde bevoegdheden. Uit de **bestand** in het menu **Open**, en gaat u naar de locatie van het hosts-bestand. Op een Windows-computer, de locatie is **C:\Windows\System32\Drivers\etc\hosts**.

   b. Voeg de volgende informatie op de **hosts** bestand:

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
1. Controleer of dat u de hoofdknooppunten pingen kunt met behulp van het IP-adres, evenals de hostnaam van de computer waarop u verbinding met de Azure-netwerk dat wordt gebruikt door het HDInsight-cluster.
1. SSH gebruiken voor verbinding met het hoofdknooppunt van het cluster door de instructies in [verbinding maken met een HDInsight-cluster via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md). Pingen vanaf het hoofdknooppunt van het cluster, het IP-adres van de computer. Test de verbinding met beide IP-adressen toegewezen aan de computer:

    - Een voor de netwerkverbinding
    - Een voor Azure virtual network

1. Herhaal de stappen voor het hoofdknooppunt.

## <a name="step-4-create-a-spark-scala-application-by-using-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>Stap 4: Een Spark Scala-toepassing maken met behulp van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ en configureren voor foutopsporing op afstand
1. Open IntelliJ IDEA en een nieuw project maken. Voer in het dialoogvenster **Nieuw project** de volgende handelingen uit:

    ![Selecteer de nieuwe projectsjabloon, maken in IntelliJ IDEA](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-hdi-scala-app.png)

    a. Selecteer **HDInsight** > **Spark in HDInsight (Scala)**.

    b. Selecteer **Volgende**.
1. In de volgende **nieuw Project** in het dialoogvenster de volgende handelingen uit en selecteer vervolgens **voltooien**:

    - Geef een projectnaam en locatie op.

    - Selecteer in de vervolgkeuzelijst **Project SDK** de optie **Java 1.8** voor het Spark 2.x-cluster of selecteer **Java 1.7** voor het Spark 1.x-cluster.

    - In de **Spark-versie** vervolgkeuzelijst, de wizard Scala-project maken voor de Spark-SDK en de SDK Scala de juiste versie kan worden geïntegreerd. Selecteer **Spark 1.x** als de Spark-clusterversie ouder is dan 2.0. Selecteer anders **Spark 2.x**. In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)** gebruikt.
  
   ![Selecteer de project-SDK en Spark-versie](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-scala-project-details.png)
  
1. Het Spark-project maakt automatisch een artefact voor u. Als u wilt het artefact weergeven, het volgende doen:

    a. Selecteer **Project Structure** in het menu **File**.

    b. In de **projectstructuur** in het dialoogvenster, selecteer **artefacten** om weer te geven van de standaard-artefacten die is gemaakt. U kunt ook uw eigen artefact maken door het selecteren van het plusteken (**+**).

   ![JAR maken](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/default-artifact.png)


1. Bibliotheken toevoegen aan uw project. Als u wilt toevoegen een bibliotheek, het volgende doen:

    a. Met de rechtermuisknop op de naam van het project in de projectstructuur van het en selecteer vervolgens **Open Module-instellingen**. 

    b. In de **projectstructuur** in het dialoogvenster, selecteer **bibliotheken**, selecteer de (**+**) symbool en selecteer vervolgens **van Maven** .

    ![Een bibliotheek toevoegen](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/add-library.png)

    c. In de **downloaden-bibliotheek vanuit Maven-opslagplaats** dialoogvenster vak, zoeken en toevoegen van de volgende bibliotheken:

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
1. Kopie `yarn-site.xml` en `core-site.xml` uit het cluster knooppunt te gaan en ze toevoegen aan het project. Gebruik de volgende opdrachten om de bestanden te kopiëren. U kunt [Cygwin](https://cygwin.com/install.html) om uit te voeren van de volgende `scp` hoofdknooppunten opdrachten de bestanden te kopiëren uit het cluster:

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Omdat we al toegevoegd voor de IP-adres van cluster-hoofdknooppunt en hostnamen voor het hosts-bestand op het bureaublad, gebruiken we de `scp` opdrachten in de volgende manier:

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Deze bestanden toevoegen aan uw project, kopieert u deze onder de **/src** map in de projectstructuur van uw, bijvoorbeeld `<your project directory>\src`.
1. Update de `core-site.xml` -bestand naar de volgende wijzigingen aanbrengen:

   a. Vervang de versleutelde sleutel. De `core-site.xml` bestand bevat de versleutelde sleutel voor het opslagaccount dat is gekoppeld aan het cluster. In de `core-site.xml` -bestand dat u hebt toegevoegd aan het project, de versleutelde sleutel vervangen door de werkelijke opslag-sleutel die is gekoppeld aan het standaardopslagaccount. Zie voor meer informatie, [beheren van uw toegangssleutels voor opslag](../../storage/common/storage-account-manage.md#access-keys).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   b. Verwijder de volgende vermeldingen uit `core-site.xml`:

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   c. Sla het bestand op.
1. De main-klasse voor uw toepassing toevoegen. Uit de **Projectverkenner**, met de rechtermuisknop op **src**, wijst u **nieuw**, en selecteer vervolgens **Scala klasse**.

    ![Selecteer de main-klasse](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code.png)
1. In de **nieuwe Scala-klasse maken** dialoogvenster vak, Geef een naam op, selecteer **Object** in de **soort** vak en selecteer vervolgens **OK**.

    ![Maak nieuwe Scala-klasse](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code-object.png)
1. In de `MyClusterAppMain.scala` bestand, plak de volgende code. Deze code maakt de Spark-context en opent een `executeJob` methode van de `SparkSample` object.

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

1. Herhaal stap 8 en 9 om toe te voegen een nieuwe Scala-object met de naam `*SparkSample`. Voeg de volgende code toe aan deze klasse. Deze code leest de gegevens uit de HVAC.csv (beschikbaar in alle HDInsight Spark-clusters). Het haalt de rijen die slechts één cijfer in de zevende kolom in het CSV-bestand hebben op en schrijft u de uitvoer naar **/HVACOut** onder de standaard-storage-container voor het cluster.

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find the rows which have only one digit in the 7th column in the CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
1. Herhaal stap 8 en 9 om toe te voegen een nieuwe klasse genaamd `RemoteClusterDebugging`. Deze klasse implementeert het framework voor het testen van Spark die wordt gebruikt om op te sporen van de toepassingen. Voeg de volgende code aan de `RemoteClusterDebugging` klasse:

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     Er zijn een aantal belangrijke dingen om te weten:

      * Voor `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, zorg ervoor dat de Spark-assembly JAR is beschikbaar in de clusteropslag in het opgegeven pad.
      * Voor `setJars`, geeft u de locatie waar de JAR-artefact wordt gemaakt. Dit is meestal `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
1. In de`*RemoteClusterDebugging` klasse, met de rechtermuisknop op de `test` trefwoord en selecteer vervolgens **RemoteClusterDebugging configuratie maken**.

    ![Een RAS-configuratie maken](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-remote-config.png)

1. In de **RemoteClusterDebugging configuratie maken** in het dialoogvenster, Geef een naam op voor de configuratie en selecteer vervolgens **testen soort** als de **testnaam**. Behoud de standaardinstellingen voor alle andere waarden. Selecteer **Apply** en vervolgens **OK**.

    ![Informatie over de configuratie toevoegen](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/provide-config-value.png)
1. U ziet nu een **extern uitvoeren** configuratie vervolgkeuzelijst in de menubalk.

    ![De externe vervolgkeuzelijst lijst met](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a>Stap 5: De toepassing uitvoeren in de foutopsporingsmodus
1. Open in uw project IntelliJ IDEA `SparkSample.scala` en maak een onderbrekingspunt naast `val rdd1`. In de **onderbrekingspunt maken voor** pop-upmenu, selecteer **regel in de functie executeJob**.

    ![Een onderbrekingspunt toevoegen](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-breakpoint.png)
1. Als u wilt de toepassing uitvoert, selecteert u de **foutopsporing uitvoeren** naast de **extern uitvoeren** vervolgkeuzelijst configuratie.

    ![Selecteer de knop foutopsporing uitvoeren](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-run-mode.png)
1. Wanneer de programma-uitvoering het onderbrekingspunt bereikt, ziet u een **foutopsporingsprogramma** tabblad in het onderste deelvenster.

    ![Het tabblad foutopsporingsprogramma weergeven](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch.png)
1. Als u wilt toevoegen een watch, selecteer de (**+**) pictogram.

    ![Selecteer het pictogram +](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable.png)

    In dit voorbeeld de toepassing voordat u de variabele verbroken `rdd1` is gemaakt. Met behulp van deze bekijken, zien we de eerste vijf rijen in de variabele `rdd`. Selecteer **Enter**.

    ![Voer het programma uit in de foutopsporingsmodus](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable-value.png)

    Wat u ziet in de vorige afbeelding is dat terabytes aan gegevens en foutopsporing tijdens runtime, mogelijk query hoe de voortgang van uw toepassing. Bijvoorbeeld, in de uitvoer wordt weergegeven in de vorige afbeelding, ziet u dat de eerste rij van de uitvoer een header is. Op basis van deze uitvoer, kunt u de code van uw toepassing om over te slaan van de rij met kolomkoppen, indien nodig wijzigen.
1. U kunt nu selecteren de **hervatten programma** pictogram om door te gaan met uw toepassing uitvoeren.

    ![Hervatten programma selecteren](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-continue-run.png)
1. Als de toepassing voltooid is, ziet u de volgende uitvoer:

    ![Console-uitvoer](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-complete.png)

## <a name="seealso"></a>Volgende stappen
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demo
* Create Scala-project (video): [Spark Scala-toepassingen maken](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Foutopsporing op afstand (video): [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand op een HDInsight-cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-hulpprogramma's](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens gebruiken](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure Toolkit voor IntelliJ gebruiken voor het maken van Spark-toepassingen voor een HDInsight-cluster](apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het Spark-toepassingen maken](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in een Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken bijhouden en fouten opsporen die worden uitgevoerd op een Apache Spark-cluster in HDInsight](apache-spark-job-debugging.md)
