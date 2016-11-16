---
title: 'Zelfstudie voor Apache Storm: aan de slag met Storm op basis van Linux in HDInsight | Microsoft Docs'
description: Aan de slag met big data-analyses met Apache Storm en de Storm Starter-voorbeelden in HDInsight op basis van Linux. Informatie over het gebruik van Storm om gegevens in realtime te verwerken.
keywords: apache storm, zelfstudie apache storm, big data-analyse, storm starter
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/12/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 84567b646e9cb5e95b2aaf5c3ce19fc227decc82


---
# <a name="apache-storm-tutorial-get-started-with-the-storm-starter-samples-for-big-data-analytics-on-hdinsight"></a>Zelfstudie voor Apache Storm: aan de slag met Storm Starter-voorbeelden voor big data-analyses in HDInsight
Apache Storm is een gedistribueerd, schaalbaar, fouttolerant en realtime berekeningssysteem voor het verwerken van gegevensstromen. Met Storm in Azure HDInsight kunt u een op een cloud gebaseerd Storm-cluster maken dat in realtime big data-analyses uitvoert.

> [!NOTE]
> Met de stappen in dit artikel maakt u een HDInsight-cluster op basis van Linux. Zie [Zelfstudie voor Apache Storm: aan de slag met het Storm Starter-voorbeeld met behulp van gegevensanalyses in HDInsight](hdinsight-apache-storm-tutorial-get-started.md) voor stappen voor het maken van een Storm-cluster in HDInsight op basis van Windows.
> 
> 

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Om deze Apache Storm-zelfstudie te voltooien, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Kennis van SSH en SCP**. Raadpleeg een van de volgende artikelen voor meer informatie over het gebruik van SSH en SCP met HDInsight:
  
  * **Linux-, Unix- en OS X-clients**: zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
  * **Windows-clients**: zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

### <a name="access-control-requirements"></a>Vereisten voor toegangsbeheer
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-storm-cluster"></a>Een Storm-cluster maken
In deze sectie maakt u een HDInsight versie 3.2-cluster (Storm versie 0.9.3) met behulp van een Azure Resource Manager-sjabloon. Zie [Versiebeheer van HDInsight-onderdelen](hdinsight-component-versioning.md) voor meer informatie over de versies van HDInsight en de bijbehorende SLA’s. Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor andere methoden voor het maken van clusters.

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-storm-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    De sjabloon bevindt zich in een openbare blobcontainer, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-storm-cluster-in-hdinsight.json*. 
2. Voer op de blade Parameters het volgende in:
   
   * **Clusternaam**: voer een naam in voor het Hadoop-cluster dat u maakt.
   * **Clusteraanmeldgegevens**: de standaardaanmeldnaam is admin.
   * **SSH-aanmeldgegevens**.
     
     Noteer deze waarden.  U hebt ze later in de zelfstudie nodig.
     
     > [!NOTE]
     > SSH wordt gebruikt voor externe toegang tot het HDInsight-cluster via een opdrachtregel. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt om via SSH verbinding te maken met het cluster. De SSH-gebruikersnaam moet bovendien uniek zijn, omdat hiermee een gebruikersaccount wordt gemaakt op alle knooppunten van het HDInsight-cluster. Hieronder volgt een aantal van de accountnamen die zijn gereserveerd voor gebruik door de services op het cluster en niet als SSH-gebruikersnaam kunnen worden gebruikt:
     > 
     > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.
     > 
     > Raadpleeg een van de volgende artikelen voor meer informatie over het gebruik van SSH met HDInsight:
     > 
     > * [SSH gebruiken met op Linux gebaseerde Hadoop op HDInsight vanaf Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
     > * [SSH gebruiken met op Linux gebaseerde Hadoop op HDInsight vanaf Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
     > 
     > 

3. Klik op **OK** om de parameters op te slaan.

4. Klik vanuit de blade **Aangepaste implementatie** op de vervolgkeuzelijst **Resourcegroep**. Klik vervolgens op **Nieuw** om een nieuwe resourcegroep te maken. De resourcegroep is een container waarin het cluster, het afhankelijke opslagaccount en andere gekoppelde resources zijn gegroepeerd.

5. Klik op **Juridische voorwaarden** en vervolgens op **Maken**.

6. Klik op **Maken**. U ziet een nieuwe tegel met de titel Implementatie indienen voor Sjabloonimplementatie. Het duurt ongeveer 20 minuten om het cluster en de SQL-database te maken.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Een Storm Starter-voorbeeld uitvoeren in HDInsight
De [Storm-starter](https://github.com/apache/storm/tree/master/examples/storm-starter)-voorbeelden zijn opgenomen in het HDInsight-cluster. In de volgende stappen voert u het WordCount-voorbeeld uit.

1. Maak verbinding met het HDInsight-cluster via SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Als u een wachtwoord hebt gebruikt om uw SSH gebruikersaccount te beveiligen, wordt u gevraagd het wachtwoord in te voeren. Als u een openbare sleutel hebt gebruikt, moet u mogelijk de `-i`-parameter gebruiken om de overeenkomende persoonlijke sleutel op te geven. Bijvoorbeeld `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
   
    Raadpleeg de volgende artikelen voor meer informatie over het gebruik van SSH met HDInsight op basis van Linux:
   
   * [SSH gebruiken met op Linux gebaseerde Hadoop op HDInsight vanaf Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
   * [SSH gebruiken met op Linux gebaseerde Hadoop op HDInsight vanaf Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
2. Gebruik de volgende opdracht om een voorbeeldtopologie te starten:
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar storm.starter.WordCountTopology wordcount
   
   > [!NOTE]
   > Het `*`-deel van de bestandsnaam komt steeds overeen met het versienummer. Dit wordt gewijzigd wanneer HDInsight wordt bijgewerkt.
   > 
   > 
   
    Hiermee wordt de WordCount-voorbeeldtopologie gestart op het cluster, met een beschrijvende naam voor 'wordcount'. Er worden willekeurig zinnen gegenereerd en het aantal keer dat elk woord in deze zinnen voorkomt, wordt geteld.
   
   > [!NOTE]
   > Bij het indienen van de topologie bij het cluster moet u eerst het jar-bestand met het cluster kopiëren voordat u de opdracht `storm` gebruikt. Dit kunt u doen met de opdracht `scp` vanaf de client waarop het bestand zich bevindt. Bijvoorbeeld: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   > 
   > Het WordCount-voorbeeld en andere Storm Starter-voorbeelden zijn al in uw cluster opgenomen in `/usr/hdp/current/storm-client/contrib/storm-starter/`.
   > 
   > 

## <a name="monitor-the-topology"></a>De topologie bewaken
De Storm-gebruikersinterface biedt een webinterface voor het werken met actieve topologieën en is opgenomen in uw HDInsight-cluster.

Voer de volgende stappen uit voor het bewaken van de topologie met behulp van de Storm-gebruikersinterface:

1. Open een webbrowser en ga naar https://CLUSTERNAAM.azurehdinsight.net/stormui, waarbij **CLUSTERNAAM** de naam van het cluster is. Hiermee wordt de Storm-gebruikersinterface geopend.
   
   > [!NOTE]
   > Als u wordt gevraagd een gebruikersnaam en een wachtwoord op te geven, voert u de gegevens voor de clusterbeheerder (admin) en het wachtwoord in die u hebt gebruikt toen u het cluster maakte.
   > 
   > 
2. Selecteer onder **Topology summary** de vermelding **wordcount** in de kolom **Name**. Hiermee wordt meer informatie over de topologie weergegeven.
   
    ![Storm-dashboard met informatie over de Storm Starter WordCount-topologie](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)
   
    Deze pagina bevat de volgende informatie:
   
   * **Topology stats**: basisinformatie over de topologieprestaties, geordend in tijdvensters.
     
     > [!NOTE]
     > Wanneer er een specifiek tijdvenster wordt geselecteerd, verandert het tijdvenster voor informatie die wordt weergegeven in andere gedeelten van de pagina.
     > 
     > 
   * **Spouts**: basisinformatie over spouts, met inbegrip van de laatste fout die door elke spout is geretourneerd.
   * **Bolts**: basisinformatie over bolts.
   * **Topology configuration**: gedetailleerde informatie over de topologieconfiguratie.
     
     Deze pagina bevat ook de acties die op de topologie kunnen worden uitgevoerd:
   * **Activate**: de verwerking van een gedeactiveerde topologie wordt hervat.
   * **Deactivate**: een actieve topologie wordt onderbroken.
   * **Rebalance**: de parallelle uitvoering van de topologie wordt aangepast. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u actieve topologieën opnieuw verdelen. Hiermee wordt de topologie aangepast aan parallelle uitvoering om te compenseren voor het toegenomen/afgenomen aantal knooppunten in het cluster. Zie [Inzicht in de parallelle uitvoering van een Storm-topologie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.
   * **Kill**: hiermee wordt een Storm-topologie na de opgegeven time-out beëindigd.
3. Selecteer op deze pagina een item in de sectie **Spouts** of **Bolts**. Hiermee wordt informatie over het geselecteerde onderdeel weergegeven.
   
    ![Storm-dashboard met informatie over de geselecteerde onderdelen.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)
   
    Deze pagina geeft de volgende informatie weer:
   
   * **Spout/Bolt stats**: basisinformatie over de prestaties van de diverse onderdelen, geordend in tijdvensters.
     
     > [!NOTE]
     > Wanneer er een specifiek tijdvenster wordt geselecteerd, verandert het tijdvenster voor informatie die wordt weergegeven in andere gedeelten van de pagina.
     > 
     > 
   * **Input stats** (alleen bolts): informatie over onderdelen die gegevens produceren die worden verbruikt door de bolt.
   * **Output stats**: informatie over gegevens die door deze bolt zijn gegenereerd.
   * **Executors**: informatie over exemplaren van dit onderdeel.
   * **Errors**: fouten die door dit onderdeel zijn gegenereerd.
4. Als u details voor een specifiek exemplaar van het onderdeel wilt weergeven, selecteert u een item in de kolom **Port** in de sectie **Executor** terwijl u de details van een spout of bolt bekijkt.
   
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]
   
    In deze gegevens kunt u zien dat het woord **seven** 1493957 keer voorkwam. Dat is het aantal keren dat het is gevonden sinds deze topologie is gestart.

## <a name="stop-the-topology"></a>De topologie stoppen
Ga terug naar de pagina **Topology summary** voor de word-count-topologie en klik vervolgens op de knop **Kill** in de sectie **Topology actions**. Wanneer hierom wordt gevraagd, voert u 10 in voor het aantal seconden dat moet worden gewacht voordat de topologie wordt gestopt. Na de time-outperiode wordt de topologie niet meer weergegeven als u de sectie **Storm UI** van het dashboard bezoekt.

## <a name="delete-the-cluster"></a>Het cluster verwijderen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="a-idnextanext-steps"></a><a id="next"></a>Volgende stappen
In deze Apache Storm-zelfstudie hebt u de Storm Starter gebruikt voor informatie over het maken van een Storm-cluster in HDInsight. Ook hebt u het Storm-dashboard gebruikt om Storm-topologieën te implementeren, te bewaken en te beheren. U kunt nu gaan leren hoe u [op Java gebaseerde topologieën met Maven ontwikkelt](hdinsight-storm-develop-java-topology.md).

Als u al bekend bent met het ontwikkelen van op Java gebaseerde topologieën en een bestaande topologie wilt implementeren in HDInsight, raadpleegt u [Apache Storm-topologieën implementeren en beheren in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).

Als u .NET-ontwikkelaar bent, kunt u C#- of hybride C#-/Java-topologieën maken met Visual Studio. Zie [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Hadoop-hulpprogramma's voor Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

Zie voor voorbeeldtopologieën die kunnen worden gebruikt met Storm op HDInsight de volgende voorbeelden:

    * [Voorbeeldtopologieën van Storm op HDInsight](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/



<!--HONumber=Nov16_HO2-->


