---
title: Inleiding tot Apache Storm op HDInsight | Microsoft Docs
description: Inleiding tot Apache Storm op HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 0cf2d7f4cbbed730d690693fd006665355155c22
ms.lasthandoff: 03/21/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Inleiding tot Apache Storm op HDInsight: realtime analyses voor Hadoop

Met Apache Storm op HDInsight kunt u gedistribueerde, realtime analyseoplossingen maken in Azure.

Apache Storm is een gedistribueerd, fouttolerant, open source berekeningssysteem waarmee u in realtime gegevens met Hadoop kunt verwerken. Dankzij de mogelijkheid om gegevens die de eerste keer niet zijn verwerkt, opnieuw te verwerken, bieden Storm-oplossingen u de garantie dat de gegevens worden verwerkt.

## <a name="why-use-storm-on-hdinsight"></a>Waarom Storm op HDInsight gebruiken?

Apache Storm op HDInsight is een beheerd cluster dat is geïntegreerd in de Azure-omgeving. Storm en andere Hadoop-componenten op HDInsight zijn gebaseerd op HDP (Hortonworks Data Platform). Het besturingssysteem voor het cluster is gebaseerd op Ubuntu (een Linux-distributie). Deze configuratie biedt een platform dat compatibel is met populaire hulpprogramma's en services in het Hadoop-ecosysteem.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight-afschaffing op Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date) voor meer informatie.

Apache Storm op HDInsight biedt de volgende belangrijke voordelen:

* Wordt uitgevoerd als een beheerde service met een SLA met 99,9% beschikbaarheid.

* Aanpassen kan eenvoudig door scripts op een cluster uit te voeren tijdens of na het maken van het cluster. Zie [HDInsight-clusters aanpassen met scriptacties](hdinsight-hadoop-customize-cluster-linux.md) voor meer informatie.

* Gebruik de gewenste taal: Storm-onderdelen kunnen worden geschreven in verschillende talen, zoals **Java**, **C#** en **Python**.
  
  * Visual Studio-integratie met HDInsight voor het ontwikkelen, beheren en controleren van C#-topologieën. Zie [C# Storm-topologieën met de hulpprogramma's van HDInsight voor Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

  * Biedt ondersteuning voor de **Trident** Java-interface. Met deze interface kunt u Storm-topologieën maken die ondersteuning bieden voor een eenmalige verwerking van berichten, transactionele DataStore-persistentie en een aantal algemene Stream Analytics-bewerkingen.

* Eenvoudig omhoog en omlaag schalen van het cluster: voeg worker-knooppunten toe of verwijder ze zonder de actieve Storm-topologieën te beïnvloeden.

* Integratie met de volgende Azure-services:
  
    * Event Hubs
    * Virtueel netwerk
    * SQL Database
    * Azure Storage
    * DocumentDB
  
  * Combineer op veilige wijze de mogelijkheden van meerdere HDInsight-clusters door gebruik te maken van Azure Virtual Network: maak analytische pijplijnen die HDInsight-, HBase- of Hadoop-clusters gebruiken.

Zie [Companies Using Apache Storm](https://storm.apache.org/documentation/Powered-By.html) (Bedrijven die Apache Storm gebruiken) voor een lijst met bedrijven die gebruikmaken van Apache Storm.

Zie [Aan de slag met Storm op HDInsight][gettingstarted] om aan de slag te gaan met Storm.

### <a name="ease-of-creation"></a>Eenvoudig te maken

U kunt in enkele minuten een nieuw Storm op HDInsight-cluster inrichten. Geef de clusternaam, de grootte, het beheerdersaccount en het opslagaccount op. Azure maakt het cluster, inclusief voorbeeldtopologieën en een dashboard voor beheer via het web.

> [!NOTE]
> U kunt ook Storm-clusters inrichten met de [Azure CLI](../cli-install-nodejs.md) of [Azure PowerShell](/powershell/azureps-cmdlets-docs).

Binnen 15 minuten nadat u een aanvraag hebt verzonden, beschikt u over een nieuw actief Storm-cluster en bent u klaar voor uw eerste realtime analysepijplijn.

### <a name="ease-of-use"></a>Gebruiksgemak

* __Secure Shell-connectiviteit__: U hebt via internet toegang tot de hoofdknooppunten van het HDInsight-cluster met behulp van SSH. Met SSH kunt u opdrachten rechtstreeks op het cluster uitvoeren.

  Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* __Webconnectiviteit__: HDInsight-clusters bieden de Ambari-webgebruikersinterface. Met de Ambare-webgebruikersinterface kunt u eenvoudig services op het cluster controleren, configureren en beheren. Storm op HDInsight biedt ook de Storm-gebruikersinterface, waarmee u Storm-topologieën kunt controleren en beheren vanuit de browser.

  Zie [HDInsight beheren met behulp van de Ambari-webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) en [Controleren en beheren met behulp van de Storm-gebruikersinterface](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-using-the-storm-ui).

* __Azure PowerShell en CLI__: zowel Azure PowerShell en Azure CLI bieden opdrachtregelprogramma's die u vanuit het clientsysteem kunt gebruiken om te werken met HDInsight en andere Azure-services.

* __Visual Studio-integratie__: Data Lake-tools voor Visual Studio bevatten projectsjablonen voor het maken van C# Storm-topologieën en tools voor het bewaken van Storm op HDInsight. U kunt C#-topologieën maken, implementeren, bewaken en beheren vanuit Visual Studio.

  Zie [C# Storm-topologieën met de hulpprogramma's van HDInsight voor Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

* __integratie met andere Azure-services__

  * Voor __Java__-ontwikkeling gebruikt Microsoft, waar mogelijk, bestaande Storm-onderdelen om te integreren met andere Azure-services. In sommige gevallen is mogelijk een onderdeel of oplossing vereist die specifiek is voor de service.

    * __Azure Data Lake Store__: topologieën op basis van Java hebben toegang tot Data Lake Store via de Storm-HDFS-bolt met het URI-voorvoegsel `adl://`. Zie [Azure Data Lake Store-bolt gebruiken met Apache Storm op HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-write-data-lake-store) voor een voorbeeld van het gebruik van de Storm-HDFS-bolt.

    * __Azure Storage__ (wanneer deze wordt gebruikt als opslag voor HDInsight): topologieën op basis van Java hebben toegang tot Azure Storage door gebruik te maken van de Storm-HDFS-bolt met het URI-voorvoegsel `wasb://`.

    * __Azure Event Hubs__: zijn toegankelijk via de onderdelen EventHubSpout en EventHubBolt die worden geleverd door Microsoft. Deze onderdelen zijn geschreven in Java en worden geleverd als een zelfstandig JAR-bestand.

    Zie [Een topologie op basis van Java ontwikkelen voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md) voor meer informatie over het ontwikkelen van Java-oplossingen.

  * Voor __C#__-ontwikkeling kunt u doorgaans de .NET SDK voor de Azure-service gebruiken. In sommige gevallen is de SDK mogelijk afhankelijk van frameworks die niet beschikbaar zijn in Linux (het hostbesturingssysteem voor HDInsight 3.4 en hoger). In dit geval kunt u Java-onderdelen gebruiken uit de C#-oplossing.

    * Voorbeelden van werken met __SQL DB__, __DocumentDB__, __EventHub__ en __HBase__ zijn opgenomen als sjablonen in de Azure Data Lake-tools voor Visual Studio. Zie [Een C#-topologie voor Storm op HDInsight ontwikkelen](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

    * __Azure Event Hubs__: zie [Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md) voor een voorbeeld van het gebruik van Java-onderdelen uit een C#-oplossing.

    Zie [Een C#-topologie voor Storm op HDInsight ontwikkelen](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie over het ontwikkelen van C#-oplossingen.

### <a name="reliability"></a>Betrouwbaarheid

Apache Storm zorgt ervoor dat elk binnenkomend bericht altijd volledig wordt verwerkt, zelfs wanneer de gegevensanalyse is verspreid over honderden knooppunten.

Het **Nimbus-knooppunt** biedt een vergelijkbare functionaliteit aan de Hadoop JobTracker en wijst taken toe aan andere knooppunten in het cluster via **Zookeeper**. Zookeeper-knooppunten bieden coördinatie voor het cluster en faciliteren de communicatie tussen Nimbus en het **Supervisor**-proces op de werkrolknooppunten. Als een verwerkingsknooppunt wordt uitgeschakeld, wordt het Nimbus-knooppunt hiervan op de hoogte gesteld en worden de taak en de bijbehorende gegevens toegewezen aan een ander knooppunt.

Apache Storm wordt standaard geconfigureerd met slechts één Nimbus knooppunt. Storm op HDInsight voert twee Nimbus-knooppunten uit. Als het primaire knooppunt mislukt, schakelt het HDInsight-cluster over naar het secundaire knooppunt en wordt het primaire knooppunt hersteld.

![Diagram van nimbus, zookeeper en supervisor](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Schalen

U kunt tijdens het maken het aantal knooppunten in het cluster opgeven, maar mogelijk wilt u het cluster vergroten of verkleinen zodat het overeenkomt met de workload. Alle HDInsight-clusters bieden u de mogelijkheid het aantal knooppunten in het cluster te wijzigen, zelfs terwijl de gegevens worden verwerkt.

> [!NOTE]
> Als u wilt profiteren van nieuwe knooppunten die zijn toegevoegd door te schalen, moet u de topologieën die zijn gestart voordat de clustergrootte werd uitgebreid, opnieuw verdelen.

### <a name="support"></a>Ondersteuning

Storm op HDInsight wordt geleverd met een 24-uurs ondersteuning op ondernemingsniveau. Storm op HDInsight beschikt over een SLA van 99,9%. Dit betekent dat het cluster gegarandeerd minimaal 99,9% van de tijd extern verbinding heeft.

## <a name="common-use-cases-for-real-time-analytics"></a>Algemene gebruiksvoorbeelden voor realtime analyses

Hier volgen enkele algemene scenario's waarvoor u Apache Storm op HDInsight kunt gebruiken. Lees [How companies are using Storm](https://storm.apache.org/documentation/Powered-By.html) (Hoe bedrijven Storm gebruiken) voor informatie over praktijkscenario's.

* Internet of Things (IoT)
* Fraudedetectie
* Sociale analyses
* Extraction, Transformation, Loading (ETL)
* Netwerkbewaking
* Search
* Mobile Engagement

## <a name="how-is-data-in-hdinsight-storm-processed"></a>Hoe worden gegevens in HDInsight Storm verwerkt?

Apache Storm voert **topologieën** uit in plaats van de MapReduce-taken die u mogelijk kent uit HDInsight of Hadoop. Een Storm op HDInsight-cluster bevat twee soorten knooppunten: hoofdknooppunten met **Nimbus** en werkrolknooppunten met **Supervisor**.

* **Nimbus**: net als bij de JobTracker in Hadoop is Nimbus verantwoordelijk voor het distribueren van de code in het cluster, het toewijzen van taken aan virtuele machines en het controleren op fouten. HDInsight biedt twee Nimbus-knooppunten. Er is dus geen Single Point of Failure voor Storm op HDInsight.
* **Supervisor**: de supervisor voor elk werkrolknooppunt is verantwoordelijk voor het starten en stoppen van de **werkprocessen** op het knooppunt.
* **Werkproces**: voert een subset van een **topologie** uit. Een actieve topologie wordt gedistribueerd over verschillende werkprocessen in het cluster.
* **Topologie**: definieert een grafiek van de berekening die **stromen** gegevens verwerkt. In tegenstelling tot MapReduce-taken worden topologieën uitgevoerd totdat u ze stopt.
* **Stream**: een niet-afhankelijke verzameling **tuples**. Streams worden geproduceerd door **spouts** en **bolts** en ze worden verbruikt door **bolts**.
* **Tuple**: een benoemde lijst met dynamische getypeerde waarden.
* **Spout**: gebruikt gegevens van een gegevensbron en verzendt een of meer **streams**.
  
  > [!NOTE]
  > Vaak worden gegevens uit een wachtrij gelezen, zoals Kafka of Azure Event Hubs. De wachtrij zorgt ervoor dat gegevens bij een storing worden bewaard.

* **Bolt**: verbruikt **streams**, voert verwerkingstaken op **tuples** uit en verzendt **streams**. Bolts zijn ook verantwoordelijk voor het schrijven van gegevens naar externe opslaglocaties, zoals een wachtrij, HDInsight, HBase, een blob of een andere gegevensopslag.
* **Apache Thrift**: een softwareframework voor schaalbare meertalige serviceontwikkeling. Hiermee kunt u services bouwen die zowel voor C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk als andere talen werken.

Zie de [Storm-zelfstudie][apachetutorial] op apache.org voor meer informatie over Storm-onderdelen.

## <a name="what-programming-languages-can-i-use"></a>Welke programmeertalen kan ik gebruiken?

### <a name="c35"></a>C&#35;

Dankzij de Data Lake-tools voor Visual Studio kunnen .NET-ontwikkelaars een topologie in C# ontwerpen en implementeren. U kunt ook hybride topologieën maken die gebruikmaken van Java- en C#-onderdelen.

Zie [C#-topologieën met Visual Studio ontwikkelen voor Apache Storm op HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

### <a name="java"></a>Java

De meeste Java-voorbeelden die u tegenkomt, zijn reguliere Java of Trident. Trident is een abstractie op hoog niveau waarmee u gemakkelijker bepaalde bewerkingen kunt uitvoeren, zoals samenvoegen, verzamelen, groeperen en filteren. Trident voert echter bewerkingen uit op batches tuples, terwijl een onbewerkte Java-oplossing een stream tuple voor tuple verwerkt.

Zie de [Trident-zelfstudie](https://storm.apache.org/documentation/Trident-tutorial.html) op apache.org voor meer informatie over Trident.

Zie de [lijst met voorbeelden van Storm-topologieën](hdinsight-storm-example-topology.md) of de Storm Starter-voorbeelden op uw HDInsight-cluster voor voorbeelden van Java- en Trident-topologieën.

De Storm Starter-voorbeelden bevinden zich in de map **/usr/hdp/current/storm-client/contrib/storm-starter** in het HDInsight-cluster.

### <a name="python"></a>Python

Zie [Develop Storm topologies using Python on HDInsight](hdinsight-storm-develop-python-topology.md) (Storm-topologieën ontwikkelen met Python op HDInsight) voor een voorbeeld van het gebruik van Python-onderdelen.

## <a name="what-are-some-common-development-patterns"></a>Wat zijn enkele algemene ontwikkelingspatronen?

### <a name="guaranteed-message-processing"></a>Gegarandeerde berichtverwerking

Storm kan verschillende niveaus van gegarandeerde berichtverwerking bieden. Een eenvoudige Storm-toepassing biedt u bijvoorbeeld de garantie dat de gegevens minimaal één keer worden verwerkt. Trident kan u de garantie bieden dat gegevens exact één keer worden verwerkt.

Zie [Guarantees on data processing](https://storm.apache.org/about/guarantees-data-processing.html) (Garanties met betrekking tot de gegevensverwerking) op apache.org voor meer informatie.

### <a name="ibasicbolt"></a>IBasicBolt

Het patroon voor het lezen van een invoer-tuple, het verzenden van nul of meer tuples om de invoer-tuple vervolgens onmiddellijk aan het einde van de uitvoeringsmethode te bevestigen, is een veelgebruikte methode. Storm biedt de [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html)-interface voor het automatiseren van dit patroon.

### <a name="joins"></a>Samenvoegingen

Hoe gegevensstromen worden gekoppeld, varieert per toepassing. U kunt bijvoorbeeld elke tuple uit meerdere streams samenvoegen in één nieuwe stream, of u kunt alleen batches tuples voor een specifiek venster samenvoegen. In beide gevallen kunt u samenvoeging realiseren met [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), wat een manier is om te definiëren hoe tuples worden gerouteerd naar bolts.

In het volgende Java-voorbeeld wordt fieldsGrouping gebruikt om tuples die afkomstig zijn uit de onderdelen&1;,&2; en&3; te routeren naar de **MyJoiner**-bolt.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Batchverwerking

Storm is een intern timingmechanisme dat ook wel een tik-tuple wordt genoemd. Dit kan worden gebruikt om elke X seconden een batch te verzenden.

Zie de [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java) voor een voorbeeld van het gebruik van een tick tuple uit een C#-onderdeel.

Als u Trident gebruikt, is dit gebaseerd op het verwerken van batches tuples.

### <a name="caching"></a>Caching

In-memory caching wordt vaak gebruikt als mechanisme om de verwerking te versnellen, omdat de veelgebruikte assets in het geheugen blijven staan. Omdat een topologie wordt verdeeld over meerdere knooppunten en meerdere processen binnen elk knooppunt, is het goed om het gebruik van [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) te overwegen. `fieldsGrouping` zorgt ervoor dat tuples met velden die worden gebruikt voor het opzoeken van de cache, altijd naar hetzelfde proces worden gerouteerd. Met deze groeperingsfunctionaliteit voorkomt u dubbele cachevermeldingen voor verschillende processen.

### <a name="streaming-top-n"></a>Top N streamen

Wanneer uw topologie afhankelijk is van de berekening van een top N-waarde, moet u de top N-waarde parallel berekenen en de uitvoer van deze berekeningen vervolgens samenvoegen in een algemene waarde. Dit kan worden gedaan door [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) te gebruiken om per veld te routeren voor een parallelle verwerking en vervolgens te routeren naar een bolt die globaal de top N-waarde bepaalt.

Zie het [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java)-voorbeeld voor een voorbeeld van een berekening van een top N-waarde.

## <a name="what-type-of-logging-does-storm-use"></a>Wat voor type logboekregistratie gebruikt Storm?

Storm gebruikt Apache Log4j om informatie in een logboek te registreren. Standaard wordt een grote hoeveelheid gegevens geregistreerd en kan het lastig zijn om de informatie te doorzoeken. U kunt een configuratiebestand voor logboekregistratie opnemen als onderdeel van uw Storm-topologie om de werking van de logboekregistratie te bepalen.

Zie het voorbeeld van een [op Java gebaseerde woordentelling](hdinsight-storm-develop-java-topology.md) voor Storm op HDInsight, voor een voorbeeldtopologie die aantoont hoe u de logboekregistratie moet configureren.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over realtime analyseoplossingen met Apache Storm in HDInsight:

* [Aan de slag met Storm op HDInsight][gettingstarted]
* [Voorbeeldtopologieën van Storm op HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

