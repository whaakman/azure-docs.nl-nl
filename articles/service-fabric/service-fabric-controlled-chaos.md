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
ms.date: 08/09/2017
ms.author: motanv
ms.openlocfilehash: 3b3b93bc9ec5ecdcfc289e5b62e84de6aa4172ed
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Veroorzaken gecontroleerde Chaos in Service Fabric-clusters
Grote gedistribueerde systemen zoals cloudinfrastructuren inherent onbetrouwbaar worden. Azure Service Fabric kunnen ontwikkelaars schrijven betrouwbare gedistribueerde services op een onbetrouwbaar-infrastructuur. Ontwikkelaars moeten kunnen testen van de stabiliteit van hun services terwijl de onderliggende infrastructuur onbetrouwbaar wordt verzonden via ingewikkeld statusovergangen als gevolg van fouten voor het schrijven van robuuste gedistribueerde services op een onbetrouwbaar-infrastructuur.

De [fout injectie en analyse van de clusterservice](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (ook wel bekend als de fout Analysis Service) biedt ontwikkelaars de mogelijkheid om te veroorzaken fouten voor het testen van hun services. Deze gericht fouten, zoals gesimuleerde [opnieuw starten van een partitie](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), kunt u de meest voorkomende statusovergangen uitoefenen. Echter fouten gerichte gesimuleerde fouten per definitie zijn gericht en dus mist mogelijk die weergeven van alleen in moeilijk te voorspellen, lang en gecompliceerd reeks statusovergangen. Voor een zuivere testen, kunt u Chaos.

Chaos simuleert periodieke, interleaved fouten (correcte en geforceerde afsluiting) in het cluster gedurende langere perioden. Wanneer u Chaos met de frequentie en het soort fouten hebt geconfigureerd, kunt u beginnen Chaos via C# of Powershell API om te beginnen met het genereren van fouten in het cluster en in uw services. U kunt Chaos om uit te voeren voor een opgegeven periode (bijvoorbeeld: voor één uur) automatisch na waarbij stopt Chaos configureren of u StopChaos API (C# of Powershell) om deze te stoppen op elk gewenst moment kunt aanroepen.

> [!NOTE]
> In de huidige vorm induceert Chaos alleen veilige fouten die impliceert dat bij gebrek aan externe fouten een quorumverlies of verlies van gegevens nooit plaatsvindt.
>

Terwijl Chaos wordt uitgevoerd, is het resultaat van andere gebeurtenissen die de status van de uitvoeren op het moment dat vastleggen. Een ExecutingFaultsEvent bevat bijvoorbeeld de fouten die Chaos heeft besloten uit te voeren in die iteratie. Een ValidationFailedEvent bevat de details van een mislukte validatie (health of stabiliteit problemen) die tijdens de validatie van het cluster is gevonden. U kunt de API GetChaosReport (C# of Powershell) als u het rapport over de uitgevoerde Chaos aanroepen. Deze gebeurtenissen ophalen permanent in een [betrouwbare woordenlijst](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), heeft een beleid moet worden afgekapt is bepaald door twee configuraties: **MaxStoredChaosEventCount** (de standaardwaarde is 25000) en  **StoredActionCleanupIntervalInSeconds** (de standaardwaarde is 3600). Elke *StoredActionCleanupIntervalInSeconds* Chaos controles en alle maar het meest recente *MaxStoredChaosEventCount* gebeurtenissen, permanent worden verwijderd van het betrouwbare woordenboek.

## <a name="faults-induced-in-chaos"></a>Fouten worden bewerkstelligd in Chaos
Chaos genereert fouten over de hele Service Fabric-cluster en comprimeert fouten die zijn zichtbaar in maanden of jaren in een paar uur. De combinatie van interleaved fouten met de snelheid van hoge fouttolerantie vindt hoek aanvragen die mogelijk anders worden overgeslagen. In deze oefening dank leidt tot een aanzienlijke verbetering van de kwaliteit van de code van de service.

Chaos induceert fouten uit de volgende categorieën:

* Een knooppunt opnieuw opstarten
* Een geïmplementeerde codepakket niet opnieuw starten
* Een replica verwijderen
* Een replica starten
* Verplaatsen van een primaire replica (configureren)
* Verplaatsen van een secundaire replica (configureren)

Chaos in meerdere pogingen uitgevoerd. Elke herhaling bestaat uit de fouten en clustervalidatie voor de opgegeven periode. De tijd voor het cluster stabiel en voor de validatie mislukt, kunt u configureren. Als een fout in validatie wordt gevonden, wordt Chaos genereert en een ValidationFailedEvent met de UTC-timestamp en de foutdetails zich blijft voordoen. Neem bijvoorbeeld een exemplaar dank dat is ingesteld op een uur worden uitgevoerd met een maximum van drie gelijktijdige fouten. Chaos induceert drie fouten en vervolgens valideert de status van het cluster. Doorlopen de vorige stap totdat deze expliciet gestopt via de API StopChaosAsync of één uur wordt doorgegeven. Als het cluster in elke iteratie slecht (dat wil zeggen, het heeft geen regulering binnen de MaxClusterStabilizationTimeout doorgegeven), Chaos genereert een ValidationFailedEvent. Deze gebeurtenis geeft aan dat er iets een opgetreden fout is en verder onderzoek moet mogelijk.

Als u welke fouten Chaos wordt veroorzaakt, kunt u GetChaosReport API (powershell of C#). De API Hiermee haalt u het volgende segment van het Chaos rapport op basis van de doorgegeven vervolgtoken of doorgegeven in een tijd-bereik. Kunt u de ContinuationToken als u het volgende segment van het rapport Chaos opgeven of u kunt het tijdsbereik via StartTimeUtc en EndTimeUtc opgeven, maar u niet zowel de ContinuationToken als het tijdsbereik opgeven in dezelfde aanroep. Wanneer er meer dan 100 Chaos gebeurtenissen, wordt het rapport Chaos geretourneerd in segmenten waarbij een segment niet meer dan 100 Chaos gebeurtenissen bevat.

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
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

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

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

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
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

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
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
