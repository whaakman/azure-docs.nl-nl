---
title: Programmeerhandleiding voor SCP.NET | Microsoft Docs
description: "Informatie over het gebruik van SCP.NET om te maken. Storm-topologieën op basis van NET voor gebruik met Storm op HDInsight."
services: hdinsight
documentationcenter: 
author: raviperi
manager: jhubbard
editor: cgronlun
ms.assetid: 34192ed0-b1d1-4cf7-a3d4-5466301cf307
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: raviperi
ms.openlocfilehash: a0ce92ba58fbcda812a3d4e5e275178b73400d6c
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="scp-programming-guide"></a>Programmeerhandleiding voor SCP
SCP is een platform voor het bouwen van realtime, betrouwbare en consistente en hoge prestaties gegevensverwerking toepassing. Het is gebouwd boven [Apache Storm](http://storm.incubator.apache.org/) --een stroom verwerking door de community's van de OSS-systeem. Storm door Nathan Marz is ontworpen en is open source door Twitter. Hierbij wordt gebruikgemaakt van [Apache ZooKeeper](http://zookeeper.apache.org/), een ander Apache project om in te schakelen uiterst betrouwbare gedistribueerd beheer coördinatie en status. 

Niet alleen het SCP-project overgebracht Storm op Windows maar ook het project toegevoegd uitbreidingen en aanpassingen voor het Windows-ecosysteem. De uitbreidingen functionaliteit voor .NET-ontwikkelaars en bibliotheken bevatten, de aanpassing omvat implementatie op basis van Windows. 

De uitbreiding en aanpassing is gedaan zodanig dat we hoeft niet te vertakken de OSS-projecten en we kunnen gebruikmaken van afgeleide ecosystemen gebouwd op Storm.

## <a name="processing-model"></a>Het verwerken van model
De gegevens in SCP is gemodelleerd als ononderbroken streams van tuples. De tuples doorgaans stromen in sommige wachtrij eerst en vervolgens opgenomen en getransformeerd door zakelijke logica gehost binnen een Storm-topologie, ten slotte de uitvoer tuples naar een ander SCP-systeem kan de eigenschappen of worden doorgevoerd voor stores, zoals gedistribueerd bestandssysteem of databases zoals SQL Server.

![Een diagram van een wachtrij gegevens op de verwerking die een gegevensarchief feeds voeding](./media/apache-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

In Storm definieert u de Toepassingstopologie van een een grafiek van de berekening. Elk knooppunt in een topologie bevat de logica voor verwerking en koppelingen tussen knooppunten gegevensstroom aangeven. De knooppunten invoergegevens invoeren in de topologie worden genoemd _spouts_, die kan worden gebruikt om te sequentiëren van de gegevens. De ingevoerde gegevens kan zich bevinden in het bestand Logboeken, transactionele database, systeem-prestatiemeteritem enzovoort. De knooppunten met beide stromen en uitvoergegevens heten _bolts_, wat doen de actuele gegevens filteren en selecties en cumulatie-instellingen.

SCP ondersteunt pogingen op in de minste eenmaal en precies-eenmaal verwerken van gegevens. In een gedistribueerde toepassing voor streaming verwerken, kunnen zich voordoen diverse fouten tijdens het verwerken van gegevens, zoals netwerkuitval, machinestoringen of fout in gebruikerscode enzovoort. Op de minste eenmaal verwerken zorgt ervoor dat alle gegevens ten minste één keer wordt verwerkt door automatisch dezelfde gegevens als er treedt een fout. Op de minste eenmaal verwerken eenvoudig en betrouwbaar en ook veel toepassingen aansluit. Als u een toepassing moet exact tellen, is verwerking op de minste eenmaal echter onvoldoende omdat dezelfde gegevens kan mogelijk worden afgespeeld in de Toepassingstopologie. In dat geval, precies-zodra de verwerking is ontworpen om te controleren of het resultaat juist is zelfs wanneer de gegevens kan worden onderschept en verwerkt meerdere keren.

SCP kan .NET-ontwikkelaars realtime gegevens proces om toepassingen te ontwikkelen tijdens het gebruik van op Java Virtual Machine (JVM) met Storm onder de behandelt. De .NET- en JVM communiceren via lokale TCP-sockets. Elke Spout/Bolt bestaat uit in feite een paar .net/Java-proces waarin de gebruiker logica wordt uitgevoerd in .net-proces als een invoegtoepassing.

Als u wilt maken van een toepassing gegevensverwerking bovenop SCP, zijn verschillende stappen nodig:

* Ontwerpen en implementeren van de Spouts ophalen van gegevens uit de wachtrij.
* Ontwerpen en implementeren van Bolts voor het verwerken van de invoergegevens en opslaan van gegevens voor externe stores, zoals een Database.
* De topologie ontwerpen, indienen en uitvoeren van de topologie. De topologie definieert hoekpunten en de gegevens stromen tussen de hoekpunten. SCP zal duren voordat de topologie-specificatie en deze implementeren op een Storm-cluster, waarin elk hoekpunt wordt uitgevoerd op één knooppunt van de logische. De failover- en vergroten/verkleinen wordt worden afgehandeld door de Storm-Taakplanner.

Dit document gebruikt enkele eenvoudige voorbeelden voor het bouwen van gegevensverwerking toepassing met SCP doorlopen.

## <a name="scp-plugin-interface"></a>SCP-invoegtoepassing Interface
SCP-invoegtoepassingen (of toepassingen) zijn zelfstandige exe die beide tijdens kunnen uitvoeren in Visual Studio de ontwikkelingsfase en worden aangesloten op de Storm-pijplijn na de implementatie in productie. Het schrijven van de invoegtoepassing SCP dezelfde manier als andere standaard Windows-consoletoepassingen schrijven is. SCP.NET platform declareert een interface voor spout/bolt en de code van de invoegtoepassing gebruiker moet deze interfaces implementeren. Het belangrijkste doel van dit ontwerp is dat de gebruiker zich op hun eigen logics bedrijven en andere dingen concentreren kan moet worden verwerkt door het platform SCP.NET verlaten.

De code van de invoegtoepassing gebruiker moet een van de volgende interfaces implementeren, afhankelijk van of de topologie is transactionele of niet-transactionele en Hiermee wordt aangegeven of het onderdeel is van een spout of bolt.

* ISCPSpout
* ISCPBolt
* ISCPTxSpout
* ISCPBatchBolt

### <a name="iscpplugin"></a>ISCPPlugin
ISCPPlugin is de algemene interface voor alle soorten invoegtoepassingen gebruikt. Het is momenteel een dummy-interface.

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a>ISCPSpout
ISCPSpout is de interface voor niet-transactionele spout.

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

Wanneer `NextTuple()` wordt aangeroepen, de C\# gebruikerscode kan een of meer tuples verzenden. Als er niets om te verzenden, moet deze methode zonder iets tekensetcodering retourneren. Houd er rekening mee dat `NextTuple()`, `Ack()`, en `Fail()` worden genoemd in een lus in een enkele thread in C\# proces. Wanneer er geen tuples verzenden, is het beleefd NextTuple slaapstand gedurende een korte tijd (zoals 10 milliseconden) niet te verspillen te veel CPU hebben.

`Ack()`en `Fail()` alleen wanneer het ack-mechanisme is ingeschakeld in spec bestand worden genoemd. De `seqId` wordt gebruikt voor het identificeren van de tuple op die is bevestigd of is mislukt. Dus als ack in niet-transactionele topologie is ingeschakeld, kan de volgende emit-functie moet worden gebruikt in Spout:

    public abstract void Emit(string streamId, List<object> values, long seqId); 

Als ack wordt niet ondersteund in niet-transactionele topologie de `Ack()` en `Fail()` kan blijven als lege functie.

De `parms` invoerparameter in deze functies een woordenboek leeg is, is gereserveerd voor toekomstig gebruik.

### <a name="iscpbolt"></a>ISCPBolt
ISCPBolt is de interface voor niet-transactionele bolt.

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

Wanneer nieuwe tuple beschikbaar is, de `Execute()` functie wordt aangeroepen om te verwerken.

### <a name="iscptxspout"></a>ISCPTxSpout
ISCPTxSpout is de interface voor transactionele spout.

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

Net als bij hun niet-transactionele tegenpost `NextTx()`, `Ack()`, en `Fail()` worden genoemd in een lus in een enkele thread in C\# proces. Wanneer er geen gegevens verzenden, is het beleefd hebben `NextTx` slaapstand gedurende een korte tijd (10 milliseconden) niet te verspillen te veel CPU.

`NextTx()`wordt aangeroepen voor het starten van een nieuwe transactie, de out-parameter `seqId` wordt gebruikt voor het identificeren van de transactie, wordt ook gebruikt in `Ack()` en `Fail()`. In `NextTx()`, gebruiker gegevens naar Java kant kunt verzenden. De gegevens worden opgeslagen in ZooKeeper ter ondersteuning van opnieuw afspelen. Omdat de capaciteit van ZooKeeper beperkt is, moet de gebruiker alleen metagegevens verzenden, geen grote hoeveelheid gegevens in een transactionele spout.

Storm een transactie automatisch wordt herhaald als het mislukt, dus `Fail()` mag niet worden aangeroepen in normale geval. Maar als een SCP kan de metagegevens die door transactionele spout controleren, kan worden aangeroepen `Fail()` wanneer de metagegevens is ongeldig.

De `parms` invoerparameter in deze functies een woordenboek leeg is, is gereserveerd voor toekomstig gebruik.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt
ISCPBatchBolt is de interface voor transactionele bolt.

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

`Execute()`wordt aangeroepen wanneer er nieuwe tuple die binnenkomen bij de bolt. `FinishBatch()`wordt aangeroepen wanneer deze transactie is beëindigd. De `parms` invoerparameter is gereserveerd voor toekomstig gebruik.

Voor transactionele topologie is een belangrijke concepten – `StormTxAttempt`. Deze twee velden heeft `TxId` en `AttemptId`. `TxId`wordt gebruikt om een bepaalde transactie identificeren en voor een gegeven transactie, kunnen er meerdere pogingen als de transactie mislukt en wordt opnieuw en nu afgespeeld. SCP.NET maakt een nieuw ISCPBatchBolt-object voor het verwerken van elk `StormTxAttempt`, net zoals Storm in Java biedt. Het doel van dit ontwerp is ondersteuning voor parallelle transacties verwerkt. Gebruiker Houd er rekening mee dat als de poging van de transactie is voltooid, het bijbehorende ISCPBatchBolt-object wordt vernietigd en de garbage collector zijn verzameld.

## <a name="object-model"></a>Objectmodel
SCP.NET biedt ook een eenvoudige reeks key objecten voor ontwikkelaars voor het programmeren met. Ze zijn **Context**, **StateStore**, en **SCPRuntime**. Ze worden in het gedeelte van de rest van deze sectie beschreven.

### <a name="context"></a>Context
Context biedt een actieve omgeving naar de toepassing. Elk exemplaar ISCPPlugin (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) heeft een corresponderende contextexemplaar. De functionaliteit van Context kan worden onderverdeeld in twee delen: (1) het statische deel, die beschikbaar in de hele C is\# verwerken, (2) het dynamische deel, die alleen beschikbaar voor het specifieke exemplaar van de Context.

### <a name="static-part"></a>Statische deel
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

`Logger`is beschikbaar voor logboek-doel.

`pluginType`wordt gebruikt om aan te geven van het type van de invoegtoepassing van de C\# proces. Als het C\# proces wordt uitgevoerd in de lokale testmodus (zonder Java), het type van de invoegtoepassing is `SCP_NET_LOCAL`.

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

`Config`ophalen van parameters voor de configuratie van Java-zijde is opgegeven. De parameters worden doorgegeven van Java-kant wanneer C\# invoegtoepassing is geïnitialiseerd. De `Config` parameters worden onderverdeeld in twee delen: `stormConf` en `pluginConf`.

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

`stormConf`parameters zijn gedefinieerd door Storm is en `pluginConf` de parameters die zijn gedefinieerd door SCP is. Bijvoorbeeld:

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

`TopologyContext`wordt geleverd als u de topologie-context, is het meest geschikt voor onderdelen met meerdere parallelle uitvoering. Hier volgt een voorbeeld:

    //demo how to get TopologyContext info
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)                      
    {
        Context.Logger.Info("TopologyContext info:");
        TopologyContext topologyContext = Context.TopologyContext;                    
        Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());          
        taskIndex = topologyContext.GetThisTaskIndex();
        Context.Logger.Info("taskIndex: {0}", taskIndex);
        string componentId = topologyContext.GetThisComponentId();                    
        Context.Logger.Info("componentId: {0}", componentId);
        List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
        Context.Logger.Info("taskNum: {0}", componentTasks.Count);                    
    }

### <a name="dynamic-part"></a>Dynamische deel
De volgende interfaces zijn relevant zijn voor een bepaald contextexemplaar. Het Context-exemplaar is gemaakt door SCP.NET platform en doorgegeven aan de gebruikerscode:

    // Declare the Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple to default stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple to the specific stream.
    public abstract void Emit(string streamId, List<object> values);  

Voor niet-transactionele spout ondersteunende ack, krijgt u de volgende methode:

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

Voor niet-transactionele bolt ack ondersteunen, moet deze expliciet `Ack()` of `Fail()` de tuple deze ontvangen. En bij het genereren van nieuwe tuple, moet deze ook de ankers van de nieuwe tuple opgeven. De volgende methoden zijn beschikbaar.

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a>StateStore
`StateStore`metagegevens van services, monotone reeks genereren en wacht gratis coördinatie biedt. Een hoger niveau gedistribueerde gelijktijdigheid abstracties kunnen worden gebaseerd op `StateStore`, met inbegrip van gedistribueerde vergrendelingen, gedistribueerde wachtrijen, barrières en transactieservices.

SCP-toepassingen kunnen gebruikmaken van de `State` object voor het persistent maken van sommige gegevens in ZooKeeper, met name voor transactionele topologie. Dit doet, als transactionele spout is vastgelopen en opnieuw start, kunt de benodigde informatie van ZooKeeper ophalen en kunt u de pijplijn opnieuw.

De `StateStore` is hoofdzakelijk deze methoden van object:

    /// <summary>
    /// Static method to retrieve a state store of the given path and connStr 
    /// </summary>
    /// <param name="storePath">StateStore Path</param>
    /// <param name="connStr">StateStore Address</param>
    /// <returns>Instance of StateStore</returns>
    public static StateStore Get(string storePath, string connStr);

    /// <summary>
    /// Create a new state object in this state store instance
    /// </summary>
    /// <returns>State from StateStore</returns>
    public State Create();

    /// <summary>
    /// Retrieve all states that were previously uncommitted, excluding all aborted states 
    /// </summary>
    /// <returns>Uncommited States</returns>
    public IEnumerable<State> GetUnCommitted();

    /// <summary>
    /// Get all the States in the StateStore
    /// </summary>
    /// <returns>All the States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all the committed states
    /// </summary>
    /// <returns>Registries contain the Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all the Aborted State in the StateStore
    /// </summary>
    /// <returns>Registries contain the Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of the State</typeparam>
    public State GetState(long stateId)

De `State` is hoofdzakelijk deze methoden van object:

    /// <summary>
    /// Set the status of the state object to commit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set the status of the state object to abort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under the give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get the attribute value associated with the given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

Voor de `Commit()` methode, wanneer simpleMode is ingesteld op true, wordt verwijderd de bijbehorende ZNode in ZooKeeper. Anders worden de huidige ZNode en het toevoegen van een nieuw knooppunt in de toegewezen\_pad.

### <a name="scpruntime"></a>SCPRuntime
SCPRuntime biedt de volgende twee methoden:

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

`Initialize()`wordt gebruikt voor het initialiseren van de SCP runtime-omgeving. Bij deze methode wordt de C\# proces maakt verbinding met de Java-zijde en parameters voor de configuratie en -topologie context opgehaald.

`LaunchPlugin()`wordt gebruikt voor de verwerking berichtenlus starten. In deze lus, de C\# invoegtoepassing ontvangt berichten formulier Java kant (inclusief signalen tuples en control) en vervolgens verwerken de berichten, bijvoorbeeld het aanroepen van de interfacemethode bieden door de gebruikerscode. De invoerparameter voor methode `LaunchPlugin()` is een gemachtigde die kan resulteren in een object dat ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt-interface implementeren.

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

Voor ISCPBatchBolt, krijgen we `StormTxAttempt` van `parms`, en deze gebruiken om te beoordelen of het is een poging tot herhaald. De controle op een poging replay vaak wordt uitgevoerd op de commit-bolt en dit wordt geïllustreerd in de `HelloWorldTx` voorbeeld.

In het algemeen kan het SCP-invoegtoepassingen in twee modi hier worden uitgevoerd:

1. Lokale testmodus: In deze modus wordt het SCP-invoegtoepassingen (de C\# gebruikerscode) in Visual Studio wordt uitgevoerd tijdens de ontwikkelingsfase bevindt. `LocalContext`in deze modus methode biedt voor het serialiseren van de verzonden tuples tot lokale bestanden en ze te lezen in het geheugen kan worden gebruikt.
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. Normale modus: In deze modus wordt het SCP-invoegtoepassingen worden gestart door storm java-proces.
   
    Hier volgt een voorbeeld van het SCP-invoegtoepassing te starten:
   
        namespace Scp.App.HelloWorld
        {
        public class Generator : ISCPSpout
        {
            … …
            public static Generator Get(Context ctx, Dictionary<string, Object> parms)
            {
            return new Generator(ctx);
            }
        }
   
        class HelloWorld
        {
            static void Main(string[] args)
            {
            /* Setting the environment variable here can change the log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a>Topologie specificatietaal
Specificatie van het SCP-topologie is een domeinspecifieke taal voor het beschrijven en topologieën SCP te configureren. Deze is gebaseerd op de Storm Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) en SCP is verlengd.

Topologie specificaties kunnen worden verstuurd rechtstreeks naar de storm-cluster voor uitvoering via de ***runspec*** opdracht.

SCP.NET heeft de volgende functies om te definiëren transactionele topologieën toegevoegd:

| **Nieuwe functies** | **Parameters** | **Beschrijving** |
| --- | --- | --- |
| **TX topolopy** |topologie-naam<br />spout-kaart<br />bolt-kaart |Definieer een transactionele-topologie met de naam van de topologie &nbsp;spouts definitie kaart en de bolts definitie-kaart |
| **SCP-tx-spout** |exec-naam<br />argumenten<br />Velden |Definieer een transactionele spout. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met ***args***.<br /><br />De ***velden*** wordt de uitvoer-velden voor spout |
| **SCP-tx-batch-bolt** |exec-naam<br />argumenten<br />Velden |Definieer een transactionele Batch Bolt. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met ***argumenten.***<br /><br />De velden is de uitvoer-velden voor bolt. |
| **SCP-tx-doorvoeren-bolt** |exec-naam<br />argumenten<br />Velden |Definieer een transactionele commit-bolt. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met ***args***.<br /><br />De ***velden*** wordt de uitvoer-velden voor bolt |
| **nontx topolopy** |topologie-naam<br />spout-kaart<br />bolt-kaart |Definieer een niet-transactionele-topologie met de naam van de topologie&nbsp; spouts definitie kaart en de bolts definitie-kaart |
| **SCP spout** |exec-naam<br />argumenten<br />Velden<br />parameters |Definieer een niet-transactionele spout. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met ***args***.<br /><br />De ***velden*** wordt de uitvoer-velden voor spout<br /><br />De ***parameters*** zijn optioneel, met bepaalde parameters zoals 'nontransactional.ack.enabled' opgeven. |
| **SCP-bolt** |exec-naam<br />argumenten<br />Velden<br />parameters |Definieer een niet-transactionele Bolt. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met ***args***.<br /><br />De ***velden*** wordt de uitvoer-velden voor bolt<br /><br />De ***parameters*** zijn optioneel, met bepaalde parameters zoals 'nontransactional.ack.enabled' opgeven. |

SCP.NET heeft de volgende sleutelwoorden gedefinieerd:

| **Trefwoorden** | **Beschrijving** |
| --- | --- |
| **: de naam** |De naam van de topologie definiëren |
| **: topologie** |Definieer de topologie met behulp van de vorige functies en bouwen in toepassingsgroepen. |
| **: p** |Definieer de parallelle uitvoering hint op voor elke spout of bolt. |
| **: config** |Definieer de parameter configureren of een bestaande bijwerken |
| **: schema** |Definieer het Schema van de stroom. |

En veelgebruikte parameters:

| **Parameter** | **Beschrijving** |
| --- | --- |
| **'plugin.name'** |naam van de exe-bestand van de C#-invoegtoepassing |
| **'plugin.args'** |invoegtoepassing argumenten |
| **'output.schema'** |Uitvoerschema |
| **'nontransactional.ack.enabled'** |Hiermee wordt aangegeven of ack is ingeschakeld voor niet-transactionele-topologie |

De opdracht runspec samen met de bits is geïmplementeerd, lijkt op het gebruik:

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

De ***resource dir*** parameter is optioneel, moet u dit opgeven als u wilt een C plug\# toepassings- en deze map bevat de toepassing en de afhankelijkheden en configuraties.

De ***classpath*** parameter ook is optioneel. Deze wordt gebruikt om het klassepad Java geven als de spec bestand Java Spout of Bolt bevat.

## <a name="miscellaneous-features"></a>Diverse functies
### <a name="input-and-output-schema-declaration"></a>Invoer en uitvoer Schemadeclaratie
Gebruikers kunnen verzenden tuples in C\# processen, het platform moet de tuple serialiseren naar byte [], overdragen naar Java aan clientzijde en Storm deze tuple draagt bij de doelen. Ondertussen in downstream-onderdelen C\# processen wordt ontvangen tuples terug van java-zijde en converteer deze naar de oorspronkelijke typen per platform, alle deze bewerkingen zijn verborgen door het Platform.

Ter ondersteuning van serialisatie en deserialisatie, moet gebruikerscode het schema van de invoer en uitvoer declareren.

Het schema i/o-stroom wordt gedefinieerd als een woordenlijst. De sleutel is de StreamId. De waarde is de typen van de kolommen. Het onderdeel kan meerdere streams gedeclareerd hebben.

    public class ComponentStreamSchema
    {
        public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
        public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
        public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
        {
            InputStreamSchema = input;
            OutputStreamSchema = output;
        }
    }


In de Context-object hebben we de volgende API toegevoegd:

    public void DeclareComponentSchema(ComponentStreamSchema schema)

Ontwikkelaars moeten ervoor zorgen dat de tuples verzonden zich aan het schema is gedefinieerd voor deze stroom, anders het systeem een runtime-uitzondering genereert.

### <a name="multi-stream-support"></a>Ondersteuning voor meerdere Stream
SCP ondersteunt gebruikerscode om te verzenden of ontvangen van meerdere afzonderlijke streams op hetzelfde moment. De ondersteuning weerspiegelt in de Context-object omdat de methode Emit een optionele stroom-ID-parameter houdt.

Twee methoden voor het object Context SCP.NET er zijn toegevoegd. Ze worden gebruikt voor het verzenden van Tuple of Tuples om op te geven StreamId. De StreamId is een tekenreeks en het moet in beide C consistent\# en de topologie definitie-specificaties.

        /* Emit tuple to the specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

De tekensetcodering naar een niet-bestaande stream zorgt ervoor dat de runtime-uitzonderingen.

### <a name="fields-grouping"></a>Velden groeperen
De ingebouwde die velden groeperen in Strom niet goed in SCP.NET werkt. De Java-Proxy-zijde alle velden gegevenstypen zijn daadwerkelijk byte [] en de velden groeperen byte [] object hash-code gebruikt voor het uitvoeren van de groepering. De byte [] object hash-code is het adres van dit object in het geheugen. De groepering worden dus verkeerde voor twee-byte [] objecten die dezelfde inhoud, maar niet hetzelfde adres delen.

Een groeperingsmethode met de aangepaste SCP.NET toegevoegd en de inhoud van de byte [] worden gebruikt voor de groepering. In **SPEC** -bestand, de syntaxis is als volgt:

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


Hier kunt

1. "scp-veld-beheergroep" betekent 'Aangepast veld groepering wordt geïmplementeerd door SCP'.
2. ': tx 'of': niet-tx ' betekent dat als transactionele topologie is. Aangezien de startIndex in tx versus niet tx topologieën verschilt moeten we deze informatie.
3. [0,1] houdt een set hash-veld-id's, begint bij 0.

### <a name="hybrid-topology"></a>Hybride topologie
De systeemeigen Storm is geschreven in Java. En SCP.Net is uitgebreid om te schakelen C\# ontwikkelaars kunnen schrijven C\# code voor het verwerken van hun zakelijke logica. Maar ondersteunt ook hybride topologieën die bevat niet alleen C\# spouts/bolts, maar ook Java Spout/Bolts.

### <a name="specify-java-spoutbolt-in-spec-file"></a>Java Spout/Bolt in spec bestand opgeven
In spec bestand 'scp-spout' en 'scp-bolt' kunnen ook worden gebruikt om Java Spouts en Bolts te geven, Hier volgt een voorbeeld:

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

Hier `microsoft.scp.example.HybridTopology.Generator` is de naam van de Spout van Java-klasse.

### <a name="specify-java-classpath-in-runspec-command"></a>Java-klassenpad in runSpec opdracht opgeven
Als u verzenden met Java Spouts of Bolts topologie wilt, moet u eerst de Java Spouts of Bolts compileren en ophalen van de Jar-bestanden. Vervolgens moet u de java-klassenpad met de Jar-bestanden bij het indienen van de topologie. Hier volgt een voorbeeld:

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

Hier **voorbeelden\\HybridTopology\\java\\doel\\**  is de map waarin de Java-Spout/Bolt Jar-bestand.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Serialisatie en deserialisatie tussen Java en C\#
SCP onderdeel bevat Java kant- en C\# aan clientzijde. Om te communiceren met systeemeigen Java Spouts/Bolts, serialisatie/deserialisatie moet worden uitgevoerd tussen Java kant- en C\# zijde, zoals geïllustreerd in de volgende grafiek.

![diagram van java-component verzenden naar SCP onderdeel verzenden naar Java-onderdeel](./media/apache-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. **Serialisatie in Java kant- en deserialisatie in C\# aan clientzijde**
   
   Eerst bieden we standaardimplementatie voor aan de kant van Java-serialisatie en deserialisatie in C\# aan clientzijde. De serialisatiemethode in Java aan clientzijde kan worden opgegeven in SPEC bestand:
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   De methode deserialisatie in C\# kant moet worden opgegeven in de C\# gebruikerscode:
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   Deze standaardimplementatie afhandelt meestal opgegeven het gegevenstype niet te complex is. Voor bepaalde gevallen, omdat het gegevenstype van de gebruiker is te complex is of omdat de prestaties van onze standaardimplementatie voldoet niet aan voor de gebruiker vereiste, gebruikers kunnen invoegtoepassing hun eigen implementatie.
   
   De interface serialiseren in java aan clientzijde wordt gedefinieerd als:
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   De interface deserialize in C\# aan clientzijde is gedefinieerd als:
   
   openbare interface ICustomizedInteropCSharpDeserializer
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. **Serialisatie in C\# kant- en deserialisatie aan de kant van Java**
   
   De serialisatiemethode in C\# kant moet worden opgegeven in de C\# gebruikerscode:
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   De methode deserialisatie in Java kant moet worden opgegeven in SPEC bestand:
   
     (scp-spout
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   Hier 'microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer' is de naam van Deserializer en 'microsoft.scp.example.HybridTopology.Person' is dat de doelklasse van de gegevens voor wordt gedeserialiseerd.
   
   Gebruiker kan ook plug-in hun eigen implementatie van C\# serialisatiefunctie en Java Deserializer. Deze code is de interface voor C\# serialisatiefunctie:
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   Deze code is de interface voor Java Deserializer:
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a>SCP Host modus
In deze modus kan gebruiker hun codes naar dll-bestand te compileren en geleverd door een SCP SCPHost.exe gebruikt voor het verzenden van topologie. Het bestand spec ziet deze code uit:

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

Hier `plugin.name` is opgegeven als `SCPHost.exe` geleverd door een SCP-SDK. SCPHost.exe accepteert drie parameters:

1. De eerste is de DLL-naam, die is `"HelloWorld.dll"` in dit voorbeeld.
2. Het tweede is de klassenaam, die is `"Scp.App.HelloWorld.Generator"` in dit voorbeeld.
3. Het derde is de naam van een openbare statische methode, waarbij kan worden aangeroepen om een exemplaar van ISCPPlugin.

In de modus host gebruikerscode is gecompileerd als dll-bestand en wordt aangeroepen door het SCP-platform. SCP-platform kan dus volledig beheer over de hele verwerking logica krijgen. We raden u dus aan onze klanten verzenden topologie in de modus voor SCP-host, omdat deze kunt de ervaring voor de ontwikkeling vereenvoudigen en bring ons meer flexibiliteit en betere achterwaartse compatibiliteit voor latere release ook.

## <a name="scp-programming-examples"></a>SCP programmering voorbeelden
### <a name="helloworld"></a>Hallo wereld
**HelloWorld** is een eenvoudig voorbeeld om een smaak van SCP.Net weer te geven. Wordt een niet-transactionele-topologie met een spout aangeroepen **generator**, en twee bolts aangeroepen **splitser** en **teller**. De spout **generator** willekeurig zinnen gegenereerd en verzenden van deze zinnen naar **splitser**. De bolt **splitser** splitst de zinnen woorden en deze woorden om naar te verzenden **teller** bolt. De bout 'teller' maakt gebruik van een woordenboek voor het vastleggen van het nummer van het exemplaar van elk woord.

Er zijn twee bestanden spec **HelloWorld.spec** en **HelloWorld\_EnableAck.spec** voor dit voorbeeld. In de C\# code, het vindt of ack is ingeschakeld door de pluginConf van Java-kant.

    /* demo how to get pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

Als ack is ingeschakeld, wordt een woordenlijst in de spout gebruikt in de cache van de tuples die niet bevestigd zijn. Als Fail() wordt aangeroepen, wordt de mislukte tuple replay:

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get the cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay the failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a>HelloWorldTx
De **HelloWorldTx** voorbeeld laat zien hoe transactionele topologie implementeren. Heeft een spout aangeroepen **generator**, aangeroepen voor een batch-bolt **gedeeltelijk aantal**, en een bolt doorvoeren aangeroepen **aantal som**. Er zijn ook drie vooraf gemaakte txt-bestanden: **DataSource0.txt**, **DataSource1.txt**, en **DataSource2.txt**.

In elke transactie, de spout **generator** willekeurig worden twee bestanden geselecteerd in de vooraf gemaakte drie bestanden en verzenden van de namen van de twee bestanden naar de **gedeeltelijk aantal** bolt. De bolt **gedeeltelijk aantal** haalt de bestandsnaam van de ontvangen tuple vervolgens opent u het bestand en tel het aantal woorden in dit bestand en ten slotte het word-nummer te verzenden de **aantal som** bolt. De **aantal som** bolt bevat een overzicht van het totale aantal.

Als u de **exact één keer** semantiek, de commit-bolt **aantal som** moet te beoordelen of het is een herhaald transactie. In dit voorbeeld heeft een statisch lidvariabele:

    public static long lastCommittedTxId = -1; 

Wanneer een ISCPBatchBolt-exemplaar is gemaakt, krijgt de `txAttempt` van invoerparameters:

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from the input parms */
        if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
        {
            StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
            return new CountSum(ctx, txAttempt);
        }
        else
        {
            throw new Exception("null txAttempt");
        }
    }

Wanneer `FinishBatch()` wordt aangeroepen, de `lastCommittedTxId` worden bijgewerkt als het is niet een herhaald transactie.

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update the totalCount and lastCommittedTxId value */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a>HybridTopology
Deze topologie bevat een Java-Spout en een C\# Bolt. De serialisatie en deserialisatie standaardimplementatie geleverd door een SCP platform wordt gebruikt. Zie de **HybridTopology.spec** in **voorbeelden\\HybridTopology** map voor de spec bestandsgegevens en **SubmitTopology.bat** voor het opgeven van Java Classpath.

### <a name="scphostdemo"></a>SCPHostDemo
In dit voorbeeld is hetzelfde als HelloWorld in wezen. Het enige verschil is dat de gebruikerscode wordt gecompileerd als dll-bestand en de topologie wordt ingediend via SCPHost.exe. Zie de sectie 'SCP Host modus' voor meer gedetailleerde uitleg.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende documenten voor voorbeelden van Storm-topologieën die zijn gemaakt met behulp van de SCP's:

* [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)
* [Procesgebeurtenissen van Azure Event Hubs met Storm op HDInsight](apache-storm-develop-csharp-event-hub-topology.md)
* [Verwerken van sensorgegevens vehicle van Event Hubs met behulp van Storm op HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Uitpakken, transformeren en laden (ETL) uit Azure Event Hubs voor HBase](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
