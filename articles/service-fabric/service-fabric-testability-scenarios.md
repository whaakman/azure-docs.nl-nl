---
title: Maken van chaos en failover testen voor Azure microservices | Microsoft Docs
description: Met behulp van de Service Fabric testen chaos test en failover-scenario's te veroorzaken fouten en controleer of de betrouwbaarheid van uw services.
services: service-fabric
documentationcenter: .net
author: motanv
manager: rsinha
editor: toddabel
ms.assetid: 8eee7e89-404a-4605-8f00-7e4d4fb17553
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: d06026c750e01ad5825338a78d9af331265f434a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="testability-scenarios"></a>Testbaarheid scenario 's
Grote gedistribueerde systemen zoals cloudinfrastructuren inherent onbetrouwbaar worden. Azure Service Fabric biedt ontwikkelaars de mogelijkheid om services voor uitvoering op onbetrouwbare infrastructuren. Ontwikkelaars moeten kunnen veroorzaken dergelijke onbetrouwbaar infrastructuur voor het testen van de stabiliteit van hun services om te schrijven van hoge kwaliteit services.

De fout Analysis Services biedt ontwikkelaars de mogelijkheid om te veroorzaken acties voor het testen van services in geval van problemen veroorzaakt. Echter krijgt gerichte gesimuleerde fouten u alleen tot nu toe. Als u wilt de tests meer, kunt u de test-scenario's in Service Fabric: een test chaos en een failover uit. Deze scenario's simuleren continue interleaved fouten correcte en geforceerde afsluiting in het cluster gedurende langere perioden. Wanneer een test is geconfigureerd met de frequentie en het soort fouten, kan het kan worden gestart via C#-API's of PowerShell, voor het genereren van fouten in het cluster en de service.

> [!WARNING]
> ChaosTestScenario wordt vervangen door een Chaos toleranter, op basis van een service. Raadpleeg het nieuwe artikel [Chaos beheerd](service-fabric-controlled-chaos.md) voor meer informatie.
> 
> 

## <a name="chaos-test"></a>Chaos test
Het scenario chaos genereert fouten over de hele Service Fabric-cluster. Het scenario comprimeren fouten in het algemeen gezien in maanden of jaren tot enkele uren. De combinatie van interleaved fouten met de snelheid van hoge fouttolerantie vindt complexere cases die anders zijn gemist. Dit leidt tot een aanzienlijke verbetering van de kwaliteit van de code van de service.

### <a name="faults-simulated-in-the-chaos-test"></a>Fouten in de test chaos gesimuleerde
* Een knooppunt opnieuw opstarten
* Een geïmplementeerde codepakket niet opnieuw starten
* Een replica verwijderen
* Een replica starten
* Verplaatsen van een primaire replica (optioneel)
* Verplaatsen van een secundaire replica (optioneel)

De chaos-test meerdere herhalingen van de fouten en cluster validaties worden uitgevoerd voor de opgegeven periode. De tijd voor het cluster stabiel en voor de validatie mislukt, kan ook worden geconfigureerd. Het scenario mislukt wanneer u een storing in clustervalidatie bereikt.

Neem bijvoorbeeld een test te worden uitgevoerd voor één uur met een maximum van drie gelijktijdige fouten. De test drie fouten veroorzaken en vervolgens de status van het cluster te valideren. De test wordt de vorige stap doorlopen tot het cluster slecht wordt of één uur wordt doorgegeven. Als het cluster slecht functioneert in elke iteratie, dat wil zeggen het niet binnen een geconfigureerde tijd biedt regulering, mislukt de test met een uitzondering. Deze uitzondering geeft aan dat er iets een opgetreden fout is en verder onderzoek.

De engine voor het genereren van fouten in de test chaos induceert in de huidige vorm alleen veilige fouten. Dit betekent dat bij gebrek aan externe fouten een quorum of verlies van gegevens nooit plaats.

### <a name="important-configuration-options"></a>Belangrijke configuratie-opties
* **TimeToRun**: totale tijd die de test uitgevoerd voordat het met succes is voltooid. De test kan eerder in plaats van een mislukte validatie worden voltooid.
* **MaxClusterStabilizationTimeout**: maximumhoeveelheid wachttijd voor het cluster wordt omgezet in orde voordat het mislukken van de test. Controles zijn of de status van de cluster OK is, de servicestatus van de is OK en de doelgrootte van replicaset is bereikt voor de partitie van de service geen InBuild-replica's bestaan.
* **MaxConcurrentFaults**: maximumaantal gelijktijdige fouten die in elke iteratie. Hoe hoger het getal, de agressievere de test, waardoor daarom complexere failovers en overgang combinaties. De test wordt gegarandeerd dat afwezigheid van externe fouten er wordt automatisch een quorum of verlies van gegevens, ongeacht hoe hoog deze configuratie is.
* **EnableMoveReplicaFaults**: Hiermee schakelt u de fouten die worden veroorzaakt door de verplaatsing van de primaire of secundaire replica's of uit. Deze fouten worden standaard uitgeschakeld.
* **WaitTimeBetweenIterations**: tijd moet worden gewacht tussen iteraties, dat wil zeggen na een ronde van fouten en bijbehorende validatie.

### <a name="how-to-run-the-chaos-test"></a>Het uitvoeren van de test chaos
C#-voorbeeld

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

PowerShell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a>Failover testen
Het scenario voor het testen van failover is een versie van het scenario voor het testen van chaos die gericht is op een specifieke service-partitie. Deze test het effect van failover op een specifieke service partitie terwijl de andere services niet is beïnvloed. Zodra deze geconfigureerd met de doel-partitiegegevens en andere parameters, deze wordt uitgevoerd als een client-side '-hulpprogramma dat gebruikmaakt van C#-API's of PowerShell voor het genereren van fouten voor de partitie van een service. Het scenario doorlopen een reeks gesimuleerde fouten en servicevalidatie tijdens het uitvoeren van uw bedrijfslogica op de zijde bieden een werkbelasting. Een fout in servicevalidatie geeft een probleem dat verder onderzoek moet.

### <a name="faults-simulated-in-the-failover-test"></a>Fouten in de test failover gesimuleerde
* Een geïmplementeerd codepakket waarop de partitie wordt gehost opnieuw starten
* Een primaire en secundaire replica of stateless exemplaar verwijderen
* Een primaire, secundaire replica opnieuw opstarten (indien een permanente service)
* Verplaatsen van een primaire replica
* Verplaatsen van een secundaire replica
* De partitie opnieuw starten

De failover-test induceert een gekozen veroorzaakt en validatie wordt uitgevoerd op de service om te controleren of de stabiliteit. De failover-test induceert slechts één fout tegelijk in plaats van mogelijk meerdere fouten in de test chaos. Als de service-partitie heeft niet binnen de geconfigureerde time-out regulering na elke fout, mislukt de test. De test induceert alleen veilige fouten. Dit betekent dat geen externe fouten, een quorum of verlies van gegevens wordt niet uitgevoerd.

### <a name="important-configuration-options"></a>Belangrijke configuratie-opties
* **PartitionSelector**: Selector-object waarmee de partitie die moet worden toegepast.
* **TimeToRun**: totale tijd dat de test wordt uitgevoerd voordat u voltooit.
* **MaxServiceStabilizationTimeout**: maximumhoeveelheid wachttijd voor het cluster wordt omgezet in orde voordat het mislukken van de test. De controles uitgevoerd worden of servicestatus OK is, de doelgrootte van replicaset is bereikt voor alle partities en geen InBuild-replica's bestaan.
* **WaitTimeBetweenFaults**: tijd moet worden gewacht tussen elke cyclus veroorzaakt en validatie.

### <a name="how-to-run-the-failover-test"></a>Het uitvoeren van de failover-test
**C#**

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        FailoverTestScenario failoverScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await failoverScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


**PowerShell**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
