---
title: Verwerken van gebeurtenissen van Event Hubs met Storm - Azure HDInsight | Microsoft Docs
description: Informatie over het verwerken van gegevens uit Azure Event Hubs met een C# Storm-topologie in Visual Studio gemaakt met behulp van de HDInsight tools voor Visual Studio.
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: larryfr
ms.openlocfilehash: 9ad160377a8779ae917e6fd2d605ee01b12c3e2a
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Procesgebeurtenissen van Azure Event Hubs met Storm op HDInsight (C#)

Informatie over het werken met Azure Event Hubs van Apache Storm op HDInsight. Dit document wordt een C# Storm-topologie te lezen en schrijven van gegevens uit Evbent Hubs

> [!NOTE]
> Zie voor een Java-versie van dit project, [verwerken van gebeurtenissen van Azure Event Hubs met Storm op HDInsight (Java)](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/).

## <a name="scpnet"></a>SCP.NET

De stappen in dit document SCP.NET, een NuGet-pakket waarmee u eenvoudig te maken van C#-topologieën en -onderdelen voor gebruik met Storm op HDInsight kunt gebruiken.

> [!IMPORTANT]
> Terwijl de stappen in dit document, is afhankelijk van een Windows-ontwikkelomgeving met Visual Studio, kan het project gecompileerd naar een Storm op HDInsight-cluster dat gebruik maakt van Linux worden verzonden. Op basis van Linux-clusters die zijn gemaakt na 28 oktober 2016 ondersteunen alleen SCP.NET topologieën.

HDInsight 3.4 en groter gebruiken Mono voor het uitvoeren van C#-topologieën. Het voorbeeld in dit document gebruikt werkt met HDInsight 3.6. Als u van plan om uw eigen .NET-oplossingen voor HDInsight te maken bent, controleert u de [Mono compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/) document voor potentiële compatibiliteitsproblemen.

> [!WARNING]
> Als u problemen bouwen van de projecten die gebruikmaken van SCP.NET versie 1.0.0.x, neem contact op met Microsoft ondersteuning voor hulp.

### <a name="cluster-versioning"></a>Cluster-versies

Het Microsoft.SCP.Net.SDK NuGet-pakket dat u voor uw project gebruikt moet overeenkomen met de primaire versie van Storm op HDInsight is geïnstalleerd. HDInsight versie 3.5 en 3.6 Storm gebruiken 1.x, dus moet u SCP.NET versie 1.0.x.x met deze clusters gebruiken.

> [!IMPORTANT]
> In het voorbeeld in dit document verwacht een HDInsight-3.5 of 3,6 cluster.
>
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

C#-topologieën moeten ook .NET 4.5 gericht.

## <a name="how-to-work-with-event-hubs"></a>Werken met Event Hubs

Microsoft biedt een set van Java-onderdelen die kunnen worden gebruikt om te communiceren met Event Hubs van een Storm-topologie. U vindt de Java-archief (JAR)-bestand met een compatibele versie van HDInsight 3.6 van deze onderdelen bij [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

> [!IMPORTANT]
> Terwijl de onderdelen zijn geschreven in Java, kunt u deze eenvoudig gebruiken van een C#-topologie.

De volgende onderdelen worden gebruikt in dit voorbeeld:

* __EventHubSpout__: leest de gegevens uit Event Hubs.
* __EventHubBolt__: schrijft gegevens naar Event Hubs.
* __EventHubSpoutConfig__: gebruikt voor het configureren van EventHubSpout.
* __EventHubBoltConfig__: gebruikt voor het configureren van EventHubBolt.

### <a name="example-spout-usage"></a>Voorbeeld van spout syntaxis

SCP.NET biedt methoden voor het toevoegen van een EventHubSpout aan uw topologie. Deze methoden kunnen gemakkelijker toevoegen van een spout dan het gebruik van de algemene methoden voor het toevoegen van een Java-component. Het volgende voorbeeld laat zien hoe een spout maken met behulp van de __SetEventHubSpout__ en **EventHubSpoutConfig** methoden van SCP.NET:

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

Het vorige voorbeeld maakt u een nieuw spout-onderdeel met de naam __EventHubSpout__, en geconfigureerd om te communiceren met een event hub. De parallelle uitvoering geheugensteun voor het onderdeel is ingesteld op het aantal partities in de event hub. Deze instelling kunt Storm een exemplaar van het onderdeel voor elke partitie te maken.

### <a name="example-bolt-usage"></a>Voorbeeld van bolt syntaxis

Gebruik de **JavaComponmentConstructor** methode voor het maken van een exemplaar van de bolt. Het volgende voorbeeld toont hoe maken en configureren van een nieuw exemplaar van de **EventHubBolt**:

```csharp
// Java construcvtor for the Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set the bolt to subscribe to data from the spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> In dit voorbeeld wordt een Clojure expressie doorgegeven als een tekenreeks, in plaats van **JavaComponentConstructor** maken een **EventHubBoltConfig**, zoals in het voorbeeld spout heeft. De methode werkt. Gebruik de methode die u het beste werkt.

## <a name="download-the-completed-project"></a>Downloaden voltooid project

U kunt een volledige versie van het project gemaakt in deze zelfstudie uit downloaden [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). U moet echter nog steeds configuratieinstellingen opgeven door de stappen in deze zelfstudie te volgen.

### <a name="prerequisites"></a>Vereisten

* Een [Apache Storm op HDInsight-cluster versie 3.5 of 3.6](apache-storm-tutorial-get-started-linux.md).

    > [!WARNING]
    > Het voorbeeld in dit document gebruikt vereist Storm op HDInsight versie 3.5 of 3.6. Dit werkt niet met oudere versies van HDInsight, als gevolg van wijzigingen in klasse de naam op te splitsen. Zie voor een versie van dit voorbeeld dat met oudere clusters werkt [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* Een [Azure event hub](../../event-hubs/event-hubs-create.md).

* De [Azure .NET SDK](http://azure.microsoft.com/downloads/).

* De [HDInsight tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

* Java JDK 1.8 of hoger op uw ontwikkelomgeving. JDK downloads zijn beschikbaar op [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

  * De **JAVA_HOME** omgevingsvariabele moet verwijzen naar de map waarin zich Java.
  * De **%JAVA_HOME%/bin** directory in het pad moet worden uitgevoerd.

## <a name="download-the-event-hubs-components"></a>De Event Hubs-onderdelen downloaden

Download de spout Event Hubs en Bolts onderdeel uit [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

Maak een map met de naam `eventhubspout`, en sla het bestand naar de map.

## <a name="configure-event-hubs"></a>Event Hubs configureren

Event Hubs is de gegevensbron voor dit voorbeeld. Gebruik de informatie in de sectie 'Een event hub maken' van [aan de slag met Event Hubs](../../event-hubs/event-hubs-create.md).

1. Nadat de event hub is gemaakt, weergeven de **EventHub** blade in de Azure portal en selecteer **gedeeld toegangsbeleid**. Selecteer **+ toevoegen** toevoegen van het volgende beleid:

   | Naam | Machtigingen |
   | --- | --- |
   | schrijver |Verzenden |
   | lezer |Luisteren |

    ![Schermafbeelding van de Share access beleid-venster](./media/apache-storm-develop-csharp-event-hub-topology/sas.png)

2. Selecteer de **lezer** en **writer** beleid. Kopieer en de waarde van de primaire sleutel voor beide beleidsregels niet opslaan omdat deze waarden later worden gebruikt.

## <a name="configure-the-eventhubwriter"></a>De EventHubWriter configureren

1. Als u hebt niet is gebeurd de nieuwste versie van de HDInsight tools voor Visual Studio, Zie [aan de slag met HDInsight tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Downloaden van de oplossing van [eventhub-storm-hybride](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

3. In de **EventHubWriter** project, open de **App.config** bestand. Gebruik de informatie van de event hub die u eerder hebt geconfigureerd in de waarde voor de volgende sleutels te vullen:

   | Sleutel | Waarde |
   | --- | --- |
   | EventHubPolicyName |schrijver (als u een andere naam voor het beleid met gebruikt *verzenden* toestemming hebben, in plaats daarvan gebruikt.) |
   | EventHubPolicyKey |De sleutel voor het beleid writer. |
   | EventHubNamespace |De naamruimte die uw event hub bevat. |
   | EventHubName |De naam van de event hub. |
   | EventHubPartitionCount |Het aantal partities in de event hub. |

4. Sla op en sluit de **App.config** bestand.

## <a name="configure-the-eventhubreader"></a>De EventHubReader configureren

1. Open de **EventHubReader** project.

2. Open de **App.config** bestand voor de **EventHubReader**. Gebruik de informatie van de event hub die u eerder hebt geconfigureerd in de waarde voor de volgende sleutels te vullen:

   | Sleutel | Waarde |
   | --- | --- |
   | EventHubPolicyName |lezer (als u een andere naam voor het beleid met gebruikt *luisteren* toestemming hebben, in plaats daarvan gebruikt.) |
   | EventHubPolicyKey |De sleutel voor het beleid van de lezer. |
   | EventHubNamespace |De naamruimte die uw event hub bevat. |
   | EventHubName |De naam van de event hub. |
   | EventHubPartitionCount |Het aantal partities in de event hub. |

3. Sla op en sluit de **App.config** bestand.

## <a name="deploy-the-topologies"></a>De topologieën implementeren

1. Van **Solution Explorer**, met de rechtermuisknop op de **EventHubReader** project en selecteer **indienen Storm op HDInsight**.

    ![Schermopname van Solution Explorer met Storm op HDInsight gemarkeerd indienen](./media/apache-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Op de **topologie indienen** in het dialoogvenster, selecteer uw **Storm-Cluster**. Vouw **aanvullende configuraties**, selecteer **Java bestandspaden**, selecteer **...** , en selecteer de map waarin het JAR-bestand dat u eerder hebt gedownload. Tot slot op **indienen**.

    ![Dialoogvenster Schermafbeelding van de indienings-topologie](./media/apache-storm-develop-csharp-event-hub-topology/submit.png)

3. Wanneer de topologie is ingediend, de **Storm-topologieën Viewer** wordt weergegeven. Als u informatie over de topologie, selecteer de **EventHubReader** topologie in het linkerdeelvenster.

    ![Schermopname van Storm-topologieën Viewer](./media/apache-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. Van **Solution Explorer**, met de rechtermuisknop op de **EventHubWriter** project en selecteer **indienen Storm op HDInsight**.

5. Op de **topologie indienen** in het dialoogvenster, selecteer uw **Storm-Cluster**. Vouw **aanvullende configuraties**, selecteer **Java bestandspaden**, selecteer **...** , en selecteer de map waarin het JAR-bestand dat u eerder hebt gedownload. Tot slot op **indienen**.

6. Wanneer de topologie is ingediend, vernieuw de lijst topologie in de **Storm-topologieën Viewer** om te controleren dat beide topologieën worden uitgevoerd op het cluster.

7. In **Storm-topologieën Viewer**, selecteer de **EventHubReader** topologie.

8. Als u wilt de samenvatting voor de bolt component openen, dubbelklikt u op de **LogBolt** onderdeel in het diagram.

9. In de **Executor** sectie, selecteer een van de koppelingen in de **poort** kolom. U ziet nu informatie die wordt geregistreerd door het onderdeel. De geregistreerde gegevens is vergelijkbaar met de volgende tekst:

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-the-topologies"></a>De topologieën stoppen

Als u wilt de topologieën stoppen, selecteert u elke topologie in de **Storm-topologie Viewer**, klikt u vervolgens op **Kill**.

![Schermopname van Storm-topologie-Viewer met Kill knop gemarkeerd](./media/apache-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Uw cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe gebruikt u de spout Java Event Hubs en Bolts uit een C#-topologie wilt werken met gegevens in Azure Event Hubs. Zie de volgende onderwerpen voor meer informatie over het maken van C#-topologieën:

* [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)
* [Programmeerhandleiding voor SCP](apache-storm-scp-programming-guide.md)
* [Voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md)
