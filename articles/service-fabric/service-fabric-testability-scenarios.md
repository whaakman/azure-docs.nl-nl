---
title: Chaos- en failovertests maken voor Azure Service Fabric | Microsoft Docs
description: Met behulp van de Service Fabric chaos-test- en failover-en Testscenario's veroorzaken fouten en te controleren of de betrouwbaarheid van uw services.
services: service-fabric
documentationcenter: .net
author: motanv
manager: rsinha
editor: toddabel
ms.assetid: 8eee7e89-404a-4605-8f00-7e4d4fb17553
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: d12c5097d4ba5e0ccfe0e2b2cbc8ccd758c32d98
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051286"
---
# <a name="testability-scenarios"></a>Testbaarheidsscenario 's
Grote gedistribueerde systemen cloudinfrastructuren zijn inherent onbetrouwbaar. Azure Service Fabric biedt ontwikkelaars de mogelijkheid om services om uit te voeren op onbetrouwbare infrastructuur te ontwikkelen. Ontwikkelaars moeten om te kunnen schrijven hoogwaardige services, om een dergelijke onbetrouwbare infrastructuur voor het testen van de stabiliteit van de services te kunnen.

De Fault Analysis Service biedt ontwikkelaars de mogelijkheid om fouttolerantie bewerkingen voor het testen van services in geval van problemen veroorzaken. Echter, gerichte gesimuleerde fouten krijgt u alleen tot nu toe. Als u de controle verder, kunt u de Testscenario's gebruiken in Service Fabric: een chaos-test en een failovertest-. Deze scenario's simuleren continue interleaved fouten, verwijdering en geforceerde afsluiting in het cluster gedurende langere tijd. Wanneer een test is geconfigureerd met de snelheid en het soort fouten, kan het kan worden gestart via C#-API's of PowerShell, voor het genereren van fouten in het cluster en wordt uw service.

> [!WARNING]
> ChaosTestScenario wordt vervangen door een Chaos meer flexibele, op basis van een service. Raadpleeg het nieuwe artikel [beheerd Chaos](service-fabric-controlled-chaos.md) voor meer informatie.
> 
> 

## <a name="chaos-test"></a>Chaos-test
Het scenario chaos genereert fouten in de hele Service Fabric-cluster. Het scenario comprimeert fouten in het algemeen gezien in maanden of jaren tot enkele uren. De combinatie van interleaved fouten met de snelheid van hoge fouttolerantie vindt hoek gevallen die anders zijn gemist. Dit leidt tot een aanzienlijke verbetering van de kwaliteit van de code van de service.

### <a name="faults-simulated-in-the-chaos-test"></a>Fouten in de simulatie in de chaos-test
* Een knooppunt opnieuw starten
* Opnieuw opstarten van een geïmplementeerde codepakket
* Een replica verwijderen
* Opnieuw opstarten van een replica
* Verplaatsen van een primaire replica (optioneel)
* Verplaatsen van een secundaire replica (optioneel)

De chaos-test meerdere pogingen van fouten en cluster validaties worden uitgevoerd voor de opgegeven periode. De tijd voor het cluster te laten stabiliseren en voor de validatie te voltooien, kan ook worden geconfigureerd. Het scenario mislukt wanneer u een storing in de clustervalidatie bereikt.

Bijvoorbeeld, kunt u overwegen een test ingesteld op gedurende één uur worden uitgevoerd met een maximum van drie gelijktijdige fouten. De test worden drie fouten veroorzaken en controleer vervolgens of de clusterstatus. De test wordt door de vorige stap herhalen tot het cluster slecht wordt of één uur wordt doorgegeven. Als het cluster wordt niet in orde in elke iteratie, dat wil zeggen het heeft niet stabiel binnen een geconfigureerde periode, de test mislukt met een uitzondering. Deze uitzondering geeft aan dat er iets is iets verkeerd gegaan en verder onderzoek nodig.

In de huidige vorm induceert de engine voor het genereren van fouttolerantie in de chaos-test alleen veilige fouten. Dit betekent dat in de afwezigheid van externe fouten kunnen een quorum of gegevensverlies wordt nooit uitgevoerd.

### <a name="important-configuration-options"></a>Belangrijke configuratie-opties
* **TimeToRun**: totale tijd dat de test wordt uitgevoerd voordat u met succes is voltooid. De test kan eerder voltooid in plaats van een mislukte validatie.
* **MaxClusterStabilizationTimeout**: maximumhoeveelheid tijd moet worden gewacht op het cluster op in orde worden voordat de test mislukt. Controles zijn of de clusterstatus van de OK is, de servicestatus van de is OK, de grootte van de doel-replica is bereikt voor de servicepartitie en geen InBuild-replica's bestaan.
* **MaxConcurrentFaults**: maximumaantal gelijktijdige fouten die in elke iteratie. Hoe hoger het getal, de agressiever de test, dus dit resulteert in meer complexe failovers en overgang combinaties. De test wordt gegarandeerd dat op de afwezigheid van externe fouten niet een quorum of gegevensverlies worden, ongeacht hoe hoog deze configuratie is.
* **EnableMoveReplicaFaults**: Hiermee schakelt u de fouten die worden veroorzaakt door het verplaatsen van de primaire of secundaire replica's of uit. Deze fouten zijn standaard uitgeschakeld.
* **WaitTimeBetweenIterations**: hoeveelheid tijd die moet worden gewacht tussen pogingen, dat wil zeggen na een ronde van fouten en de bijbehorende validatie.

### <a name="how-to-run-the-chaos-test"></a>Het uitvoeren van de chaos-test
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
Het scenario voor het testen van failover is een versie van het scenario voor het testen van chaos die gericht is op een specifieke service-partitie. Deze test het effect van failover op de partitie van een specifieke service terwijl de andere services niet beïnvloed. Nadat deze geconfigureerd met de doel-partitie-informatie en andere parameters, wordt deze uitgevoerd als een client-side-hulpprogramma dat gebruikmaakt van C#-API's of PowerShell voor het genereren van fouten voor de servicepartitie van een. Het scenario doorloopt een reeks gesimuleerde fouten en servicevalidatie, terwijl uw zakelijke logica wordt uitgevoerd aan de zijde voor een werkbelasting. Een fout in de servicevalidatie geeft een probleem dat moet worden verder onderzoek.

### <a name="faults-simulated-in-the-failover-test"></a>Fouten in de failovertest gesimuleerde
* Een geïmplementeerde codepakket waar de partitie wordt gehost opnieuw opstarten
* Een primaire/secundaire replica of een stateless exemplaar verwijderen
* Een primaire, secundaire replica opnieuw opstarten (indien een persistente service)
* Verplaatsen van een primaire replica
* Verplaatsen van een secundaire replica
* De partitie opnieuw starten

De failovertest induceert een fout is gekozen en voert vervolgens de validatie van de service om te controleren of de stabiliteit. De failovertest induceert maar één fout op een tijdstip, in plaats van mogelijk meerdere fouten in de chaos-test. Als de servicepartitie niet binnen de geconfigureerde time-out na elke fout stabiliseren heeft, wordt de test mislukt. De test induceert alleen veilige fouten. Dit betekent dat in de afwezigheid van externe storingen, een quorum of gegevensverlies wordt niet uitgevoerd.

### <a name="important-configuration-options"></a>Belangrijke configuratie-opties
* **PartitionSelector**: Selector-object dat Hiermee geeft u de partitie die moet worden toegepast.
* **TimeToRun**: totale tijd dat de test wordt uitgevoerd voordat u klaar bent.
* **MaxServiceStabilizationTimeout**: maximumhoeveelheid tijd moet worden gewacht op het cluster op in orde worden voordat de test mislukt. Controles zijn of de servicestatus van de OK is, de grootte van de doel-replica is bereikt voor alle partities en geen InBuild-replica's bestaan.
* **WaitTimeBetweenFaults**: hoeveelheid tijd die moet worden gewacht tussen elke cyclus domeinen met fouten en validatie.

### <a name="how-to-run-the-failover-test"></a>Het uitvoeren van de failovertest
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
