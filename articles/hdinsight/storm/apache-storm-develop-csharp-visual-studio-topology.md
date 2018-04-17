---
title: Apache Storm-topologieën met Visual Studio en C# - Azure HDInsight | Microsoft Docs
description: Informatie over het maken van Storm-topologieën in C#. Een eenvoudige word-count-topologie in Visual Studio maakt met behulp van de Hadoop-hulpprogramma's voor Visual Studio.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: ''
ms.devlang: java
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 1e35d795fb65c837f7a4152920f701da5bf8f506
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="develop-c-topologies-for-apache-storm-by-using-the-data-lake-tools-for-visual-studio"></a>C#-topologieën ontwikkelen voor Apache Storm met behulp van het Data Lake tools voor Visual Studio

Informatie over het maken van een C# Storm-topologie met behulp van de Azure Data Lake (Hadoop)-hulpprogramma's voor Visual Studio. Dit document helpt bij het proces van het maken van een Storm-project in Visual Studio, lokaal te testen en deze implementeert in een Apache Storm op Azure HDInsight-cluster.

U leert ook hoe u hybride topologieën maken die met C# en Java-onderdelen.

> [!NOTE]
> Terwijl de stappen in dit document, is afhankelijk van een Windows-ontwikkelomgeving met Visual Studio, kan de gecompileerde project worden verzonden naar een Linux- of Windows gebaseerde HDInsight-cluster. Op basis van Linux-clusters die zijn gemaakt na 28 oktober 2016 ondersteunen alleen SCP.NET topologieën.

Als u wilt gebruiken een C#-topologie met een cluster op basis van Linux, moet u het Microsoft.SCP.Net.SDK NuGet-pakket gebruikt door uw project naar versie 0.10.0.6 of hoger bijwerken. De versie van het pakket moet ook overeenkomen met de primaire versie van Storm die op HDInsight is geïnstalleerd.

| HDInsight-versie | Storm-versie | SCP.NET versie | Standaard Mono-versie |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.3 |0.10.x |0.10.x.x</br>(alleen op HDInsight op basis van Windows) | N.v.t. |
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3,5 | 1.0.2.x | 1.0.0.x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0.x | 4.2.8 |

> [!IMPORTANT]
> C#-topologieën met op Linux gebaseerde clusters moeten .NET 4.5 en Mono gebruiken om op het HDInsight-cluster te worden uitgevoerd. Controleer [Mono compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/) voor potentiële compatibiliteitsproblemen.

## <a name="install-visual-studio"></a>Visual Studio installeren

U kunt de C#-topologieën met SCP.NET ontwikkelen met behulp van een van de volgende versies van Visual Studio:

* Visual Studio 2012 met [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

* Visual Studio 2013 met [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) of [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)

* Visual Studio 2015 of [Visual Studio 2015-Community](https://go.microsoft.com/fwlink/?LinkId=532606)

* Visual Studio 2017 (alle versies)

## <a name="install-data-lake-tools-for-visual-studio"></a>Installatie van Data Lake tools voor Visual Studio

Volg de stappen in voor de installatie van Data Lake tools voor Visual Studio, [aan de slag met Data Lake tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

## <a name="install-java"></a>Java installeren

Wanneer u een Storm-topologie vanuit Visual Studio indient, genereert SCP.NET een zip-bestand dat de topologie en de afhankelijkheden bevat. Java wordt gebruikt voor het maken van deze zip-bestanden omdat deze gebruikmaakt van een indeling die is beter geschikt voor clusters op basis van Linux.

1. Installeer de Java Developer Kit (JDK) 7 of hoger op uw ontwikkelomgeving. U krijgt de Oracle-JDK van [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html). U kunt ook [andere Java-distributies](http://openjdk.java.net/).

2. De `JAVA_HOME` omgevingsvariabele moet verwijzen naar de map waarin zich Java.

3. De `PATH` omgevingsvariabele moet bevatten de `%JAVA_HOME%\bin` directory.

U kunt de volgende C#-consoletoepassing gebruiken om te controleren of Java en de JDK correct zijn geïnstalleerd:

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a>Storm-sjablonen

De Data Lake tools voor Visual Studio bieden de volgende sjablonen:

| Projecttype | Demonstreert |
| --- | --- |
| Storm-toepassing |Een leeg Storm-topologie-project. |
| Storm Azure SQL Writer voorbeeld |Het schrijven naar Azure SQL Database. |
| Voorbeeld van storm Azure Cosmos DB lezer |Klik hier voor meer informatie over het lezen van de Azure Cosmos-database. |
| Storm Azure Cosmos DB Writer voorbeeld |Het schrijven naar Azure Cosmos DB. |
| Voorbeeld van storm EventHub-lezer |Klik hier voor meer informatie over het lezen uit Azure Event Hubs. |
| Voorbeeld van storm EventHub-schrijver |Het schrijven naar Azure Event Hubs. |
| Voorbeeld van storm-HBase-lezer |Klik hier voor meer informatie over het lezen van HBase op HDInsight-clusters. |
| Voorbeeld van storm-HBase-schrijver |Het schrijven naar HBase op HDInsight-clusters. |
| Storm hybride voorbeeld |Het gebruik van een Java-component. |
| Storm-voorbeeld |Een basic word-count-topologie. |

> [!WARNING]
> Niet alle sjablonen werken met HDInsight op basis van Linux. NuGet-pakketten die worden gebruikt door de sjablonen zijn mogelijk niet compatibel met Mono. Controleer de [Mono compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/) documenteren en gebruik de [.NET draagbaarheid Analyzer](../hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) om potentiële problemen te identificeren.

In de stappen in dit document kunt u het type basic Storm-toepassing een topologie maakt.

### <a name="hbase-templates-notes"></a>Opmerkingen bij de HBase-sjablonen

De HBase REST API, niet de HBase Java API de HBase-lezer en -schrijver sjablonen gebruiken om te communiceren met een HBase op HDInsight-cluster.

### <a name="eventhub-templates-notes"></a>Opmerkingen bij de EventHub-sjablonen

> [!IMPORTANT]
> De EventHub Java gebaseerde spout onderdeel van de lezer van de EventHub-sjabloon werkt mogelijk niet met Storm op HDInsight versie 3.5 of hoger. Een bijgewerkte versie van dit onderdeel is beschikbaar op [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).

Zie voor een topologie die gebruikmaakt van dit onderdeel en werkt met Storm op HDInsight 3.5, [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>Maak een C#-topologie

1. Open Visual Studio, selecteer **bestand** > **nieuw**, en selecteer vervolgens **Project**.

2. Van de **nieuw Project** venster Vouw **geïnstalleerde** > **sjablonen**, en selecteer **Azure Data Lake**. Selecteer in de lijst met sjablonen **Storm-toepassing**. Voer op de onderkant van het scherm **WordCount** als de naam van de toepassing.

    ![Schermopname van nieuw Project-venster](./media/apache-storm-develop-csharp-visual-studio-topology/new-project.png)

3. Nadat u het project hebt gemaakt, hebt u de volgende bestanden:

   * **Program.cs**: dit bestand definieert de topologie voor uw project. Standaard wordt een standaard-topologie die uit één spout en één bolt bestaat gemaakt.

   * **Spout.cs**: een voorbeeld spout die willekeurige getallen verzendt.

   * **Bolt.cs**: een voorbeeld-bolt die een aantal van de getallen die worden verzonden door de spout houdt.

     Wanneer u het project maakt, downloadt u de meest recente NuGet [SCP.NET pakket](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).

     [!INCLUDE [scp.net version important](../../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-the-spout"></a>De spout implementeren

1. Open **Spout.cs**. Spouts worden gebruikt om gegevens te lezen in een topologie van een externe bron. De belangrijkste onderdelen van een spout zijn:

   * **NextTuple**: door Storm wordt aangeroepen wanneer de spout is toegestaan voor het verzenden van nieuwe tuples.

   * **ACK** (alleen voor transactionele topologie): bevestigingen geïnitieerd door de andere onderdelen in de topologie voor tuples verzonden vanaf de spout verwerkt. Een tuple zijn bevestigd, kunt de spout weten dat deze is verwerkt door downstream-onderdelen.

   * **Mislukken** (alleen voor transactionele topologie): tuples die zijn mislukt-verwerking van andere onderdelen in de topologie worden verwerkt. Implementatie van een methode is mislukt, kunt u de tuple opnieuw verzenden zodat deze opnieuw kan worden verwerkt.

2. Vervang de inhoud van de **Spout** klasse met de volgende tekst: een zin deze spout willekeurig verzendt naar de topologie.

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "the cow jumped over the moon",
        "an apple a day keeps the doctor away",
        "four score and seven years ago",
        "snow white and the seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set the instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // The schema for the default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of the spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // The sentence to be emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-the-bolts"></a>De bolts implementeren

1. Verwijder de bestaande **Bolt.cs** bestand van het project.

2. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **toevoegen** > **nieuw item**. Selecteer in de lijst **Storm Bolt**, en voer **Splitter.cs** als de naam. Herhaal dit proces voor het maken van een tweede bolt met de naam **Counter.cs**.

   * **Splitter.cs**: implementeert een bolt die zinnen splitst in afzonderlijke woorden en verzendt een nieuwe reeks woorden.

   * **Counter.cs**: implementeert een bolt die elk woord telt en verzendt een nieuwe reeks woorden en de telling voor elk woord.

     > [!NOTE]
     > Deze bolts lezen en schrijven naar stromen, maar u kunt ook een bolt gebruiken om te communiceren met bronnen zoals een database of service.

3. Open **Splitter.cs**. Slechts één methode heeft standaard: **Execute**. De Execute-methode wordt aangeroepen wanneer de bolt een tuple voor verwerking ontvangt. Hier vindt u binnenkomende tuples verwerken en verzenden van uitgaande tuples.

4. Vervang de inhoud van de **splitser** klasse met de volgende code:

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (the sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (the word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of the bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the sentence from the tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. Open **Counter.cs**, en vervang de inhoud van de klasse door de volgende code:

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - the word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - the word and the word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the word from the tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for the word in the dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment the count
        count++;
        // Update the count in the dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit the word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-the-topology"></a>De topologie definiëren

Spouts en bolts worden gerangschikt in een grafiek, waarmee wordt gedefinieerd hoe de gegevens tussen onderdelen loopt. Voor deze topologie wordt is de grafiek als volgt uit:

![Diagram van hoe onderdelen worden gerangschikt](./media/apache-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Zinnen zijn verzonden vanaf de spout en worden gedistribueerd naar exemplaren van de splitser bolt. De splitser bout verdeelt de zinnen in woorden die worden gedistribueerd naar de teller bolt.

Omdat het aantal woorden is die lokaal zijn opgeslagen in het exemplaar van prestatiemeteritem, u om ervoor te zorgen dat bepaalde woorden naar hetzelfde exemplaar van prestatiemeteritem bolt vloeien. Elk exemplaar houdt van specifieke woorden. Aangezien de splitser bout geen status onderhoudt, het echt maakt niet uit welke instantie van de splitser bepaald welke zin ontvangt.

Open **Program.cs**. De methode voor belangrijke **GetTopologyBuilder**, dat wordt gebruikt voor het definiëren van de topologie die wordt verzonden naar Storm. Vervang de inhoud van **GetTopologyBuilder** met de volgende code voor het implementeren van de topologie die eerder zijn beschreven:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add the spout to the topology.
// Name the component 'sentences'
// Name the field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add the splitter bolt to the topology.
// Name the component 'splitter'
// Name the field that is emitted 'word'
// Use suffleGrouping to distribute incoming tuples
//   from the 'sentences' spout across instances
//   of the splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add the counter bolt to the topology.
// Name the component 'counter'
// Name the fields that are emitted 'word' and 'count'
// Use fieldsGrouping to ensure that tuples are routed
//   to counter instances based on the contents of field
//   position 0 (the word). This could also have been
//   List<string>(){"word"}.
//   This ensures that the word 'jumped', for example, will always
//   go to the same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-the-topology"></a>De topologie verzenden

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **indienen Storm op HDInsight**.

   > [!NOTE]
   > Als u wordt gevraagd, voert u de referenties voor uw Azure-abonnement. Als u meer dan één abonnement hebt, moet u zich aanmelden bij de database met uw Storm op HDInsight-cluster.

2. Selecteer uw Storm op HDInsight-cluster van de **Storm-Cluster** vervolgkeuzelijst en selecteer vervolgens **indienen**. U kunt controleren als de verzending voltooid met is de **uitvoer** venster.

3. Wanneer de topologie is ingediend, de **Storm-topologieën** voor het cluster moet worden weergegeven. Selecteer de **WordCount** topologie in de lijst om informatie over de actieve topologie weer te geven.

   > [!NOTE]
   > U kunt ook weergeven **Storm-topologieën** van **Server Explorer**. Vouw **Azure** > **HDInsight**, met de rechtermuisknop op een Storm op HDInsight-cluster en selecteer vervolgens **weergave Storm-topologieën**.

    Als u informatie over de onderdelen in de topologie, dubbelklikt u op het onderdeel in het diagram.

4. Van de **Topology Summary** weergeven, klikt u op **Kill** stoppen van de topologie.

   > [!NOTE]
   > Storm-topologieën blijven actief totdat ze worden gedeactiveerd of het cluster wordt verwijderd.

## <a name="transactional-topology"></a>Transactionele-topologie

De vorige topologie is niet-transactionele. Functionaliteit voor het afspelen van berichten wordt niet geïmplementeerd door de onderdelen in de topologie. Voor een voorbeeld van een transactionele topologie, maak een project en selecteer **Storm voorbeeld** als het projecttype.

Transactionele topologieën implementeren de volgende ter ondersteuning van replay van gegevens:

* **Metagegevens opslaan in cache**: de spout moet slaan metagegevens over de gegevens verzonden, zodat de gegevens worden opgehaald en opnieuw verzonden als er een fout optreedt. Omdat de gegevens die door het voorbeeld klein is, wordt de onbewerkte gegevens voor elke tuple opgeslagen in een woordenlijst door voor opnieuw afspelen.

* **ACK**: elke bolt in de topologie kunt aanroepen `this.ctx.Ack(tuple)` om te bevestigen dat deze is een tuple verwerkt. Wanneer alle bolts bevestigd de tuple hebt de `Ack` methode van de spout wordt aangeroepen. De `Ack` methode kunt u de spout om gegevens in de cache om replaydetectie is te verwijderen.

* **Mislukken**: elke bolt kunt aanroepen `this.ctx.Fail(tuple)` om aan te geven dat de verwerking is mislukt voor een tuple. De fout die is doorgegeven aan de `Fail` methode van de spout, waarbij de tuple kan worden cookies met behulp van metagegevens in cache.

* **Sequentiëren ID**: bij het genereren van een tuple een unieke reeks-ID kan worden opgegeven. Deze waarde geeft de tuple voor de verwerking van opnieuw afspelen (Ack en mislukken). Bijvoorbeeld, de spout in de **Storm voorbeeld** project wordt het volgende bij het genereren van gegevens:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Deze code verzendt een tuple met een zin naar de standaardstroom met de waarde van de reeks-ID opgenomen in **lastSeqId**. In dit voorbeeld **lastSeqId** voor elke tuple verzonden wordt verhoogd.

Zoals wordt beschreven in de **Storm voorbeeld** project, of een onderdeel is transactionele kan worden ingesteld tijdens runtime, op basis van configuratie.

## <a name="hybrid-topology-with-c-and-java"></a>Hybride topologie met C# en Java

U kunt Data Lake tools voor Visual Studio ook gebruiken voor het maken van hybride topologieën waarin bepaalde onderdelen zijn C# en anderen Java zijn.

Voor een voorbeeld van een hybride-topologie, maak een project en selecteer **Storm hybride voorbeeld**. Dit Voorbeeldtype ziet u de volgende concepten:

* **Java-spout** en **C# bolt**: gedefinieerd in **HybridTopology_javaSpout_csharpBolt**.

    * Een transactionele versie is gedefinieerd in **HybridTopologyTx_javaSpout_csharpBolt**.

* **C# spout** en **Java bolt**: gedefinieerd in **HybridTopology_csharpSpout_javaBolt**.

    * Een transactionele versie is gedefinieerd in **HybridTopologyTx_csharpSpout_javaBolt**.

  > [!NOTE]
  > Deze versie wordt ook beschreven hoe Clojure code uit een tekstbestand gebruiken als een Java-component.


Als u wilt overschakelen van de topologie die wordt gebruikt wanneer het project wordt ingediend, gaan de `[Active(true)]` instructie aan de topologie die u gebruiken wilt, alvorens deze aan het cluster.

> [!NOTE]
> Alle Java-bestanden die vereist zijn, worden geleverd als onderdeel van dit project in de **JavaDependency** map.

Overweeg het volgende bij het maken en verzenden van een hybride-topologie:

* Gebruik **JavaComponentConstructor** voor het maken van een exemplaar van de Java-klasse voor een spout of Bolts.

* Gebruik **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** voor het serialiseren van de gegevens van of naar Java-onderdelen vanuit Java-objecten naar JSON.

* Bij het indienen van de topologie met de server, moet u de **aanvullende configuraties** optie om op te geven de **Java bestandspaden**. Het opgegeven pad moet de map waarin de JAR-bestanden die uw Java-klassen bevatten.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.NET versie 0.9.4.203 introduceert een nieuwe klasse en de methode specifiek voor het werken met de Event Hub spout (een Java-spout die uit Event Hubs lezen). Wanneer u een topologie die gebruikmaakt van een Event Hub spout maakt, moet u de volgende methoden gebruiken:

* **EventHubSpoutConfig** klasse: Hiermee maakt u een object dat de configuratie voor het onderdeel spout bevat.

* **TopologyBuilder.SetEventHubSpout** methode: het Event Hub spout onderdeel toegevoegd aan de topologie.

> [!NOTE]
> U moet nog steeds gebruiken de **CustomizedInteropJSONSerializer** voor het serialiseren van de gegevens die worden geproduceerd door de spout.

## <a id="configurationmanager"></a>Configuration Manager gebruiken

Gebruik geen **ConfigurationManager** configuratiewaarden ophalen van bolt en spout onderdelen. In dat geval kan een uitzondering voor een null-aanwijzer veroorzaken. In plaats daarvan is de configuratie voor uw project in de Storm-topologie doorgegeven als een sleutel-waardepaar in de context van de topologie. Elk onderdeel dat is afhankelijk van configuratiewaarden moet ze opgehaald van de context tijdens de initialisatie.

De volgende code laat zien hoe u deze waarden ophaalt:

```csharp
public class MyComponent : ISCPBolt
{
    // To hold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of the context for this component instance
        this.ctx = ctx;
        // If it exists, load the configuration for the component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve the value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Als u een `Get` methode om te retourneren van een exemplaar van het onderdeel moet u ervoor zorgen dat dit wordt doorgegeven zowel de `Context` en `Dictionary<string, Object>` parameters voor de constructor. Het volgende voorbeeld is een eenvoudige `Get` methode die deze waarden correct is geslaagd:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a>Het bijwerken van SCP.NET

Recente versies van SCP.NET ondersteuning pakketupgrade via NuGet. Wanneer een nieuwe update beschikbaar is, ontvangt u een upgrade melding. U kunt handmatig controleren voor een upgrade door de volgende stappen uit:

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **NuGet-pakketten beheren**.

2. Selecteer in de Pakketbeheer **Updates**. Als een update beschikbaar is, wordt het weergegeven. Klik op **Update** voor het pakket te installeren.

> [!IMPORTANT]
> Als uw project is gemaakt met een eerdere versie van SCP.NET die NuGet niet gebruikt, moet u de volgende stappen uit om bij te werken naar een nieuwere versie uitvoeren:
>
> 1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **NuGet-pakketten beheren**.
> 2. Met behulp van de **Search** veld, zoeken en vervolgens toevoegt, **Microsoft.SCP.Net.SDK** aan het project.

## <a name="troubleshoot-common-issues-with-topologies"></a>Algemene problemen met topologieën

### <a name="null-pointer-exceptions"></a>Null-aanwijzer uitzonderingen

Wanneer u van een C#-topologie met een Linux gebaseerde HDInsight-cluster gebruikmaakt, Bolts en onderdelen die gebruikmaken van spout **ConfigurationManager** lezen van configuratie-instellingen tijdens runtime kunnen null-aanwijzer uitzonderingen retourneren.

De configuratie voor uw project is doorgegeven aan de Storm-topologie als een sleutel-waardepaar in de context van de topologie. Het kan worden opgehaald uit de dictionary-object dat wordt doorgegeven aan de onderdelen van uw wanneer ze zijn geïnitialiseerd.

Zie voor meer informatie de [ConfigurationManager](#configurationmanager) gedeelte van dit document.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Wanneer u van een C#-topologie met een Linux gebaseerde HDInsight-cluster gebruikmaakt, kunt u de volgende fout kan optreden:

    System.TypeLoadException: Failure has occurred while loading a type.

Deze fout treedt op wanneer u een binair bestand dat niet compatibel met de versie van .NET die ondersteuning biedt voor Mono gebruiken.

Zorg ervoor dat uw project maakt gebruik van binaire bestanden voor .NET 4.5 compiled Linux gebaseerde HDInsight-clusters.

### <a name="test-a-topology-locally"></a>Een topologie lokaal testen

Hoewel het eenvoudig voor het implementeren van een topologie met een cluster, in sommige gevallen moet u wellicht een topologie lokaal testen. Gebruik de volgende stappen uitvoeren en testen van de voorbeeldtopologie in deze zelfstudie lokaal in uw ontwikkelomgeving.

> [!WARNING]
> Lokale testen werkt alleen voor basic, C#-topologieën voor alleen. U de lokale testen voor hybride topologieën of topologieën met meerdere streams niet gebruiken.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer **eigenschappen**. Wijzig in de Projecteigenschappen de **type uitvoer** naar **consoletoepassing**.

    ![Schermafbeelding van de Projecteigenschappen met uitvoertype gemarkeerd](./media/apache-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > Moet u de **type uitvoer** terug naar **Class Library** voordat u de topologie voor een cluster implementeren.

2. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer vervolgens **toevoegen** > **Nieuw Item**. Selecteer **klasse**, en voer **LocalTest.cs** als naam van de klasse. Tot slot op **toevoegen**.

3. Open **LocalTest.cs**, en voeg de volgende **met** instructie boven:

    ```csharp
    using Microsoft.SCP;
    ```

4. De volgende code gebruiken als de inhoud van de **LocalTest** klasse:

    ```csharp
    // Drives the topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test the spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of the spout, using the local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext to persist the data stream to file
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test the splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set the data stream to the data created by the spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test the counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set the data stream to the data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    Neem even de tijd om te lezen via de code een toelichting. Deze code gebruikt **LocalContext** om uit te voeren van de onderdelen in de ontwikkelomgeving, en zich blijft voordoen de gegevensstroom tussen onderdelen naar tekstbestanden op de lokale schijf.

1. Open **Program.cs**, en het volgende toevoegen aan de **Main** methode:

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize the runtime
    SCPRuntime.Initialize();

    //If we are not running under the local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. De wijzigingen opslaan en klik vervolgens op **F5** of selecteer **Debug** > **foutopsporing starten** om het project te starten. Een consolevenster moet worden weergegeven en meld u aan status als de tests uitgevoerd. Wanneer **Tests voltooid** wordt weergegeven, drukt u op een willekeurige toets om het venster te sluiten.

3. Gebruik **Windows Verkenner** vinden van de map waarin uw project. Bijvoorbeeld: **C:\Users\<gebruikersnaam > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Open in deze map **Bin**, en klik vervolgens op **Debug**. U ziet de tekstbestanden die zijn gemaakt wanneer de tests uitgevoerd: sentences.txt counter.txt en splitter.txt. Open elk tekstbestand en controleren van de gegevens.

   > [!NOTE]
   > Tekenreeksgegevens zich blijft voordoen als een matrix met decimale waarden in deze bestanden. Bijvoorbeeld: \[[97,103,111]] in de **splitter.txt** bestand is het woord *en*.

> [!NOTE]
> Stel de **projecttype** terug naar **Class Library** voordat u implementeert een Storm op HDInsight-cluster.

### <a name="log-information"></a>Logboekgegevens

U kunt eenvoudig gegevens van de onderdelen van uw topologie vastleggen met behulp van `Context.Logger`. De volgende opdracht maakt bijvoorbeeld een informatieve logboekvermelding:

```csharp
Context.Logger.Info("Component started");
```

Logboekgegevens kan bekeken worden vanuit de **Hadoop-serviceaccount voor aanmelden**, die is gevonden **Server Explorer**. Vouw de vermelding voor uw Storm op HDInsight-cluster uit en vouw vervolgens **Hadoop-serviceaccount voor aanmelden**. Selecteer ten slotte het logboekbestand om weer te geven.

> [!NOTE]
> De logboeken worden opgeslagen in de Azure storage-account dat wordt gebruikt door het cluster. Als u de logboeken wilt in Visual Studio, moet u zich aanmeldt bij de Azure-abonnement dat eigenaar is van het opslagaccount.

### <a name="view-error-information"></a>Fout-informatie weergeven

Om fouten te bekijken die hebben plaatsgevonden in een actieve topologie, gebruikt u de volgende stappen uit:

1. Van **Server Explorer**, met de rechtermuisknop op het Storm op HDInsight-cluster en selecteert u **weergave Storm-topologieën**.

2. Voor de **Spout** en **Bolts**, wordt de **laatste fout** kolom bevat informatie over de laatste fout.

3. Selecteer de **Spout Id** of **Bolt Id** voor het onderdeel waarvoor een fout weergegeven. Informatie wordt weergegeven op de detailpagina die wordt weergegeven, extra fout de **fouten** sectie aan de onderkant van de pagina.

4. Als u meer informatie, selecteer een **poort** van de **Executor** sectie van de pagina, Zie het logboek van de werknemer Storm voor de laatste paar minuten.

### <a name="errors-submitting-topologies"></a>Fouten verzonden topologieën

Als er een HDInsight-topologie wordt verzonden fouten optreden, kunt u Logboeken vinden voor de serverzijde-onderdelen die de verzending van de topologie op uw HDInsight-cluster te verwerken. Gebruik de volgende opdracht vanaf een opdrachtregel voor het ophalen van deze logboeken:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

Vervang __sshuser__ met het SSH-gebruikersaccount voor het cluster. Vervang __clustername__ met de naam van het HDInsight-cluster. Voor meer informatie over het gebruik van `scp` en `ssh` met HDInsight, raadpleegt u [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

Voorbeelden kunnen om meerdere redenen mislukken:

* JDK is niet geïnstalleerd of is niet in het pad.
* Vereiste Java-afhankelijkheden zijn niet opgenomen in het indienen.
* Niet-compatibele afhankelijkheden.
* Dubbele namen van de topologie.

Als de `hdinsight-scpwebapi.out` logboek bevat een `FileNotFoundException`, dit wordt mogelijk veroorzaakt door de volgende omstandigheden:

* De JDK die zich niet in het pad op de ontwikkelomgeving. Controleer of de JDK die is geïnstalleerd in de ontwikkelomgeving, en die `%JAVA_HOME%/bin` zich in het pad.
* Ontbreekt er een Java-afhankelijkheid. Zorg ervoor dat u alle vereiste JAR-bestanden als onderdeel van de verzending van opneemt.

## <a name="next-steps"></a>Volgende stappen

Zie voor een voorbeeld van het verwerken van gegevens uit Event Hubs, [verwerken van gebeurtenissen van Azure Event Hubs met Storm op HDInsight](apache-storm-develop-csharp-event-hub-topology.md).

Zie voor een voorbeeld van een C#-topologie die stroomgegevens in meerdere streams splitst [C# Storm-voorbeeld](https://github.com/Blackmist/csharp-storm-example).

Zie voor meer informatie over het maken van C#-topologieën detecteren, [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Zie de volgende documenten voor meer manieren om te werken met HDInsight en meer Storm op HDInsight-voorbeelden:

**Microsoft SCP.NET**

* [Programmeerhandleiding voor SCP](apache-storm-scp-programming-guide.md)

**Apache Storm op HDInsight**

* [Implementeren en bewaken topologieën met Apache Storm op HDInsight](apache-storm-deploy-monitor-topology.md)
* [Voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md)

**Apache Hadoop in HDInsight**

* [Hive gebruiken met Hadoop in HDInsight](../hadoop/hdinsight-use-hive.md)
* [Pig gebruiken met Hadoop in HDInsight](../hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](../hadoop/hdinsight-use-mapreduce.md)

**Apache HBase in HDInsight**

* [Aan de slag met HBase in HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md)
