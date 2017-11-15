---
title: Chaos veroorzaken in Service Fabric-clusters | Microsoft Docs
description: Fout injectie en Cluster Analysis Service-API's gebruiken voor het beheren van Chaos in het cluster.
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/10/2017
ms.author: motanv
ms.openlocfilehash: c78d9e77d807f3ccf8c1f56d856abad8135989c2
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Veroorzaken gecontroleerde Chaos in Service Fabric-clusters
Grote gedistribueerde systemen zoals cloudinfrastructuren inherent onbetrouwbaar worden. Azure Service Fabric kunnen ontwikkelaars schrijven betrouwbare gedistribueerde services op een onbetrouwbaar-infrastructuur. Ontwikkelaars moeten kunnen testen van de stabiliteit van hun services terwijl de onderliggende infrastructuur onbetrouwbaar wordt verzonden via ingewikkeld statusovergangen als gevolg van fouten voor het schrijven van robuuste gedistribueerde services op een onbetrouwbaar-infrastructuur.

De [fout injectie en analyse van de clusterservice](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (ook wel bekend als de fout Analysis Service) biedt ontwikkelaars de mogelijkheid om te veroorzaken fouten voor het testen van hun services. Deze gericht fouten, zoals gesimuleerde [opnieuw starten van een partitie](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), kunt u de meest voorkomende statusovergangen uitoefenen. Echter fouten gerichte gesimuleerde fouten per definitie zijn gericht en dus mist mogelijk die weergeven van alleen in moeilijk te voorspellen, lang en gecompliceerd reeks statusovergangen. Voor een zuivere testen, kunt u Chaos.

Chaos simuleert periodieke, interleaved fouten (correcte en geforceerde afsluiting) in het cluster gedurende langere perioden. Een correcte fout bestaat uit een reeks Service Fabric-API-aanroepen, opnieuw opstarten replica fault is bijvoorbeeld een correcte veroorzaakt omdat dit een sluiten gevolgd door een open op een replica. Replica verwijderen, verplaatsen van de primaire replica en verplaatsen secundaire replica zijn de andere systemen op correct wijze fouten door Chaos uitgeoefend. Geforceerde afsluiting fouten zijn proces wordt afgesloten, zoals knooppunt en restrat code pacakge opnieuw opstarten. 

Wanneer u Chaos met de frequentie en het soort fouten hebt geconfigureerd, kunt u beginnen Chaos met C#, Powershell of REST-API om te beginnen met het genereren van fouten in het cluster en in uw services. U kunt Chaos om uit te voeren voor een opgegeven periode (bijvoorbeeld: voor één uur) automatisch na waarbij stopt Chaos configureren of u StopChaos API (C#, Powershell of REST) om deze te stoppen op elk gewenst moment kunt aanroepen.

> [!NOTE]
> In de huidige vorm induceert Chaos alleen veilige fouten die impliceert dat bij gebrek aan externe fouten een quorumverlies of verlies van gegevens nooit plaatsvindt.
>

Terwijl Chaos wordt uitgevoerd, is het resultaat van andere gebeurtenissen die de status van de uitvoeren op het moment dat vastleggen. Een ExecutingFaultsEvent bevat bijvoorbeeld de fouten die Chaos heeft besloten uit te voeren in die iteratie. Een ValidationFailedEvent bevat de details van een mislukte validatie (health of stabiliteit problemen) die tijdens de validatie van het cluster is gevonden. U kunt de API GetChaosReport (C#, Powershell of REST) als u het rapport over de uitgevoerde Chaos aanroepen. Deze gebeurtenissen ophalen permanent in een [betrouwbare woordenlijst](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), heeft een beleid moet worden afgekapt is bepaald door twee configuraties: **MaxStoredChaosEventCount** (de standaardwaarde is 25000) en  **StoredActionCleanupIntervalInSeconds** (de standaardwaarde is 3600). Elke *StoredActionCleanupIntervalInSeconds* Chaos controles en alle maar het meest recente *MaxStoredChaosEventCount* gebeurtenissen, permanent worden verwijderd van het betrouwbare woordenboek.

## <a name="faults-induced-in-chaos"></a>Fouten worden bewerkstelligd in Chaos
Chaos genereert fouten over de hele Service Fabric-cluster en comprimeert fouten die zijn zichtbaar in maanden of jaren in een paar uur. De combinatie van interleaved fouten met de snelheid van hoge fouttolerantie vindt hoek aanvragen die mogelijk anders worden overgeslagen. In deze oefening dank leidt tot een aanzienlijke verbetering van de kwaliteit van de code van de service.

Chaos induceert fouten uit de volgende categorieën:

* Een knooppunt opnieuw opstarten
* Een geïmplementeerde codepakket niet opnieuw starten
* Een replica verwijderen
* Een replica starten
* Verplaatsen van een primaire replica (configureren)
* Verplaatsen van een secundaire replica (configureren)

Chaos in meerdere pogingen uitgevoerd. Elke herhaling bestaat uit de fouten en clustervalidatie voor de opgegeven periode. De tijd voor het cluster stabiel en voor de validatie mislukt, kunt u configureren. Als een fout in validatie wordt gevonden, wordt Chaos genereert en een ValidationFailedEvent met de UTC-timestamp en de foutdetails zich blijft voordoen. Neem bijvoorbeeld een exemplaar dank dat is ingesteld op een uur worden uitgevoerd met een maximum van drie gelijktijdige fouten. Chaos induceert drie fouten en vervolgens valideert de status van het cluster. Doorlopen de vorige stap totdat deze expliciet gestopt via de API StopChaosAsync of één uur wordt doorgegeven. Als het cluster in elke iteratie slecht (dat wil zeggen, deze niet is gestabiliseerd of deze niet wordt weergegeven in de MaxClusterStabilizationTimeout doorgegeven in orde), Chaos genereert een ValidationFailedEvent. Deze gebeurtenis geeft aan dat er iets een opgetreden fout is en verder onderzoek moet mogelijk.

Als u welke fouten Chaos wordt veroorzaakt, kunt u GetChaosReport API (powershell, C# of REST). De API Hiermee haalt u het volgende segment van het Chaos rapport op basis van de doorgegeven vervolgtoken of doorgegeven in een tijd-bereik. Kunt u de ContinuationToken als u het volgende segment van het rapport Chaos opgeven of u kunt het tijdsbereik via StartTimeUtc en EndTimeUtc opgeven, maar u niet zowel de ContinuationToken als het tijdsbereik opgeven in dezelfde aanroep. Wanneer er meer dan 100 Chaos gebeurtenissen, wordt het rapport Chaos geretourneerd in segmenten waarbij een segment niet meer dan 100 Chaos gebeurtenissen bevat.

## <a name="important-configuration-options"></a>Belangrijke configuratie-opties
* **TimeToRun**: totale tijd die Chaos wordt uitgevoerd voordat het met succes is voltooid. U kunt Chaos stoppen voordat deze is uitgevoerd voor de periode TimeToRun via de API StopChaos.

* **MaxClusterStabilizationTimeout**: de maximale hoeveelheid wachttijd voor het cluster wordt omgezet in orde voordat het opstellen van een ValidationFailedEvent. Deze wachttijd is het verminderen van de belasting op het cluster, terwijl deze wordt hersteld. De controles uitgevoerd worden:
  * Als de status van het cluster OK is
  * Als de servicestatus van de OK is
  * Als de doelreplica grootte ingesteld wordt voor de partitie van de service bereikt
  * Dat er geen InBuild-replica's bestaan
* **MaxConcurrentFaults**: het maximum aantal gelijktijdige fouten die in elke iteratie veroorzaakt. De hoger het getal, hoe meer agressieve Chaos is en de failovers en de status overgang combinaties die het cluster doorloopt zijn ook complexere. 

> [!NOTE]
> Ongeacht hoe hoge waarde *MaxConcurrentFaults* heeft, Chaos garandeert - bij gebrek aan externe fouten - er is geen quorumverlies of verlies van gegevens.
>

* **EnableMoveReplicaFaults**: Hiermee schakelt u de fouten die ertoe leiden de primaire of secundaire replica's dat te verplaatsen of uit. Deze fouten worden standaard uitgeschakeld.
* **WaitTimeBetweenIterations**: de hoeveelheid tijd moet worden gewacht tussen pogingen. Dat wil zeggen, wordt de hoeveelheid tijd Chaos onderbroken na het uitvoeren van een ronde van fouten en de bijbehorende validatie van de status van het cluster hebben voltooid. Hoe hoger de waarde des te lager is de frequentie van de gemiddelde paginafouten injectie.
* **WaitTimeBetweenFaults**: de hoeveelheid tijd moet worden gewacht tussen twee opeenvolgende fouten in een enkel iteratie. Hoe hoger de waarde, hoe lager de gelijktijdigheid van (of de overlapping tussen) bedrijfsstoringen.
* **ClusterHealthPolicy**: Cluster statusbeleid wordt gebruikt voor het valideren van de status van het cluster Between Chaos iteraties. Als de status van het cluster onjuist is of als een onverwachte uitzondering tijdens het uitvoeren van de fout gebeurt, wacht u totdat 30 minuten voordat de volgende-statuscontrole - om te voorzien van het cluster enige tijd recuperate Chaos.
* **Context**: een verzameling van (string, string) typt u sleutel-waardeparen. De kaart kan worden gebruikt voor het vastleggen van informatie over het Chaos-run. Er mag niet meer dan 100 dergelijke paren en elke tekenreeks (sleutel of waarde) mag hoogstens 4095 tekens bevatten. Deze kaart is ingesteld door de starter de dank uitvoeren voor het opslaan van de context over de specifieke uitvoeren (optioneel).
* **ChaosTargetFilter**: dit filter kan worden gebruikt voor doel Chaos fouten alleen bepaalde knooppunttypen of alleen bepaalde exemplaren van een toepassing. Als ChaosTargetFilter niet gebruikt wordt, bedrijfsstoringen Chaos alle entiteiten van het cluster. Als ChaosTargetFilter wordt gebruikt, bedrijfsstoringen Chaos alleen de entiteiten die voldoen aan de ChaosTargetFilter-specificatie. Toestaan dat alleen union semantiek NodeTypeInclusionList en ApplicationInclusionList. Met andere woorden, is het niet mogelijk om op te geven van een snijpunt van NodeTypeInclusionList en ApplicationInclusionList. Het is bijvoorbeeld niet mogelijk om op te geven "fault deze toepassing alleen wanneer deze zich op dit knooppunttype." Zodra een entiteit is opgenomen in NodeTypeInclusionList of ApplicationInclusionList, kan niet die entiteit worden uitgesloten ChaosTargetFilter gebruiken. Zelfs als applicationX niet wordt weergegeven in ApplicationInclusionList, in sommige iteratie Chaos kunt applicationX mislukt omdat dit gebeurt op een knooppunt van nodeTypeY die is opgenomen in NodeTypeInclusionList. Als zowel NodeTypeInclusionList als ApplicationInclusionList null of leeg zijn, wordt een ArgumentException gegenereerd.
    * **NodeTypeInclusionList**: een lijst met knooppunttypen in Chaos fouten op te nemen. Alle soorten fouten (knooppunt opnieuw opstarten, codepackage opnieuw replica verwijderen, opnieuw opstarten replica, verplaatsen primaire en secundaire verplaatsen) zijn ingeschakeld voor de knooppunten van de volgende knooppunttypen. Als een nodetype (spreken NodeTypeX) niet wordt weergegeven in de NodeTypeInclusionList en vervolgens niveau fouten knooppunt (zoals NodeRestart) voor de knooppunten van NodeTypeX nooit worden ingeschakeld, maar het pakket en replica fouten code kunnen nog steeds worden ingeschakeld voor NodeTypeX als een toepassing in de ApplicationInclusionList gebeurt zich bevinden op een knooppunt van NodeTypeX. Maximaal 100 knooppunt Typenamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van de configuratie is vereist voor MaxNumberOfNodeTypesInChaosTargetFilter configuratie.
    * **ApplicationInclusionList**: een lijst van toepassing URI's in Chaos fouten op te nemen. Alle replica's die horen bij de services van deze toepassingen zijn voorwerp replica fouten (opnieuw opstarten replica, replica verwijderen, verplaatsen primaire en secundaire verplaatsen) door Chaos. Chaos mogelijk opnieuw opstarten van een codepakket alleen als het codepakket fungeert als host van de replica's van deze toepassingen alleen. Als een toepassing niet in deze lijst wordt weergegeven, kan deze nog steeds mislukt in sommige iteratie Chaos als de toepassing zich op een knooppunt van een knooppunttype dat incuded in NodeTypeInclusionList belandt. Echter als applicationX is gekoppeld aan nodeTypeY via plaatsingsbeperkingen en applicationX afwezig is van ApplicationInclusionList en nodeTypeY is afwezig van NodeTypeInclusionList, klikt u vervolgens applicationX nooit mislukt. Maximaal 1000 toepassingsnamen kunnen worden opgenomen in deze lijst, dit aantal vergroten, een upgrade van de configuratie is vereist voor MaxNumberOfApplicationsInChaosTargetFilter configuratie.

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
GIT 