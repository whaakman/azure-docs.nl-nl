---
title: Taken met meerdere instanties gebruiken voor het uitvoeren van MPI-toepassingen - Azure Batch | Microsoft Docs
description: Informatie over het uitvoeren van Message Passing Interface (MPI)-toepassingen met behulp van het type van de taak meerdere instanties in Azure Batch.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.date: 06/12/2018
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 420f69e246b322eff28b56237613cea250be7530
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766551"
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Taken met meerdere instanties gebruiken voor het uitvoeren van Message Passing Interface (MPI) applications in Batch

Taken met meerdere instanties kunnen u een Azure Batch-taak tegelijkertijd op meerdere rekenknooppunten worden uitgevoerd. Deze taken kunnen high performance computing-scenario's zoals Message Passing Interface (MPI) applications in Batch. In dit artikel leert u hoe u voor het uitvoeren van taken met meerdere instanties met behulp van de [Batch .NET] [ api_net] bibliotheek.

> [!NOTE]
> Hoewel de voorbeelden in dit artikel ligt de nadruk op Batch .NET, MS-MPI, en Windows-rekenknooppunten, de hier besproken concepten voor de meerdere exemplaren-taak van toepassing zijn op andere platforms en technologieën (Python en Intel MPI op Linux-knooppunten, bijvoorbeeld).
>
>

## <a name="multi-instance-task-overview"></a>Overzicht van de taak meerdere exemplaren
In een Batch, elke taak wordt normaal uitgevoerd op één rekenknooppunt--u meerdere taken aan een taak indienen en de Batch-service plant u elke taak voor uitvoering in een knooppunt. Echter, door het configureren van een taak **instellingen voor meerdere instanties**, zien van Batch in plaats daarvan één primaire taak en verschillende subtaken die vervolgens worden uitgevoerd op meerdere knooppunten te maken.

![Overzicht van de taak meerdere exemplaren][1]

Wanneer u een taak met de instellingen voor meerdere instanties aan een taak verzendt, worden in de verschillende stappen die uniek zijn voor taken met meerdere instanties uitgevoerd in Batch:

1. De Batch-service maakt een **primaire** en verschillende **subtaken** op basis van de instellingen voor meerdere instanties. Het totale aantal taken (primaire plus alle subtaken) overeenkomt met het aantal **exemplaren** (rekenknooppunten) u opgeeft in de instellingen voor meerdere instanties.
2. Batch wijst een van de rekenknooppunten als de **master**, en plant u de primaire taak uit te voeren op de hoofddoelserver. Hiermee plant u de subtaken om uit te voeren op de rest van de rekenknooppunten die zijn toegewezen aan de taak met meerdere instanties, één subtaak per knooppunt.
3. De primaire en alle subtaken downloaden **algemene resourcebestanden** u opgeeft in de instellingen voor meerdere instanties.
4. Na de algemene resource bestanden zijn gedownload, de primaire en subtaken uit te voeren de **coördinatie opdracht** u opgeeft in de instellingen voor meerdere instanties. De opdracht voor coördinatie wordt meestal gebruikt om voor te bereiden knooppunten voor het uitvoeren van de taak. Het kan hierbij gaan Achtergrondservices starten (zoals [Microsoft MPI][msmpi_msdn]van `smpd.exe`) en verifiëren dat de knooppunten gereed zijn voor het verwerken van berichten tussen knooppunten.
5. De primaire taak wordt uitgevoerd de **toepassingsopdracht** op het hoofdknooppunt *nadat* de coördinatie-opdracht is voltooid door de primaire en alle subtaken. De toepassingsopdracht is van de opdrachtregel van de taak met meerdere instanties zelf, en alleen door de primaire taak wordt uitgevoerd. In een [MS-MPI][msmpi_msdn]-oplossing op basis van dit is waar u uw MPI-toepassingen met uitvoeren `mpiexec.exe`.

> [!NOTE]
> Al functioneel uniek is, de 'taken met meerdere instanties' is niet een unieke taaktype, zoals de [StartTask] [ net_starttask] of [JobPreparationTask] [ net_jobprep]. De taak met meerdere instanties is gewoon een standaard batchtaak ([CloudTask] [ net_task] in Batch .NET) met instellingen voor meerdere instanties zijn geconfigureerd. In dit artikel, verwijzen we naar dit als de **taken met meerdere instanties**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Vereisten voor taken met meerdere instanties
Taken met meerdere instanties vereist een pool met **communicatie tussen knooppunten is ingeschakeld**, en met **uitgeschakeld van de uitvoering van gelijktijdige taken**. Schakel de uitvoering van gelijktijdige taken instellen de [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool) eigenschap in op 1.

> [!NOTE]
> Batch [limieten](batch-quota-limit.md#other-limits) de grootte van een groep met de communicatie tussen knooppunten is ingeschakeld.


Dit codefragment laat zien hoe u een groep maken voor taken met meerdere instanties met behulp van de Batch .NET-bibliotheek.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> Als u probeert uit te voeren van een taak met meerdere instanties in een pool met de communicatie tussen knooppunten is uitgeschakeld, of met een *maxTasksPerNode* waarde groter dan 1, de taak is nooit gepland--voor onbepaalde tijd blijft in de status 'actief'. 


### <a name="use-a-starttask-to-install-mpi"></a>StartTask gebruiken voor het installeren van de MPI
Als u wilt uitvoeren van MPI-toepassingen met een taak met meerdere instanties, moet u eerst een MPI-implementatie (MS-MPI of Intel MPI, bijvoorbeeld) installeren op de rekenknooppunten in de groep. Dit is een goed moment om te gebruiken een [StartTask][net_starttask], die wordt uitgevoerd zodra een knooppunt aan een pool wordt toegevoegd of opnieuw wordt opgestart. Dit codefragment maakt een StartTask die Hiermee geeft u het installatiepakket MS-MPI als een [bronbestand][net_resourcefile]. De opdrachtregel van de begintaak wordt uitgevoerd nadat de resourcebestand is gedownload naar het knooppunt. In dit geval voert de opdrachtregel een installatie zonder toezicht van MS-MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Remote direct memory access (RDMA)
Als u ervoor kiest een [RDMA-compatibele grootte](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zoals A9 voor de rekenknooppunten in uw Batch-pool, uw MPI-toepassingen kan profiteren van Azure van hoge prestaties en lage latentie remote direct memory access (RDMA) netwerk.

Zoeken naar de grootte die is opgegeven als 'RDMA-compatibel' in de volgende artikelen:

* **CloudServiceConfiguration** pools

  * [Groottes voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md) (alleen Windows)
* **VirtualMachineConfiguration** pools

  * [Grootten voor virtuele machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Grootten voor virtuele machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> Om te profiteren van RDMA op [Linux-rekenknooppunten](batch-linux-nodes.md), moet u **Intel MPI** op de knooppunten. 
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Maken van een taak met meerdere instanties met Batch .NET
Nu we de groep vereisten en installatie van het pakket MPI hebben besproken, maken we de taak met meerdere instanties. In dit fragment maakt u een standaard [CloudTask][net_task], configureert u de [MultiInstanceSettings] [ net_multiinstance_prop] eigenschap. Zoals eerder vermeld, wordt de taak met meerdere instanties niet is het type van een afzonderlijke taak, maar een standaard batchtaak dat is geconfigureerd met instellingen voor meerdere instanties.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primaire taken en subtaken
Wanneer u de instellingen voor meerdere instanties voor een taak maakt, geeft u het aantal rekenknooppunten die zijn voor het uitvoeren van de taak. Wanneer u de taak aan een taak verzendt, met de Batch-service maakt een **primaire** taak en voldoende **subtaken** die samen overeenkomen met het aantal knooppunten dat u hebt opgegeven.

Deze taken zijn toegewezen een integer-id in het bereik van 0 tot en met *numberOfInstances* : 1. De taak met de id 0 is de primaire taak en alle andere id's zijn subtaken. Als u de volgende instellingen voor meerdere instanties voor een taak maakt, de primaire taak moet een id van 0 en de subtaken id's 1 t/m 9 zou hebben.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Hoofdknooppunt
Wanneer u een taak met meerdere instanties verzendt, wordt de Batch-service een van de rekenknooppunten aanwijst als de 'master' node en plant u de primaire taak uit te voeren op het hoofdknooppunt. De subtaken zijn gepland om uit te voeren op de rest van de knooppunten die zijn toegewezen aan de taak met meerdere instanties.

## <a name="coordination-command"></a>Coördinatie opdracht
De **coördinatie opdracht** wordt uitgevoerd door de primaire en de subtaken.

Het aanroepen van de opdracht voor coördinatie wordt geblokkeerd door--Batch niet de toepassingsopdracht wordt uitgevoerd totdat de coördinatie-opdracht is voor alle subtaken heeft geretourneerd. De opdracht coördinatie moet daarom start alle vereiste Achtergrondservices, Ga na of deze gereed voor gebruik en sluit vervolgens af. Bijvoorbeeld: met deze opdracht coördinatie voor een oplossing met behulp van MS-MPI versie 7 de SMPD-service wordt gestart op het knooppunt vervolgens afgesloten:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Let op het gebruik van `start` in deze opdracht coördinatie. Dit is nodig omdat de `smpd.exe` toepassing levert niet onmiddellijk na de uitvoering. Zonder het gebruik van de [start] [ cmd_start] opdracht, met deze opdracht coördinatie retourneerde niet, en daarom blokkeren de toepassingsopdracht wordt uitgevoerd.

## <a name="application-command"></a>Toepassingsopdracht
Zodra de primaire taak en alle subtaken klaar bent met de opdracht voor coördinatie wordt uitgevoerd, vanaf de opdrachtregel van de taak meerdere exemplaren wordt uitgevoerd door de primaire taak *alleen*. We noemen dit de **toepassingsopdracht** te onderscheiden van de opdracht coördinatie.

Voor MS-MPI-toepassingen, gebruikt u de toepassingsopdracht voor het uitvoeren van MPI-toepassingen met `mpiexec.exe`. Dit is bijvoorbeeld een toepassingsopdracht voor een oplossing met behulp van MS-MPI versie 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Omdat de MS-MPI `mpiexec.exe` maakt gebruik van de `CCP_NODES` variabele standaard (Zie [omgevingsvariabelen](#environment-variables)) voorbeeld van de toepassing vanaf de opdrachtregel bovenstaande sluit het.
>
>

## <a name="environment-variables"></a>Omgevingsvariabelen
Batch maakt [omgevingsvariabelen] [ msdn_env_var] specifiek voor taken met meerdere instanties op de rekenknooppunten die zijn toegewezen aan een taak met meerdere instanties. Uw opdrachtregels coördinatie en toepassing kunt verwijzen naar deze omgevingsvariabelen biedt als de scripts en programma's die ze uitvoeren.

De volgende omgevingsvariabelen zijn gemaakt door de Batch-service voor gebruik door taken met meerdere instanties:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Zie voor volledige informatie over deze en de andere partij omgevingsvariabelen, waaronder de inhoud en de zichtbaarheid, rekenknooppunt [omgevingsvariabelen Rekenknooppunt][msdn_env_var].

> [!TIP]
> De Batch Linux MPI-voorbeeldcode bevat een voorbeeld van hoe verschillende van deze omgevingsvariabelen kunnen worden gebruikt. De [coördinatie cmd] [ coord_cmd_example] Bash-script algemene toepassing downloadt en invoer van Azure Storage-bestanden, kunt u een Network File System (NFS)-share op het hoofdknooppunt en configureert u de andere knooppunten toegewezen aan de taak met meerdere instanties als NFS-clients.
>
>

## <a name="resource-files"></a>Bronbestanden
Er zijn twee sets met bronbestanden voor taken met meerdere instanties: **algemene resourcebestanden** die *alle* taken downloaden (zowel de primaire en subtaken), en de **resourcebestanden** opgegeven voor de meerdere exemplaren taak zelf, die *alleen de primaire* taak downloads.

Kunt u een of meer **algemene resourcebestanden** in de instellingen voor meerdere instanties voor een taak. Deze algemene resourcebestanden worden gedownload van [Azure Storage](../storage/common/storage-introduction.md) op van elk knooppunt **gedeelde map** door de primaire en alle subtaken. U hebt toegang tot de gedeelde map van de taak van toepassing en coördinatie van opdrachtregels via de `AZ_BATCH_TASK_SHARED_DIR` omgevingsvariabele. De `AZ_BATCH_TASK_SHARED_DIR` pad is identiek zijn op elk knooppunt dat is toegewezen aan de taak met meerdere instanties, waardoor u een opdracht één coördinatie tussen de primaire en alle subtaken kunt delen. Batch is niet 'delen' de map in een zin externe toegang, maar u kunt gebruiken als een koppelpunt of punt zoals eerder beschreven in de tip op omgevingsvariabelen delen.

Resourcebestanden die u opgeeft voor de taak met meerdere instanties zelf worden gedownload naar de werkmap van de taak, `AZ_BATCH_TASK_WORKING_DIR`, standaard. Zoals is vermeld, in tegenstelling tot algemene resourcebestanden, downloadt alleen de primaire taak resourcebestanden die zijn opgegeven voor de taak met meerdere instanties zelf.

> [!IMPORTANT]
> Gebruik altijd de omgevingsvariabelen `AZ_BATCH_TASK_SHARED_DIR` en `AZ_BATCH_TASK_WORKING_DIR` om te verwijzen naar deze mappen in uw opdrachtregels. Probeer niet om de paden handmatig samen te stellen.
>
>

## <a name="task-lifetime"></a>De levensduur van taken
De levensduur van de primaire taak bepaalt de levensduur van de taak met volledige meerdere instanties. Wanneer de primaire afsluit, worden alle subtaken beëindigd. De afsluitcode van de primaire is de afsluitcode van de taak en wordt dus gebruikt om te bepalen van het slagen of mislukken van de taak voor opnieuw proberen.

Als een van de subtaken mislukt, wordt afgesloten met de retourcode nul, bijvoorbeeld de volledige meerdere exemplaren taak mislukt. De taak met meerdere instanties vervolgens wordt beëindigd en opnieuw worden geprobeerd tot de limiet voor opnieuw proberen.

Wanneer u een taak met meerdere instanties verwijdert, worden ook de primaire en alle subtaken verwijderd door de Batch-service. Alle mappen van een subtaak en hun bestanden worden verwijderd uit de compute-knooppunten, net als bij een standard-taak.

[TaskConstraints] [ net_taskconstraints] voor een taak met meerdere instanties, zoals de [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], en [RetentionTime] [ net_taskconstraint_retention] eigenschappen, zoals ze zijn voor een taak en van toepassing op de primaire en alle subtaken worden herkend. Echter, als u wijzigt de [RetentionTime] [ net_taskconstraint_retention] eigenschap na het toevoegen van de taak met meerdere instanties voor de job deze wijziging wordt alleen toegepast op de primaire taak. Alle subtaken echter ook doorgaan met de oorspronkelijke [RetentionTime][net_taskconstraint_retention].

Een rekenknooppunt lijst met recente taken geeft de id van een subtaak als voor de recente taak is onderdeel van een taak met meerdere instanties.

## <a name="obtain-information-about-subtasks"></a>Informatie over subtaken
Als u informatie op subtaken met behulp van de Batch .NET-bibliotheek, aanroepen de [CloudTask.ListSubtasks] [ net_task_listsubtasks] methode. Deze methode retourneert informatie over alle subtaken, en informatie over de compute-knooppunt die de taken uitgevoerd. Met deze informatie kunt u bepalen de hoofdmap van elke subtaak, de groeps-id, de huidige status, afsluitcode en meer. U kunt deze informatie gebruiken in combinatie met de [PoolOperations.GetNodeFile] [ poolops_getnodefile] methode van de subtaak-bestanden te downloaden. Houd er rekening mee dat deze methode retourneert geen informatie over de primaire taak (id 0).

> [!NOTE]
> Tenzij anders vermeld, Batch .NET-methoden die worden uitgevoerd op meerdere exemplaren [CloudTask] [ net_task] zelf toepassen *alleen* naar de primaire taak. Bijvoorbeeld, wanneer u aanroepen de [CloudTask.ListNodeFiles] [ net_task_listnodefiles] methode voor een taak met meerdere instanties, alleen de bestanden van de primaire taak worden geretourneerd.
>
>

Het volgende codefragment laat zien hoe u subtaakgegevens ophalen, evenals aanvragen van inhoud van een bestand van de knooppunten waarop deze wordt uitgevoerd.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Codevoorbeeld
De [MultiInstanceTasks] [ github_mpi] voorbeeldcode op GitHub, ziet u hoe u een taak met meerdere instanties gebruiken om uit te voeren een [MS-MPI] [ msmpi_msdn] toepassing op Batch-rekenknooppunten. Volg de stappen in [voorbereiding](#preparation) en [uitvoering](#execution) om uit te voeren van het voorbeeld.

### <a name="preparation"></a>Voorbereiding
1. De eerste twee stappen in [over het compileren en uitvoeren van een eenvoudig programma met MS-MPI][msmpi_howto]. Deze voldoet aan de vereisten voor de volgende stap.
2. Bouw een *Release* versie van de [MPIHelloWorld] [ helloworld_proj] voorbeeldprogramma MPI. Dit is het programma dat door de taak met meerdere instanties op rekenknooppunten worden uitgevoerd.
3. Maak een zip-bestand met `MPIHelloWorld.exe` (welke hebt gebouwd stap 2) en `MSMpiSetup.exe` (die u hebt gedownload stap 1). U hebt dit zip-bestand uploaden als een toepassingspakket in de volgende stap.
4. Gebruik de [Azure-portal] [ portal] te maken van een Batch [toepassing](batch-application-packages.md) 'MPIHelloWorld' genoemd, en geef het zip-bestand dat u in de vorige stap hebt gemaakt als versie "1.0" van de toepassingspakket. Zie [uploaden en beheren van toepassingen](batch-application-packages.md#upload-and-manage-applications) voor meer informatie.

> [!TIP]
> Bouw een *Release* versie van `MPIHelloWorld.exe` zodat u hoeft extra afhankelijkheden bevatten (bijvoorbeeld `msvcp140d.dll` of `vcruntime140d.dll`) in uw toepassingspakket.
>
>

### <a name="execution"></a>Uitvoering
1. Download de [azure-batch-samples] [ github_samples_zip] vanuit GitHub.
2. Open de MultiInstanceTasks **oplossing** in Visual Studio 2017. De `MultiInstanceTasks.sln` oplossingsbestand bevindt zich in:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Voer de referenties voor uw Batch- en Storage-account in `AccountSettings.settings` in de **Microsoft.Azure.Batch.Samples.Common** project.
4. **Bouwen en uitvoeren** de MultiInstanceTasks-oplossing voor het uitvoeren van de MPI-voorbeeldtoepassing op rekenknooppunten in een Batch-pool.
5. *Optioneel*: Gebruik de [Azure-portal] [ portal] of [Batch Explorer] [ batch_labs] om te controleren van de voorbeeld-groep, de taak en de taak ("MultiInstanceSamplePool"," MultiInstanceSampleJob","MultiInstanceSampleTask") voordat u de resources verwijderen.

> [!TIP]
> U kunt downloaden [Visual Studio Community] [ visual_studio] gratis als u nog geen Visual Studio.
>
>

Uitvoer van `MultiInstanceTasks.exe` is vergelijkbaar met het volgende:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen
* De Microsoft HPC en Batch-Team van Azure-blog wordt besproken [MPI-ondersteuning voor Linux op Azure Batch][blog_mpi_linux], en bevat informatie over het gebruik van [OpenFOAM] [ openfoam] met Batch. Vindt u voorbeelden van Python-code voor de [OpenFOAM voorbeeld op GitHub][github_mpi].
* Meer informatie over het [pools van Linux-rekenknooppunten maken](batch-linux-nodes.md) voor gebruik in uw Azure Batch MPI-oplossingen.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Overzicht van meerdere exemplaren"
