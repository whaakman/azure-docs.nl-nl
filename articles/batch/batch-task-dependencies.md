---
title: Taakafhankelijkheden gebruiken voor het uitvoeren van taken op basis van de voltooiing van andere taken - Azure Batch | Microsoft Docs
description: Maak taken die afhankelijk van de voltooiing van andere taken zijn voor het verwerken van MapReduce-stijl en vergelijkbare big data-workloads in Azure Batch.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6a9b44ed56774466bae2f0f5d48b5e012382721b
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865230"
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a>Afhankelijkheden van de taak voor het uitvoeren van taken die afhankelijk van andere taken zijn maken

Afhankelijkheden van de taak voor het uitvoeren van een taak of een set taken pas als een bovenliggende taak is voltooid, kunt u definiëren. Sommige scenario's waarbij taakafhankelijkheden nuttig zijn zijn:

* MapReduce-stijl-workloads in de cloud.
* Taken waarvan taken gegevensverwerking kunnen worden uitgedrukt als een directed acyclic graph (DAG).
* Rendering van vooraf en achteraf renderen processen, waarbij elke taak voordat u de volgende taak uitvoeren moet kunnen beginnen.
* Een andere taak waarbij downstream-taken afhankelijk van de uitvoer van de upstream-taken zijn.

Bij taakafhankelijkheden Batch kunt kunt u taken die zijn gepland voor uitvoering op rekenknooppunten na de voltooiing van een of meer bovenliggende taken maken. Bijvoorbeeld, kunt u een taak die elke frame van een 3D-film met afzonderlijke, parallelle taken wordt weergegeven. De laatste taak--de "samenvoegen taak"--samenvoegingen de gerenderde frames in de volledige film pas nadat alle frames met succes zijn weergegeven.

Standaard worden afhankelijke taken gepland voor uitvoering pas nadat de bovenliggende taak is voltooid. U kunt een actie van de afhankelijkheid voor het standaardgedrag negeren en taken uitvoeren als de bovenliggende taak is mislukt. Zie de [afhankelijkheid acties](#dependency-actions) sectie voor meer informatie.  

U kunt taken die afhankelijk van andere taken in een-op-een- of een-op-veel-relatie zijn maken. U kunt ook een bereik-afhankelijkheid waar een taak afhangt van de voltooiing van een reeks taken binnen een opgegeven bereik van de taak-id's maken. U kunt deze drie algemene scenario's voor het maken van veel-op-veel relaties kunt combineren.

## <a name="task-dependencies-with-batch-net"></a>Afhankelijkheden van de taak met Batch .NET
In dit artikel bespreken we taakafhankelijkheden configureren met behulp van de [Batch .NET] [ net_msdn] bibliotheek. Laten we eerst zien u hoe aan [afhankelijkheid inschakelen](#enable-task-dependencies) op uw taken, en vervolgens laten zien hoe u [configureren van een taak met afhankelijkheden](#create-dependent-tasks). Er wordt ook beschreven hoe u om op te geven van een afhankelijkheid actie om uit te voeren van afhankelijke taken als het bovenliggende item is mislukt. Ten slotte wordt besproken hoe de [afhankelijkheid scenario's](#dependency-scenarios) die ondersteuning biedt voor Batch.

## <a name="enable-task-dependencies"></a>Taakafhankelijkheden inschakelen
Voor het gebruik van afhankelijkheden van taken in uw Batch-toepassing, moet u eerst de taak voor het gebruik van taakafhankelijkheden configureren. In Batch .NET, schakelt u deze op uw [CloudJob] [ net_cloudjob] door in te stellen de [UsesTaskDependencies] [ net_usestaskdependencies] eigenschap `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

In het voorgaande codefragment 'batchClient' is een exemplaar van de [BatchClient] [ net_batchclient] klasse.

## <a name="create-dependent-tasks"></a>Afhankelijke taken maken
Als u wilt een taak die afhankelijk zijn van de voltooiing van een of meer bovenliggende taken maakt, kunt u opgeven dat de taak 'afhankelijk van"de andere taken is. In Batch .NET, configureert u de [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] eigenschap met een exemplaar van de [TaskDependencies] [ net_taskdependencies] klasse:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Dit codefragment maakt u een afhankelijke taak met taak-ID 'Bloemen'. De taak 'Bloemen', is afhankelijk van taken "Regen" en "Zo". Taak "bloemen' wordt gepland om uit te voeren op een rekenknooppunt alleen na taken"Regen"en"Zo"zijn voltooid.

> [!NOTE]
> Standaard een taak is voltooid wanneer deze wordt beschouwd als de **voltooid** status en de bijbehorende **afsluitcode** is `0`. In Batch .NET, betekent dit dat een [CloudTask][net_cloudtask].[ Status] [ net_taskstate] eigenschapswaarde van `Completed` en van de CloudTask [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] eigenschapswaarde is `0`. Voor informatie over het wijzigen, Zie de [afhankelijkheid acties](#dependency-actions) sectie.
> 
> 

## <a name="dependency-scenarios"></a>Afhankelijkheid scenario 's
Er zijn drie algemene taak afhankelijkheid scenario's die u in Azure Batch gebruiken kunt:-op-een, een-op-veel en taak-ID bereik afhankelijkheid. Deze kunnen worden gecombineerd voor het bieden van een vierde veel-op-veel-scenario.

| Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Voorbeeld |  |
|:---:| --- | --- |
|  [One-to-one](#one-to-one) |*Taakb* is afhankelijk van *Taaka* <p/> *Taakb* zal niet worden gepland voor uitvoering tot *Taaka* is voltooid |![Diagram:-op-een afhankelijkheid][1] |
|  [Een-op-veel](#one-to-many) |*taakC* is afhankelijk van *taakA* én *taakB* <p/> *Taakc* zal niet worden gepland voor uitvoering tot beide *Taaka* en *Taakb* zijn voltooid |![Diagram: een-op-veel-afhankelijkheid][2] |
|  [Taak-ID-bereik](#task-id-range) |*Taakd* afhankelijk is van een bereik van taken <p/> *Taakd* zal niet worden gepland voor uitvoering totdat de taken met de id's *1* via *10* zijn voltooid |![Diagram: Taak-id-bereik afhankelijkheid][3] |

> [!TIP]
> U kunt maken **veel-op-veel** relaties, zoals waar taken C, D, E, en F elke afhankelijk van taken A en B. zijn Dit is handig, bijvoorbeeld in geparallelliseerde voorverwerking scenario's waar uw downstream-taken afhankelijk van de uitvoer van meerdere upstream taken zijn.
> 
> In de voorbeelden in deze sectie wordt een afhankelijke taak alleen wordt uitgevoerd nadat de bovenliggende taken is voltooid. Dit gedrag is het standaardgedrag voor een afhankelijke taak. Nadat een bovenliggende taak is mislukt door een afhankelijkheid-actie voor het standaardgedrag negeren op te geven, kunt u een afhankelijke taak uitvoeren. Zie de [afhankelijkheid acties](#dependency-actions) sectie voor meer informatie.

### <a name="one-to-one"></a>-Op-een
In een-op-een-relatie is een taak afhangt van de voltooiing van een bovenliggende taak. Voor het maken van de afhankelijkheid, bieden een enkele taak-ID voor de [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statische methode vullen van de [DependsOn] [ net_dependson] eigenschap van [CloudTask] [ net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Een-op-veel
In een een-op-veel-relatie is een taak afhangt van de voltooiing van taken met meerdere bovenliggende. Voor het maken van de afhankelijkheid, bieden u een verzameling van taak-id's aan de [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statische methode vullen van de [DependsOn] [ net_dependson] eigenschap van [CloudTask] [ net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a>Taak-ID-bereik
In een afhankelijkheid van een bereik van bovenliggende taken, is een taak afhangt van de voltooiing van taken waarvoor id's binnen een bereik liggen.
Voor het maken van de afhankelijkheid, geef de eerste en laatste taak-id's in het bereik tot de [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statische methode vullen van de [DependsOn] [ net_dependson] eigenschap van [CloudTask] [ net_cloudtask].

> [!IMPORTANT]
> Wanneer u taak-ID bereiken voor uw afhankelijkheden, wordt alleen taken met de id's voor gehele getallen worden geselecteerd door het bereik. Dus het bereik `1..10` selecteert taken `3` en `7`, maar niet `5flamingoes`. 
> 
> Voorloopnullen zijn niet significante bij het evalueren van bereik afhankelijkheden, zodat taken met een tekenreeks-id's `4`, `04` en `004` worden alle *binnen* het bereik en ze zullen worden behandeld als taak `4`, zodat het eerste item om te voltooien, voldoen aan de afhankelijkheid.
> 
> Elke taak in het bereik moet voldoen aan de afhankelijkheid, door het niet wordt voltooid of door te voeren met een fout die gekoppeld aan een afhankelijkheid-actie ingesteld op **Satisfy**. Zie de [afhankelijkheid acties](#dependency-actions) sectie voor meer informatie.
>
>

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a>Acties van afhankelijkheid

Standaard wordt een afhankelijke taak of een set taken uitgevoerd nadat een bovenliggende taak is voltooid. In sommige scenario's kunt u afhankelijke taken uitvoeren, zelfs als de bovenliggende taak is mislukt. U kunt het standaardgedrag vervangen door een afhankelijkheid actie op te geven. Een actie afhankelijkheid geeft aan of een afhankelijke taak in aanmerking komende uit te voeren, op basis van het slagen of mislukken van de bovenliggende taak. 

Stel bijvoorbeeld dat een afhankelijke taak wacht op gegevens van de voltooiing van de upstream-taak. Als de upstream-taak is mislukt, de afhankelijke taak nog steeds mogelijk om uit te voeren met behulp van oudere gegevens. In dit geval kunt een afhankelijkheid actie opgeven dat de afhankelijke taak komt in aanmerking om uit te voeren ondanks de fout van de bovenliggende taak.

Een actie afhankelijkheid is gebaseerd op een voorwaarde voor het afsluiten van de bovenliggende taak. U kunt een actie van de afhankelijkheid voor een van de volgende afsluitvoorwaarden; opgeven voor .NET, Zie de [ExitConditions] [ net_exitconditions] klasse voor meer informatie:

- Als een voorverwerkingsfout optreedt.
- Fout treedt op wanneer een bestand uploadt. Als de taak wordt beëindigd met een afsluitcode die is opgegeven via **exitCodes** of **exitCodeRanges**, en vervolgens een bestand uploaden fout, de actie die is opgegeven door de afsluitcode tegenkomt voorrang.
- Wanneer de taak wordt beëindigd met een afsluitcode die is gedefinieerd door de **ExitCodes** eigenschap.
- Wanneer de taak wordt beëindigd met een afsluitcode die binnen een bereik dat is opgegeven valt door de **ExitCodeRanges** eigenschap.
- Als de taak wordt beëindigd met een afsluitcode die niet zijn gedefinieerd door de standaardsituatie **ExitCodes** of **ExitCodeRanges**, of als de taak wordt beëindigd met een voorverwerkingsfout en de **PreProcessingError** eigenschap niet is ingesteld, of als de taak is mislukt met een bestand uploaden fout en de **FileUploadError** eigenschap niet is ingesteld. 

Als een afhankelijkheid actie in .NET opgeven, stelt u de [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] eigenschap voor de voorwaarde voor het afsluiten. De **DependencyAction** voor deze eigenschap is een van twee waarden:

- Instellen van de **DependencyAction** eigenschap **Satisfy** geeft aan dat afhankelijke taken die in aanmerking komen voor het uitvoeren als de bovenliggende taak wordt beëindigd met een opgegeven fout.
- Instellen van de **DependencyAction** eigenschap **blok** geeft aan dat afhankelijke taken niet in aanmerking komende om uit te voeren.

De standaardinstelling voor de **DependencyAction** eigenschap **Satisfy** voor afsluitcode 0, en **blok** voor alle andere afsluitvoorwaarden.

De volgende code codefragment stelt de **DependencyAction** eigenschap voor een bovenliggende taak. Als de bovenliggende taak met een voorverwerkingsfout of met de opgegeven foutcodes afgesloten, wordt de afhankelijke taak geblokkeerd. Als de bovenliggende taak met een andere niet-nul-fout afgesloten, wordt de afhankelijke taak in aanmerking komende om uit te voeren.

```csharp
// Task A is the parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with the specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible to run 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible to run depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a>Codevoorbeeld
De [TaskDependencies] [ github_taskdependencies] voorbeeldproject is een van de [Azure Batch-codevoorbeelden] [ github_samples] op GitHub. Deze Visual Studio-oplossing wordt getoond:

- Taakafhankelijkheid op een andere taak inschakelen
- Over het maken van taken die afhankelijk van andere taken zijn
- Klik hier voor meer informatie over het uitvoeren van deze taken op een pool van rekenknooppunten.

## <a name="next-steps"></a>Volgende stappen
### <a name="application-deployment"></a>Toepassingsimplementatie
De [toepassingspakketten](batch-application-packages.md) functie van Batch biedt een eenvoudige manier om beide te implementeren en de versie die de toepassingen die uw taken worden uitgevoerd op rekenknooppunten.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en faseert gegevens
Zie [installeren van toepassingen en faseert gegevens op Batch-rekenknooppunten] [ forum_post] in de Azure Batch-forum voor een overzicht van methoden voor het voorbereiden van uw knooppunten om uit te voeren taken. Dit bericht is geschreven door een van de Azure Batch-teamleden, is een goede inleiding over de verschillende manieren toepassingen, invoergegevens van de taak en andere bestanden kopiëren naar uw rekenknooppunten.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_exitconditions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions#Microsoft_Azure_Batch_ExitOptions_DependencyAction
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram:-op-een afhankelijkheid"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: een-op-veel-afhankelijkheid"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: taak-id-bereik afhankelijkheid"
