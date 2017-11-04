---
title: "Implementeren en beheren van Apache Storm-topologieën op HDInsight | Microsoft Docs"
description: "Informatie over het implementeren, bewaken en beheren van Apache Storm-topologieën op HDInsight met behulp van het Storm-Dashboard. Hadoop-hulpprogramma's voor Visual Studio gebruiken."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 92c1a02cd7d435809914e7f5bb43b2f8d6aa0cdb
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Windows

Het Storm-Dashboard kunt u eenvoudig implementeren en uitvoeren van Apache Storm-topologieën met uw HDInsight-cluster met behulp van uw webbrowser. U kunt het dashboard ook gebruiken om te controleren en beheren van actieve topologieën. Als u Visual Studio gebruikt, geeft u de HDInsight Tools voor Visual Studio soortgelijke functies in Visual Studio.

Het Storm-Dashboard en de Storm-onderdelen in HDInsight Tools, is afhankelijk van de Storm REST API, die kan worden gebruikt voor het maken van uw eigen bewaking en oplossingen.

> [!IMPORTANT]
> De stappen in dit document moet een Storm op HDInsight-cluster dat gebruik maakt van Windows als het besturingssysteem. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Zie voor meer informatie over het implementeren en beheren van Storm-topologieën met een HDInsight-cluster dat gebruik maakt van Linux [implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Linux](apache-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Vereisten

* **Apache Storm op HDInsight** -Zie [aan de slag met Apache Storm op HDInsight](apache-storm-tutorial-get-started-linux.md) voor stapsgewijze instructies voor het maken van een cluster.

* Voor de **Storm-Dashboard**: een moderne webbrowser met ondersteuning voor HTML5.

* Voor **Visual Studio** -SDK van Azure 2.5.1 of hoger en de HDInsight Tools voor Visual Studio. Zie [aan de slag met HDInsight Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md) installeren en configureren van de HDInsight tools voor Visual Studio.

    Een van de volgende versies van Visual Studio:

  * Visual Studio 2012 met Update 4

  * Visual Studio 2013 met Update 4 of Visual Studio 2013 Community

  * Visual Studio 2015 (alle versies)

  * Visual Studio 2017 (alle versies)

## <a name="storm-dashboard"></a>Storm-Dashboard

Het Storm-Dashboard is een webpagina die beschikbaar is op uw Storm-cluster. De URL is **https://&lt;clustername >.azurehdinsight.net/**, waarbij **clustername** is de naam van uw Storm op HDInsight-cluster.

Selecteer in de bovenkant van het Storm-Dashboard **indienen topologie**. Volg de instructies op de pagina voor het uitvoeren van een voorbeeld topologie of voor het uploaden en uitvoeren van een topologie die u hebt gemaakt.

![de pagina van de topologie verzenden][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm-gebruikersinterface

Selecteer in het Storm-Dashboard de **Storm-gebruikersinterface** koppeling. U ziet nu informatie over het cluster, naast eventuele actieve topologieën.

![de storm-gebruikersinterface][storm-dashboard-ui]

> [!NOTE]
> In sommige versies van Internet Explorer ontdekt u mogelijk de Storm-gebruikersinterface worden niet vernieuwd nadat u het eerst hebt bezocht. Bijvoorbeeld, kan niet worden weergegeven de nieuwe topologieën die u hebt ingediend, of een topologie als actief kan worden weergegeven wanneer eerder gedeactiveerd. Microsoft is op de hoogte van dit probleem en werkt aan een oplossing.

#### <a name="main-page"></a>Hoofdpagina

De hoofdpagina van de Storm-gebruikersinterface biedt de volgende informatie:

* **Cluster samenvatting**: algemene informatie over de Storm-cluster.

* **Topologie samenvatting**: een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke topologieën weergeven.

* **Supervisor samenvatting**: informatie over de supervisor Storm.

* **Nimbus configuratie**: Nimbus-configuratie voor het cluster.

#### <a name="topology-summary"></a>Topologie samenvatting

Als u een koppeling van de **Topology summary** sectie vindt u de volgende informatie over de topologie:

* **Topologie samenvatting**: algemene informatie over de topologie.

* **Topologie acties**: acties die u voor de topologie uitvoeren kunt.

  * **Activeren**: verwerking van een gedeactiveerde topologie wordt hervat.

  * **Deactiveren**: een actieve topologie wordt onderbroken.

  * **Opnieuw verdelen**: de parallelle uitvoering van de topologie wordt aangepast. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u actieve topologieën opnieuw verdelen. Hiermee wordt de topologie aangepast aan parallelle uitvoering om te compenseren voor het toegenomen of afgenomen aantal knooppunten in het cluster.

      Zie voor meer informatie [inzicht in de parallelle uitvoering van een Storm-topologie (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **Kill**: een Storm-topologie na de opgegeven time-out beëindigd.

* **Topology stats**: statistieken over de topologie. Gebruik de koppelingen in de **venster** kolom het tijdskader voor de overige vermeldingen instellen op de pagina.

* **Spouts**: de spouts die wordt gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke spouts.

* **Bolts**: de bolts gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke bolts.

* **Topologieconfiguratie**: de configuratie van de geselecteerde topologie.

#### <a name="spout-and-bolt-summary"></a>Spout en Bolt samenvatting

Selecteren van een spout van de **Spouts** of **Bolts** secties bevat de volgende informatie over het geselecteerde item:

* **Overzicht van onderdelen**: algemene informatie over de spout of bolt.

* **Spout/Bolt stats**: statistieken over de spout of bolt. Gebruik de koppelingen in de **venster** kolom het tijdskader voor de overige vermeldingen instellen op de pagina.

* **Input stats** (alleen Bolts): informatie over de invoer stromen verbruikt door de bolt.

* **Output stats**: informatie over de stromen die door dit spout of Bolts.

* **Executor**: informatie over de exemplaren van de spout of bolt. Selecteer de **poort** vermelding voor een specifieke executor een logboek van diagnostische gegevens te bekijken die voor dit exemplaar wordt geproduceerd.

* **Fouten**: alle informatie over de fout voor deze spout of Bolts.

## <a name="hdinsight-tools-for-visual-studio"></a>HDInsight-hulpprogramma's voor Visual Studio

De HDInsight Tools kan worden gebruikt om in te dienen C# of hybride topologieën naar uw Storm-cluster. De volgende stappen gebruikt een voorbeeld van een toepassing. Zie voor meer informatie over het maken van uw eigen topologieën met behulp van de HDInsight Tools [C#-topologieën ontwikkelen met HDInsight Tools voor Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

Gebruik de volgende stappen een voorbeeld implementeren naar uw Storm op HDInsight-cluster, en vervolgens weergeven en beheren van de topologie.

1. Als u hebt niet is gebeurd de nieuwste versie van de HDInsight Tools voor Visual Studio, Zie [aan de slag met HDInsight Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Open Visual Studio, selecteer **bestand** > **nieuw** > **Project**.

3. In de **nieuw Project** dialoogvenster Vouw **geïnstalleerde** > **sjablonen**, en selecteer vervolgens **HDInsight**. Selecteer in de lijst met sjablonen **Storm voorbeeld**. Typ een naam voor de toepassing aan de onderkant van het dialoogvenster.

    ![Afbeelding](./media/apache-storm-deploy-monitor-topology/sample.png)

4. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **indienen Storm op HDInsight**.

   > [!NOTE]
   > Als u wordt gevraagd, voert u de aanmeldingsreferenties voor uw Azure-abonnement. Als u meer dan één abonnement hebt, moet u zich aanmelden bij de database met uw Storm op HDInsight-cluster.

5. Selecteer uw Storm op HDInsight-cluster van de **Storm-Cluster** vervolgkeuzelijst en selecteer vervolgens **indienen**. U kunt controleren of het verzenden voltooid met is de **uitvoer** venster.

6. Wanneer de topologie is ingediend, de **Storm-topologieën** voor het cluster moet worden weergegeven. Selecteer de topologie in de lijst om informatie over de actieve topologie weer te geven.

    ![monitor voor Visual studio](./media/apache-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > U kunt ook weergeven **Storm-topologieën** van **Server Explorer** door het uitbreiden van **Azure** > **HDInsight**, en klik vervolgens met de rechtermuisknop op een Storm op HDInsight-cluster en het selecteren van **weergave Storm-topologieën**.

    Selecteer de vorm voor de spouts of bolts om informatie over deze onderdelen weer te geven. Er wordt een nieuw venster geopend voor elk item geselecteerd.

   > [!NOTE]
   > De naam van de topologie is de naam van de klasse van de topologie (in dit geval `HelloWord`,) met een tijdstempel toegevoegd.

7. Van de **Topology Summary** weergave, selecteer **Kill** stoppen van de topologie.

   > [!NOTE]
   > Storm-topologieën blijft in uitvoering totdat ze zijn gestopt of het cluster wordt verwijderd.


## <a name="rest-api"></a>REST API

De Storm-gebruikersinterface is ingebouwd in de REST API, zodat u kunt vergelijkbare management en de controlefunctionaliteit met behulp van de REST-API. U kunt de REST-API maken voor aangepaste hulpprogramma's voor beheer en controle van Storm-topologieën.

Zie voor meer informatie [Storm UI REST-API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). De volgende informatie is specifiek voor het gebruik van de REST-API met Apache Storm op HDInsight.

### <a name="base-uri"></a>Basis-URI

De basis-URI voor de REST-API op HDInsight-clusters is **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, waarbij **clustername** is de naam van uw Storm op HDInsight-cluster.

### <a name="authentication"></a>Authentication

Aanvragen voor de REST-API moeten gebruiken **basisverificatie**, zodat u de beheerder de naam van HDInsight-cluster en het wachtwoord gebruiken.

> [!NOTE]
> Omdat basisverificatie wordt verzonden met behulp van de versleutelde tekst, moet u **altijd** HTTPS gebruiken voor het beveiligen van communicatie met het cluster.

### <a name="return-values"></a>Waarden geretourneerd

Informatie die wordt geretourneerd door de REST-API is mogelijk alleen bruikbaar van binnen het cluster of de virtuele machines op de hetzelfde virtuele Azure-netwerk als het cluster. Bijvoorbeeld, de volledig gekwalificeerde domeinnaam (FQDN) geretourneerd voor Zookeeper-servers zijn niet toegankelijk is vanaf Internet.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe implementeren en topologieën bewaken met behulp van het Storm-Dashboard, informatie over hoe:

* [C#-topologieën met HDInsight Tools voor Visual Studio ontwikkelen](apache-storm-develop-csharp-visual-studio-topology.md)

* [Java gebaseerde topologieën met Maven ontwikkelen](apache-storm-develop-java-topology.md)

Zie voor een lijst van meer voorbeeldtopologieën [voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md).

[hdinsight-dashboard]: ./media/apache-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/apache-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/apache-storm-deploy-monitor-topology/storm-ui-summary.png
