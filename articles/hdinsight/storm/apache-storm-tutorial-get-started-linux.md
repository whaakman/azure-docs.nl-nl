---
title: Storm-Starter-voorbeelden voor Apache Storm in HDInsight - Azure | Microsoft Docs
description: Lees hoe u big data kunt analyseren en gegevens in realtime kunt verwerken met Apache Storm in HDInsight.
keywords: storm-starter, apache storm voorbeeld
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
ms.date: 12/05/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 19ab428913517e4f3df156c93782fe23f1cd67ec
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-the-storm-starter-examples"></a>Aan de slag met Apache Storm in HDInsight met behulp van Storm-Starter-voorbeelden

Leer hoe u Apache Storm gebruikt in HDInsight aan de hand van de Storm-Starter-voorbeelden.

Apache Storm is een gedistribueerd, schaalbaar, fouttolerant en realtime berekeningssysteem voor het verwerken van gegevensstromen. Met Storm in Azure HDInsight kunt u een op een cloud gebaseerd Storm-cluster maken dat in realtime big data-analyses uitvoert.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Kennis van SSH en SCP**. Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

## <a name="create-a-storm-cluster"></a>Een Storm-cluster maken

Gebruik de volgende stappen om een Storm in een HDInsight-cluster te maken:

1. In de [Azure-portal](https://portal.azure.com) selecteert u **+ NIEUW**, **Gegevens en analyses** en vervolgens **HDInsight**.

    ![Een HDInsight-cluster maken](./media/apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. Geef in de sectie **Basisbeginselen** de volgende gegevens op:

    * **Clusternaam**: de naam van het HDInsight-cluster.
    * **Abonnement**: selecteer het abonnement dat u wilt gebruiken.
    * **Aanmeldingsgebruikersnaam** en -**wachtwoord** van cluster: de aanmeldingsgegevens voor toegang tot het cluster via HTTPS. U gebruikt deze referenties voor toegang tot services als de Ambari-webinterface of de REST-API.
    * **SSH-gebruikersnaam (Secure Shell)**: de aanmeldingsgegevens voor toegang tot het cluster via SSH. Het wachtwoord is standaard hetzelfde als het aanmeldingswachtwoord van het cluster.
    * **Resourcegroep**: de resourcegroep waarin het cluster wordt gemaakt.
    * **Locatie**: de Azure-regio waarin het cluster wordt gemaakt.

   ![Abonnement selecteren](./media/apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. Selecteer **Clustertype** en stel de volgende waarden in op de sectie **Clusterconfiguratie**:

    * **Clustertype**: Storm

    * **Besturingssysteem**: Linux

    * **Versie**: Storm 1.1.0 (HDI 3.6)

    * **Clusterlaag**: standaard

   Gebruik ten slotte de knop **Selecteren** om de instellingen op te slaan.

    ![Clustertype selecteren](./media/apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. Nadat u het clustertype hebt geselecteerd, gebruikt u de knop __Selecteren__ om het clustertype te selecteren. Gebruik vervolgens de knop __Volgende__ om de basisconfiguratie te voltooien.

5. In de sectie **Opslag** selecteert of maakt u een opslagaccount. Voor de stappen in dit document gebruikt u voor de andere velden in deze sectie de standaardwaarden. Gebruik de knop __Volgende__ om de opslagconfiguratie op te slaan.

    ![De instellingen van het opslagaccount voor HDInsight configureren](./media/apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. Controleer in de sectie **Samenvatting** de configuratie van het cluster. Gebruik de koppeling __Bewerken__ om onjuiste instellingen te wijzigen. Gebruik tot slot de knop __Maken__ om het cluster te maken.

    ![Samenvatting clusterconfiguratie](./media/apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > Het kan tot 20 minuten duren om het cluster te maken.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Een Storm Starter-voorbeeld uitvoeren in HDInsight

1. Maak verbinding met het HDInsight-cluster via SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!TIP]
    > De SSH-client geeft mogelijk aan dat de authenticiteit van de host niet kan worden vastgesteld. Als dat het geval is, voert u `yes` in om verder te gaan.

    > [!NOTE]
    > Als u een wachtwoord hebt gebruikt om uw SSH gebruikersaccount te beveiligen, wordt u gevraagd het wachtwoord in te voeren. Als u een openbare sleutel hebt gebruikt, moet u mogelijk de parameter `-i` gebruiken om de overeenkomende persoonlijke sleutel op te geven. Bijvoorbeeld `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

    Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Gebruik de volgende opdracht om een voorbeeldtopologie te starten:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    Met deze opdracht wordt de WordCount-voorbeeldtopologie gestart op het cluster. Deze topologie genereert willekeurige zinnen en telt hoe vaak woorden voorkomen. De beschrijvende naam van de topologie is `wordcount`.

    > [!NOTE]
    > Bij het indienen van uw eigen topologieën bij het cluster moet u eerst het JAR-bestand met het cluster kopiëren voordat u de opdracht `storm` gebruikt. Gebruik de opdracht `scp` om het bestand te kopiëren. Bijvoorbeeld: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Het WordCount-voorbeeld en andere Storm Starter-voorbeelden zijn al in uw cluster opgenomen op `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Wilt u de bron van de Storm Starter-voorbeelden bekijken? U vindt de code hier: [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter). Deze koppeling is voor Storm 1.1.x, geleverd bij HDInsight 3.6. Gebruik voor andere versies van Storm de knop __Vertakking__ boven aan de pagina om een andere Storm-versie te selecteren.

## <a name="monitor-the-topology"></a>De topologie bewaken

De Storm-gebruikersinterface biedt een webinterface voor het werken met actieve topologieën en is opgenomen in uw HDInsight-cluster.

Voer de volgende stappen uit voor het bewaken van de topologie met behulp van de Storm-gebruikersinterface:

1. Als u de Storm-gebruikersinterface wilt weergeven, opent u `https://CLUSTERNAME.azurehdinsight.net/stormui` in een webbrowser. Vervang **CLUSTERNAME** door de naam van uw cluster.

    > [!NOTE]
    > Als u wordt gevraagd een gebruikersnaam en een wachtwoord op te geven, voert u de gegevens voor de clusterbeheerder (admin) en het wachtwoord in die u hebt gebruikt toen u het cluster maakte.

2. Selecteer onder **Topology summary** de vermelding **wordcount** in de kolom **Name**. Er wordt informatie over de topologie weergegeven.

    ![Storm-dashboard met informatie over de Storm-Starter WordCount-topologie.](./media/apache-storm-tutorial-get-started-linux/topology-summary.png)

    Deze pagina bevat de volgende informatie:

    * **Topology stats**: basisinformatie over de topologieprestaties, geordend in tijdvensters.

        > [!NOTE]
        > Wanneer er een specifiek tijdvenster wordt geselecteerd, verandert het tijdvenster voor informatie die wordt weergegeven in andere gedeelten van de pagina.

    * **Spouts**: basisinformatie over spouts, met inbegrip van de laatste fout die door elke spout is geretourneerd.

    * **Bolts**: basisinformatie over bolts.

    * **Topology configuration**: gedetailleerde informatie over de topologieconfiguratie.

    Deze pagina bevat ook de acties die op de topologie kunnen worden uitgevoerd:

    * **Activate**: de verwerking van een gedeactiveerde topologie wordt hervat.

    * **Deactivate**: een actieve topologie wordt onderbroken.

    * **Rebalance**: de parallelle uitvoering van de topologie wordt aangepast. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u actieve topologieën opnieuw verdelen. Met het opnieuw verdelen wordt de parallelle uitvoering aangepast om te compenseren voor het toegenomen/afgenomen aantal knooppunten in het cluster. Zie [Inzicht in de parallelle uitvoering van een Storm-topologie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    * **Kill**: hiermee wordt een Storm-topologie na de opgegeven time-out beëindigd.

3. Selecteer op deze pagina een item in de sectie **Spouts** of **Bolts**. Er wordt informatie over het geselecteerde onderdeel weergegeven.

    ![Storm-dashboard met informatie over de geselecteerde onderdelen.](./media/apache-storm-tutorial-get-started-linux/component-summary.png)

    Deze pagina geeft de volgende informatie weer:

    * **Spout/Bolt stats**: basisinformatie over de prestaties van de diverse onderdelen, geordend in tijdvensters.

        > [!NOTE]
        > Wanneer er een specifiek tijdvenster wordt geselecteerd, verandert het tijdvenster voor informatie die wordt weergegeven in andere gedeelten van de pagina.

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

    In dit voorbeeld komt het woord **seven** 1.493.957 keer voor. Dit getal is het aantal keer dat het woord is gevonden sinds deze topologie is gestart.

## <a name="stop-the-topology"></a>De topologie stoppen

Ga terug naar de pagina **Topology summary** voor de word-count-topologie en klik vervolgens op de knop **Kill** in de sectie **Topology actions**. Wanneer hierom wordt gevraagd, voert u 10 in voor het aantal seconden dat moet worden gewacht voordat de topologie wordt gestopt. Na de time-outperiode wordt de topologie niet meer weergegeven als u het gedeelte **Storm UI** van het dashboard bezoekt.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Zie [Vereisten voor toegangsbeheer](../hdinsight-administer-use-portal-linux.md#create-clusters) als u een probleem ondervindt met het maken van een HDInsight-cluster.

## <a id="next"></a>Volgende stappen

In deze zelfstudie voor Apache Storm hebt u de basisbeginselen van het werken met Storm op HDInsight geleerd. U kunt nu gaan leren hoe u [op Java gebaseerde topologieën met Maven ontwikkelt](apache-storm-develop-java-topology.md).

Als u al bekend bent met het ontwikkelen van op Java gebaseerde topologieën, raadpleegt u het document [Apache Storm-topologieën implementeren en beheren in HDInsight](apache-storm-deploy-monitor-topology-linux.md).

Als u .NET-ontwikkelaar bent, kunt u C#- of hybride C#-/Java-topologieën maken met Visual Studio. Zie [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Hadoop-hulpprogramma's voor Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

Zie voor voorbeeldtopologieën die kunnen worden gebruikt met Storm op HDInsight de volgende voorbeelden:

* [Voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
