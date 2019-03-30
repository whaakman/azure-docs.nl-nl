---
title: Fouten in Azure-microservices simuleren | Microsoft Docs
description: In dit artikel vertelt de testbaarheidsacties gevonden in de Microsoft Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: motanv
manager: chackdan
editor: heeldin
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: 37a794387f3a2f02124805705d380ad9f1fc1270
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662853"
---
# <a name="testability-actions"></a>Testbaarheidsacties
Om te simuleren een onbetrouwbare infrastructuur, biedt Azure Service Fabric u, de ontwikkelaar is, manieren om verschillende echte storingen en statusovergangen te simuleren. Deze worden weergegeven als testbaarheidsacties. De acties zijn de laag niveau API's die ertoe leiden een specifieke foutinjectie, statusovergang of validatie dat. U kunt uitgebreide Testscenario's voor uw services schrijven door een combinatie van deze acties.

Service Fabric biedt dat enkele algemene test-scenario's bestaan uit deze acties. Het is raadzaam dat u gebruikmaken van deze ingebouwde scenario's die zorgvuldig gekozen zijn voor het testen van algemene statusovergangen en mislukte aanvragen. Acties kunnen echter worden gebruikt om te maken van aangepaste Testscenario's wanneer u wilt toevoegen, dekking voor scenario's die zijn niet wordt gedekt door de ingebouwde scenario's nog of die zijn aangepast die is ontworpen voor uw toepassing.

C#implementaties van de acties die zijn gevonden in de assembly System.Fabric.dll. Het systeem Fabric PowerShell-module is gevonden in de assembly Microsoft.ServiceFabric.Powershell.dll. Als onderdeel van de runtime-installatie, wordt de ServiceFabric-PowerShell-module geïnstalleerd om toe te staan voor gebruiksgemak.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Correcte versus geforceerde afsluiting fault-acties
Testbaarheidsacties worden onderverdeeld in twee belangrijke buckets:

* Geforceerde afsluiting fouten: Deze fouten simuleren fouten, zoals de machine opnieuw is opgestart en crashes verwerken. In dergelijke gevallen van fouten, uitvoeringscontext van proces abrupt. Dit betekent dat er geen opschoning van de status kan worden uitgevoerd voordat de toepassing opnieuw wordt opgestart.
* Correcte fouten: Deze fouten simuleren vensters acties, zoals replica verplaatst en val geactiveerd door de taakverdeling. In dergelijke gevallen kan de service wordt een melding van het sluiten en de status kunt opschonen voordat u afsluit.

Voor betere kwaliteit validatie, moet u de workload van de service en zakelijke uitgevoerd tijdens verschillende vensters en geforceerde afsluiting fouten veroorzaken. Geforceerde afsluiting fouten oefenen scenario's waar het serviceproces onverwacht wordt afgesloten in het midden van een werkstroom. Dit wordt het herstelpad gecontroleerd zodra de service-replica is hersteld door Service Fabric. Hiermee kunt u testen van de consistentie van gegevens en of de status van de service op een correct na fouten wordt bijgehouden. De andere set met fouten (de correcte fouten)-test of de service goed reageert op replica's door Service Fabric wordt verplaatst. Deze test verwerking van de annulering in de methode RunAsync. De service nodig heeft om te controleren voor de annulering token wordt ingesteld, wordt de status goed opslaan en sluiten van de methode RunAsync.

## <a name="testability-actions-list"></a>Lijst met acties van testbaarheid
| Bewerking | Description | Beheerde API | PowerShell-cmdlet | Correcte/geforceerde afsluiting fouten |
| --- | --- | --- | --- | --- |
| CleanTestState |Hiermee verwijdert u alle statussen van de test uit het cluster in het geval van een onjuist afsluiten van de test-stuurprogramma. |CleanTestStateAsync |Remove-ServiceFabricTestState |Niet van toepassing |
| InvokeDataLoss |Induceert verlies van gegevens in een servicepartitie. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Verwijdering |
| InvokeQuorumLoss |De partitie van een bepaalde stateful service in quorumverlies wordt geplaatst. |InvokeQuorumLossAsync |Invoke-ServiceFabricQuorumLoss |Verwijdering |
| MovePrimary |De opgegeven primaire replica van een stateful service verplaatst naar het gespecificeerde clusterknooppunt. |MovePrimaryAsync |Move-ServiceFabricPrimaryReplica |Verwijdering |
| MoveSecondary |De huidige secundaire replica van een stateful service verplaatst naar een ander clusterknooppunt. |MoveSecondaryAsync |Move-ServiceFabricSecondaryReplica |Verwijdering |
| RemoveReplica |Een replica-fout simuleert door het verwijderen van een replica uit een cluster. Deze de replica wordt gesloten en verandert deze in rol 'None', verwijderen van alle van de status van het cluster. |RemoveReplicaAsync |Remove-ServiceFabricReplica |Verwijdering |
| RestartDeployedCodePackage |Simuleert een code-pakket mislukt door een codepakket geïmplementeerd op een knooppunt in een cluster opnieuw te starten. Dit Hiermee het proces voor het pakket van code, die opnieuw alle gebruiker service replica's die worden gehost in het proces opgestart wordt wordt afgebroken. |RestartDeployedCodePackageAsync |Restart-ServiceFabricDeployedCodePackage |Geforceerde afsluiting |
| RestartNode |Simuleert een knooppuntfout voor Service Fabric-cluster door een knooppunt opnieuw te starten. |RestartNodeAsync |Restart-ServiceFabricNode |Geforceerde afsluiting |
| RestartPartition |Simuleert een datacenter onleesbaar of het cluster onleesbaar scenario door sommige of alle replica's van een partitie opnieuw te starten. |RestartPartitionAsync |Restart-ServiceFabricPartition |Verwijdering |
| RestartReplica |Simuleert een replica mislukt door een persistente replica opnieuw te starten in een cluster, de replica wordt gesloten en opent u deze opnieuw. |RestartReplicaAsync |Restart-ServiceFabricReplica |Verwijdering |
| StartNode |Start een knooppunt in een cluster dat is al gestopt. |StartNodeAsync |Start-ServiceFabricNode |Niet van toepassing |
| StopNode |Simuleert een knooppuntfout door het stoppen van een knooppunt in een cluster. Het knooppunt wordt naar beneden blijven totdat beginknooppunt wordt genoemd. |StopNodeAsync |Stop-ServiceFabricNode |Geforceerde afsluiting |
| ValidateApplication |Valideert de beschikbaarheid en de status van alle Service Fabric-services binnen een toepassing, meestal na enkele fouten in het systeem veroorzaken. |ValidateApplicationAsync |Test-ServiceFabricApplication |Niet van toepassing |
| ValidateService |Valideert de beschikbaarheid en de status van een Service Fabric-service, meestal na enkele fouten in het systeem veroorzaken. |ValidateServiceAsync |Test-ServiceFabricService |Niet van toepassing |

## <a name="running-a-testability-action-using-powershell"></a>Uitvoeren van een testbaarheidsactie met behulp van PowerShell
Deze zelfstudie leert u hoe u een testbaarheidsactie uitvoert met behulp van PowerShell. U leert hoe u een testbaarheidsactie uitvoeren op een lokale (1-box)-cluster of een Azure-cluster. Microsoft.Fabric.Powershell.dll--de Service Fabric PowerShell-module wordt automatisch geïnstalleerd tijdens de installatie van het Microsoft-Service Fabric-MSI-bestand. De module wordt automatisch geladen bij het openen van een PowerShell-prompt.

Zelfstudie segmenten:

* [Een actie uitvoeren in een cluster in-één](#run-an-action-against-a-one-box-cluster)
* [Een actie uitvoeren in een Azure-cluster](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Een actie uitvoeren in een cluster in-één
Als u wilt een testbaarheidsactie uitvoeren in een lokaal cluster, eerst verbinding met het cluster en open de PowerShell-prompt in de beheerdersmodus. We bekijken de **opnieuw opstarten-ServiceFabricNode** actie.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Hier de actie **opnieuw opstarten-ServiceFabricNode** wordt uitgevoerd op een knooppunt met de naam 'Knooppunt1'. De voltooiing-modus geeft aan dat deze niet controleren moet of de actie van het knooppunt opnieuw starten daadwerkelijk verwijderd. De voltooiing-modus opgeven als 'Verifiëren' gaan om te controleren of de actie opnieuw opstarten daadwerkelijk verwijderd. In plaats van rechtstreeks op te geven het knooppunt door de naam, kunt u deze via een partitiesleutel en het soort replica, als volgt:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Opnieuw opstarten-ServiceFabricNode** moet worden gebruikt om een Service Fabric-knooppunt in een cluster opnieuw opstarten. Hiermee stopt u de Fabric.exe-proces, dat alle van de service en gebruiker service systeemreplica's die worden gehost op dat knooppunt opnieuw wordt gestart. Met behulp van deze API om uw service te testen, kunt verwerven op basis bugs langs de failover-recovery-paden. Het helpt simuleren knooppuntfouten in het cluster.

De volgende schermafbeelding ziet u de **opnieuw opstarten-ServiceFabricNode** testbaarheid opdracht in actie.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

De uitvoer van de eerste **Get-ServiceFabricNode** (een cmdlet uit de Service Fabric PowerShell-module) wordt aangegeven dat het lokale cluster vijf knooppunten bevat: Node.1 naar Node.5. Na de testbaarheidsactie (cmdlet) **opnieuw opstarten-ServiceFabricNode** wordt uitgevoerd op het knooppunt met de naam Node.4, zien we dat de uptime van het knooppunt is opnieuw ingesteld.

### <a name="run-an-action-against-an-azure-cluster"></a>Een actie uitvoeren in een Azure-cluster
Met het uitvoeren van een testbaarheidsactie (met behulp van PowerShell) in een Azure-cluster is vergelijkbaar met het uitvoeren van de actie op basis van een lokaal cluster. Het enige verschil is voordat u de actie, in plaats van de verbinding te maken met het lokale cluster kunt uitvoeren moet u eerst verbinding met het Azure-cluster.

## <a name="running-a-testability-action-using-c35"></a>Uitvoeren van een testbaarheidsactie met c#&#35;
Een testbaarheidsactie uitvoeren met behulp van C#, moet u eerst verbinding met het cluster met behulp van FabricClient. Download daarna de parameters die nodig zijn voor de actie uitvoeren. Andere parameters kunnen worden gebruikt om uit te voeren dezelfde actie.
De actie RestartServiceFabricNode bekijkt, is één manier uit te voeren met behulp van de knooppuntgegevens (naam van het knooppunt en exemplaar-ID van knooppunt) in het cluster.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Uitleg van de parameter:

* **CompleteMode** geeft aan dat de modus niet controleren moet of de actie opnieuw opstarten daadwerkelijk verwijderd. De voltooiing-modus opgeven als 'Verifiëren' gaan om te controleren of de actie opnieuw opstarten daadwerkelijk verwijderd.  
* **OperationTimeout** stelt u de hoeveelheid tijd voor de bewerking is voltooid voordat een TimeoutException-uitzondering is opgetreden.
* **CancellationToken** kunt u een aanroep van in behandeling zijn geannuleerd.

In plaats van rechtstreeks op te geven het knooppunt door de naam, kunt u deze opgeven via een partitiesleutel en het type van de replica.

Zie voor meer informatie, PartitionSelector en ReplicaSelector.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector en ReplicaSelector
### <a name="partitionselector"></a>PartitionSelector
PartitionSelector is een helper die beschikbaar zijn in testbaarheid en wordt gebruikt voor het selecteren van een specifieke partitie waarop een van de testbaarheidsacties uitvoeren. Het kan worden gebruikt voor het selecteren van een specifieke partitie als de partitie-ID vooraf bekend is. Of u kunt de partitiesleutel opgeven en de bewerking wordt de partitie-ID intern opgelost. U hebt ook de mogelijkheid van het selecteren van een willekeurige partitie.

Maken van het object PartitionSelector en selecteren van de partitie met behulp van een van de Select *-methoden voor het gebruik van het hulpprogramma. Vervolgens geeft u het object PartitionSelector met de API die vereist is. Als er geen optie is geselecteerd, wordt standaard een willekeurige partitie.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector is een helper in testbaarheid weergegeven en wordt gebruikt om u te helpen bij het selecteren van een replica waarop een van de testbaarheidsacties uitvoeren. Het kan worden gebruikt voor het selecteren van een specifieke replica als de replica-ID vooraf bekend is. Bovendien hebt u de mogelijkheid van het selecteren van een primaire replica of een willekeurige secundaire site. ReplicaSelector is afgeleid van PartitionSelector, dus u moet zowel de replica en de partitie waarop u wilt uitvoeren van de bewerking testbaarheid selecteren.

Voor het gebruik van het hulpprogramma een ReplicaSelector-object maken en instellen van de manier waarop u wilt dat de replica en de partitie te selecteren. U kunt deze vervolgens doorgeven aan de API die vereist is. Als er geen optie is geselecteerd, wordt standaard een willekeurige replica en een willekeurige partitie.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Volgende stappen
* [Testbaarheidsscenario 's](service-fabric-testability-scenarios.md)
* Uw service testen
  * [Storingen simuleren tijdens servicewerkbelastingen](service-fabric-testability-workload-tests.md)
  * [Service-naar-servicecommunicatie fouten](service-fabric-testability-scenarios-service-communication.md)

