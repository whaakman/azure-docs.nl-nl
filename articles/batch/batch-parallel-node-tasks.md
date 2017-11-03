---
title: "Taken uitvoeren ter aanvulling van rekenresources efficiënt - gebruik Azure Batch | Microsoft Docs"
description: "Hogere efficiëntie en lagere kosten via minder rekenknooppunten en actief gelijktijdige taken op elk knooppunt in een Azure Batch-pool"
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eae6359b5fb36bd0317391ce2330afb7dd7bfe3b
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Taken uitvoeren gelijktijdig maximaal gebruik van de Batch-rekenknooppunten 

Door meer dan één taak tegelijkertijd uitgevoerd op elk rekenknooppunt in uw Azure Batch-pool, kunt u gebruik van bronnen op een kleiner aantal knooppunten in de pool maximaliseren. Voor sommige werkbelastingen, kan dit resulteren in een kortere taaktijden en lagere kosten.

Hoewel sommige scenario's profiteren van alle resources van een knooppunt dat aan een enkele taak, profiteert verschillende situaties zodat meerdere taken die resources delen:

* **Overdracht van gegevens voor het minimaliseren** wanneer taken kunnen geen gegevens delen. In dit scenario kunt u gegevensoverdracht kosten aanzienlijk verminderen door gedeelde gegevens kopiëren naar een kleiner aantal knooppunten en het uitvoeren van taken parallel op elk knooppunt. Dit geldt vooral als de gegevens worden gekopieerd naar elk knooppunt moet worden overgebracht tussen geografische regio's.
* **Het optimaliseren van geheugengebruik** wanneer taken vereisen dat een grote hoeveelheid geheugen, maar alleen gedurende korte perioden tijd en op variabele momenten tijdens de uitvoering. U kunt gebruikmaken van meer geheugen voor het afhandelen van dergelijke pieken efficiënt minder, maar groter, rekenknooppunten. Deze knooppunten zou hebben meerdere taken parallel uitgevoerd op elk knooppunt, maar elke taak wilt profiteren van de knooppunten grote hoeveelheid geheugen op verschillende tijdstippen.
* **Aantal grenzen knooppunt beperkende** wanneer de communicatie tussen knooppunten is vereist in een pool. Toepassingen die zijn geconfigureerd voor communicatie tussen knooppunten zijn momenteel beperkt tot 50 rekenknooppunten. Als elk knooppunt in deze groep kunnen taken parallel uitvoeren, kan een groter aantal taken tegelijkertijd worden uitgevoerd.
* **Bezig met het repliceren van een lokale compute cluster**, zoals wanneer u eerst een compute-omgeving naar Azure verplaatsen. Als uw huidige on-premises-oplossing wordt meerdere taken per rekenknooppunt uitgevoerd, kunt u het maximum aantal taken voor het spiegelen van nauwkeuriger die configuratie knooppunt verhogen.

## <a name="example-scenario"></a>Voorbeeldscenario 's
Als voorbeeld ter illustratie van de voordelen van de uitvoering van parallelle taken, Stel dat uw taaktoepassing CPU en geheugen heeft dat [standaard\_D1](../cloud-services/cloud-services-sizes-specs.md) knooppunten zijn voldoende. Maar als u klaar bent met de taak in de tijd die nodig, 1.000 van deze knooppunten zijn vereist.

In plaats van met behulp van standaard\_D1 knooppunten die 1 CPU-kern hebt, kunt u [standaard\_D14](../cloud-services/cloud-services-sizes-specs.md) knooppunten met 16 kernen en uitvoering van de parallelle taak inschakelen. Daarom *16 keer minder knooppunten* kan alleen worden gebruikt--in plaats van 1000 knooppunten 63 zijn vereist. Bovendien als grote toepassingsbestanden of referentiegegevens vereist voor elk knooppunt zijn, zijn de duur van de taak en efficiëntie opnieuw verbeterd omdat de gegevens gekopieerd naar maximaal 63 knooppunten.

## <a name="enable-parallel-task-execution"></a>Uitvoering van de parallelle taak inschakelen
Bij het configureren van rekenknooppunten voor parallelle uitvoering op het niveau van de groep van toepassingen. Met de Batch .NET-bibliotheek, stel de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap bij het maken van een groep. Als u de Batch REST-API gebruikt, stelt u de [maxTasksPerNode] [ rest_addpool] element in de aanvraagtekst tijdens het maken van de groep van toepassingen.

Azure Batch kunt u maximum aantal taken per knooppunt maximaal vier keer instellen (x 4) het aantal kernen knooppunt. Bijvoorbeeld, als de groep van toepassingen is geconfigureerd met de knooppunten van het formaat van 'Groot' (vier kernen) vervolgens `maxTasksPerNode` kan worden ingesteld op 16. Zie voor meer informatie over het aantal kernen voor elk van de grootte van het knooppunt, [grootten voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md). Zie voor meer informatie over Servicelimieten [quota en limieten voor de Azure Batch-service](batch-quota-limit.md).

> [!TIP]
> Zorg ervoor dat in aanmerking te nemen de `maxTasksPerNode` waarde als u een [formule voor automatisch schalen] [ enable_autoscaling] voor uw toepassingen. Bijvoorbeeld, een formule waarvan de evaluatie `$RunningTasks` aanzienlijk kunnen worden beïnvloed door een toename van de taken per knooppunt. Zie [automatisch schalen rekenknooppunten in een Azure Batch-pool](batch-automatic-scaling.md) voor meer informatie.
>
>

## <a name="distribution-of-tasks"></a>Verdeling van taken
Wanneer de rekenknooppunten in een groep gelijktijdig taken uitvoeren kunnen, is het belangrijk om te geven hoe u wilt dat de taken verdeeld over de knooppunten in de pool.

Met behulp van de [CloudPool.TaskSchedulingPolicy] [ task_schedule] eigenschap, kunt u opgeven dat taken gelijkmatig over alle knooppunten in de groep ('spreiden") moeten worden toegewezen. Of u kunt opgeven dat zo zo veel mogelijk taken moeten worden toegewezen aan elk knooppunt voordat er taken zijn toegewezen aan een ander knooppunt in de groep ('verpakking').

Een voorbeeld van hoe deze functie waardevolle is, kunt u de groep van [standaard\_D14](../cloud-services/cloud-services-sizes-specs.md) knooppunten (in het bovenstaande voorbeeld) die is geconfigureerd met een [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] waarde van 16. Als de [CloudPool.TaskSchedulingPolicy] [ task_schedule] is geconfigureerd met een [ComputeNodeFillType] [ fill_type] van *Pack*, zou het gebruik van alle 16 kernen van elk knooppunt maximaliseren en toestaan een [automatisch schalen van toepassingen](batch-automatic-scaling.md) te verwijderen van ongebruikte knooppunten uit de groep (knooppunten zonder geen taken toegewezen). Hierdoor minimaliseert brongebruik en bespaart u geld.

## <a name="batch-net-example"></a>Batch .NET-voorbeeld
Dit [Batch .NET] [ api_net] API codefragment toont een aanvraag voor het maken van een groep die vier grote knooppunten met een maximum van vier taken per knooppunt bevat. Hiermee wordt een taak plannen beleid dat elk knooppunt worden gevuld met taken vóór de taken toe te wijzen naar een ander knooppunt in de groep. Zie voor meer informatie over het toevoegen van groepen met behulp van de Batch .NET API [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Batch REST-voorbeeld
Dit [Batch REST] [ api_rest] API fragment toont een aanvraag voor het maken van een groep die twee grote knooppunten met een maximum van vier taken per knooppunt bevat. Zie voor meer informatie over het toevoegen van groepen met behulp van de REST-API [een groep toevoegen aan een account][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> U kunt instellen de `maxTasksPerNode` element en [MaxTasksPerComputeNode] [ maxtasks_net] eigenschap alleen tijdens de aanmaak van toepassingen. Ze kunnen niet worden gewijzigd nadat een pool al zijn gemaakt.
>
>

## <a name="code-sample"></a>Codevoorbeeld
De [ParallelNodeTasks] [ parallel_tasks_sample] project op GitHub ziet u het gebruik van de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap.

Deze C#-consoletoepassing maakt gebruik van de [Batch .NET] [ api_net] bibliotheek aan een pool maken met een of meer rekenknooppunten. Een configureerbare aantal taken wordt uitgevoerd op die knooppunten om te simuleren variabele laden. Uitvoer van de toepassing geeft aan welke knooppunten uitgevoerd elke taak. De toepassing bevat ook een samenvatting van de taakparameters en duur. Het gedeelte van de samenvatting van de uitvoer van twee verschillende uitvoeringen van de voorbeeldtoepassing wordt hieronder weergegeven.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

De eerste uitvoering van de voorbeeldtoepassing blijkt dat met één knooppunt in de groep en de standaardinstelling van één taak per knooppunt, de duur van de taak is meer dan 30 minuten.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

De tweede uitvoering van het voorbeeld bevat een aanzienlijke daling van de duur van de taak. Dit is omdat de groep met vier taken per knooppunt, waarmee voor parallelle uitvoering van de taak voltooid in bijna een kwartaal van de tijd die is geconfigureerd.

> [!NOTE]
> De duur van de taak in de bovenstaande samenvattingen omvatten geen tijd voor het maken van toepassingen. Elk van de bovenstaande taken is ingediend bij de eerder gemaakte groepen waarvan rekenknooppunten zijn in de *inactief* status tijdens verzending.
>
>

## <a name="next-steps"></a>Volgende stappen
### <a name="batchlabs-heat-map"></a>BatchLabs Heatmap
[BatchLabs][batch_labs] is een gratis, uitgebreid, zelfstandig clienthulpprogramma voor het maken en bewaken van en opsporen van fouten in Azure Batch-toepassingen. BatchLabs bevat een *Heatmap* functie waarmee visualisatie van de uitvoering van de taak. Wanneer u bent uitvoeren van de [ParallelTasks] [ parallel_tasks_sample] voorbeeldtoepassing, kunt u de functie Heatmap eenvoudig visualiseren parallelle taken op elk knooppunt worden uitgevoerd.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchLabs/
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

