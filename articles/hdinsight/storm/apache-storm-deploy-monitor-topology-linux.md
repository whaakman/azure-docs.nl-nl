---
title: "Implementeren en beheren van Apache Storm-topologieën op Linux gebaseerde HDInsight | Microsoft Docs"
description: "Informatie over het implementeren, bewaken en beheren van Apache Storm-topologieën met het Storm-Dashboard op Linux gebaseerde HDInsight. Hadoop-hulpprogramma's voor Visual Studio gebruiken."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: larryfr
ms.openlocfilehash: a972344e2b6205fbcf69d2969c42211ec5b24869
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a>Implementeren en beheren van Apache Storm-topologieën op HDInsight

Informatie over de basisprincipes van beheer en controle van Storm-topologieën uitgevoerd op Storm op HDInsight-clusters in dit document.

> [!IMPORTANT]
> De stappen in dit artikel moet een op Linux gebaseerde Storm op HDInsight-cluster. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. 
>
> Zie voor informatie over het implementeren en bewaken van topologieën op HDInsight op basis van Windows, [implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Windows](apache-storm-deploy-monitor-topology.md)


## <a name="prerequisites"></a>Vereisten

* **Een op Linux gebaseerde Storm op HDInsight-cluster**: Zie [aan de slag met Apache Storm op HDInsight](apache-storm-tutorial-get-started-linux.md) voor stapsgewijze instructies voor het maken van een cluster

* (Optioneel) **Kennis van SSH en SCP**: Zie voor meer informatie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

* (Optioneel) **Visual Studio**: Azure SDK 2.5.1 of hoger en de Data Lake Tools voor Visual Studio. Zie voor meer informatie [aan de slag met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    Een van de volgende versies van Visual Studio:

  * Visual Studio 2012 met [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

  * Visual Studio 2013 met [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) of [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/)

  * Visual Studio 2015 (alle versies)

  * Visual Studio 2017 (alle versies). Data Lake Tools voor Visual Studio 2017 worden geïnstalleerd als onderdeel van de werkbelasting van Azure.

## <a name="submit-a-topology-visual-studio"></a>Verzenden van een topologie: Visual Studio

De HDInsight Tools kan worden gebruikt om in te dienen C# of hybride topologieën naar uw Storm-cluster. De volgende stappen gebruikt een voorbeeld van een toepassing. Zie voor meer informatie over het maken van over het gebruik van de HDInsight Tools [C#-topologieën ontwikkelen met HDInsight Tools voor Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

1. Als u hebt niet is gebeurd de nieuwste versie van het Data Lake tools voor Visual Studio, Zie [aan de slag met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]
    > De Data Lake Tools voor Visual Studio werden voorheen de HDInsight Tools voor Visual Studio.
    >
    > Data Lake Tools voor Visual Studio zijn opgenomen in de __Azure werkbelasting__ voor Visual Studio 2017.

2. Open Visual Studio, selecteer **bestand** > **nieuw** > **Project**.

3. In de **nieuw Project** dialoogvenster Vouw **geïnstalleerde** > **sjablonen**, en selecteer vervolgens **HDInsight**. Selecteer in de lijst met sjablonen **Storm voorbeeld**. Typ een naam voor de toepassing aan de onderkant van het dialoogvenster.

    ![Afbeelding](./media/apache-storm-deploy-monitor-topology-linux/sample.png)

4. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **indienen Storm op HDInsight**.

   > [!NOTE]
   > Als u wordt gevraagd, voert u de aanmeldingsreferenties voor uw Azure-abonnement. Als u meer dan één abonnement hebt, moet u zich aanmelden bij de database met uw Storm op HDInsight-cluster.

5. Selecteer uw Storm op HDInsight-cluster van de **Storm-Cluster** vervolgkeuzelijst en selecteer vervolgens **indienen**. U kunt controleren of het verzenden voltooid met is de **uitvoer** venster.

## <a name="submit-a-topology-ssh-and-the-storm-command"></a>Verzenden van een topologie: SSH en de Storm-opdracht

1. SSH gebruiken voor verbinding met het HDInsight-cluster. Vervang **gebruikersnaam** de naam van uw SSH-aanmelding. Vervang **CLUSTERNAME** met de naam van uw HDInsight-cluster:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Zie voor meer informatie over het gebruik van SSH verbinding maken met uw HDInsight-cluster [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Gebruik de volgende opdracht om een voorbeeldtopologie te starten:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    Met deze opdracht wordt de WordCount-voorbeeldtopologie gestart op het cluster. Deze topologie willekeurig zinnen gegenereerd en vervolgens het exemplaar van elk woord in de zinnen telt.

   > [!NOTE]
   > Bij het indienen van de topologie bij het cluster moet u eerst het jar-bestand met het cluster kopiëren voordat u de opdracht `storm` gebruikt. Als u wilt kopiëren van het bestand aan het cluster, kunt u de `scp` opdracht. Bijvoorbeeld: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   >
   > Het WordCount-voorbeeld en andere Storm Starter-voorbeelden zijn al in uw cluster opgenomen in `/usr/hdp/current/storm-client/contrib/storm-starter/`.

## <a name="submit-a-topology-programmatically"></a>Verzenden van een topologie: programmatisch

U kunt een topologie met behulp van de service Nimbus programmatisch implementeren. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) bevat een voorbeeld van Java-toepassing die laat zien hoe u implementeert en een topologie via de Nimbus-service start.

## <a name="monitor-and-manage-visual-studio"></a>Bewaken en beheren: Visual Studio

Wanneer een topologie wordt ingediend met Visual Studio de **Storm-topologieën** weergegeven. Selecteer de topologie in de lijst om informatie over de actieve topologie weer te geven.

![monitor voor Visual studio](./media/apache-storm-deploy-monitor-topology-linux/vsmonitor.png)

> [!NOTE]
> U kunt ook weergeven **Storm-topologieën** van **Server Explorer** door het uitbreiden van **Azure** > **HDInsight**, en klik vervolgens met de rechtermuisknop op een Storm op HDInsight-cluster en het selecteren van **weergave Storm-topologieën**.

Selecteer de vorm voor de spouts of bolts om informatie over deze onderdelen weer te geven. Er wordt een nieuw venster geopend voor elk item geselecteerd.

### <a name="deactivate-and-reactivate"></a>Deactiveren en opnieuw activeren

Een topologie deactiveren onderbreekt het totdat dit is afgesloten of opnieuw geactiveerd. Gebruik voor deze bewerkingen uitvoeren, de __deactiveren__ en __opnieuw activeren__ knoppen aan de bovenkant van de __Topology Summary__.

### <a name="rebalance"></a>Opnieuw verdelen

Een topologie herverdeling kan het systeem voor het bijwerken van de parallelle uitvoering van de topologie. Als het formaat van het cluster om toe te voegen meer notities kan herverdeling bijvoorbeeld een topologie voor een overzicht van de nieuwe knooppunten.

Als u wilt opnieuw verdelen een topologie, gebruiken de __opnieuw verdelen__ knop aan de bovenkant van de __Topology Summary__.

> [!WARNING]
> Eerst een topologie herverdeling deactiveert u de topologie opnieuw werknemers gelijkmatig verdeeld over het cluster vervolgens, de topologie tot slot wordt de status was voordat herverdeling is opgetreden. Dus als de topologie actief was, wordt het actieve opnieuw. Als deze is uitgeschakeld, blijft deze gedeactiveerd.

### <a name="kill-a-topology"></a>Een topologie voor afsluiten

Storm-topologieën blijft in uitvoering totdat ze zijn gestopt of het cluster wordt verwijderd. Voor het beëindigen van een topologie gebruikt de __Kill__ knop aan de bovenkant van de __Topology Summary__.

## <a name="monitor-and-manage-ssh-and-the-storm-command"></a>Bewaken en beheren: SSH en de Storm-opdracht

De `storm` hulpprogramma kunt u werken met actieve topologieën vanaf de opdrachtregel. Gebruik `storm -h` voor een volledige lijst met opdrachten.

### <a name="list-topologies"></a>Lijst met topologieën

Gebruik de volgende opdracht om een lijst weer met alle actieve topologieën:

    storm list

Met deze opdracht wordt informatie geretourneerd die lijkt op de volgende tekst:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deactiveren en opnieuw activeren

Een topologie deactiveren onderbreekt het totdat dit is afgesloten of opnieuw geactiveerd. Gebruik de volgende opdracht om te deactiveren en opnieuw activeren:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Voor het afsluiten van een actieve topologie

Storm-topologieën, na het starten, gaat u verder uitgevoerd totdat deze wordt gestopt. Als u wilt een topologie stoppen, moet u de volgende opdracht gebruiken:

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a>Opnieuw verdelen

Een topologie herverdeling kan het systeem voor het bijwerken van de parallelle uitvoering van de topologie. Als het formaat van het cluster om toe te voegen meer notities kan herverdeling bijvoorbeeld een topologie voor een overzicht van de nieuwe knooppunten.

> [!WARNING]
> Eerst een topologie herverdeling deactiveert u de topologie opnieuw werknemers gelijkmatig verdeeld over het cluster vervolgens, de topologie tot slot wordt de status was voordat herverdeling is opgetreden. Dus als de topologie actief was, wordt het actieve opnieuw. Als deze is uitgeschakeld, blijft deze gedeactiveerd.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a>Bewaken en beheren: Storm-gebruikersinterface

De Storm-gebruikersinterface biedt een webinterface voor het werken met actieve topologieën en is opgenomen in uw HDInsight-cluster. Om weer te geven de Storm-gebruikersinterface, gebruikt u een webbrowser openen **https://CLUSTERNAME.azurehdinsight.NET/stormui**, waarbij **CLUSTERNAME** is de naam van uw cluster.

> [!NOTE]
> Als u wordt gevraagd een gebruikersnaam en een wachtwoord op te geven, voert u de gegevens voor de clusterbeheerder (admin) en het wachtwoord in die u hebt gebruikt toen u het cluster maakte.

### <a name="main-page"></a>Hoofdpagina

De hoofdpagina van de Storm-gebruikersinterface biedt de volgende informatie:

* **Cluster samenvatting**: algemene informatie over de Storm-cluster.
* **Topologie samenvatting**: een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke topologieën weergeven.
* **Supervisor samenvatting**: informatie over de supervisor Storm.
* **Nimbus configuratie**: Nimbus-configuratie voor het cluster.

### <a name="topology-summary"></a>Topologie samenvatting

Als u een koppeling van de **Topology summary** sectie vindt u de volgende informatie over de topologie:

* **Topologie samenvatting**: algemene informatie over de topologie.
* **Topologie acties**: acties die u voor de topologie uitvoeren kunt.

  * **Activeren**: verwerking van een gedeactiveerde topologie wordt hervat.
  * **Deactiveren**: een actieve topologie wordt onderbroken.
  * **Opnieuw verdelen**: de parallelle uitvoering van de topologie wordt aangepast. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u actieve topologieën opnieuw verdelen. Deze bewerking kunt de topologie aangepast aan parallelle uitvoering om te compenseren voor het toegenomen of afgenomen aantal knooppunten in het cluster.

    Zie <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Inzicht in de parallelle uitvoering van een Storm-topologie</a> voor meer informatie.
  * **Kill**: een Storm-topologie na de opgegeven time-out beëindigd.
* **Topology stats**: statistieken over de topologie. Gebruik de koppelingen in het tijdskader voor de overige vermeldingen op de pagina stelt de **venster** kolom.
* **Spouts**: de spouts die wordt gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke spouts.
* **Bolts**: de bolts gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke bolts.
* **Topologieconfiguratie**: de configuratie van de geselecteerde topologie.

### <a name="spout-and-bolt-summary"></a>Spout en Bolt samenvatting

Selecteren van een spout van de **Spouts** of **Bolts** secties bevat de volgende informatie over het geselecteerde item:

* **Overzicht van onderdelen**: algemene informatie over de spout of bolt.
* **Spout/Bolt stats**: statistieken over de spout of bolt. Gebruik de koppelingen in het tijdskader voor de overige vermeldingen op de pagina stelt de **venster** kolom.
* **Input stats** (alleen Bolts): informatie over de invoer stromen verbruikt door de bolt.
* **Output stats**: informatie over de stromen die door de spout of bolt.
* **Executor**: informatie over de exemplaren van de spout of bolt. Selecteer de **poort** vermelding voor een specifieke executor een logboek van diagnostische gegevens te bekijken die voor dit exemplaar wordt geproduceerd.
* **Fouten**: alle informatie over de fout voor de spout of bolt.

## <a name="monitor-and-manage-rest-api"></a>Bewaken en beheren: REST-API

De Storm-gebruikersinterface is ingebouwd in de REST API, zodat u kunt vergelijkbare management en de controlefunctionaliteit met behulp van de REST-API. U kunt de REST-API maken voor aangepaste hulpprogramma's voor beheer en controle van Storm-topologieën.

Zie voor meer informatie [Storm UI REST-API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). De volgende informatie is specifiek voor het gebruik van de REST-API met Apache Storm op HDInsight.

> [!IMPORTANT]
> De Storm REST-API is niet openbaar beschikbaar via het internet en moet worden geopend met behulp van een SSH-tunnel met het hoofdknooppunt van het HDInsight-cluster. Zie voor meer informatie over het maken en gebruiken van een SSH-tunnel [SSH-Tunneling gebruiken voor toegang tot de Ambari-webgebruikersinterface, ResourceManager JobHistory, NameNode, Oozie en andere web UI](../hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Basis-URI

De basis-URI voor de REST-API op Linux gebaseerde HDInsight-clusters is beschikbaar op het hoofdknooppunt op **https://HEADNODEFQDN:8744/api/v1/**. De domeinnaam van het hoofdknooppunt is gegenereerd tijdens het maken van het cluster en is niet statisch.

U vindt de volledig gekwalificeerde domeinnaam (FQDN) voor het hoofdknooppunt van het cluster op verschillende manieren:

* **Tijdens een SSH-sessie**: Gebruik de opdracht `headnode -f` tijdens een SSH-sessie op het cluster.
* **Vanaf Ambari Web**: Selecteer **Services** vanaf de bovenkant van de pagina, selecteer **Storm**. Van de **samenvatting** tabblad **Storm UI Server**. De FQDN-naam van het knooppunt dat als host fungeert voor de Storm-gebruikersinterface en de REST-API wordt weergegeven boven aan de pagina.
* **Van de Ambari REST-API**: Gebruik de opdracht `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` informatie ophalen over het knooppunt dat de Storm-gebruikersinterface en de REST-API op worden uitgevoerd. Vervang **wachtwoord** met de Administrator-wachtwoord voor het cluster. Vervang **CLUSTERNAME** met de naam van het cluster. In het antwoord bevat de vermelding 'hostnaam' de FQDN-naam van het knooppunt.

### <a name="authentication"></a>Authentication

Aanvragen voor de REST-API moeten gebruiken **basisverificatie**, zodat u de beheerder de naam van HDInsight-cluster en het wachtwoord gebruiken.

> [!NOTE]
> Omdat basisverificatie wordt verzonden met behulp van de versleutelde tekst, moet u **altijd** HTTPS gebruiken voor het beveiligen van communicatie met het cluster.

### <a name="return-values"></a>Waarden geretourneerd

Informatie die wordt geretourneerd door de REST-API is mogelijk alleen bruikbaar van binnen het cluster. Bijvoorbeeld, is de volledig gekwalificeerde domeinnaam (FQDN) dat is geretourneerd voor Zookeeper servers niet toegankelijk is vanaf Internet.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [ontwikkelen van Java gebaseerde topologieën met Maven](apache-storm-develop-java-topology.md).

Zie voor een lijst van meer voorbeeldtopologieën [voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md).
