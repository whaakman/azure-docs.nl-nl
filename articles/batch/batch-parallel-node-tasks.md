---
title: Uitvoeren van taken gelijktijdig gebruik van compute-resources efficiënt - Azure Batch | Microsoft Docs
description: Lagere kosten en efficiëntie verhogen met behulp van minder compute-knooppunten en actief gelijktijdige taken op elk knooppunt in een Azure Batch-pool
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c52c9fc6b47b03b3ca6db96decb8b4777577d00e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174372"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Voer taken gelijktijdig maximaal gebruik van Batch-rekenknooppunten 

Door meer dan één taak tegelijk uitgevoerd op elk knooppunt in de Azure Batch-pool, kunt u gebruik van bronnen op een kleiner aantal knooppunten in de pool maximaliseren. Voor sommige werkbelastingen, kan dit resulteren in kortere taaktijden en lagere kosten.

Hoewel sommige scenario's profiteren van alle resources van een knooppunt toe aan een enkele taak, profiteert verschillende situaties zodat meerdere taken voor het delen van deze resources:

* **Gegevensoverdracht minimaliseren** wanneer taken zijn kunnen delen van gegevens. In dit scenario kunt u kosten voor gegevensoverdracht aanzienlijk verkorten door gedeelde gegevens kopiëren naar een kleiner aantal knooppunten en het uitvoeren van taken parallel op elk knooppunt. Dit geldt met name als de gegevens moeten worden gekopieerd naar elk knooppunt moet worden verzonden tussen de geografische regio's.
* **Geheugengebruik maximaliseren** wanneer taken vereisen een grote hoeveelheid geheugen, maar alleen tijdens korte perioden van tijd, en op variabele momenten tijdens de uitvoering. U kunt gebruikmaken van minder, maar grotere, compute-knooppunten met meer geheugen aan deze pieken efficiënt verwerken. Deze knooppunten zou hebben meerdere taken die gelijktijdig worden uitgevoerd op elk knooppunt, maar elke taak wilt profiteren van de grote hoeveelheid geheugen van de knooppunten op verschillende tijdstippen.
* **Aantal grenzen knooppunt beperkende** wanneer de communicatie tussen knooppunten is vereist in een pool. Groepen die zijn geconfigureerd voor communicatie tussen knooppunten zijn momenteel beperkt tot 50 compute-knooppunten. Als elk knooppunt in een dergelijke toepassingen kan taken parallel uitvoeren, kan een groter aantal taken gelijktijdig worden uitgevoerd.
* **Het repliceren van een on-premises rekencluster**, zoals wanneer u eerst een compute-omgeving naar Azure verplaatsen. Als uw huidige on-premises-oplossing wordt uitgevoerd voor meerdere taken per knooppunt, kunt u het maximum aantal knooppunttaken voor het spiegelen beter aansluiten dat de configuratie van verhogen.

## <a name="example-scenario"></a>Voorbeeldscenario
Een voorbeeld ter illustratie van de voordelen van de uitvoering van de parallelle taak: Stel dat uw taaktoepassing vereisten voor CPU en geheugen heeft dat [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) knooppunten zijn voldoende. Maar als u klaar bent met de taak in de tijd die nodig is, wilt 1000 van deze knooppunten nodig zijn.

In plaats van Standard\_D1-knooppunten die 1 CPU-kern hebt, kunt u [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) knooppunten die beschikken over 16 kernen en uitvoering van de parallelle taak inschakelen. Daarom *16 keer minder knooppunten* kan alleen worden gebruikt, in plaats van 1000 knooppunten 63 zijn vereist. Bovendien als grote toepassingsbestanden of referentiegegevens vereist voor elk knooppunt zijn, worden de duur van de taak en efficiëntie opnieuw verbeterd omdat de gegevens worden gekopieerd naar alleen 63 knooppunten.

## <a name="enable-parallel-task-execution"></a>Uitvoering van de parallelle taak inschakelen
U configureren rekenknooppunten voor de uitvoering van de parallelle taak op het niveau van de groep van toepassingen. Met de Batch .NET-bibliotheek, stel de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap wanneer u een pool maakt. Als u de Batch REST-API, stelt u de [maxTasksPerNode] [ rest_addpool] -element in de hoofdtekst van de aanvraag tijdens het maken van toepassingen.

Azure Batch kunt u het maximum aantal taken per knooppunt maximaal vier keer instellen (4 x) het aantal kernen knooppunt. Bijvoorbeeld, als de groep is geconfigureerd met knooppunten van het formaat van "Large' (vier kernen), klikt u vervolgens `maxTasksPerNode` kan worden ingesteld op 16. Zie voor meer informatie over het aantal kernen voor elk van de grootte, [groottes voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Zie voor meer informatie over Servicelimieten [quota en limieten voor de Azure Batch-service](batch-quota-limit.md).

> [!TIP]
> Zorg ervoor dat u rekening mee dat de `maxTasksPerNode` waarde als u een [formule voor automatisch schalen] [ enable_autoscaling] voor uw pool. Bijvoorbeeld, een formule die evalueert `$RunningTasks` aanzienlijk kunnen worden beïnvloed door een toename in taken per knooppunt. Zie [automatisch schalen rekenknooppunten in een Azure Batch-pool](batch-automatic-scaling.md) voor meer informatie.
>
>

## <a name="distribution-of-tasks"></a>Distributie van taken
Wanneer de rekenknooppunten in een pool kunnen taken gelijktijdig worden uitgevoerd, is het belangrijk om op te geven hoe u wilt dat de taken die moeten worden verdeeld over de knooppunten in de pool.

Met behulp van de [CloudPool.TaskSchedulingPolicy] [ task_schedule] eigenschap, kunt u opgeven dat taken gelijkmatig moeten worden toegewezen voor alle knooppunten in de groep ('verspreiden'). U kunt ook opgeven dat net zoveel taken uit mogelijk moeten worden toegewezen aan elk knooppunt voordat er taken zijn toegewezen aan een ander knooppunt in de groep ('packing').

Een voorbeeld van hoe deze functie nuttig is, kunt u de groep van [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) knooppunten (in het bovenstaande voorbeeld) die is geconfigureerd met een [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] waarde van 16. Als de [CloudPool.TaskSchedulingPolicy] [ task_schedule] is geconfigureerd met een [ComputeNodeFillType] [ fill_type] van *Pack*, zou het gebruik van alle 16 kernen van elk knooppunt te maximaliseren en toestaan een [automatisch schalen van toepassingen](batch-automatic-scaling.md) te verwijderen van ongebruikte knooppunten uit de pool (knooppunten zonder alle taken die zijn toegewezen). Dit wordt geminimaliseerd Resourcegebruik en bespaart u geld.

## <a name="batch-net-example"></a>Batch .NET-voorbeeld
Dit [Batch .NET] [ api_net] API codefragment toont een aanvraag voor het maken van een groep met vier knooppunten met een maximum van vier taken per knooppunt. Het geeft een taak plannen die elk knooppunt worden gevuld met taken vóór de taken toe te wijzen naar een ander knooppunt in de groep. Zie voor meer informatie over het toevoegen van groepen met behulp van de Batch .NET API [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Voorbeeld van de batch REST
Dit [Batch REST] [ api_rest] API fragment toont een aanvraag voor het maken van een pool met twee grote knooppunten met een maximum van vier taken per knooppunt. Zie voor meer informatie over het toevoegen van groepen met behulp van de REST-API, [een groep toevoegen aan een account][rest_addpool].

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
> U kunt instellen dat de `maxTasksPerNode` element en [MaxTasksPerComputeNode] [ maxtasks_net] eigenschap alleen tijdens de aanmaak van de groep van toepassingen. Ze kunnen niet worden gewijzigd nadat een groep is al gemaakt.
>
>

## <a name="code-sample"></a>Codevoorbeeld
De [ParallelNodeTasks] [ parallel_tasks_sample] project op GitHub illustreert het gebruik van de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap.

Deze C#-consoletoepassing maakt gebruik van de [Batch .NET] [ api_net] bibliotheek naar een pool maken met een of meer rekenknooppunten. Het uitvoeren van een configureerbaar aantal taken op deze knooppunten om te simuleren variabele. Uitvoer van de toepassing geeft aan welke knooppunten uitgevoerd elke taak. De toepassing biedt ook een samenvatting van de taakparameters en de duur. De samenvatting gedeelte van de uitvoer van twee verschillende uitvoeringen van de voorbeeldtoepassing wordt hieronder weergegeven.

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

De tweede uitvoering van het voorbeeld wordt een aanzienlijke daling van de duur van de taak. Dit is omdat de groep is geconfigureerd met vier taken per knooppunt, waardoor de uitvoering van de parallelle taak voor het voltooien van de taak in bijna een kwart van de tijd.

> [!NOTE]
> De duur van de taak in de bovenstaande samenvattingen omvatten niet de tijd voor het maken van toepassingen. Elk van de bovenstaande taken is ingediend bij de eerder gemaakte groepen waarvan compute-knooppunten zijn in de *niet-actief* status tegelijk verzenden.
>
>

## <a name="next-steps"></a>Volgende stappen
### <a name="batch-explorer-heat-map"></a>Batch Explorer-Heatmap
[Batch Explorer] [ batch_labs] is een gratis, uitgebreid, zelfstandig clienthulpprogramma voor het maken, foutopsporing en Azure Batch-toepassingen bewaken. Batch Explorer bevat een *Heat Map* functie waarmee de visualisatie van de uitvoering van de taak. Wanneer u bent uitvoeren van de [ParallelTasks] [ parallel_tasks_sample] voorbeeldtoepassing, kunt u de functie Heat Map gebruiken voor het visualiseren van eenvoudig het uitvoeren van parallelle taken op elk knooppunt.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

