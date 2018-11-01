---
title: Gebeurtenissen verwerken van Event Hubs met Storm - Azure HDInsight
description: Leer hoe u gegevens van Azure Event Hubs verwerken met een C# Storm-topologie die in Visual Studio zijn gemaakt met behulp van de HDInsight-hulpprogramma's voor Visual Studio.
services: hdinsight,notification hubs
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: cda21c5b0d06335b13ba1aff0e9113544ca11c80
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414196"
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Process events from Azure Event Hubs met Storm op HDInsight (C#) gebeurtenissen

Meer informatie over het werken met Azure Event Hubs van Apache Storm op HDInsight. Dit document maakt gebruik van een C# Storm-topologie te lezen en schrijven van gegevens uit Event Hubs

> [!NOTE]
> Zie voor een Java-versie van dit project [gebeurtenissen uit Azure Event Hubs met Storm op HDInsight (Java) verwerkt](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/).

## <a name="scpnet"></a>SCP.NET

De stappen in dit document gebruiken SCP.NET, een NuGet-pakket waarmee u eenvoudig te maken van C#-topologieën en -onderdelen voor gebruik met Storm op HDInsight.

> [!IMPORTANT]
> Hoewel de stappen in dit document, is afhankelijk van een Windows-ontwikkelomgeving met Visual Studio, kan het project gecompileerd worden verstuurd naar een Storm op HDInsight-cluster dat gebruik maakt van Linux. Op basis van Linux-clusters die zijn gemaakt na 28 oktober 2016 vallen, ondersteunen alleen SCP.NET-topologieën.

HDInsight 3.4 en hoger gebruik Mono om uit te voeren van C#-topologieën. Het voorbeeld in dit document gebruikt werkt met HDInsight 3.6. Als u van plan bent over het maken van uw eigen .NET-oplossingen voor HDInsight, controleert u de [Mono-compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/) document voor mogelijke compatibiliteitsproblemen.

### <a name="cluster-versioning"></a>Cluster-versiebeheer

Het Microsoft.scp.NET.SDK dat NuGet-pakket dat u voor uw project moet overeenkomen met de belangrijkste versie van Storm op HDInsight is geïnstalleerd. Storm voor HDInsight versie 3.5 en 3.6 gebruiken 1.x, dus moet u SCP.NET versie 1.0.x.x met deze clusters.

> [!IMPORTANT]
> Het voorbeeld in dit document wordt verwacht dat een HDInsight 3.5 of 3.6-cluster.
>
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

C#-topologieën ook verwijst naar .NET 4.5.

## <a name="how-to-work-with-event-hubs"></a>Over het werken met Event Hubs

Microsoft biedt een set van Java-onderdelen die kunnen worden gebruikt om te communiceren met Event Hubs in een Storm-topologie. U vindt het Java-archief (JAR)-bestand met een compatibele versie van HDInsight 3.6 van deze onderdelen op [ https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar ](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

> [!IMPORTANT]
> Terwijl de onderdelen worden geschreven in Java, kunt u ze eenvoudig vanuit een C#-topologie gebruiken.

De volgende onderdelen worden gebruikt in dit voorbeeld:

* __EventHubSpout__: leest gegevens uit Event Hubs.
* __Eventhubbolt die worden__: schrijft gegevens naar Event Hubs.
* __EventHubSpoutConfig__: gebruikt voor het configureren van EventHubSpout.
* __EventHubBoltConfig__: het configureren van eventhubbolt die worden gebruikt.

### <a name="example-spout-usage"></a>Voorbeeld spout gebruik

SCP.NET biedt methoden voor het toevoegen van een EventHubSpout aan uw topologie. Deze methoden wordt het eenvoudiger om toe te voegen een spout dan het gebruik van de algemene methoden voor het toevoegen van een Java-component. Het volgende voorbeeld ziet u hoe u een spout maken met behulp van de __SetEventHubSpout__ en **EventHubSpoutConfig** methoden die door SCP.NET:

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

Het vorige voorbeeld wordt een nieuw spout-onderdeel met de naam __EventHubSpout__, en configureert deze om te communiceren met een event hub. De hint parallelle uitvoering voor de component is ingesteld op het aantal partities in de event hub. Deze instelling kunt Storm om een exemplaar van het onderdeel voor elke partitie te maken.

### <a name="example-bolt-usage"></a>Gebruik van de voorbeeld-bolt

Gebruik de **JavaComponmentConstructor** methode voor het maken van een exemplaar van de bolt. Het volgende voorbeeld ziet u hoe u maken en configureren van een nieuw exemplaar van de **eventhubbolt die worden**:

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
> In dit voorbeeld wordt een Clojure expressie doorgegeven als een tekenreeks, in plaats van **JavaComponentConstructor** maken een **EventHubBoltConfig**, zoals in het voorbeeld spout heeft. Een van beide methoden werkt. Gebruik de methode die u het beste werkt.

## <a name="download-the-completed-project"></a>Het voltooide project downloaden

U kunt een volledige versie van het project gemaakt in deze zelfstudie uit downloaden [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). U moet echter wel voor configuratie-instellingen met de volgende stappen in deze zelfstudie.

### <a name="prerequisites"></a>Vereisten

* Een [Apache Storm op HDInsight-clusterversie 3.5 of 3.6](apache-storm-tutorial-get-started-linux.md).

    > [!WARNING]
    > Het voorbeeld in dit document is Storm op HDInsight versie 3.5 of 3.6 vereist. Dit werkt niet met oudere versies van HDInsight, vanwege de belangrijke wijzigingen voor klasse-naam. Zie voor een versie van dit voorbeeld dat met oudere clusters buiten bedrijf werkt [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* Een [Azure event hub](../../event-hubs/event-hubs-create.md).

* De [Azure .NET SDK](http://azure.microsoft.com/downloads/).

* De [HDInsight tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

* Java JDK 1.8 of hoger op uw ontwikkelomgeving. JDK downloads zijn beschikbaar op [Oracle](https://aka.ms/azure-jdks).

  * De **JAVA_HOME** omgevingsvariabele moet verwijzen naar de map met Java.
  * De **%JAVA_HOME%/bin** directory in het pad moet worden uitgevoerd.

## <a name="download-the-event-hubs-components"></a>De Event Hubs-onderdelen downloaden

Download de spout Event Hubs en bolt onderdeel van de [ https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar ](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

Maak een map met de naam `eventhubspout`, en sla het bestand naar de map.

## <a name="configure-event-hubs"></a>Eventhubs configureren

Eventhubs is de gegevensbron voor dit voorbeeld. Gebruik de informatie in de sectie 'Een event hub maken' van [aan de slag met Event Hubs](../../event-hubs/event-hubs-create.md).

1. Nadat de event hub is gemaakt, geven de **EventHub** instellingen in de Azure portal en selecteer **beleid voor gedeelde toegang**. Selecteer **+ toevoegen** om toe te voegen van de volgende beleidsregels:

   | Naam | Machtigingen |
   | --- | --- |
   | schrijver |Verzenden |
   | lezer |Luisteren |

    ![Schermafbeelding van de Share beleid Toegangsvenster](./media/apache-storm-develop-csharp-event-hub-topology/sas.png)

2. Selecteer de **lezer** en **schrijver** beleid. Kopieer en bewaar de waarde van de primaire sleutel voor beide beleidsregels, zoals deze waarden worden later gebruikt.

## <a name="configure-the-eventhubwriter"></a>De EventHubWriter configureren

1. Als u de meest recente versie van de HDInsight-hulpprogramma's zijn niet voor Visual Studio hebt geïnstalleerd, raadpleegt u [aan de slag met HDInsight tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Download de oplossing van [eventhub-storm-hybride](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

3. In de **EventHubWriter** project, open de **App.config** bestand. Gebruik de informatie uit de event hub die u eerder hebt geconfigureerd om in te vullen in de waarde voor de volgende sleutels:

   | Sleutel | Waarde |
   | --- | --- |
   | EventHubPolicyName |schrijver (als u een andere naam voor het beleid met gebruikt *verzenden* machtiging, in plaats daarvan gebruiken.) |
   | EventHubPolicyKey |De sleutel voor de schrijver-beleid. |
   | EventHubNamespace |De naamruimte waarin uw event hub. |
   | EventHubName |De naam van de event hub. |
   | EventHubPartitionCount |Het aantal partities in de event hub. |

4. Opslaan en sluiten de **App.config** bestand.

## <a name="configure-the-eventhubreader"></a>De EventHubReader configureren

1. Open de **EventHubReader** project.

2. Open de **App.config** bestand voor de **EventHubReader**. Gebruik de informatie uit de event hub die u eerder hebt geconfigureerd om in te vullen in de waarde voor de volgende sleutels:

   | Sleutel | Waarde |
   | --- | --- |
   | EventHubPolicyName |lezer (als u een andere naam voor het beleid met gebruikt *luisteren* machtiging, in plaats daarvan gebruiken.) |
   | EventHubPolicyKey |De sleutel voor het beleid van de lezer. |
   | EventHubNamespace |De naamruimte waarin uw event hub. |
   | EventHubName |De naam van de event hub. |
   | EventHubPartitionCount |Het aantal partities in de event hub. |

3. Opslaan en sluiten de **App.config** bestand.

## <a name="deploy-the-topologies"></a>De topologieën implementeren

1. Van **Solution Explorer**, met de rechtermuisknop op de **EventHubReader** project en selecteer **indienen bij Storm op HDInsight**.

    ![Schermopname van Solution Explorer met Storm op HDInsight gemarkeerd indienen](./media/apache-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Op de **indienen topologie** in het dialoogvenster, selecteer uw **Storm-Cluster**. Vouw **aanvullende configuraties**, selecteer **Java bestandspaden**, selecteer **...** , en selecteer de map waarin het JAR-bestand dat u eerder hebt gedownload. Klik tot slot, **indienen**.

    ![Schermafbeelding van indienen topologie van het dialoogvenster](./media/apache-storm-develop-csharp-event-hub-topology/submit.png)

3. Wanneer de topologie is ingediend, de **Storm-topologieën Viewer** wordt weergegeven. Als u informatie over de topologie, selecteer de **EventHubReader** topologie in het linkerdeelvenster.

    ![Schermafbeelding van Viewer voor Storm-topologieën](./media/apache-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. Van **Solution Explorer**, met de rechtermuisknop op de **EventHubWriter** project en selecteer **indienen bij Storm op HDInsight**.

5. Op de **indienen topologie** in het dialoogvenster, selecteer uw **Storm-Cluster**. Vouw **aanvullende configuraties**, selecteer **Java bestandspaden**, selecteer **...** , en selecteer de map waarin het JAR-bestand dat u eerder hebt gedownload. Klik tot slot, **indienen**.

6. Als de topologie is verzonden, vernieuw de lijst van de topologie in de **Storm-topologieën Viewer** om te controleren of dat beide topologieën worden uitgevoerd op het cluster.

7. In **Storm-topologieën Viewer**, selecteer de **EventHubReader** topologie.

8. Als u wilt openen van het overzicht voor de bolt onderdeel, dubbelklikt u op de **LogBolt** onderdeel in het diagram.

9. In de **Executor** sectie, selecteer een van de koppelingen in de **poort** kolom. U ziet nu informatie wordt vastgelegd door het onderdeel. De vastgelegde informatie is vergelijkbaar met de volgende tekst:

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-the-topologies"></a>De topologieën stoppen

Als u wilt de topologieën stoppen, selecteert u elke topologie in de **Storm-topologie Viewer**, klikt u vervolgens op **Kill**.

![Schermafbeelding van de Storm-topologie-Viewer met Kill knop gemarkeerd](./media/apache-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Verwijder uw cluster

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u de Java Event Hubs-spout en bolt uit een C#-topologie om te werken met gegevens in Azure Event Hubs. Voor meer informatie over het maken van C#-topologieën, Zie de volgende:

* [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)
* [SCP-programmeergids](apache-storm-scp-programming-guide.md)
* [Voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md)
