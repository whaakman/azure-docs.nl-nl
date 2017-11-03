---
title: Fouten in Azure microservices simuleren | Microsoft Docs
description: In dit artikel wordt gesproken over de acties testbaarheid is gevonden in Microsoft Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: motanv
manager: timlt
editor: toddabel
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv;heeldin
ms.openlocfilehash: c8ddc7732999ae555323bebaef60aa34c8f2ec17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="testability-actions"></a>Testbaarheid acties
Om te simuleren een onbetrouwbare infrastructuur, biedt u de ontwikkelaar, manieren om te simuleren verschillende echte fouten en statusovergangen van Azure Service Fabric. Deze zijn beschikbaar als testbaarheid acties. De acties zijn de op laag niveau API's die ertoe leiden een specifieke fout injectie, statusovergang of validatie dat. U kunt uitgebreide Testscenario's voor uw services schrijven door een combinatie van deze acties.

Service Fabric bevat dat enkele algemene scenario's voor test bestaan uit deze acties. Het is raadzaam dat u gebruikmaken van deze ingebouwde scenario's, zorgvuldig worden gekozen voor het testen van algemene statusovergangen en mislukte aanvragen. Acties kunnen echter worden gebruikt voor het maken van aangepaste Testscenario's wanneer u wilt toevoegen voor scenario's die zijn niet wordt gedekt door de ingebouwde scenario's nog of die aangepaste speciaal afgestemd op uw toepassing.

C#-implementaties van de acties die zijn gevonden in de assembly System.Fabric.dll. De System Fabric PowerShell-module is gevonden in de assembly Microsoft.ServiceFabric.Powershell.dll. Als onderdeel van de runtime-installatie, wordt de ServiceFabric PowerShell-module geïnstalleerd zodat voor eenvoudig te gebruiken.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Correcte versus geforceerde afsluiting veroorzaakt acties
Testbaarheid acties worden ingedeeld in twee belangrijke buckets:

* Geforceerde afsluiting fouten: deze fouten simuleren fouten zoals machine opnieuw is opgestart en crashes verwerken. In dergelijke gevallen van fouten uitvoeringscontext van proces abrupt. Dit betekent dat er geen opschoning van de status kunt uitvoeren voordat de toepassing opnieuw wordt opgestart.
* Correcte fouten: deze fouten simuleren correcte acties zoals replica wordt verplaatst en verwijdert geactiveerd door de taakverdeling. In dergelijke gevallen de service opgehaald van een melding van het sluiten en de status van de op te ruimen voordat u afsluit.

Voor betere quality-validatie uitvoeren, de service en zakelijke werkbelasting terwijl verschillende systemen op correct wijze en geforceerde afsluiting fouten veroorzaken. Geforceerde afsluiting fouten oefenen scenario's waar het serviceproces onverwacht wordt afgesloten in het midden van een werkstroom. Deze test het herstelpad zodra de service-replica is hersteld door de Service Fabric. Dit helpt testen gegevensconsistentie en of de blijft de servicestatus goed na fouten. De andere set van fouten (de correcte fouten) testen dat de service juist reageert op de replica's door de Service Fabric wordt verplaatst. Dit verwerking van de annulering wordt getest in de RunAsync-methode. De service moet controleren voor de annulering-token wordt ingesteld, wordt de status goed opslaan en sluiten van de RunAsync-methode.

## <a name="testability-actions-list"></a>Lijst van acties testbaarheid
| Actie | Beschrijving | Beheerde API | PowerShell-cmdlet | Correcte/geforceerde afsluiting fouten |
| --- | --- | --- | --- | --- |
| CleanTestState |Hiermee verwijdert u alle statussen van de test uit het cluster in geval van een onjuist afsluiten van het stuurprogramma voor de test. |CleanTestStateAsync |Remove-ServiceFabricTestState |Niet van toepassing |
| InvokeDataLoss |Induceert verlies van gegevens in een service-partitie. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Correcte |
| InvokeQuorumLoss |De partitie van een bepaalde stateful service plaatst in quorumverlies. |InvokeQuorumLossAsync |Aanroepen ServiceFabricQuorumLoss |Correcte |
| Primaire verplaatsen |De opgegeven primaire replica van een stateful service verplaatst naar het gespecificeerde clusterknooppunt. |MovePrimaryAsync |Verplaatsing ServiceFabricPrimaryReplica |Correcte |
| Secundaire verplaatsen |De huidige secundaire replica van een stateful service verplaatst naar een ander clusterknooppunt. |MoveSecondaryAsync |Verplaatsing ServiceFabricSecondaryReplica |Correcte |
| RemoveReplica |Een replica-fout simuleert door het verwijderen van een replica van een cluster. Dit de replica wordt gesloten en verandert deze in rol 'None', verwijderen van alle van de status van het cluster. |RemoveReplicaAsync |Verwijder ServiceFabricReplica |Correcte |
| RestartDeployedCodePackage |Simuleert een fout in het pakket code door een codepakket geïmplementeerd op een knooppunt in een cluster opnieuw te starten. Hiermee annuleert het proces voor het pakket van code, die wordt alle gebruiker service replica's die worden gehost in het proces opnieuw opgestart. |RestartDeployedCodePackageAsync |Opnieuw opstarten ServiceFabricDeployedCodePackage |Geforceerde afsluiting |
| Restartnode uitgevoerd |Simuleert een knooppuntfout voor Service Fabric-cluster door een knooppunt opnieuw te starten. |RestartNodeAsync |Opnieuw opstarten ServiceFabricNode |Geforceerde afsluiting |
| RestartPartition |Simuleert een datacenter onleesbaar of cluster onleesbaar scenario door enkele of alle replica's van een partitie opnieuw te starten. |RestartPartitionAsync |Restart-ServiceFabricPartition |Correcte |
| RestartReplica |Simuleert een replica mislukt door een persistente replica opnieuw te starten in een cluster, het sluiten van de replica en het vervolgens opnieuw te openen. |RestartReplicaAsync |Opnieuw opstarten ServiceFabricReplica |Correcte |
| StartNode |Start een knooppunt in een cluster dat is al gestopt. |StartNodeAsync |Start-ServiceFabricNode |Niet van toepassing |
| StopNode |Simuleert een storing op een knooppunt door het stoppen van een knooppunt in een cluster. Het knooppunt blijft omlaag totdat beginknooppunt wordt aangeroepen. |StopNodeAsync |Stop-ServiceFabricNode |Geforceerde afsluiting |
| ValidateApplication |Valideert de beschikbaarheid en de status van alle Service Fabric-services in een toepassing, meestal na een bepaalde fout veroorzaken in het systeem. |ValidateApplicationAsync |Test ServiceFabricApplication |Niet van toepassing |
| ValidateService |Valideert de beschikbaarheid en de status van een Service Fabric-service, meestal na een bepaalde fout veroorzaken in het systeem. |ValidateServiceAsync |Test ServiceFabricService |Niet van toepassing |

## <a name="running-a-testability-action-using-powershell"></a>Uitvoeren van een testbaarheid actie met behulp van PowerShell
Deze zelfstudie laat zien hoe u een actie testbaarheid uitvoert met behulp van PowerShell. U leert hoe u een actie testbaarheid uitvoert op basis van een lokaal (1-box)-cluster of een Azure-cluster. Microsoft.Fabric.Powershell.dll--de Service Fabric PowerShell-module wordt automatisch geïnstalleerd tijdens de installatie van het Microsoft-Service Fabric-MSI-bestand. De module wordt automatisch geladen bij het openen van een PowerShell-prompt.

Zelfstudie segmenten:

* [Een actie uitvoeren op een cluster met één verpakking](#run-an-action-against-a-one-box-cluster)
* [Een actie uitvoeren op een Azure-cluster](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Een actie uitvoeren op een cluster met één verpakking
Als u wilt een testbaarheid actie uitvoeren op een lokaal cluster, eerst verbinding met het cluster en open de PowerShell-prompt in de beheerdersmodus. Laat het ons Bekijk de **herstart ServiceFabricNode** in te grijpen.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Hier de actie **herstart ServiceFabricNode** wordt uitgevoerd op een knooppunt met de naam 'Knooppunt1'. De voltooiing van modus geeft aan dat deze niet controleren moet of het knooppunt opnieuw opstarten actie daadwerkelijk is geslaagd. De voltooiing modus op te geven als 'Verifiëren' gaan om te controleren of de actie opnieuw opstarten daadwerkelijk verwijderd. In plaats van het knooppunt direct met de naam op te geven, kunt u deze via een partitiesleutel en het soort replica, als volgt:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Opnieuw opstarten ServiceFabricNode** moet worden gebruikt om een Service Fabric-knooppunt in een cluster opnieuw opstarten. Hiermee stopt u het proces Fabric.exe, die alle van de systeem-service en de gebruiker service replica's die worden gehost op dat knooppunt wordt opnieuw opgestart. Deze API voor het testen van uw service kunt beter onthullen bugs langs de paden voor de failover-herstel. Zo kunt u simuleren knooppuntfouten in het cluster.

De volgende schermafbeelding ziet u de **herstart ServiceFabricNode** testbaarheid opdracht in te grijpen.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

De uitvoer van de eerste **Get-ServiceFabricNode** (een cmdlet uit de Service Fabric PowerShell-module) wordt aangegeven dat het lokale cluster vijf knooppunten bevat: Node.1 naar Node.5. Nadat de actie testbaarheid (cmdlet) **herstart ServiceFabricNode** wordt uitgevoerd op het knooppunt met de naam Node.4, zien we dat de beschikbaarheid van het knooppunt is teruggezet.

### <a name="run-an-action-against-an-azure-cluster"></a>Een actie uitvoeren op een Azure-cluster
Met het uitvoeren van een actie testbaarheid (via PowerShell) in een Azure-cluster is vergelijkbaar met het uitvoeren van de actie tegen een lokaal cluster. Het enige verschil is voordat u de actie, in plaats van de verbinding met het lokale cluster kunt uitvoeren moet u eerst verbinding met de Azure-cluster.

## <a name="running-a-testability-action-using-c35"></a>Uitvoeren van een testbaarheid actie met C &#35;
Een testbaarheid als actie wilt uitvoeren met behulp van C#, moet u eerst verbinding met het cluster via FabricClient. Moet u de parameters die nodig zijn voor de actie uitvoert. Andere parameters kunnen worden gebruikt om uit te voeren dezelfde actie.
De actie RestartServiceFabricNode bekijkt, is een manier uit te voeren met behulp van de knooppuntgegevens (naam van het knooppunt en knooppunt exemplaar-ID) in het cluster.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Uitleg van de parameter:

* **CompleteMode** geeft aan dat de modus niet controleren moet of de herstart actie daadwerkelijk is geslaagd. De voltooiing modus op te geven als 'Verifiëren' gaan om te controleren of de actie opnieuw opstarten daadwerkelijk verwijderd.  
* **OperationTimeout** stelt u de hoeveelheid tijd voor de bewerking is voltooid voordat een TimeoutException-uitzondering is opgetreden.
* **CancellationToken** kunt u een aanroep van in behandeling worden geannuleerd.

In plaats van het knooppunt direct met de naam op te geven, kunt u deze via een partitiesleutel en de aard van de replica.

Zie voor meer informatie [PartitionSelector en ReplicaSelector](#partition_replica_selector).

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
PartitionSelector is een helper worden weergegeven in testbaarheid en wordt gebruikt om een specifieke partitie waarop moet worden uitgevoerd op een van de acties testbaarheid te selecteren. Het kan worden gebruikt om te selecteren van een specifieke partitie als de partitie-ID vooraf bekend. Of u kunt de partitiesleutel bieden en de bewerking wordt de partitie-ID intern opgelost. U hebt ook de optie van het selecteren van een willekeurige partitie.

Het object PartitionSelector niet maken voor het gebruik van deze helper en selecteren van de partitie met behulp van een van de methoden Select *. Vervolgens worden in het PartitionSelector-object doorgegeven aan de API dat vereist is. Als er geen optie is geselecteerd, wordt standaard een willekeurige partitie.

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
ReplicaSelector is een helper worden weergegeven in testbaarheid en wordt gebruikt om te selecteren van een replica waarop een van de testbaarheid acties uitvoert. Het kan worden gebruikt om te selecteren van een specifieke replica als de replica-ID vooraf bekend. Bovendien hebt u de mogelijkheid van het selecteren van een primaire replica of een willekeurige secundaire site. ReplicaSelector is afgeleid van PartitionSelector, dus moet u de replica en de partitie waarop u wilt uitvoeren van de bewerking testbaarheid selecteren.

Voor het gebruik van het hulpprogramma een ReplicaSelector-object maken en de manier waarop u wilt selecteren, de replica en de partitie ingesteld. U kunt deze vervolgens doorgeven in de API die dit vereist is. Als er geen optie is geselecteerd, wordt standaard een willekeurige replica en willekeurige partitie.

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
* [Testbaarheid scenario 's](service-fabric-testability-scenarios.md)
* Het testen van uw service
  * [Fouten tijdens de service werkbelastingen simuleren](service-fabric-testability-workload-tests.md)
  * [Fouten in de service-naar-service](service-fabric-testability-scenarios-service-communication.md)

