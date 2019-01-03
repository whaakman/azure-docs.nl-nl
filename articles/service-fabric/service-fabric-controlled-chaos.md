---
title: Chaos veroorzaken in Service Fabric-clusters | Microsoft Docs
description: Gebruik Foutinjectie en Cluster Analysis Service-API's voor het beheren van Chaos in het cluster.
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/05/2018
ms.author: motanv
ms.openlocfilehash: a1b334b34e8e234d9ce5cc5ad5cd77bf5ba7118c
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555520"
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Gecontroleerde Chaos in Service Fabric-clusters veroorzaken
Grote schaal gedistribueerde systemen cloudinfrastructuren zijn inherent onbetrouwbaar. Azure Service Fabric biedt ontwikkelaars de mogelijkheid om betrouwbare gedistribueerde services op een onbetrouwbaar-infrastructuur te ontwikkelen. Ontwikkelaars moeten mogelijk voor het testen van de stabiliteit van hun services terwijl de onderliggende infrastructuur onbetrouwbare gecompliceerd statusovergangen vanwege fouten ondergaat voor het schrijven van robuuste gedistribueerde services op een onbetrouwbaar-infrastructuur.

De [Foutinjectie en analyse-clusterservice](https://docs.microsoft.com/azure/service-fabric/service-fabric-testability-overview) (ook wel bekend als de Fault Analysis Service) biedt ontwikkelaars de mogelijkheid om te veroorzaken fouten voor het testen van hun services. Deze gericht fouten, zoals gesimuleerde [opnieuw starten van een partitie](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), kunt u de meest voorkomende statusovergangen uitoefenen. Echter fouten gerichte gesimuleerde fouten per definitie zijn gericht en dus kunnen missen waarin u alleen in moeilijk te voorspellen, lang en gecompliceerd reeks statusovergangen. Voor een zuivere testen, kunt u de Chaos gebruiken.

Chaos simuleert periodieke, interleaved fouten (vensters en geforceerde afsluiting) in het cluster gedurende langere tijd. Een correcte fout bestaat uit een verzameling van Service Fabric-API-aanroepen, opnieuw opstarten replica fouttolerantie is bijvoorbeeld een correcte veroorzaakt omdat dit een gevolgd door een open op een replica sluit. Replica verwijderen toe, plaats de primaire replica en secundaire replica verplaatsen worden de andere vensters fouten door Chaos uitgeoefend. Geforceerde afsluiting fouten zijn proces wordt afgesloten, als een knooppunt opnieuw opstarten en codepakket opnieuw. 

Zodra u Chaos met de snelheid en het soort fouten hebt geconfigureerd, kunt u de Chaos via C#, Powershell of REST-API om te beginnen met het genereren van fouten in het cluster en uw services kunt starten. U kunt na welke stopt Chaos automatisch Chaos om uit te voeren voor een opgegeven periode (bijvoorbeeld voor één uur), configureren, of u StopChaos API (C#, Powershell of REST) om te voorkomen dat deze op elk gewenst moment kunt aanroepen.

> [!NOTE]
> In de huidige vorm induceert Chaos alleen veilige fouten, dit houdt dat in de afwezigheid van externe fouten een quorumverlies of verlies van gegevens nooit wordt uitgevoerd.
>

Terwijl Chaos wordt uitgevoerd, is het resultaat van andere gebeurtenissen die de status van de uitvoering op dit moment vastleggen. Een ExecutingFaultsEvent bevat bijvoorbeeld de fouten die Chaos heeft besloten om uit te voeren in die iteratie. Een ValidationFailedEvent bevat de details van een mislukte validatie (status of de stabiliteit problemen) die tijdens de validatie van het cluster is gevonden. U kunt de GetChaosReport-API (C#, Powershell of REST) om op te halen van het rapport van Chaos uitgevoerd aanroepen. Deze gebeurtenissen ophalen bewaard in een [betrouwbare dictionary](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-reliable-collections), heeft een beleid moet worden afgekapt is bepaald door twee configuraties: **MaxStoredChaosEventCount** (de standaardwaarde is 25000) en **StoredActionCleanupIntervalInSeconds** (de standaardwaarde is 3600). Elke *StoredActionCleanupIntervalInSeconds* Chaos controles en alle, maar de meest recente *MaxStoredChaosEventCount* gebeurtenissen worden verwijderd uit de betrouwbare dictionary.

## <a name="faults-induced-in-chaos"></a>Fouten die in Chaos
Chaos genereert fouten in de hele Service Fabric-cluster en fouten die zichtbaar zijn in maanden of jaren in een paar uur worden gecomprimeerd. De combinatie van interleaved fouten met de snelheid van hoge fouttolerantie vindt hoek gevallen die anders mogelijk worden gemist. In deze oefening van Chaos leidt tot een aanzienlijke verbetering van de kwaliteit van de code van de service.

Chaos induceert fouten in de volgende categorieën:

* Een knooppunt opnieuw starten
* Opnieuw opstarten van een geïmplementeerde codepakket
* Een replica verwijderen
* Opnieuw opstarten van een replica
* Verplaatsen van een primaire replica (configureerbaar)
* Verplaatsen van een secundaire replica (configureerbaar)

Chaos uitgevoerd in meerdere pogingen. Elke herhaling bestaat uit de fouten en de clustervalidatie voor de opgegeven periode. U kunt de tijd voor het cluster te laten stabiliseren en voor de validatie te voltooien. Als een fout wordt gevonden in de clustervalidatie, genereert Chaos- en een ValidationFailedEvent met de UTC-timestamp en de foutdetails van de zich blijft voordoen. Neem bijvoorbeeld een exemplaar van Chaos die is ingesteld op een uur worden uitgevoerd met een maximum van drie gelijktijdige fouten. Chaos induceert drie fouten en vervolgens valideert de clusterstatus. Deze doorloopt de vorige stap totdat deze bewerking expliciet is gestopt via de API StopChaosAsync of één uur is wordt doorgegeven. Als het cluster niet in orde in elke iteratie wordt (dat wil zeggen, deze niet wordt bepaald of het niet wordt weergegeven in orde binnen de MaxClusterStabilizationTimeout doorgegeven in), Chaos genereert een ValidationFailedEvent. Deze gebeurtenis geeft aan dat er iets is iets verkeerd gegaan en mogelijk verder onderzoek.

Als u welke fouten Chaos veroorzaakte, kunt u GetChaosReport API (powershell, C# of REST). De API haalt het volgende segment van de Chaos-rapport op basis van het vervolgtoken doorgegeven of het doorgegeven-tijdsbereik. U kunt de ContinuationToken om op te halen van de volgende segment van het rapport Chaos opgeven of kunt u het tijdsbereik via StartTimeUtc en EndTimeUtc, maar u kunt zowel de ContinuationToken en het tijdsbereik opgeven in de aanroep van dezelfde. Wanneer er meer dan 100 Chaos-gebeurtenissen, wordt het rapport Chaos geretourneerd in segmenten waarin een segment bevat niet langer zijn dan 100 Chaos-gebeurtenissen.

## <a name="important-configuration-options"></a>Belangrijke configuratie-opties
* **TimeToRun**: Totale uitvoeringstijd van Chaos uitgevoerd voordat het klaar is met succes. U kunt Chaos stoppen voordat deze via de API StopChaos voor de periode TimeToRun uitgevoerd.

* **MaxClusterStabilizationTimeout**: De maximale hoeveelheid tijd moet worden gewacht op het cluster worden in orde voordat een ValidationFailedEvent produceren. Deze wachttijd is het verminderen van de werkbelasting op het cluster, terwijl deze wordt hersteld. Controles zijn:
  * Als de clusterstatus van het OK is
  * Als de servicestatus van de OK is
  * Als de doelreplica grootte instellen is voor de servicepartitie bereikt
  * Dat er geen InBuild-replica's zijn
* **MaxConcurrentFaults**: Het maximale aantal gelijktijdige fouten die zijn die in elke iteratie. De hogere Chaos van het getal, hoe meer agressief is en de failovers en de status overgang combinaties dat het cluster wordt verstuurd via zijn ook complexere. 

> [!NOTE]
> Ongeacht hoe een waardevolle *MaxConcurrentFaults* heeft, Chaos garandeert - in de afwezigheid van externe fouten, is er geen quorum verloren gaan of het verlies van gegevens.
>

* **EnableMoveReplicaFaults**: Hiermee schakelt de fouten die ertoe leiden de primaire of secundaire replica's dat te verplaatsen of. Deze fouten worden standaard ingeschakeld.
* **WaitTimeBetweenIterations**: De hoeveelheid tijd moet worden gewacht tussen pogingen. Dat wil zeggen, wordt de hoeveelheid tijd Chaos onderbroken na het uitvoeren van een ronde van fouten en de bijbehorende validatie van de status van het cluster hebben voltooid. Hoe hoger de waarde des te lager is de snelheid van gemiddelde fault-injectie.
* **WaitTimeBetweenFaults**: De hoeveelheid tijd moet worden gewacht tussen twee opeenvolgende fouten in een enkele iteratie. Hoe hoger de waarde, des te lager de waarde voor concurrency van (of de overlapping tussen) bedrijfsstoringen.
* **ClusterHealthPolicy**: Cluster statusbeleid wordt gebruikt om te valideren dat de status van het cluster tussen Chaos herhalingen. Als de clusterstatus fout heeft of als het geval is een onverwachte uitzondering opgetreden tijdens het uitvoeren van de fout, Chaos 30 minuten voordat u de volgende-statuscontrole wacht-zodat het cluster beschikken over enige tijd recuperate.
* **Context**: Een verzameling van (string, string) typt u sleutel-waardeparen. De kaart kan worden gebruikt om vast te leggen van de informatie over het uitvoeren van Chaos. Er mag niet meer dan 100 dergelijke paren en elke tekenreeks (sleutel of waarde) mag maximaal 4095 tekens lang zijn. Deze kaart is ingesteld door de starter van de Chaos uitgevoerd voor het opslaan van de context over de specifieke uitvoering (optioneel).
* **Chaostargetfilter kunnen**: Dit filter kan worden gebruikt voor doel Chaos fouten alleen naar bepaalde typen of alleen naar bepaalde exemplaren van de toepassing. Als chaostargetfilter kunnen niet wordt gebruikt, bedrijfsstoringen Chaos alle entiteiten van het cluster. Als chaostargetfilter kunnen wordt gebruikt, bedrijfsstoringen Chaos alleen de entiteiten die voldoen aan de specificatie chaostargetfilter kunnen. NodeTypeInclusionList en ApplicationInclusionList kunt alleen union semantiek. Met andere woorden, is het niet mogelijk om op te geven van een snijpunt van NodeTypeInclusionList en ApplicationInclusionList. Het is bijvoorbeeld niet mogelijk om op te geven "fault deze toepassing alleen wanneer deze zich op dit knooppunttype." Zodra een entiteit is opgenomen in NodeTypeInclusionList of ApplicationInclusionList, kan niet die entiteit worden uitgesloten chaostargetfilter kunnen gebruiken. Zelfs als applicationX niet wordt weergegeven in ApplicationInclusionList, in bepaalde iteratie Chaos applicationX kunnen worden is mislukt omdat dit gebeurt op een knooppunt van het nodeTypeY die is opgenomen in NodeTypeInclusionList. Als zowel NodeTypeInclusionList en ApplicationInclusionList null of leeg zijn zijn, is er een ArgumentException gegenereerd.
    * **NodeTypeInclusionList**: Een lijst met typen in Chaos fouten moeten worden opgenomen. Alle typen fouten (knooppunt opnieuw opstarten, opnieuw opstarten codepackage replica verwijderen, replica opnieuw, verplaatsen primaire en secundaire verplaatsen) zijn ingeschakeld voor de knooppunten van de volgende knooppunttypen. Als een nodetype (bijvoorbeeld NodeTypeX) niet wordt weergegeven in de NodeTypeInclusionList en vervolgens op fouten knooppunt (zoals NodeRestart) nooit worden ingeschakeld voor de knooppunten van NodeTypeX, maar fouten voor het pakket en de replica van code kunnen nog steeds worden ingeschakeld voor NodeTypeX als een toepassing in de ApplicationInclusionList gebeurt zich bevinden op een knooppunt van NodeTypeX. Maximaal 100 knooppunt Typenamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van een configuratie is vereist voor MaxNumberOfNodeTypesInChaosTargetFilter configuratie.
    * **ApplicationInclusionList**: Een lijst van toepassing URI's in Chaos fouten moeten worden opgenomen. Alle replica's die behoren tot de services van deze toepassingen zijn aanmerking voor replica-fouten (opnieuw opstarten replica, replica verwijderen, verplaatsen primaire en secundaire verplaatsen) door Chaos. Chaos mogelijk opnieuw opstarten van een codepakket alleen als het codepakket als host fungeert voor replica's van deze toepassingen alleen. Als een toepassing niet in deze lijst wordt weergegeven, kan het nog steeds mislukt in bepaalde iteratie Chaos als beëindigd door de toepassing van op een knooppunt van een knooppunttype dat is opgenomen in NodeTypeInclusionList. Maar als applicationX is gekoppeld aan nodeTypeY door middel van plaatsingsbeperkingen en applicationX afwezig is van ApplicationInclusionList en nodeTypeY is afwezig van NodeTypeInclusionList, klikt u vervolgens applicationX wordt nooit worden is mislukt. Maximaal 1000 toepassingsnamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van een configuratie is vereist voor MaxNumberOfApplicationsInChaosTargetFilter configuratie.

## <a name="how-to-run-chaos"></a>Het uitvoeren van Chaos

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }
        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;

            // The maximum amount of time to wait for all cluster entities to become stable and healthy. 
            // Chaos executes in iterations and at the start of each iteration it validates the health of cluster entities. 
            // During validation if a cluster entity is not stable and healthy within MaxClusterStabilizationTimeoutInSeconds, Chaos generates a validation failed event.
            var maxClusterStabilizationTimeout = TimeSpan.FromSeconds(30.0);

            var timeToRun = TimeSpan.FromMinutes(60.0);

            // MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration. 
            // Chaos executes in iterations and two consecutive iterations are separated by a validation phase. 
            // The higher the concurrency, the more aggressive the injection of faults -- inducing more complex series of states to uncover bugs. 
            // The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
            var maxConcurrentFaults = 3;

            // Describes a map, which is a collection of (string, string) type key-value pairs. The map can be used to record information about
            // the Chaos run. There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.
            // This map is set by the starter of the Chaos run to optionally store the context about the specific run.
            var startContext = new Dictionary<string, string>{{"ReasonForStart", "Testing"}};

            // Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the lower the fault injection rate.
            var waitTimeBetweenIterations = TimeSpan.FromSeconds(10);

            // Wait time (in seconds) between consecutive faults within a single iteration. 
            // The larger the value, the lower the overlapping between faults and the simpler the sequence of state transitions that the cluster goes through. 
            // The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
            var waitTimeBetweenFaults = TimeSpan.Zero;

            // Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
            var clusterHealthPolicy = new ClusterHealthPolicy
            {
                ConsiderWarningAsError = false,
                MaxPercentUnhealthyApplications = 100,
                MaxPercentUnhealthyNodes = 100
            };

            // All types of faults, restart node, restart code package, restart replica, move primary replica, and move secondary replica will happen
            // for nodes of type 'FrontEndType'
            var nodetypeInclusionList = new List<string> { "FrontEndType"};

            // In addition to the faults included by nodetypeInclusionList, 
            // restart code package, restart replica, move primary replica, move secondary replica faults will happen for 'fabric:/TestApp2'
            // even if a replica or code package from 'fabric:/TestApp2' is residing on a node which is not of type included in nodeypeInclusionList.
            var applicationInclusionList = new List<string> { "fabric:/TestApp2" };

            // List of cluster entities to target for Chaos faults.
            var chaosTargetFilter = new ChaosTargetFilter
            {
                NodeTypeInclusionList = nodetypeInclusionList,
                ApplicationInclusionList = applicationInclusionList
            };

            var parameters = new ChaosParameters(
                maxClusterStabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun,
                startContext,
                waitTimeBetweenIterations,
                waitTimeBetweenFaults,
                clusterHealthPolicy) {ChaosTargetFilter = chaosTargetFilter};

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            string continuationToken = null;

            while (true)
            {
                ChaosReport report;
                try
                {
                    report = string.IsNullOrEmpty(continuationToken)
                        ? client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult()
                        : client.TestManager.GetChaosReportAsync(continuationToken).GetAwaiter().GetResult();
                }
                catch (Exception e)
                {
                    if (e is FabricTransientException)
                    {
                        Console.WriteLine("A transient exception happened: '{0}'", e);
                    }
                    else if(e is TimeoutException)
                    {
                        Console.WriteLine("A timeout exception happened: '{0}'", e);
                    }
                    else
                    {
                        throw;
                    }

                    Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
                    continue;
                }

                continuationToken = report.ContinuationToken;

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$clusterConnectionString = "localhost:19000"
$timeToRunMinute = 60

# The maximum amount of time to wait for all cluster entities to become stable and healthy. 
# Chaos executes in iterations and at the start of each iteration it validates the health of cluster entities. 
# During validation if a cluster entity is not stable and healthy within MaxClusterStabilizationTimeoutInSeconds, Chaos generates a validation failed event.
$maxClusterStabilizationTimeSecs = 30

# MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration. 
# Chaos executes in iterations and two consecutive iterations are separated by a validation phase. 
# The higher the concurrency, the more aggressive the injection of faults -- inducing more complex series of states to uncover bugs. 
# The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
$maxConcurrentFaults = 3

# Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the lower the fault injection rate.
$waitTimeBetweenIterationsSec = 10

# Wait time (in seconds) between consecutive faults within a single iteration. 
# The larger the value, the lower the overlapping between faults and the simpler the sequence of state transitions that the cluster goes through. 
# The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
$waitTimeBetweenFaultsSec = 0

# Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
$clusterHealthPolicy = new-object -TypeName System.Fabric.Health.ClusterHealthPolicy
$clusterHealthPolicy.MaxPercentUnhealthyNodes = 100
$clusterHealthPolicy.MaxPercentUnhealthyApplications = 100
$clusterHealthPolicy.ConsiderWarningAsError = $False

# Describes a map, which is a collection of (string, string) type key-value pairs. The map can be used to record information about
# the Chaos run. There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.
# This map is set by the starter of the Chaos run to optionally store the context about the specific run.
$context = @{"ReasonForStart" = "Testing"}

#List of cluster entities to target for Chaos faults.
$chaosTargetFilter = new-object -TypeName System.Fabric.Chaos.DataStructures.ChaosTargetFilter
$chaosTargetFilter.NodeTypeInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# All types of faults, restart node, restart code package, restart replica, move primary replica, and move secondary replica will happen
# for nodes of type 'FrontEndType'
$chaosTargetFilter.NodeTypeInclusionList.AddRange( [string[]]@("FrontEndType") )
$chaosTargetFilter.ApplicationInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# In addition to the faults included by nodetypeInclusionList, 
# restart code package, restart replica, move primary replica, move secondary replica faults will happen for 'fabric:/TestApp2'
# even if a replica or code package from 'fabric:/TestApp2' is residing on a node which is not of type included in nodeypeInclusionList.
$chaosTargetFilter.ApplicationInclusionList.Add("fabric:/TestApp2")

Connect-ServiceFabricCluster $clusterConnectionString

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRunMinute -MaxConcurrentFaults $maxConcurrentFaults -MaxClusterStabilizationTimeoutSec $maxClusterStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ClusterHealthPolicy $clusterHealthPolicy -ChaosTargetFilter $chaosTargetFilter

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                Write-Host $e
                # When Chaos stops, a StoppedEvent is created.
                # If a StoppedEvent is found, exit the loop.
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    return
                }
            }
        }
    }

    Start-Sleep -Seconds 1
}
```
