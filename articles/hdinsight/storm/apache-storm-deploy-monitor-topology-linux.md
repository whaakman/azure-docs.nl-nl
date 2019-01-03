---
title: Apache Storm-topologieën op Azure HDInsight implementeren en beheren
description: Informatie over het implementeren, bewaken en beheren van Apache Storm-topologieën met behulp van de Storm-Dashboard op Linux gebaseerde HDInsight. Hadoop-hulpprogramma's voor Visual Studio gebruiken.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.openlocfilehash: d194a5929e648c09eb204860c528e48bc55259ee
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53635394"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-azure-hdinsight"></a>Apache Storm-topologieën op Azure HDInsight implementeren en beheren 

In dit document leert de basisbeginselen van het beheren en controleren van [Apache Storm](https://storm.apache.org/) topologieën voor Storm op HDInsight-clusters waarop.

> [!IMPORTANT]  
> De stappen in dit artikel is een op Linux gebaseerde Storm op HDInsight-cluster vereist. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. 
>
> Zie voor meer informatie over de implementatie en controle-topologieën op HDInsight op basis van Windows [implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Windows](apache-storm-deploy-monitor-topology.md).


## <a name="prerequisites"></a>Vereisten

* **Een op Linux gebaseerde Storm op HDInsight-cluster**: Zie [aan de slag met Apache Storm op HDInsight](apache-storm-tutorial-get-started-linux.md) voor stapsgewijze instructies voor het maken van een cluster

* (Optioneel) **Kennis van SSH en SCP**: Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* (Optioneel) **Visual Studio**: Azure SDK 2.5.1 of hoger en de Data Lake Tools voor Visual Studio. Zie voor meer informatie, [aan de slag met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    Een van de volgende versies van Visual Studio:

  * Visual Studio 2012 update 4

  * Visual Studio 2013 met Update 4 of [Visual Studio 2013 Community](https://go.microsoft.com/fwlink/?LinkId=517284)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/)

  * Visual Studio 2015 (alle edities)

  * Visual Studio 2017 (willekeurige editie). Data Lake Tools voor Visual Studio 2017 zijn geïnstalleerd als onderdeel van de Azure-Workload.

## <a name="submit-a-topology-visual-studio"></a>Indienen van een topologie: Visual Studio

De HDInsight-hulpprogramma's kan worden gebruikt om in te dienen C# of hybride topologieën met uw Storm-cluster. De volgende stappen gebruikt een voorbeeld van toepassing. Zie voor meer informatie over het maken van over het gebruik van de hulpprogramma's voor HDInsight [C#-topologieën ontwikkelen met de HDInsight Tools voor Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

1. Als u de meest recente versie van de Data Lake-hulpprogramma's zijn niet voor Visual Studio hebt geïnstalleerd, raadpleegt u [aan de slag met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]  
    > De Data Lake Tools voor Visual Studio werden voorheen de HDInsight-hulpprogramma's voor Visual Studio.
    >
    > Data Lake Tools voor Visual Studio zijn opgenomen in de __Azure Workload__ voor Visual Studio 2017.

2. Open Visual Studio, selecteer **bestand** > **nieuw** > **Project**.

3. In de **nieuw Project** dialoogvenster Vouw **geïnstalleerde** > **sjablonen**, en selecteer vervolgens **HDInsight**. Selecteer in de lijst met sjablonen **Storm voorbeeld**. Typ een naam voor de toepassing aan de onderkant van het dialoogvenster.

    ![image](./media/apache-storm-deploy-monitor-topology-linux/sample.png)

4. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **indienen bij Storm op HDInsight**.

   > [!NOTE]  
   > Als u hierom wordt gevraagd, voert u de aanmeldingsreferenties voor uw Azure-abonnement. Als u meer dan één abonnement hebt, moet u zich aanmelden bij de optie die uw Storm op HDInsight-cluster bevat.

5. Selecteer uw Storm op HDInsight-cluster op basis van de **Storm-Cluster** vervolgkeuzelijst en selecteer vervolgens **indienen**. U kunt controleren of het verzenden voltooid met behulp van is de **uitvoer** venster.

## <a name="submit-a-topology-ssh-and-the-storm-command"></a>Indienen van een topologie: SSH en de Storm-opdracht

1. Gebruik SSH verbinding maken met de HDInsight-cluster. Vervang **gebruikersnaam** de naam van uw SSH-aanmelding. Vervang **CLUSTERNAME** met de naam van uw HDInsight-cluster:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Zie voor meer informatie over het gebruik van SSH verbinding maken met uw HDInsight-cluster [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Gebruik de volgende opdracht om een voorbeeldtopologie te starten:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    Met deze opdracht wordt de WordCount-voorbeeldtopologie gestart op het cluster. Deze topologie genereert willekeurig zinnen en telt vervolgens het exemplaar van elk woord in de zinnen.

   > [!NOTE]  
   > Bij het indienen van de topologie bij het cluster moet u eerst het jar-bestand met het cluster kopiëren voordat u de opdracht `storm` gebruikt. Als u wilt kopiëren van het bestand aan het cluster, kunt u de `scp` opdracht. Bijvoorbeeld: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   >
   > Het WordCount-voorbeeld en andere Storm Starter-voorbeelden zijn al in uw cluster opgenomen in `/usr/hdp/current/storm-client/contrib/storm-starter/`.

## <a name="submit-a-topology-programmatically"></a>Indienen van een topologie: via een programma

U kunt een topologie met behulp van het Nimbus-service programmatisch implementeren. [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) Geeft een voorbeeld Java-toepassing die u laat zien hoe u kunt implementeren en starten van een topologie via het Nimbus-service.

## <a name="monitor-and-manage-visual-studio"></a>Controleren en beheren: Visual Studio

Wanneer een topologie wordt verzonden met behulp van Visual Studio, de **Storm-topologieën** weergegeven. Selecteer de topologie in de lijst om informatie over de actieve topologie weer te geven.

![monitor voor Visual studio](./media/apache-storm-deploy-monitor-topology-linux/vsmonitor.png)

> [!NOTE]  
> U kunt ook weergeven **Storm-topologieën** van **Server Explorer** door uit te vouwen **Azure** > **HDInsight**, en vervolgens met de rechtermuisknop op een Storm op HDInsight-cluster en het selecteren van **Zobrazit topologie Stormu**.

Selecteer de shape voor de spouts of bolts om informatie over deze onderdelen weer te geven. Er wordt een nieuw venster geopend voor elk item geselecteerd.

### <a name="deactivate-and-reactivate"></a>Deactiveren en opnieuw activeren

Het deactiveren van een topologie wordt onderbroken deze totdat deze wordt afgesloten of opnieuw geactiveerd. Als u wilt deze bewerkingen wilt uitvoeren, gebruiken de __deactiveren__ en __opnieuw activeren__ knoppen aan de bovenkant van de __Topology Summary__.

### <a name="rebalance"></a>Opnieuw verdelen

Herverdeling van een topologie, kan het systeem voor het bijwerken van de parallelle uitvoering van de topologie. Als u het cluster om toe te voegen meer notities hebt uitgebreid, kan opnieuw verdelen bijvoorbeeld een topologie om te zien van de nieuwe knooppunten.

Als u wilt opnieuw verdelen een topologie, gebruikt u de __opnieuw verdelen__ knop aan de bovenkant van de __Topology Summary__.

> [!WARNING]  
> Eerst een topologie herverdeling deactiveert u de topologie gelijkmatig herdistribueert u werknemers in het cluster vervolgens, tot slot wordt de topologie naar de status was voordat de herverdeling is opgetreden. Dus als de topologie actief was, wordt het actieve opnieuw. Als deze is gedeactiveerd, blijft het gedeactiveerd.

### <a name="kill-a-topology"></a>Een topologie kill

Storm-topologieën blijven uitvoeren totdat ze zijn gestopt of het cluster wordt verwijderd. Als u wilt een topologie stoppen, gebruikt u de __Kill__ knop aan de bovenkant van de __Topology Summary__.

## <a name="monitor-and-manage-ssh-and-the-storm-command"></a>Controleren en beheren: SSH en de Storm-opdracht

De `storm` hulpprogramma kunt u werken met actieve topologieën vanaf de opdrachtregel. Gebruik `storm -h` voor een volledige lijst met opdrachten.

### <a name="list-topologies"></a>Lijst met topologieën

Gebruik de volgende opdracht om een lijst van alle actieve topologieën:

    storm list

Met deze opdracht wordt informatie geretourneerd die lijkt op de volgende tekst:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deactiveren en opnieuw activeren

Het deactiveren van een topologie wordt onderbroken deze totdat deze wordt afgesloten of opnieuw geactiveerd. Gebruik de volgende opdracht uit om te deactiveren en opnieuw activeren:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Een actieve topologie kill

Storm-topologieën, één keer wordt gestart, gaat u verder uitgevoerd totdat deze wordt gestopt. Als u wilt een topologie stoppen, gebruikt u de volgende opdracht uit:

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a>Opnieuw verdelen

Herverdeling van een topologie, kan het systeem voor het bijwerken van de parallelle uitvoering van de topologie. Als u het cluster om toe te voegen meer notities hebt uitgebreid, kan opnieuw verdelen bijvoorbeeld een topologie om te zien van de nieuwe knooppunten.

> [!WARNING]  
> Eerst een topologie herverdeling deactiveert u de topologie gelijkmatig herdistribueert u werknemers in het cluster vervolgens, tot slot wordt de topologie naar de status was voordat de herverdeling is opgetreden. Dus als de topologie actief was, wordt het actieve opnieuw. Als deze is gedeactiveerd, blijft het gedeactiveerd.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a>Controleren en beheren: Storm-gebruikersinterface

De Storm-gebruikersinterface biedt een webinterface voor het werken met actieve topologieën en is opgenomen in uw HDInsight-cluster. Als u de Storm-gebruikersinterface, gebruikt u een webbrowser openen **https://CLUSTERNAME.azurehdinsight.net/stormui**, waarbij **CLUSTERNAME** is de naam van uw cluster.

> [!NOTE]  
> Als u wordt gevraagd een gebruikersnaam en een wachtwoord op te geven, voert u de gegevens voor de clusterbeheerder (admin) en het wachtwoord in die u hebt gebruikt toen u het cluster maakte.

### <a name="main-page"></a>Hoofdpagina

De startpagina van de Storm-gebruikersinterface biedt de volgende informatie:

* **Samenvatting voor cluster**: Algemene informatie over de Storm-cluster.
* **Topologie samenvatting**: Een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie om meer informatie over specifieke topologieën weer te geven.
* **Samenvatting supervisor**: Informatie over de supervisor Storm.
* **Nimbus-configuratie**: Nimbus-configuratie van het cluster.

### <a name="topology-summary"></a>Topologie samenvatting

Selecteren van een koppeling van de **Topology summary** sectie vindt u de volgende informatie over de topologie:

* **Topologie samenvatting**: Algemene informatie over de topologie.
* **Topologie acties**: Beheeracties die u voor de topologie uitvoeren kunt.

  * **Activeren**: Hervat de verwerking van een gedeactiveerde topologie.
  * **Deactiveren**: Een actieve topologie wordt onderbroken.
  * **Opnieuw verdelen**: Hiermee past u de parallelle uitvoering van de topologie. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u actieve topologieën opnieuw verdelen. Met deze bewerking kunt de topologie aangepast aan parallelle uitvoering om te compenseren voor het grotere of kleinere aantal knooppunten in het cluster.

    Zie voor meer informatie, <a href="https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">inzicht in de parallelle uitvoering van een Apache Storm-topologie</a>.
  * **Kill**: Een Storm-topologie beëindigd na de opgegeven time-out.
* **Topology stats**: Statistieken over de topologie. Om in te stellen het tijdsbestek voor de overige items op de pagina, gebruik de koppelingen in de **venster** kolom.
* **Spouts**: De spouts die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie om meer informatie over specifieke spouts weer te geven.
* **Bolts**: De bolts die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke bolts.
* **Topologieconfiguratie**: De configuratie van de geselecteerde topologie.

### <a name="spout-and-bolt-summary"></a>Spout en Bolt-overzicht

Selecteren van een spout uit de **Spouts** of **Bolts** secties bevat de volgende informatie over het geselecteerde item:

* **Overzicht van onderdelen**: Algemene informatie over de spout of bolt.
* **Spout/Bolt stats**: Statistieken over de spout of bolt. Om in te stellen het tijdsbestek voor de overige items op de pagina, gebruik de koppelingen in de **venster** kolom.
* **Input stats** (alleen Bolts): Informatie over de invoer stromen die worden gebruikt door de bolt.
* **Output stats**: Informatie over de stromen die door de spout of bolt.
* **Executor**: Informatie over de exemplaren van een spout of bolt. Selecteer de **poort** vermelding voor een specifieke executor om een logboek van diagnostische gegevens weer te geven die wordt geproduceerd voor dit exemplaar.
* **Fouten**: Foutgegevens voor de spout of bolt.

## <a name="monitor-and-manage-rest-api"></a>Controleren en beheren: REST-API

De Storm-gebruikersinterface is gebaseerd op de REST-API, zodat u soortgelijke beheer en controle van functies met behulp van de REST-API kunt uitvoeren. De REST-API kunt u aangepaste hulpprogramma's voor het beheren en controleren van Storm-topologieën maken.

Zie voor meer informatie, [REST-API voor Apache Storm-gebruikersinterface](https://storm.apache.org/releases/current/STORM-UI-REST-API.html). De volgende informatie is specifiek voor de REST-API gebruiken met Apache Storm op HDInsight.

> [!IMPORTANT]  
> De Storm-REST-API is niet openbaar beschikbaar via het internet, en moeten worden geopend via een SSH-tunnel naar het hoofdknooppunt van HDInsight-cluster. Zie voor meer informatie over het maken en gebruiken van een SSH-tunnel [SSH-Tunneling gebruiken voor toegang tot de Apache Ambari-Webgebruikersinterface, ResourceManager JobHistory, NameNode, Apache Oozie en andere webgebruikersinterfaces](../hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Basis-URI

De basis-URI voor de REST-API op Linux gebaseerde HDInsight-clusters is beschikbaar op het hoofdknooppunt op **https://HEADNODEFQDN:8744/api/v1/**. De domeinnaam van het hoofdknooppunt is gegenereerd tijdens het maken van clusters en is niet statisch.

U vindt de volledig gekwalificeerde domeinnaam (FQDN) voor het hoofdknooppunt van het cluster op verschillende manieren:

* **Vanuit een SSH-sessie**: Gebruik de opdracht `headnode -f` van een SSH-sessie met het cluster.
* **Van Ambari Web**: Selecteer **Services** vanaf de bovenkant van de pagina, selecteert u vervolgens **Storm**. Uit de **samenvatting** tabblad **Storm UI Server**. De FQDN-naam van het knooppunt dat als host fungeert voor de Storm-gebruikersinterface en de REST-API wordt weergegeven boven aan de pagina.
* **Van de Ambari REST-API**: Gebruik de opdracht `curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` informatie ophalen over het knooppunt dat de Storm-gebruikersinterface en de REST-API op worden uitgevoerd. Vervang **CLUSTERNAME** met de naam van het cluster. Wanneer u hierom wordt gevraagd, voert u het wachtwoord voor het aanmeldingswachtwoord (beheerder)-account. In het antwoord bevat de vermelding 'hostnaam' de FQDN-naam van het knooppunt.

### <a name="authentication"></a>Verificatie

Aanvragen voor de REST-API moeten gebruiken **basisverificatie**, zodat u de beheerder de naam van HDInsight-cluster en het wachtwoord gebruiken.

> [!NOTE]  
> Omdat basisverificatie wordt verzonden met behulp van niet-versleutelde tekst, moet u **altijd** HTTPS gebruikt voor het beveiligen van communicatie met het cluster.

### <a name="return-values"></a>Retourwaarden

Informatie die wordt geretourneerd vanaf de REST-API kan alleen worden gebruikt in binnen het cluster. Bijvoorbeeld, de volledig gekwalificeerde domeinnaam (FQDN) geretourneerd voor [Apache ZooKeeper](https://zookeeper.apache.org/) servers is niet toegankelijk vanaf Internet.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [ontwikkelen op Java gebaseerde topologieën met behulp van Apache Maven](apache-storm-develop-java-topology.md).

Zie voor een lijst van meer voorbeeldtopologieën, [voorbeeldtopologieën van Apache Storm op HDInsight](apache-storm-example-topology.md).
