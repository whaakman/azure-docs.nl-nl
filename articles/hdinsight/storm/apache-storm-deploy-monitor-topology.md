---
title: Apache Storm-topologieën in Azure HDInsight implementeren en beheren
description: Informatie over het implementeren, bewaken en beheren van Apache Storm-topologieën op HDInsight op basis van Windows met behulp van de Storm-Dashboard. Hadoop-hulpprogramma's voor Visual Studio gebruiken.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 03/01/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: ce39e3ffce0b7721bde84254c7e5a35ec28465dc
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583156"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Windows

De [Apache Storm](http://storm.apache.org/) Dashboard kunt u eenvoudig implementeren en uitvoeren van Apache Storm-topologieën op uw HDInsight-cluster met behulp van uw webbrowser. U kunt het dashboard ook gebruiken om te controleren en beheren van actieve topologieën. Als u Visual Studio gebruikt, geeft u de HDInsight Tools voor Visual Studio vergelijkbare functies in Visual Studio.

De Storm-Dashboard en de Storm-onderdelen in de HDInsight-hulpprogramma's, is afhankelijk van de Storm REST API, die kan worden gebruikt voor het maken van uw eigen bewaking en oplossingen voor beheer.

> [!IMPORTANT]
> De stappen in dit document moet een Storm op HDInsight-cluster dat gebruik maakt van Windows als het besturingssysteem. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.
>
> Zie voor meer informatie over het implementeren en beheren van Storm-topologieën met een HDInsight-cluster dat gebruik maakt van Linux [implementeren en beheren van Apache Storm-topologieën op HDInsight op basis van Linux](apache-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Vereisten

* **Apache Storm op HDInsight** -Zie [aan de slag met Apache Storm op HDInsight](apache-storm-tutorial-get-started-linux.md) voor stapsgewijze instructies voor het maken van een cluster.

* Voor de **Storm-Dashboard**: een moderne webbrowser met ondersteuning voor HTML5.

* Voor **Visual Studio** -Azure-SDK 2.5.1 of hoger en de HDInsight Tools voor Visual Studio. Zie [aan de slag met HDInsight Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md) installeren en configureren van de HDInsight-hulpprogramma's voor Visual Studio.

    Een van de volgende versies van Visual Studio:

  * Visual Studio 2012 update 4

  * Visual Studio 2013 update 4 of Visual Studio 2013 Community

  * Visual Studio 2015 (alle edities)

  * Visual Studio 2017 (alle edities)

## <a name="storm-dashboard"></a>Storm-Dashboard

De Storm-Dashboard is een webpagina die beschikbaar is op uw Storm-cluster. De URL is **https://&lt;clustername >.azurehdinsight.net/**, waarbij **clustername** is de naam van uw Storm op HDInsight-cluster.

Vanaf de bovenkant van het Storm-Dashboard, selecteert u **indienen topologie**. Volg de instructies op de pagina voor het uitvoeren van een voorbeeldtopologie of voor het uploaden en uitvoeren van een topologie die u hebt gemaakt.

![de topologie-pagina verzenden][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm-gebruikersinterface

Selecteer in het Storm-Dashboard, de **Storm-gebruikersinterface** koppeling. U ziet nu informatie over het cluster, naast eventuele actieve topologieën.

![de storm-gebruikersinterface][storm-dashboard-ui]

> [!NOTE]
> In sommige versies van Internet Explorer ontdekt u mogelijk de Storm-gebruikersinterface worden niet vernieuwd nadat u hebt het eerst bezocht. Het kan bijvoorbeeld niet de nieuwe topologieën weergegeven die u hebt ingediend, of een topologie als actief kan worden weergegeven wanneer u het eerder gedeactiveerd. Microsoft is op de hoogte van dit probleem en werkt aan een oplossing.

#### <a name="main-page"></a>Hoofdpagina

De startpagina van de Storm-gebruikersinterface biedt de volgende informatie:

* **Clusteroverzicht**: algemene informatie over de Storm-cluster.

* **Topologie samenvatting**: een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie om meer informatie over specifieke topologieën weer te geven.

* **Samenvatting supervisor**: informatie over de supervisor Storm.

* **Nimbus-configuratie**: Nimbus-configuratie van het cluster.

#### <a name="topology-summary"></a>Topologie samenvatting

Selecteren van een koppeling van de **Topology summary** sectie vindt u de volgende informatie over de topologie:

* **Topologie samenvatting**: algemene informatie over de topologie.

* **Topologie acties**: beheeracties die u voor de topologie uitvoeren kunt.

  * **Activeren**: verwerking van een gedeactiveerde topologie wordt hervat.

  * **Deactiveren**: een actieve topologie wordt onderbroken.

  * **Opnieuw verdelen**: Hiermee past u de parallelle uitvoering van de topologie. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u actieve topologieën opnieuw verdelen. Hiermee wordt de topologie aangepast aan parallelle uitvoering om te compenseren voor het grotere of kleinere aantal knooppunten in het cluster.

      Zie voor meer informatie, [inzicht in de parallelle uitvoering van een Apache Storm-topologie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **Kill**: een Storm-topologie na de opgegeven time-out beëindigd.

* **Topology stats**: statistieken over de topologie. Gebruik de koppelingen in de **venster** kolom om in te stellen het tijdsbestek voor de overige items op de pagina.

* **Spouts**: de spouts die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie om meer informatie over specifieke spouts weer te geven.

* **Bolts**: de bolts die worden gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke bolts.

* **Topologieconfiguratie**: de configuratie van de geselecteerde topologie.

#### <a name="spout-and-bolt-summary"></a>Spout en Bolt-overzicht

Selecteren van een spout uit de **Spouts** of **Bolts** secties bevat de volgende informatie over het geselecteerde item:

* **Overzicht van onderdelen**: algemene informatie over de spout of bolt.

* **Spout/Bolt stats**: statistieken over de spout of bolt. Gebruik de koppelingen in de **venster** kolom om in te stellen het tijdsbestek voor de overige items op de pagina.

* **Input stats** (alleen Bolts): informatie over de invoer stromen die worden gebruikt door de bolt.

* **Output stats**: informatie over de stromen die door deze spout of bolt.

* **Executor**: informatie over de exemplaren van een spout of bolt. Selecteer de **poort** vermelding voor een specifieke executor om een logboek van diagnostische gegevens weer te geven die wordt geproduceerd voor dit exemplaar.

* **Fouten**: alle foutinformatie voor dit spout of bolt.

## <a name="hdinsight-tools-for-visual-studio"></a>HDInsight Tools voor Visual Studio

De [hulpprogramma's voor HDInsight](https://azure.microsoft.com/resources/videos/hdinsight-tools-for-visual-studio/) kan worden gebruikt om in te dienen C# of hybride topologieën met uw Storm-cluster. De volgende stappen gebruikt een voorbeeld van toepassing. Zie voor meer informatie over het maken van uw eigen topologieën met behulp van de hulpprogramma's voor HDInsight [C#-topologieën ontwikkelen met de HDInsight Tools voor Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

Gebruik de volgende stappen een voorbeeld implementeren naar uw Storm op HDInsight-cluster, en vervolgens weergeven en beheren van de topologie.

1. Als u de meest recente versie van de HDInsight-hulpprogramma's zijn niet voor Visual Studio hebt geïnstalleerd, raadpleegt u [aan de slag met HDInsight Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Open Visual Studio, selecteer **bestand** > **nieuw** > **Project**.

3. In de **nieuw Project** dialoogvenster Vouw **geïnstalleerde** > **sjablonen**, en selecteer vervolgens **HDInsight**. Selecteer in de lijst met sjablonen **Storm voorbeeld**. Typ een naam voor de toepassing aan de onderkant van het dialoogvenster.

    ![image](./media/apache-storm-deploy-monitor-topology/sample.png)

4. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **indienen bij Storm op HDInsight**.

   > [!NOTE]
   > Als u hierom wordt gevraagd, voert u de aanmeldingsreferenties voor uw Azure-abonnement. Als u meer dan één abonnement hebt, moet u zich aanmelden bij de optie die uw Storm op HDInsight-cluster bevat.

5. Selecteer uw Storm op HDInsight-cluster op basis van de **Storm-Cluster** vervolgkeuzelijst en selecteer vervolgens **indienen**. U kunt controleren of het verzenden voltooid met behulp van is de **uitvoer** venster.

6. Wanneer de topologie is ingediend, de **Storm-topologieën** voor het cluster moet worden weergegeven. Selecteer de topologie in de lijst om informatie over de actieve topologie weer te geven.

    ![monitor voor Visual studio](./media/apache-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > U kunt ook weergeven **Storm-topologieën** van **Server Explorer** door uit te vouwen **Azure** > **HDInsight**, en vervolgens met de rechtermuisknop op een Storm op HDInsight-cluster en het selecteren van **Zobrazit topologie Stormu**.

    Selecteer de shape voor de spouts of bolts om informatie over deze onderdelen weer te geven. Er wordt een nieuw venster geopend voor elk item geselecteerd.

   > [!NOTE]
   > De naam van de topologie is de naam van de klasse van de topologie (in dit geval `HelloWord`,) met een tijdstempel wordt toegevoegd.

7. Uit de **Topology Summary** weergave, selecteer **Kill** stoppen van de topologie.

   > [!NOTE]
   > Storm-topologieën blijven uitvoeren totdat ze zijn gestopt of het cluster wordt verwijderd.


## <a name="rest-api"></a>REST-API

De Storm-gebruikersinterface is gebaseerd op de REST-API, zodat u soortgelijke beheer en controle van functies met behulp van de REST-API kunt uitvoeren. De REST-API kunt u aangepaste hulpprogramma's voor het beheren en controleren van Storm-topologieën maken.

Zie voor meer informatie, [REST-API voor Apache Storm-gebruikersinterface](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). De volgende informatie is specifiek voor de REST-API gebruiken met Apache Storm op HDInsight.

### <a name="base-uri"></a>Basis-URI

De basis-URI voor de REST-API op HDInsight-clusters is **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, waarbij **clustername** is de naam van uw Storm op HDInsight cluster.

### <a name="authentication"></a>Verificatie

Aanvragen voor de REST-API moeten gebruiken **basisverificatie**, zodat u de beheerder de naam van HDInsight-cluster en het wachtwoord gebruiken.

> [!NOTE]
> Omdat basisverificatie wordt verzonden met behulp van niet-versleutelde tekst, moet u **altijd** HTTPS gebruikt voor het beveiligen van communicatie met het cluster.

### <a name="return-values"></a>Retourwaarden

Informatie die wordt geretourneerd vanaf de REST-API kan alleen worden gebruikt vanuit binnen het cluster of de virtuele machines op de hetzelfde virtuele Azure-netwerk als het cluster. Bijvoorbeeld, de volledig gekwalificeerde domeinnaam (FQDN) geretourneerd voor [Apache ZooKeeper](https://zookeeper.apache.org/) servers zijn niet toegankelijk vanaf Internet.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over het implementeren en topologieën bewaken met behulp van de Storm-Dashboard kunt u meer informatie over het:

* [C#-topologieën met behulp van de HDInsight-hulpprogramma's voor Visual Studio ontwikkelen](apache-storm-develop-csharp-visual-studio-topology.md)

* [Op Java gebaseerde topologieën ontwikkelen met Apache Maven](apache-storm-develop-java-topology.md)

Zie voor een lijst van meer voorbeeldtopologieën, [voorbeeldtopologieën van Apache Storm op HDInsight](apache-storm-example-topology.md).

[hdinsight-dashboard]: ./media/apache-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/apache-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/apache-storm-deploy-monitor-topology/storm-ui-summary.png
