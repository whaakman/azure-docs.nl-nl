---
title: 'Azure Toolkit voor IntelliJ: foutopsporing op afstand in HDInsight Spark-toepassingen | Microsoft Docs'
description: Meer informatie over hoe HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ gebruiken voor foutopsporing op afstand Spark scala-toepassingen die worden uitgevoerd op HDInsight-clusters via VPN-verbinding.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 3a673d2ad58f85f5437c378a1d05aa4963893df1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-azure-toolkit-for-intellij-to-debug-spark-applications-remotely-in-hdinsight-through-vpn"></a>Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand in HDInsight via VPN-verbinding

Het is raadzaam om foutopsporing op afstand via SSH Spark scala-toepassingen. Zie voor instructies [op afstand fouten opsporen in Spark scala-toepassingen op een HDInsight-cluster in Azure werkset voor IntelliJ via SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Dit artikel bevat stapsgewijze instructies voor het HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ gebruiken voor het verzenden van een Spark-taak op een HDInsight Spark-cluster en op afstand debug vanaf uw computer. Om deze taken hebt voltooid, moet u de volgende geavanceerde stappen uitvoeren:

1. Maak een site-naar-site of een punt-naar-site virtueel Azure-netwerk. De stappen in dit document wordt ervan uitgegaan dat u een site-naar-site-netwerk.
2. Maak een Spark-cluster in HDInsight die deel uitmaakt van het virtuele netwerk van de site-naar-site.
3. Controleer of de verbinding tussen het hoofdknooppunt van het cluster en uw bureaublad.
4. Een toepassing Scala in IntelliJ IDEA maken en vervolgens configureren voor foutopsporing op afstand.
5. Voer en fouten opsporen in de toepassing.

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie voor meer informatie [ophalen van een gratis proefversie van Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een Apache Spark-cluster in HDInsight**. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* **Oracle Java development kit**. U kunt installeren vanuit de [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* **IntelliJ IDEA**. In dit artikel gebruikt versie 2017.1. U kunt installeren vanuit de [JetBrains website](https://www.jetbrains.com/idea/download/).
* **HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ**. HDInsight tools voor IntelliJ beschikbaar zijn als onderdeel van Azure Toolkit voor IntelliJ. Zie voor instructies over het installeren van Azure Toolkit [Azure Toolkit installeren voor IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation).
* **Aanmelden bij uw Azure-abonnement vanuit IntelliJ IDEA**. Volg de instructies in [gebruik Azure Toolkit voor IntelliJ Spark om toepassingen te maken voor een HDInsight-cluster](apache-spark-intellij-tool-plugin.md).
* **Tijdelijke oplossing uitzondering**. Tijdens het uitvoeren van de Spark Scala-toepassing voor foutopsporing op afstand op een Windows-computer, krijgt u mogelijk een uitzondering. Deze uitzondering wordt uitgelegd in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) en deze gebeurtenis treedt op als gevolg van een ontbrekend bestand WinUtils.exe in Windows. U kunt deze fout omzeilen, moet u [downloaden van het uitvoerbare bestand](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**. Voeg een **HADOOP_HOME** omgevingsvariabele, en stel de waarde van de variabele **C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>Stap 1: Een Azure-netwerk maken
Volg de instructies van de volgende koppelingen voor het maken van een virtuele Azure-netwerk en controleer vervolgens de connectiviteit tussen uw computer en het virtuele netwerk:

* [Maak een VNet met een site-naar-site VPN-verbinding met de Azure portal](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Maak een VNet met een site-naar-site VPN-verbinding met behulp van PowerShell](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Een punt-naar-site-verbinding met een virtueel netwerk met behulp van PowerShell configureren](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>Stap 2: Een HDInsight Spark-cluster maken
U wordt aangeraden dat u ook een Apache Spark-cluster maken in Azure HDInsight die deel uitmaakt van het Azure-netwerk dat u hebt gemaakt. Gebruik de informatie die beschikbaar zijn in [clusters in HDInsight op basis van Linux maken](../hdinsight-hadoop-provision-linux-clusters.md). Selecteer het Azure-netwerk dat u in de vorige stap hebt gemaakt als onderdeel van de optionele configuratie.

## <a name="step-3-verify-the-connectivity-between-the-cluster-head-node-and-your-desktop"></a>Stap 3: De connectiviteit tussen het hoofdknooppunt van het cluster en uw bureaublad controleren
1. Het IP-adres van het hoofdknooppunt worden opgehaald. Open Ambari UI voor het cluster. Selecteer in de cluster-blade **Dashboard**.

    ![Dashboard in Ambari selecteren](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/launch-ambari-ui.png)
2. Selecteer in de UI Ambari **Hosts**.

    ![Selecteer de Hosts in Ambari](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/ambari-hosts.png)
3. U ziet een lijst met hoofdknooppunten worker-knooppunten en zookeeper-knooppunten. De hoofdknooppunten hebben een **hn*** voorvoegsel. Selecteer de eerste hoofdknooppunt.

    ![Het hoofdknooppunt in Ambari zoeken](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/cluster-headnodes.png)
4. Van de **samenvatting** deelvenster onderaan de pagina die wordt geopend, Kopieer de **IP-adres** van het hoofdknooppunt en de **hostnaam**.

    ![Het IP-adres vinden in Ambari](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/headnode-ip-address.png)
5. Het IP-adres en de hostnaam van het hoofdknooppunt te toevoegen de **hosts** bestand op de computer waarop u wilt uitvoeren en op afstand fouten opsporen in de Spark-taak. Hiermee kunt u communiceren met het hoofdknooppunt met behulp van het IP-adres, evenals de hostnaam.

   a. Open een Kladblok-bestand met verhoogde machtigingen. Van de **bestand** selecteert u **Open**, en gaat u naar de locatie van het hosts-bestand. Op een Windows-computer, de locatie is **C:\Windows\System32\Drivers\etc\hosts**.

   b. Voeg de volgende informatie op de **hosts** bestand:

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. Controleer vanaf de computer waarmee u verbonden met het Azure-netwerk dat wordt gebruikt door het HDInsight-cluster, of dat u de hoofdknooppunten pingen kunt met behulp van het IP-adres, evenals de hostnaam.
7. SSH gebruiken voor verbinding met het hoofdknooppunt van het cluster door de instructies in [verbinding maken met een HDInsight-cluster via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md). Ping het IP-adres van de desktopcomputer van het hoofdknooppunt van de cluster. Test de connectiviteit aan beide IP-adressen toegewezen aan de computer:

    - Één voor de netwerkverbinding
    - Één voor de virtuele Azure-netwerk

8. Herhaal de stappen voor het hoofdknooppunt.

## <a name="step-4-create-a-spark-scala-application-by-using-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>Stap 4: Een Spark Scala-toepassing met behulp van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ maken en configureren voor foutopsporing op afstand
1. Open IntelliJ IDEA en maak een nieuw project. In de **nieuw Project** dialoogvenster de volgende handelingen uit:

    ![Selecteer het nieuwe projectsjabloon in IntelliJ IDEA](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-hdi-scala-app.png)

    a. Selecteer **HDInsight** > **Spark in HDInsight (Scala)**.

    b. Selecteer **volgende**.
2. In de volgende **nieuw Project** in het dialoogvenster de volgende handelingen uit en selecteer vervolgens **voltooien**:

    - Voer een naam en locatie.

    - In de **Project SDK** vervolgkeuzelijst, selecteer **Java 1.8** voor het Spark-cluster voor 2.x of selecteer **Java 1.7** voor de 1.x Spark-cluster.

    - In de **Spark versie** vervolgkeuzelijst, de wizard Scala project maken voor de Spark-SDK en de Scala SDK de juiste versie kan worden geïntegreerd. Als de Spark-cluster versie ouder dan 2.0 is, selecteert u **Spark 1.x**. Selecteer anders **Spark2.x**. In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)**.
  
   ![Selecteer de project-SDK en Spark versie](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-scala-project-details.png)
  
3. Het Spark-project wordt automatisch een artefact voor u gemaakt. Als u wilt weergeven van het artefact, het volgende doen:

    a. Van de **bestand** selecteert u **projectstructuur**.

    b. In de **projectstructuur** dialoogvenster, **artefacten** om weer te geven van de standaard-artefacten die is gemaakt. U kunt ook uw eigen artefacten maken door het plusteken (**+**).

   ![JAR maken](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/default-artifact.png)


4. Bibliotheken toevoegen aan uw project. Als u wilt toevoegen in een bibliotheek, het volgende doen:

    a. Met de rechtermuisknop op de naam van het project in de projectstructuur van het en selecteer vervolgens **Open Module-instellingen**. 

    b. In de **projectstructuur** dialoogvenster, **bibliotheken**, selecteer de (**+**) symbool en selecteer vervolgens **van Maven** .

    ![Een bibliotheek toevoegen](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/add-library.png)

    c. In de **bibliotheek downloaden uit de opslagplaats Maven** dialoogvenster vak, zoeken en toevoegen van de volgende bibliotheken:

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. Kopiëren `yarn-site.xml` en `core-site.xml` uit het cluster head knooppunt en toe te voegen aan het project. Gebruik de volgende opdrachten om de bestanden te kopiëren. U kunt [Cygwin](https://cygwin.com/install.html) om uit te voeren van de volgende `scp` hoofdknooppunten opdrachten voor het kopiëren van het cluster:

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Omdat er al toegevoegd voor het cluster hoofdknooppunt IP-adres en de hostnamen voor het bestand hosts op het bureaublad, kunnen we gebruiken de `scp` opdrachten in de volgende manier:

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Deze bestanden toevoegen aan uw project, kopieert u deze onder de **/src** map in de projectstructuur van uw, bijvoorbeeld `<your project directory>\src`.
6. Update de `core-site.xml` bestand de volgende wijzigingen aanbrengen:

   a. Vervang de versleutelde sleutel. De `core-site.xml` bestand bevat de versleutelde sleutel voor het opslagaccount die is gekoppeld aan het cluster. In de `core-site.xml` -bestand dat u hebt toegevoegd aan het project, de versleutelde sleutel vervangen door de werkelijke opslagsleutel die zijn gekoppeld aan het standaardopslagaccount. Zie voor meer informatie [beheren van uw toegangssleutels voor opslag](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   b. Verwijder de volgende vermeldingen van `core-site.xml`:

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
7. De belangrijkste klasse voor uw toepassing toevoegen. Van de **Projectverkenner**, met de rechtermuisknop op **src**, wijs **nieuw**, en selecteer vervolgens **Scala klasse**.

    ![Selecteer de belangrijkste klasse](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code.png)
8. In de **nieuwe Scala klasse maken** dialoogvenster vak, Geef een naam op, selecteer **Object** in de **soort** vak en selecteer vervolgens **OK**.

    ![Maak nieuwe Scala-klasse](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/hdi-spark-scala-code-object.png)
9. In de `MyClusterAppMain.scala` bestand, plak de volgende code. Deze code maakt de Spark context en opent een `executeJob` methode van de `SparkSample` object.

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

10. Herhaal stap 8 en 9 toevoegen van een nieuwe Scala-object aangeroepen `*SparkSample`. Voeg de volgende code toe aan deze klasse. Deze code leest de gegevens van de HVAC.csv (beschikbaar in alle HDInsight Spark-clusters). Deze worden de rijen die slechts één cijfer in de zevende kolom in het CSV-bestand hebben en schrijft u de uitvoer naar **/HVACOut** onder de standaardcontainer opslag voor het cluster.

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
11. Herhaal stap 8 en 9 toevoegen van een nieuwe klasse met de naam `RemoteClusterDebugging`. Deze klasse implementeert het Spark-test-framework dat wordt gebruikt voor foutopsporing van de toepassingen. Voeg de volgende code naar de `RemoteClusterDebugging` klasse:

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

     Er zijn een aantal belangrijke opmerkingen:

      * Voor `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, zorg ervoor dat de assembly Spark JAR is beschikbaar in de clusteropslag in het opgegeven pad.
      * Voor `setJars`, geef de locatie waar het artefact JAR wordt gemaakt. Dit is meestal `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
12. In de`*RemoteClusterDebugging` klasse, met de rechtermuisknop op de `test` sleutelwoord en selecteer vervolgens **RemoteClusterDebugging configuratie maken**.

    ![Maken van een externe configuratie](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-remote-config.png)

13. In de **RemoteClusterDebugging configuratie maken** in het dialoogvenster een naam opgeven voor de configuratie en selecteer vervolgens **testen soort** als de **testnaam**. Laat de waarden als de standaardinstellingen. Selecteer **toepassen**, en selecteer vervolgens **OK**.

    ![De configuratiegegevens van de toevoegen](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/provide-config-value.png)
14. U ziet nu een **extern uitvoeren** configuratie vervolgkeuzelijst in de menubalk.

    ![De externe, voer vervolgkeuzelijst](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/config-run.png)

## <a name="step-5-run-the-application-in-debug-mode"></a>Stap 5: De toepassing uitvoeren in de foutopsporingsmodus
1. Open in uw project IntelliJ IDEA `SparkSample.scala` en maak een onderbrekingspunt naast `val rdd1`. In de **onderbrekingspunt maken voor** pop-upmenu Selecteer **regel in de functie executeJob**.

    ![Een onderbrekingspunt toevoegen](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/create-breakpoint.png)
2. Voor het uitvoeren van de toepassing, selecteer de **foutopsporing uitvoeren** naast de **extern uitvoeren** vervolgkeuzelijst configuratie.

    ![Selecteer de knop voor foutopsporing uitvoeren](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-run-mode.png)
3. Wanneer de programma-uitvoering het onderbrekingspunt bereikt, ziet u een **foutopsporingsprogramma** tabblad in het onderste deelvenster.

    ![Raadpleeg het tabblad foutopsporingsprogramma](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch.png)
4. Als u wilt een controle wilt toevoegen, selecteert u de (**+**) pictogram.

    ![Selecteer het pictogram +](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable.png)

    In dit voorbeeld wordt de toepassing heeft voordat de variabele `rdd1` is gemaakt. Met behulp van deze controle, ziet u de eerste vijf rijen in de variabele `rdd`. Selecteer **Voer**.

    ![Voer het programma in de foutopsporingsmodus](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-add-watch-variable-value.png)

    Wat u ziet in de vorige afbeelding is dat terabytes aan gegevens en foutopsporing tijdens runtime kan opvragen hoe de voortgang van uw toepassing. Bijvoorbeeld, in de uitvoer wordt weergegeven in de vorige afbeelding ziet u dat de eerste rij van de uitvoer een koptekst is. Op basis van deze uitvoer, kunt u uw toepassingscode om over te slaan van de rij met kolomkoppen, indien nodig wijzigen.
5. U kunt nu selecteren de **hervatten programma** pictogram om door te gaan met uw toepassing uitvoeren.

    ![Selecteer programma hervatten](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-continue-run.png)
6. Als de toepassing voltooid wordt, ziet u de volgende uitvoer:

    ![Console-uitvoer](./media/apache-spark-intellij-tool-plugin-debug-jobs-remotely/debug-complete.png)

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
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark-toepassingen op afstand via SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in een Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Traceren en er fouten taken die worden uitgevoerd op een Apache Spark-cluster in HDInsight](apache-spark-job-debugging.md)
