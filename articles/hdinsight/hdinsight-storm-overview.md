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
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/31/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 1155f534869f240c1567fa8b791dbcb7750c8a40
ms.lasthandoff: 03/31/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Inleiding tot Apache Storm op HDInsight: realtime analyses voor Hadoop

Met Apache Storm op HDInsight kunt u gedistribueerde, realtime analyseoplossingen maken in Azure.

Apache Storm is een gedistribueerd, fouttolerant, open source berekeningssysteem waarmee u in realtime gegevens met Hadoop kunt verwerken. Dankzij de mogelijkheid om gegevens die de eerste keer niet zijn verwerkt, opnieuw te verwerken, bieden Storm-oplossingen u de garantie dat de gegevens worden verwerkt.

## <a name="how-does-storm-work"></a>Hoe werkt Storm

Apache Storm voert **topologieën** uit in plaats van de MapReduce-taken die u mogelijk kent uit HDInsight of Hadoop. Topologieën bestaan uit meerdere onderdelen die zijn gerangschikt in een Directed Acyclic Graph (DAG). Het volgende diagram toont hoe gegevens stromen tussen de onderdelen van een eenvoudige topologie voor het tellen van woorden

![Voorbeeld van hoe onderdelen zijn gerangschikt in een Storm-topologie](./media/hdinsight-storm-overview/wordcount-topology.png)

* Via __Spout__-onderdelen worden gegevens overgebracht naar een topologie. Deze onderdelen introduceren een of meer stromen in de topologie.

* __Bolt__-onderdelen verwerken stromen die afkomstig zijn van spouts of andere bolts. Bolts kunnen eventueel nieuwe stromen introduceren in de topologie. Bolts zijn ook verantwoordelijk voor het wegschrijven van gegevens naar permanente opslag zoals HDFS of HBase.

## <a name="why-use-storm-on-hdinsight"></a>Waarom Storm op HDInsight gebruiken?

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

## <a name="ease-of-creation"></a>Eenvoudig te maken

U kunt in enkele minuten een nieuw Storm op HDInsight-cluster inrichten. Zie [Aan de slag met Storm op HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) voor meer informatie over het maken van een Storm-cluster.

## <a name="ease-of-use"></a>Gebruiksgemak

* __Secure Shell-connectiviteit__: U hebt via internet toegang tot de hoofdknooppunten van het HDInsight-cluster met behulp van SSH. Met SSH kunt u opdrachten rechtstreeks op het cluster uitvoeren.

  Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* __Webconnectiviteit__: HDInsight-clusters bieden de Ambari-webgebruikersinterface. Met de Ambare-webgebruikersinterface kunt u eenvoudig services op het cluster controleren, configureren en beheren. Storm op HDInsight biedt ook de Storm-gebruikersinterface, waarmee u Storm-topologieën kunt controleren en beheren vanuit de browser.

  Zie [HDInsight beheren met behulp van de Ambari-webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) en [Controleren en beheren met behulp van de Storm-gebruikersinterface](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Azure PowerShell en CLI__: zowel Azure PowerShell en Azure CLI bieden opdrachtregelprogramma's die u vanuit het clientsysteem kunt gebruiken om te werken met HDInsight en andere Azure-services.

* __Visual Studio-integratie__: Data Lake Tools voor Visual Studio bevatten projectsjablonen voor het maken van C# Storm-topologieën met behulp van het SCP.Net-framework. Data Lake Tools biedt ook hulpprogramma's voor het implementeren, bewaken en beheren van oplossingen met Storm op HDInsight.

  Zie [C# Storm-topologieën met de hulpprogramma's van HDInsight voor Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

## <a name="integration-with-other-azure-services"></a>Integratie met andere Azure-services

* __Azure Data Lake Store__: voor een voorbeeld van het gebruik van Azure Data Lake Store met Storm raadpleegt u [Use Azure Data Lake Store with Apache Storm on HDInsight](hdinsight-storm-write-data-lake-store.md) (Azure Data Lake Store gebruiken met Apache Storm op HDInsight).

* __Azure Event Hubs__: raadpleeg de volgende documenten voor een voorbeeld van het gebruik van Azure Event Hubs met Storm:

    * [Develop a Java-based topology for Storm on HDInsight](hdinsight-storm-develop-java-topology.md) (Een topologie op basis van Java ontwikkelen voor Storm op HDInsight)

    * [Process events from Azure Event Hubs with Storm on HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md) (Gebeurtenissen uit Azure Event Hubs verwerken met Storm op HDInsight (C#))

* Voorbeelden van werken met __SQL DB__, __DocumentDB__, __EventHub__ en __HBase__ zijn opgenomen als sjablonen in de Azure Data Lake-tools voor Visual Studio. Zie [Een C#-topologie voor Storm op HDInsight ontwikkelen](hdinsight-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

## <a name="reliability"></a>Betrouwbaarheid

Apache Storm zorgt ervoor dat elk binnenkomend bericht altijd volledig wordt verwerkt, zelfs wanneer de gegevensanalyse is verspreid over honderden knooppunten.

Het **Nimbus-knooppunt** biedt een vergelijkbare functionaliteit aan de Hadoop JobTracker en wijst taken toe aan andere knooppunten in het cluster via **Zookeeper**. Zookeeper-knooppunten bieden coördinatie voor het cluster en faciliteren de communicatie tussen Nimbus en het **Supervisor**-proces op de werkrolknooppunten. Als een verwerkingsknooppunt wordt uitgeschakeld, wordt het Nimbus-knooppunt hiervan op de hoogte gesteld en worden de taak en de bijbehorende gegevens toegewezen aan een ander knooppunt.

Apache Storm wordt standaard geconfigureerd met slechts één Nimbus knooppunt. Storm op HDInsight voert twee Nimbus-knooppunten uit. Als het primaire knooppunt mislukt, schakelt het HDInsight-cluster over naar het secundaire knooppunt en wordt het primaire knooppunt hersteld.

![Diagram van nimbus, zookeeper en supervisor](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Schalen

U kunt tijdens het maken het aantal knooppunten in het cluster opgeven, maar mogelijk wilt u het cluster vergroten of verkleinen zodat het overeenkomt met de workload. Alle HDInsight-clusters bieden u de mogelijkheid het aantal knooppunten in het cluster te wijzigen, zelfs terwijl de gegevens worden verwerkt.

> [!NOTE]
> Als u wilt profiteren van nieuwe knooppunten die zijn toegevoegd door te schalen, moet u de topologieën die zijn gestart voordat de clustergrootte werd uitgebreid, opnieuw verdelen.

## <a name="support"></a>Ondersteuning

Storm op HDInsight wordt geleverd met een 24-uurs ondersteuning op ondernemingsniveau. Storm op HDInsight beschikt over een SLA van 99,9%. Dit betekent dat het cluster gegarandeerd minimaal 99,9% van de tijd extern verbinding heeft.

Zie [Ondersteuning van Azure](https://azure.microsoft.com/support/options/) voor meer informatie.

## <a name="common-use-cases"></a>Algemene scenario’s

Hier volgen enkele algemene scenario's waarvoor u Apache Storm op HDInsight kunt gebruiken. 

* Internet of Things (IoT)
* Fraudedetectie
* Sociale analyses
* Extraction, Transformation, Loading (ETL)
* Netwerkbewaking
* Search
* Mobile Engagement

Lees het document [How companies are using Storm](https://storm.apache.org/documentation/Powered-By.html) (Hoe bedrijven Storm gebruiken) voor informatie over praktijkscenario's.

## <a name="development"></a>Ontwikkeling

Dankzij de __Data Lake Tools voor Visual Studio__ kunnen .NET-ontwikkelaars een topologie ontwerpen en implementeren in __C#__. U kunt ook hybride topologieën maken die gebruikmaken van Java- en C#-onderdelen.

Zie [Develop C# topologies for Apache Storm on HDInsight using Hadoop tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (C#-topologieën voor Storm op HDInsight ontwikkelen met Visual Studio) voor meer informatie.

U kunt ook __Java__-oplossingen ontwikkelen met behulp van de IDE van uw keuze. Zie [Develop Java topologies for Storm on HDInsight](hdinsight-storm-develop-java-topology.md) (Java-topologieën ontwikkelen voor Storm op HDInsight) voor meer informatie.

Python kan ook worden gebruikt voor het ontwikkelen van Storm-onderdelen. Zie [Develop Storm topologies using Python on HDInsight](hdinsight-storm-develop-python-topology.md) (Storm-topologieën ontwikkelen met Python voor Storm op HDInsight) voor meer informatie.

## <a name="common-development-patterns"></a>Algemene ontwikkelingspatronen

### <a name="guaranteed-message-processing"></a>Gegarandeerde berichtverwerking

Storm kan verschillende niveaus van gegarandeerde berichtverwerking bieden. Een eenvoudige Storm-toepassing biedt u bijvoorbeeld de garantie dat de gegevens minimaal één keer worden verwerkt. Trident kan u de garantie bieden dat gegevens exact één keer worden verwerkt.

Zie [Guarantees on data processing](https://storm.apache.org/about/guarantees-data-processing.html) (Garanties met betrekking tot de gegevensverwerking) op apache.org voor meer informatie.

### <a name="ibasicbolt"></a>IBasicBolt

Het patroon voor het lezen van een invoer-tuple, het verzenden van nul of meer tuples om de invoer-tuple vervolgens onmiddellijk aan het einde van de uitvoeringsmethode te bevestigen, is een veelgebruikte methode. Storm biedt de [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html)-interface voor het automatiseren van dit patroon.

### <a name="joins"></a>Samenvoegingen

Hoe gegevensstromen worden gekoppeld, varieert per toepassing. U kunt bijvoorbeeld elke tuple uit meerdere streams samenvoegen in één nieuwe stream, of u kunt alleen batches tuples voor een specifiek venster samenvoegen. In beide gevallen kunt u samenvoeging realiseren met [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), wat een manier is om te definiëren hoe tuples worden gerouteerd naar bolts.

In het volgende Java-voorbeeld wordt fieldsGrouping gebruikt om tuples die afkomstig zijn uit de onderdelen 1, 2 en 3 te routeren naar de **MyJoiner**-bolt.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Batchverwerking

Storm is een intern timingmechanisme dat ook wel een tik-tuple wordt genoemd. Dit kan worden gebruikt om elke X seconden een batch te verzenden.

Zie de [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java) voor een voorbeeld van het gebruik van een tick tuple uit een C#-onderdeel.

Als u Trident gebruikt, is dit gebaseerd op het verwerken van batches tuples.

### <a name="caching"></a>Caching

In-memory caching wordt vaak gebruikt als mechanisme om de verwerking te versnellen, omdat de veelgebruikte assets in het geheugen blijven staan. Omdat een topologie wordt verdeeld over meerdere knooppunten en meerdere processen binnen elk knooppunt, is het goed om het gebruik van [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) te overwegen. `fieldsGrouping` zorgt ervoor dat tuples met velden die worden gebruikt voor het opzoeken van de cache, altijd naar hetzelfde proces worden gerouteerd. Met deze groeperingsfunctionaliteit voorkomt u dubbele cachevermeldingen voor verschillende processen.

### <a name="streaming-top-n"></a>Top N streamen

Wanneer uw topologie afhankelijk is van de berekening van de waarde van een 'top N', moet u die waarde in parallel berekenen. Vervolgens moet u de uitvoer van die berekeningen samenvoegen tot een globale waarde. Dit kan worden gedaan door [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) te gebruiken om per veld te routeren voor een parallelle verwerking en vervolgens te routeren naar een bolt die globaal de top N-waarde bepaalt.

Zie het [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java)-voorbeeld voor een voorbeeld van een berekening van een top N-waarde.

## <a name="logging"></a>Logboekregistratie

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

