---
title: SCP.NET-programmeergids voor Storm in Azure HDInsight
description: Informatie over het gebruik van SCP.NET te maken. NET gebaseerde Storm-topologieën voor gebruik met die worden uitgevoerd in Azure HDInsight Storm.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2016
ms.openlocfilehash: e6025ba2645c284cca87483b48b2d79a9558d574
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012527"
---
# <a name="scp-programming-guide"></a>SCP-programmeergids
SCP is een platform voor het bouwen van realtime, betrouwbare en consistente en hoogwaardige gegevensverwerking toepassing. Het is gebouwd boven [Apache Storm](http://storm.incubator.apache.org/) --een verwerkingssysteem die is ontworpen door de OSS-community's. Storm is ontworpen door Nathan Marz en is open source op Twitter. Hierbij wordt gebruikgemaakt van [Apache ZooKeeper](http://zookeeper.apache.org/), een ander Apache-project om in te schakelen zeer betrouwbare gedistribueerd beheer coördinatie en status. 

Niet alleen het SCP-project overgezet Storm op Windows, maar ook het project toegevoegd extensies en aanpassingen voor het Windows-ecosysteem. De uitbreidingen bevatten ervaring voor .NET-ontwikkelaars en -bibliotheken, de aanpassing geldt ook voor implementatie op basis van Windows. 

De uitbreiding en aanpassing wordt gedaan zodanig dat we geen hoeft een fork van de OSS-projecten en we kunnen gebruikmaken van afgeleide ecosystemen gebaseerd op Storm.

## <a name="processing-model"></a>Het verwerken van model
De gegevens in een SCP is gemodelleerd als ononderbroken streams van tuples. De tuples doorgaans flow in een wachtrij eerst wordt opgehaald en getransformeerd door zakelijke logica die wordt gehost in een Storm-topologie, ten slotte de uitvoer kan worden doorgesluisd als tuples naar een ander SCP-systeem, of worden doorgevoerd voor stores, zoals databases of gedistribueerd bestandssysteem zoals SQL Server.

![Een diagram van een wachtrij die deze gegevens voor verwerking, die een gegevensarchief-feeds](./media/apache-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

In Storm definieert u de Toepassingstopologie van een een grafiek van de berekening. Elk knooppunt in een topologie bevat verwerkingslogica en koppelingen tussen knooppunten geven aan de gegevensstroom. De knooppunten die invoergegevens invoeren in de topologie heten _spouts_, die kan worden gebruikt om de volgorde van de gegevens. De ingevoerde gegevens kan zich bevinden in het bestand Logboeken, transactionele database, systeem-prestatiemeteritem enzovoort. De knooppunten met zowel invoer- en data-stromen worden genoemd _bolts_, wat doen de werkelijke gegevens filteren en selecties en cumulatie-instellingen.

SCP ondersteunt aanbevolen inspanningen, op-één keer en precies-eenmaal gegevensverwerking. In een gedistribueerde toepassing voor streaming verwerkt, kunnen diverse fouten optreden tijdens de verwerking van gegevens, zoals netwerkstoringen, machine storing of gebruikersfout code enzovoort. Op de minste eenmaal verwerken zorgt ervoor dat alle gegevens ten minste één keer wordt verwerkt door af te spelen automatisch dezelfde gegevens als er treedt een fout. Op de minst keer worden verwerkt is eenvoudig en betrouwbaar en ook veel toepassingen geschikt. Als u een toepassing moet exact tellen, is op-één keer verwerkt echter onvoldoende omdat dezelfde gegevens kan mogelijk worden afgespeeld in de Toepassingstopologie. In dat geval, precies-nadat de verwerking is ontworpen om te controleren of het resultaat juist is, zelfs wanneer de gegevens kunnen worden opnieuw afgespeeld en meerdere keren verwerkt.

SCP kan .NET-ontwikkelaars voor het ontwikkelen van toepassingen voor realtime gegevens verwerken terwijl u gebruikmaakt van op Java Virtual Machine (JVM) met Storm op de achtergrond. De .NET- en JVM communiceert via een lokale TCP-sockets. In feite is elke Spout/Bolt de combinatie van een .net/Java-proces, waarin de logica van de gebruiker wordt uitgevoerd in .net-proces als een invoegtoepassing.

Voor het bouwen van een toepassing gegevensverwerking boven op SCP, zijn er verschillende stappen nodig:

* Ontwerp en implementeer de Spouts om op te halen gegevens uit de wachtrij.
* Ontwerpen en implementeren van Bolts voor het verwerken van de ingevoerde gegevens en gegevens opslaan voor externe stores, zoals een Database.
* De topologie ontwerpen, verzenden en uitvoeren van de topologie. De topologie definieert hoekpunten en de gegevens stromen tussen de hoekpunten. SCP zal duren voordat de topologie-specificatie en implementeer deze op een Storm-cluster, waarbij elk hoekpunt dat wordt uitgevoerd op één logische knooppunt. De failover en schalen zal worden afgehandeld door de Taakplanner van Storm.

Dit document maakt gebruik van een aantal eenvoudige voorbeelden om te zien hoe u om de toepassing gegevens verwerken met SCP te bouwen.

## <a name="scp-plugin-interface"></a>SCP-invoegtoepassing Interface
SCP-invoegtoepassingen (of toepassingen) zijn zelfstandige exe's die kunnen beide worden uitgevoerd in Visual Studio tijdens de ontwikkelingsfase bevindt, en worden aangesloten op de pijplijn Storm na de implementatie in productie. Schrijven van de SCP-invoegtoepassing is dezelfde manier als andere standaard Windows-consoletoepassingen schrijven. SCP.NET-platform declareert een interface voor spout/bolt en de code van de invoegtoepassing gebruiker moet deze interfaces implementeren. Het belangrijkste doel van dit ontwerp is dat de gebruiker zich kunt richten op hun eigen bedrijf logics en andere dingen om te worden verwerkt door SCP.NET-platform te verlaten.

De code van de invoegtoepassing gebruiker moet een van de volgende interfaces implementeren, afhankelijk van of de topologie transactionele of niet-transactionele is en of het onderdeel is van een spout of bolt.

* ISCPSpout
* ISCPBolt
* ISCPTxSpout
* ISCPBatchBolt

### <a name="iscpplugin"></a>ISCPPlugin
ISCPPlugin is de algemene interface voor alle soorten invoegtoepassingen. Het is momenteel een dummy-interface.

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

Wanneer `NextTuple()` wordt aangeroepen, de C\# gebruikerscode kan een of meer tuples verzenden. Als er niets om te verzenden, moet deze methode worden geretourneerd zonder dat u iets verzendt. Er moet worden opgemerkt dat `NextTuple()`, `Ack()`, en `Fail()` worden aangeroepen in een lus in een enkele thread in C\# proces. Als er geen tuples om te verzenden, is het beleefd aan NextTuple slaapstand gedurende een korte periode (zoals 10 milliseconden) zijn niet te verspillen te veel CPU.

`Ack()` en `Fail()` alleen wanneer het ack-mechanisme is ingeschakeld in spec bestand worden genoemd. De `seqId` wordt gebruikt voor het identificeren van de tuple op die is bevestigd of mislukt. Dus als ack in niet-transactionele topologie is ingeschakeld, kan de volgende emit-functie moet worden gebruikt in Spout:

    public abstract void Emit(string streamId, List<object> values, long seqId); 

Als ack wordt niet ondersteund in niet-transactionele topologie, de `Ack()` en `Fail()` empty-functie kan worden gehandhaafd.

De `parms` invoerparameter in deze functies is een lege woordenlijst, dit is gereserveerd voor toekomstig gebruik.

### <a name="iscpbolt"></a>ISCPBolt
ISCPBolt is de interface voor niet-transactionele bolt.

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

Wanneer nieuwe tuple beschikbaar is, is de `Execute()` functie is aangeroepen om te verwerken.

### <a name="iscptxspout"></a>ISCPTxSpout
ISCPTxSpout is de interface voor transactionele spout.

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

Net als bij hun niet-transactionele tegenpost `NextTx()`, `Ack()`, en `Fail()` worden aangeroepen in een lus in een enkele thread in C\# proces. Als er geen gegevens om te verzenden, is het beleefd hebben `NextTx` slaapstand gedurende een korte periode (10 milliseconden) niet te verspillen te veel CPU.

`NextTx()` wordt aangeroepen voor het starten van een nieuwe transactie, de out-parameter `seqId` wordt gebruikt voor het identificeren van de transactie, die ook wordt gebruikt in `Ack()` en `Fail()`. In `NextTx()`, gebruiker de gegevens naar Java-zijde kan verzenden. De gegevens worden opgeslagen in ZooKeeper ter ondersteuning van opnieuw afspelen. Omdat de capaciteit van ZooKeeper is beperkt, moet de gebruiker alleen metagegevens verzenden, niet bulksgewijs gegevens in een transactionele spout.

Storm wordt een transactie automatisch herhalen als dit mislukt, dus `Fail()` mag niet worden aangeroepen in normale geval. Maar als het SCP kunt controleren of de metagegevens die zijn gegenereerd door de transactionele spout, kan worden aangeroepen `Fail()` wanneer de metagegevens is ongeldig.

De `parms` invoerparameter in deze functies is een lege woordenlijst, dit is gereserveerd voor toekomstig gebruik.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt
ISCPBatchBolt is de interface voor transactionele bolt.

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

`Execute()` wordt aangeroepen wanneer er nieuwe tuple die binnenkomen in de bolt. `FinishBatch()` wordt aangeroepen wanneer deze transactie is beëindigd. De `parms` invoerparameter is gereserveerd voor toekomstig gebruik.

Voor transactionele topologie is een belangrijk concept – `StormTxAttempt`. Heeft twee velden, `TxId` en `AttemptId`. `TxId` wordt gebruikt voor het identificeren van een specifieke transactie, en voor een gegeven transactie, kunnen er meerdere pogingen als de transactie mislukt en opnieuw afgespeeld. SCP.NET maakt een nieuw ISCPBatchBolt-object voor het verwerken van elke `StormTxAttempt`, net als Storm in Java biedt. Het doel van dit ontwerp is voor de ondersteuning van parallelle transacties verwerken. Gebruiker moet Bewaar deze op er rekening mee dat als de poging van de transactie is voltooid, de bijbehorende ISCPBatchBolt-object wordt vernietigd en garbagecollection die worden verzameld.

## <a name="object-model"></a>Objectmodel
SCP.NET biedt ook een eenvoudige reeks sleutel objecten voor ontwikkelaars om te programmeren. Ze zijn **Context**, **StateStore**, en **SCPRuntime**. Ze worden besproken in het gedeelte van de rest van deze sectie.

### <a name="context"></a>Context
Context biedt een actieve omgeving naar de toepassing. Elk exemplaar ISCPPlugin (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) heeft een bijbehorende contextexemplaar. De functionaliteit van Context kan worden onderverdeeld in twee delen: (1) de statische deel, die beschikbaar in de hele C is\# verwerken, (2) de dynamische onderdeel, is alleen beschikbaar voor het specifieke exemplaar van de Context.

### <a name="static-part"></a>Statische deel
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

`Logger` voor logboekregistratiedoeleinden is opgegeven.

`pluginType` wordt gebruikt om aan te geven van het type van de invoegtoepassing van de C\# proces. Als de C\# proces wordt uitgevoerd in de lokale testmodus (zonder Java), het type van de invoegtoepassing is `SCP_NET_LOCAL`.

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

`Config` wordt geboden om op te halen van parameters voor de configuratie van de Java-kant. De parameters worden doorgegeven aan Java als C\# invoegtoepassing is geïnitialiseerd. De `Config` parameters worden onderverdeeld in twee delen: `stormConf` en `pluginConf`.

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

`stormConf` is van de parameters die zijn gedefinieerd door Storm en `pluginConf` is van de parameters die zijn gedefinieerd door SCP. Bijvoorbeeld:

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

`TopologyContext` wordt geleverd als u de context van de topologie, is het meest geschikt voor onderdelen met meerdere parallelle uitvoering. Hier volgt een voorbeeld:

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
De volgende interfaces zijn relevant zijn voor een bepaalde contextexemplaar. Het Context-exemplaar wordt gemaakt door SCP.NET-platform en doorgegeven aan de gebruikerscode:

    // Declare the Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple to default stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple to the specific stream.
    public abstract void Emit(string streamId, List<object> values);  

Voor niet-transactionele spout ack ondersteunen, de volgende methode beschikbaar:

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

Voor niet-transactionele bolt ack ondersteunen, moet deze expliciet `Ack()` of `Fail()` de tuple ontvangen. En bij het genereren van nieuwe tuple, moet deze ook de ankers van de nieuwe tuple opgeven. De volgende methoden zijn opgegeven.

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a>StateStore
`StateStore` metagegevensservices, monotone reeks genereren en wacht gratis coördinatie biedt. Een hoger niveau gedistribueerde gelijktijdigheid abstracties kunnen worden gebaseerd op `StateStore`, met inbegrip van gedistribueerde wordt vergrendeld, gedistribueerde wachtrijen barrières en transactieservices.

SCP-toepassingen kunnen gebruiken de `State` object om vast te leggen van sommige gegevens in ZooKeeper, met name voor transactionele topologie. Doen zodat, als transactionele spout-crashes en opnieuw opstarten, kan het ophalen van de benodigde informatie uit ZooKeeper en opnieuw opstarten van de pijplijn.

De `StateStore` object voornamelijk heeft deze methoden:

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

De `State` object voornamelijk heeft deze methoden:

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

Voor de `Commit()` methode, wanneer simpleMode is ingesteld op true, worden verwijderd de bijbehorende ZNode in ZooKeeper. Anders wordt de huidige ZNode en het toevoegen van een nieuw knooppunt in de toegewezen verwijderd\_pad.

### <a name="scpruntime"></a>SCPRuntime
SCPRuntime bevat de volgende twee methoden:

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

`Initialize()` wordt gebruikt voor het initialiseren van de SCP-runtime-omgeving. Bij deze methode hoeft de C\# proces maakt verbinding met de Java-zijde en haalt configuratieparameters en de context van de topologie.

`LaunchPlugin()` wordt gebruikt om een vliegende start de berichtenlus van verwerking. In deze lus, de C\# invoegtoepassing ontvangt berichten formulier Java aan clientzijde (inclusief tuples en beheer locatiesignalen) en vervolgens verwerken de berichten, misschien aanroepen van de interfacemethode opgeven door de gebruikerscode. De invoerparameter voor methode `LaunchPlugin()` is een gemachtigde die kan retourneren een object dat ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt-interface implementeren.

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

Voor ISCPBatchBolt, krijgen we `StormTxAttempt` van `parms`, en deze gebruiken om te beoordelen of het is een herhaalde poging. De controle op een poging tot opnieuw afspelen wordt vaak uitgevoerd op de commit-bolt en dit wordt geïllustreerd in de `HelloWorldTx` voorbeeld.

In het algemeen kunnen de SCP-invoegtoepassingen in hier twee modi worden uitgevoerd:

1. Lokale testmodus: In deze modus wordt de SCP-invoegtoepassingen (het C\# gebruikerscode) uitvoeren in Visual Studio tijdens de ontwikkelingsfase bevindt. `LocalContext` kan worden gebruikt in deze modus, waarmee u de methode voor het serialiseren van de verzonden tuples in lokale bestanden en ze terug naar het geheugen te lezen.
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. Normale modus: In deze modus wordt de SCP-invoegtoepassingen zijn gestart door storm java-proces.
   
    Hier volgt een voorbeeld van het SCP-invoegtoepassing starten:
   
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
SCP-topologie-specificatie is een taal domeinspecifieke beschrijven en SCP-topologieën te configureren. Deze is gebaseerd op de Storm Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) en wordt uitgebreid door SCP.

Topologie specificaties kunnen worden verstuurd rechtstreeks naar de storm-cluster voor de uitvoering van via de ***runspec*** opdracht.

SCP.NET heeft de volgende functies voor het definiëren van transactionele topologieën toegevoegd:

| **Nieuwe functies** | **Parameters** | **Beschrijving** |
| --- | --- | --- |
| **TX topolopy** |naam van de topologie<br />spout-kaart<br />bolt-kaart |Definieer een transactionele topologie met de topologienaam van de &nbsp;spouts definitie kaart en de bolts definition-kaart |
| **SCP-tx-spout** |exec-naam<br />argumenten<br />velden |Een transactionele spout definiëren. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met behulp van ***argumenten***.<br /><br />De ***velden*** is de uitvoer-velden voor spout |
| **SCP-tx-batch-bolt** |exec-naam<br />argumenten<br />velden |Definieer een transactionele Batch-Bolt. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met behulp van ***argumenten.***<br /><br />De velden is de uitvoer-velden voor bolt. |
| **SCP-tx-doorvoeren-bolt** |exec-naam<br />argumenten<br />velden |Definieer een transactionele commit-bolt. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met behulp van ***argumenten***.<br /><br />De ***velden*** is de uitvoer-velden voor bolt |
| **nontx topolopy** |naam van de topologie<br />spout-kaart<br />bolt-kaart |Definieer een niet-transactionele-topologie met de topologienaam van de&nbsp; spouts definitie kaart en de toewijzing van de definitie bolts |
| **SCP-spout** |exec-naam<br />argumenten<br />velden<br />parameters |Een niet-transactionele spout definiëren. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met behulp van ***argumenten***.<br /><br />De ***velden*** is de uitvoer-velden voor spout<br /><br />De ***parameters*** zijn optioneel, met bepaalde parameters, zoals 'nontransactional.ack.enabled' opgeven. |
| **SCP-bolt** |exec-naam<br />argumenten<br />velden<br />parameters |Definieer een niet-transactionele Bolt. Deze wordt uitgevoerd met de toepassing met ***exec naam*** met behulp van ***argumenten***.<br /><br />De ***velden*** is de uitvoer-velden voor bolt<br /><br />De ***parameters*** zijn optioneel, met bepaalde parameters, zoals 'nontransactional.ack.enabled' opgeven. |

SCP.NET heeft de volgende trefwoorden gedefinieerd:

| **trefwoorden** | **Beschrijving** |
| --- | --- |
| **: de naam** |Definieer de Topologienaam van de |
| **: topologie** |De topologie met behulp van de vorige functies definiëren en in resources bouwen. |
| **: p** |Definieer de parallelle uitvoering-hint voor elke spout of bolt. |
| **: config** |Definieer de parameter configureren of een bestaande bijwerken |
| **: schema** |Definieer het Schema van Stream. |

En vaak gebruikte parameters:

| **Parameter** | **Beschrijving** |
| --- | --- |
| **"plugin.name"** |naam van het exe-bestand van de C#-invoegtoepassing |
| **"plugin.args"** |invoegtoepassing argumenten |
| **"output.schema"** |Uitvoerschema |
| **"nontransactional.ack.enabled"** |Of ack is ingeschakeld voor niet-transactionele topologie |

De opdracht runspec samen met de bits is geïmplementeerd, het gebruik is, zoals:

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

De ***resource-dir*** parameter is optioneel, moet u dit opgeven wanneer u wilt een C sluit\# toepassing en deze map bevat de toepassing en de afhankelijkheden en configuraties.

De ***klassepad*** parameter is ook optioneel. Het wordt gebruikt om op te geven van het klassepad Java als de spec bestand Java Spout of Bolt bevat.

## <a name="miscellaneous-features"></a>Diverse functies
### <a name="input-and-output-schema-declaration"></a>Invoer en uitvoer Schemadeclaratie
Gebruikers kunnen verzenden tuples in C\# processen, het platform moet de tuple serialiseren in byte [], overbrengen naar Java-zijde en Storm deze tuple worden overgebracht naar de doelen. In de tussentijd zorgen in downstream-onderdelen, C\# processen tuples van java-zijde ontvangt, en deze converteren naar de oorspronkelijke typen per platform, alle deze bewerkingen worden verborgen door het Platform.

Ter ondersteuning van de serialisatie en deserialisatie, moet gebruikerscode het schema van de invoer en uitvoer declareren.

De invoer/uitvoer-stream-schema wordt gedefinieerd als een woordenlijst. De sleutel is de StreamId. De waarde is de typen van de kolommen. Het onderdeel kan meerdere gegevensstromen gedeclareerd hebben.

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

Ontwikkelaars moeten ervoor zorgen dat de tuples verzonden op het schema gedefinieerd voor deze stroom te volgen, anders wordt het systeem een runtime-uitzondering genereert.

### <a name="multi-stream-support"></a>Meerdere Stream-ondersteuning
SCP ondersteunt gebruikerscode om te verzenden of ontvangen van meerdere afzonderlijke stromen op hetzelfde moment. Terwijl de Emit-methode een optionele stream-ID-parameter gebruikt, wordt de ondersteuning in de Context-object weergegeven.

Twee methoden voor het SCP.NET-Context-object er zijn toegevoegd. Ze worden gebruikt om te verzenden Tuple of Tuples om op te geven StreamId. De StreamId is een tekenreeks en het moet consistent in beide C\# en de topologie Definition-specificaties.

        /* Emit tuple to the specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

Het verzenden van naar een niet-bestaande stream zorgt ervoor dat de runtime-uitzonderingen.

### <a name="fields-grouping"></a>Velden groeperen
De ingebouwde die velden groeperen in Strom niet goed in SCP.NET werkt. Alle gegevenstypen van de velden daadwerkelijk byte [] zijn aan de Java-Proxy en de velden groeperen van de hash-code voor byte []-object gebruikt voor het uitvoeren van de groepering. De byte [] object hash-code is het adres van dit object in het geheugen. Dus is de groepering onjuist voor twee-byte [] objecten die dezelfde inhoud, maar niet hetzelfde adres delen.

SCP.NET een groeperingsmethode met de aangepaste toegevoegd en wordt de inhoud van de byte [] voor de groepering gebruikt. In **SPEC** -bestand, de syntaxis is, zoals:

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


Hier

1. 'scp-veld-group' betekent 'Aangepast veld groepering wordt geïmplementeerd door SCP'.
2. ": tx 'of': niet-tx ' betekent dat als het transactionele topologie is. We nodig deze informatie omdat de startIndex anders in tx vs. niet-tx topologieën is.
3. [0,1] betekent een hash-set veld-id's, vanaf 0.

### <a name="hybrid-topology"></a>Hybride topologie
De systeemeigen Storm is geschreven in Java. En SCP.Net is uitgebreid om te schakelen C\# ontwikkelaars kunnen schrijven C\# code voor het verwerken van hun zakelijke logica. Maar ondersteunt ook hybride topologieën, die niet alleen C bevat\# spouts/bolts, maar ook Java Spout/Bolts.

### <a name="specify-java-spoutbolt-in-spec-file"></a>Java Spout/Bolt in spec bestand opgeven
In spec bestand "scp-spout" en "scp-bolt" kunnen ook worden gebruikt om op te geven Java Spouts en Bolts, Hier volgt een voorbeeld:

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

Hier `microsoft.scp.example.HybridTopology.Generator` is de naam van de klasse Java Spout.

### <a name="specify-java-classpath-in-runspec-command"></a>Geef het klassepad van Java in runSpec opdracht
Als u verzenden met Java Spouts of Bolts topologie wilt, moet u eerst de Java Spouts of Bolts compileren en ophalen van de Jar-bestanden. Vervolgens moet u de java-klassepad met de Jar-bestanden bij het indienen van de topologie. Hier volgt een voorbeeld:

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

Hier **voorbeelden\\HybridTopology\\java\\doel\\**  is de map met de Java-Spout/Bolt Jar-bestand.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Serialisatie en deserialisatie tussen Java- en C\#
SCP-onderdeel bevat Java kant- en C\# aan clientzijde. Om te communiceren met systeemeigen Java Spouts/Bolts, serialisatie/deserialisatie moet worden uitgevoerd tussen Java kant- en C\# zijde, zoals wordt geïllustreerd in het volgende diagram.

![diagram van java-component te verzenden naar de SCP-component te verzenden naar de Java-onderdeel](./media/apache-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. **Serialisatie in Java kant- en deserialisatie in C\# aan clientzijde**
   
   Eerst bieden we standaardimplementatie voor serialisatie in Java-kant- en deserialisatie in C\# aan clientzijde. De serialisatiemethode in de Java-zijde kan worden opgegeven in SPEC bestand:
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   De deserialisatie-methode in C\# kant moet worden opgegeven in C\# gebruikerscode:
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   Deze standaardimplementatie moet de meeste gevallen verwerken, mits het gegevenstype niet te complex is. Voor bepaalde gevallen, omdat het gegevenstype van de gebruiker te complex is of omdat de prestaties van onze standaardimplementatie voldoet niet aan de vereiste van de gebruiker, gebruikers kunnen invoegtoepassing hun eigen implementatie.
   
   De interface serialiseren in java aan clientzijde wordt gedefinieerd als:
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   De interface deserialize in C\# aan clientzijde wordt gedefinieerd als:
   
   openbare interface ICustomizedInteropCSharpDeserializer
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. **Serialisatie in C\# kant- en deserialisatie in Java-kant**
   
   De methode voor kleurserialisatie in C\# kant moet worden opgegeven in C\# gebruikerscode:
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   De deserialisatie-methode in Java kant moet worden opgegeven in SPEC bestand:
   
     (scp-spout
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   Hier 'microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer' is de naam van Deserializer en "microsoft.scp.example.HybridTopology.Person" is dat de doelklasse van de gegevens is gedeserialiseerd naar.
   
   Gebruiker kan ook plug-in hun eigen implementatie van C\# serializer en Java Deserializer. Deze code is de interface voor C\# serializer:
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   Deze code is de interface voor Java-Deserializer:
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a>SCP-Host-modus
Gebruiker kan in deze modus wordt de codes naar dll-bestand compileren en SCPHost.exe geleverd door SCP gebruiken voor het indienen van topologie. Het spec bestand ziet er uit als de volgende code:

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

Hier `plugin.name` is opgegeven als `SCPHost.exe` geleverd door SCP-SDK. SCPHost.exe accepteert drie parameters:

1. De eerste is de DLL-naam, die is `"HelloWorld.dll"` in dit voorbeeld.
2. De tweede waarde is de naam van de klasse, namelijk `"Scp.App.HelloWorld.Generator"` in dit voorbeeld.
3. De derde is de naam van een openbare statische methode, die kan worden aangeroepen om een exemplaar van ISCPPlugin.

In de modus van de host, gebruikerscode wordt gecompileerd als dll-bestand en wordt aangeroepen door SCP-platform. SCP-platform kunt dus de volledige controle over de hele verwerkingslogica ophalen. Dus aangeraden onze klanten om in te dienen topologie in de modus voor SCP-host, omdat deze kan de ontwikkelervaring vereenvoudigen en naar ons meer flexibiliteit en betere compatibiliteit voor latere versie ook brengen.

## <a name="scp-programming-examples"></a>SCP programmeren voorbeelden
### <a name="helloworld"></a>Hallo wereld
**HelloWorld** is een eenvoudig voorbeeld om een voorproefje van SCP.Net weer te geven. Hierbij een niet-transactionele-topologie met een spout met de naam **generator**, en twee bolts met de naam **splitsen** en **teller**. De spout **generator** willekeurig zinnen gegenereerd en het verzenden van deze zinnen op **splitsen**. De bolt **splitsen** splitst de zinnen op woorden en hoe deze woorden **teller** bolt. De bout 'item' maakt gebruik van een woordenlijst om vast te leggen van het nummer van het exemplaar van elk woord.

Er zijn twee bestanden spec **HelloWorld.spec** en **HelloWorld\_EnableAck.spec** voor dit voorbeeld. In de C\# code, het vindt of ack met het ophalen van de pluginConf van Java aan clientzijde is ingeschakeld.

    /* demo how to get pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

Als ack is ingeschakeld, wordt een woordenlijst in de spout gebruikt de tuples die niet bevestigd zijn in de cache. Als Fail() wordt aangeroepen, wordt de mislukte tuple replay:

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
De **HelloWorldTx** voorbeeld ziet u hoe u voor het implementeren van transactionele topologie. Heeft een spout met de naam **generator**, een batch-bolt met de naam **gedeeltelijk-count**, en een commit-bolt met de naam **aantal som**. Er zijn ook drie vooraf gemaakte txt-bestanden: **DataSource0.txt**, **DataSource1.txt**, en **DataSource2.txt**.

In elke transactie, de spout **generator** willekeurig twee bestanden uit de vooraf gemaakte drie bestanden geselecteerd, en hoe de namen van de twee bestanden naar de **gedeeltelijk-count** bolt. De bolt **gedeeltelijk-count** haalt het bestand naam van de ontvangen tuple en open het bestand en telt het aantal woorden in dit bestand en ten slotte het word-nummer te verzenden de **aantal som** bolt. De **aantal som** bolt bevat een overzicht van het totale aantal.

Om te realiseren **exact één keer** semantiek, de commit-bolt **aantal som** nodig hebt om te beoordelen of het is een herhaalde transactie. In dit voorbeeld heeft deze een statisch lidvariabele:

    public static long lastCommittedTxId = -1; 

Wanneer een ISCPBatchBolt-exemplaar is gemaakt, krijgt de `txAttempt` van invoerparameters die zijn opgegeven:

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

Wanneer `FinishBatch()` wordt aangeroepen, de `lastCommittedTxId` worden bijgewerkt als het is niet een herhaalde transactie.

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
Deze topologie bevat een Java-Spout en een C\# Bolt. Maakt gebruik van de standaard-serialisatie en deserialisatie in uitvoering zijn geleverd door SCP-platform. Zie de **HybridTopology.spec** in **voorbeelden\\HybridTopology** map voor de spec bestandsgegevens en **SubmitTopology.bat** voor informatie over het opgeven van Java het klassepad van.

### <a name="scphostdemo"></a>SCPHostDemo
In dit voorbeeld is hetzelfde als HelloWorld in wezen. Het enige verschil is dat de gebruikerscode wordt gecompileerd als dll-bestand en de topologie wordt ingediend via SCPHost.exe. Zie de sectie 'SCP Host modus' voor meer gedetailleerde uitleg.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende documenten voor voorbeelden van Storm-topologieën die zijn gemaakt met behulp van de SCP's:

* [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)
* [Process events from Azure Event Hubs met Storm op HDInsight gebeurtenissen](apache-storm-develop-csharp-event-hub-topology.md)
* [Voertuigsensorgegevens van Event Hubs met Storm op HDInsight verwerken](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Uitpakken, transformeren en laden (ETL) van Azure Eventhubs naar HBase](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
