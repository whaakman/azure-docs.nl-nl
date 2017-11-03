---
title: Taken met meerdere instanties gebruiken voor het uitvoeren van MPI-toepassingen - Azure Batch | Microsoft Docs
description: Informatie over het gebruik van het type van de taak meerdere exemplaren in Azure Batch Message Passing Interface (MPI)-toepassingen uitvoeren.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: 5/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 01da017587aed7c0f2415786fdcbf6f64024cbe3
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Taken met meerdere instanties gebruiken Message Passing Interface (MPI)-toepassingen uitvoeren in Batch

Taken met meerdere instanties kunnen u een Azure Batch-taak tegelijkertijd op meerdere rekenknooppunten worden uitgevoerd. Deze taken kunnen high performance computing-scenario's zoals Message Passing Interface (MPI) applications in Batch. In dit artikel leert u hoe uit te voeren taken met meerdere instanties met behulp van de [Batch .NET] [ api_net] bibliotheek.

> [!NOTE]
> De voorbeelden in dit artikel ligt de nadruk op Batch .NET, MS-MPI en rekenknooppunten voor Windows, zijn die hier worden besproken concepten voor meerdere exemplaren taak van toepassing op andere platforms en -technologieën (Python en Intel MPI op Linux-knooppunten, bijvoorbeeld).
>
>

## <a name="multi-instance-task-overview"></a>Overzicht van de taak meerdere exemplaren
In een Batch elke taak normaal wordt uitgevoerd op één rekenknooppunt--verzenden van meerdere taken aan een job en de Batch-service plant u elke taak voor uitvoering op een knooppunt. Echter, door het configureren van een taak **instellingen voor meerdere exemplaren van**, zien van Batch in plaats daarvan één primaire taak en verschillende subtaken die vervolgens worden uitgevoerd op meerdere knooppunten te maken.

![Overzicht van de taak meerdere exemplaren][1]

Wanneer u een taak met meerdere exemplaren instellingen voor een taak indient, voert Batch in de verschillende stappen die uniek zijn voor taken met meerdere instanties:

1. De Batch-service maakt een **primaire** en verschillende **subtaken** op basis van de instellingen van meerdere exemplaren. Het totale aantal taken (primaire plus alle subtaken) overeenkomt met het aantal **exemplaren** (rekenknooppunten) u opgeeft in de instellingen van meerdere exemplaren.
2. Batch wijst een van de rekenknooppunten als de **master**, en de belangrijkste taak moet worden uitgevoerd op de master plant. Hiermee plant u de subtaken moet worden uitgevoerd op de rest van de rekenknooppunten die zijn toegewezen aan de taak met meerdere instanties, één subtaak per knooppunt.
3. De primaire server en alle subtaken downloaden **algemene bronbestanden** u opgeeft in de instellingen van meerdere exemplaren.
4. Na de algemene resource bestanden zijn gedownload, de primaire en subtaken uit te voeren de **coördinatie opdracht** u opgeeft in de instellingen van meerdere exemplaren. De opdracht coördinatie wordt meestal gebruikt voor het voorbereiden van de knooppunten voor het uitvoeren van de taak. Het kan hierbij gaan Achtergrondservices wordt gestart (zoals [Microsoft MPI][msmpi_msdn]van `smpd.exe`) en verifiëren dat de knooppunten gereed zijn voor verwerking van berichten tussen knooppunten.
5. De belangrijkste taak wordt uitgevoerd de **toepassingsopdracht** op het hoofdknooppunt *nadat* de coördinatie-opdracht is voltooid door de primaire server en alle subtaken. De toepassingsopdracht wordt de opdrachtregel van de taak met meerdere instanties zelf en alleen door de belangrijkste taak is uitgevoerd. In een [MS MPI][msmpi_msdn]-gebaseerde oplossing, dit is waar u uw MPI-toepassingen met uitvoeren `mpiexec.exe`.

> [!NOTE]
> Hoewel functioneel distinct, de 'meerdere exemplaren-taak' is niet een unieke taaktype zoals de [StartTask] [ net_starttask] of [JobPreparationTask] [ net_jobprep]. De taak meerdere exemplaren is gewoon een Batch standaardtaak ([CloudTask] [ net_task] in Batch .NET) waarvan u meerdere exemplaren de instellingen zijn geconfigureerd. In dit artikel is als de **taak met meerdere instanties**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Vereisten voor taken met meerdere instanties
Taken met meerdere instanties vereisen een pool met **communicatie tussen knooppunten ingeschakeld**, en met **uitvoering van gelijktijdige taken uitgeschakeld**. Schakel de uitvoering van gelijktijdige taken instellen de [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) eigenschap in op 1.

Dit codefragment laat zien hoe een groep maken voor taken met meerdere instanties met behulp van de Batch .NET-bibliotheek.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> Als u probeert uit te voeren van een taak met meerdere instanties in een pool communicatie tussen knooppunten is uitgeschakeld of met een *maxTasksPerNode* waarde groter dan 1: de taak wordt nooit gepland--voor onbepaalde tijd blijft in de status 'active'. 
>
> Taken met meerdere instanties kunnen alleen op knooppunten in groepen die zijn gemaakt na 14 December 2015 uitvoeren.
>
>

### <a name="use-a-starttask-to-install-mpi"></a>StartTask gebruiken voor het installeren van MPI
Als u wilt uitvoeren van MPI-toepassingen met een taak meerdere exemplaren, moet u eerst een MPI-implementatie (MS-MPI of Intel MPI, bijvoorbeeld) installeren op de rekenknooppunten in de groep. Dit is een goed moment om het gebruik van een [StartTask][net_starttask], die wordt uitgevoerd zodra er een knooppunt aan een pool wordt toegevoegd of opnieuw is opgestart. Dit codefragment maakt u een StartTask waarmee het MS MPI-installatiepakket als een [bronbestand][net_resourcefile]. De begintaak vanaf de opdrachtregel wordt uitgevoerd nadat het bronbestand is gedownload naar het knooppunt. In dit geval wordt voert de opdrachtregel een installatie zonder toezicht van MS MPI.

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
Als u kiest een [RDMA-compatibele grootte](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zoals A9 voor de rekenknooppunten in uw Batch-pool uw MPI-toepassingen kan profiteren van Azure hoge prestaties, lage latentie remote direct memory access (RDMA) netwerk.

Zoek naar de grootte die is opgegeven als 'RDMA-compatibele' in de volgende artikelen:

* **CloudServiceConfiguration** pools

  * [Grootten voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md) (alleen Windows)
* **VirtualMachineConfiguration** pools

  * [Grootten voor virtuele machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Grootten voor virtuele machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> Om te profiteren van RDMA op [Linux-rekenknooppunten](batch-linux-nodes.md), moet u **Intel MPI** op de knooppunten. Zie voor meer informatie over CloudServiceConfiguration en VirtualMachineConfiguration groepen de Pool-sectie van de [overzicht Batch-functies](batch-api-basics.md).
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Een taak meerdere exemplaren maken met de Batch .NET
Nu dat we de pool-vereisten en installatie van MPI-pakket besproken hebben, gaan we de taak meerdere exemplaren te maken. In dit fragment maken we een standaard [CloudTask][net_task], configureert u de [MultiInstanceSettings] [ net_multiinstance_prop] eigenschap. Zoals eerder vermeld, wordt de taak meerdere exemplaren niet is een afzonderlijke taaktype, maar een Batch standaardtaak geconfigureerd met instellingen voor meerdere exemplaren.

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
Wanneer u de instellingen voor meerdere exemplaren van een taak maakt, geeft u het aantal rekenknooppunten die de taak uitvoeren. Wanneer u de taak aan een job indient, maakt de Batch-service een **primaire** taak en er voldoende **subtaken** die samen overeenkomen met het aantal knooppunten dat u hebt opgegeven.

Deze taken zijn toegewezen een integer-id in het bereik van 0 tot en met *numberOfInstances* - 1. De taak met id 0 is de belangrijkste taak en andere id's subtaken zijn. Als u de volgende instellingen voor meerdere instanties voor een taak maakt, de belangrijkste taak moet een id van 0 en subtaken ids 1 t/m 9 zou hebben.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Hoofdknooppunt
Wanneer u een taak met meerdere instanties verzendt, wordt de Batch-service wijst een van de rekenknooppunten als de 'master'-knooppunt en plant u de belangrijkste taak moet worden uitgevoerd op het hoofdknooppunt. De subtaken zijn gepland om uit te voeren op de rest van de knooppunten die zijn toegewezen aan de taak meerdere exemplaren.

## <a name="coordination-command"></a>Coördinatie opdracht
De **coördinatie opdracht** wordt uitgevoerd door de primaire en subtaken.

Het aanroepen van de opdracht coördinatie blokkeert--Batch niet de toepassingsopdracht wordt uitgevoerd totdat de opdracht coördinatie met succes voor alle subtaken heeft geretourneerd. De opdracht coördinatie moet daarom start alle vereiste Achtergrondservices, Ga na of deze klaar voor gebruik en sluit vervolgens af. Bijvoorbeeld: met deze opdracht coördinatie voor een oplossing met behulp van MS MPI-versie 7 start de service SMPD op het knooppunt vervolgens afgesloten:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Let op het gebruik van `start` in deze opdracht coördinatie. Dit is nodig omdat de `smpd.exe` toepassing geen retourneert onmiddellijk na de uitvoering. Zonder het gebruik van de [start] [ cmd_start] opdracht met deze opdracht coördinatie niet geretourneerd, en daarom aangeraden de toepassingsopdracht wordt uitgevoerd.

## <a name="application-command"></a>Toepassingsopdracht
Zodra de belangrijkste taak en alle subtaken hebt uitvoeren van de opdracht coördinatie, opdrachtregel voor de meerdere exemplaren van de taak wordt uitgevoerd door de belangrijkste taak *alleen*. We noemen dit de **toepassingsopdracht** te onderscheiden van de opdracht coördinatie.

Gebruik de toepassingsopdracht voor MS-MPI-toepassingen uitvoeren van MPI-toepassingen met `mpiexec.exe`. Dit is bijvoorbeeld een toepassingsopdracht voor een oplossing met behulp van MS-MPI versie 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Omdat MS-MPI `mpiexec.exe` maakt gebruik van de `CCP_NODES` variabele standaard (Zie [omgevingsvariabelen](#environment-variables)) het voorbeeld van de toepassing vanaf de opdrachtregel bovenstaande sluit het.
>
>

## <a name="environment-variables"></a>Omgevingsvariabelen
Batch maakt verschillende [omgevingsvariabelen] [ msdn_env_var] specifiek voor taken met meerdere instanties op de rekenknooppunten die zijn toegewezen aan een taak meerdere exemplaren. Uw opdrachtregels coördinatie en toepassing kunt verwijzen naar van deze omgevingsvariabelen zoals u kunnen de scripts en programma's die ze worden uitgevoerd.

De volgende omgevingsvariabelen zijn gemaakt door de Batch-service voor gebruik door meerdere exemplaren taken:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Zie voor meer details over deze en de andere Batch knooppunt omgevingsvariabelen COMPUTE, waaronder de inhoud en zichtbaarheid, [Compute knooppunt omgevingsvariabelen][msdn_env_var].

> [!TIP]
> Voorbeeld van de Batch Linux MPI-code bevat een voorbeeld van hoe diverse van deze omgevingsvariabelen kunnen worden gebruikt. De [coördinatie cmd] [ coord_cmd_example] Bash script downloadt algemene toepassing en invoer van Azure-opslag-bestanden, kunt u een share Network File System (NFS) op het hoofdknooppunt en configureert u de andere knooppunten toegewezen aan de taak met meerdere instanties als NFS-clients.
>
>

## <a name="resource-files"></a>Bronbestanden
Er zijn twee sets van bronbestanden voor taken met meerdere instanties: **algemene bronbestanden** die *alle* taken downloaden (zowel de primaire en subtaken), en de **bronbestanden** opgegeven voor meerdere exemplaren van de taak zelf, die *alleen de primaire* downloads van de taak.

U kunt een of meer opgeven **algemene bronbestanden** in de instellingen voor meerdere instanties voor een taak. Deze algemene resource-bestanden worden gedownload van [Azure Storage](../storage/common/storage-introduction.md) in elk knooppunt **taak gedeelde map** door de primaire server en alle subtaken. U kunt de gedeelde taakmap openen vanaf de toepassing en coördinatie opdrachtregels met behulp van de `AZ_BATCH_TASK_SHARED_DIR` omgevingsvariabele. De `AZ_BATCH_TASK_SHARED_DIR` pad is identiek zijn op elk knooppunt dat is toegewezen aan de taak met meerdere instanties, dus kunt u een opdracht één coördinatie tussen de primaire en alle subtaken delen. Batch geen 'gedeeld' de map in een zin externe toegang, maar u kunt gebruiken als een koppeling of punt zoals eerder beschreven in de tip op omgevingsvariabelen delen.

Bronbestanden die u opgeeft voor de taak met meerdere instanties zelf worden gedownload naar de werkmap van de taak, `AZ_BATCH_TASK_WORKING_DIR`, standaard. Zoals gezegd, in tegenstelling tot de bronbestanden voor algemene, downloadt alleen de belangrijkste taak resource-bestanden opgegeven voor de taak met meerdere instanties zelf.

> [!IMPORTANT]
> Gebruik altijd de omgevingsvariabelen `AZ_BATCH_TASK_SHARED_DIR` en `AZ_BATCH_TASK_WORKING_DIR` om te verwijzen naar deze mappen in uw opdrachtregels. Probeer niet de paden handmatig maken.
>
>

## <a name="task-lifetime"></a>Taak levensduur
De levensduur van de belangrijkste taak bepaalt de levensduur van de taak voor volledige meerdere exemplaren. Wanneer de primaire wordt afgesloten, worden alle subtaken beëindigd. De afsluitcode van de primaire is de afsluitcode van de taak en wordt daarom gebruikt om te bepalen het slagen of mislukken van de taak voor opnieuw proberen doeleinden.

Als een van de subtaken mislukt, afgesloten met een niet-nul-retourcode, bijvoorbeeld de volledige meerdere exemplaren taak mislukt. De taak met meerdere instanties vervolgens wordt beëindigd en opnieuw worden geprobeerd tot de limiet voor opnieuw proberen.

Wanneer u een taak met meerdere instanties verwijdert, worden ook de primaire server en alle subtaken verwijderd door de Batch-service. Alle mappen een subtaak en hun bestanden worden verwijderd uit de rekenknooppunten, net als voor een taak.

[TaskConstraints] [ net_taskconstraints] voor een taak met meerdere instanties, zoals de [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], en [RetentionTime] [ net_taskconstraint_retention] -eigenschappen, zoals ze zijn voor een standaardtaak en gelden voor de primaire server en alle subtaken worden gehonoreerd. Echter, als u wijzigt de [RetentionTime] [ net_taskconstraint_retention] eigenschap na het toevoegen van de taak met meerdere instanties voor de job deze wijziging wordt alleen toegepast op de belangrijkste taak. Alle subtaken blijven gebruiken van de oorspronkelijke [RetentionTime][net_taskconstraint_retention].

Een rekenknooppunt lijst met recente taken komt overeen met de id van een subtaak als de recente taak deel van een taak meerdere exemplaren uitmaakte.

## <a name="obtain-information-about-subtasks"></a>Verzamel informatie over subtaken
Aanroepen om informatie te verkrijgen op subtaken met behulp van de Batch .NET-bibliotheek, de [CloudTask.ListSubtasks] [ net_task_listsubtasks] methode. Deze methode retourneert informatie over alle subtaken en informatie over het rekenknooppunt die de taken uitgevoerd. Met deze informatie kunt u bepalen de hoofdmap van elke subtaak, de pool-id, de huidige status, afsluitcode en meer. U kunt deze informatie gebruiken in combinatie met de [PoolOperations.GetNodeFile] [ poolops_getnodefile] methode van de subtaak-bestanden te downloaden. Houd er rekening mee dat deze methode heeft geen informatie geretourneerd voor de belangrijkste taak (id 0).

> [!NOTE]
> Tenzij anders vermeld, Batch .NET-methoden die worden uitgevoerd voor meerdere exemplaren [CloudTask] [ net_task] zelf toepassen *alleen* voor de primaire taak. Bijvoorbeeld, als u aanroept de [CloudTask.ListNodeFiles] [ net_task_listnodefiles] methode op een taak met meerdere instanties, alleen de belangrijkste taak bestanden worden geretourneerd.
>
>

Het volgende codefragment laat zien hoe subtaak informatie verkrijgen, evenals bestandsinhoud aanvragen bij de knooppunten waarop ze worden uitgevoerd.

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
De [MultiInstanceTasks] [ github_mpi] codevoorbeeld op GitHub laat zien hoe een taak met meerdere instanties gebruiken om uit te voeren een [MS MPI] [ msmpi_msdn] toepassing op Batch-rekenknooppunten. Volg de stappen in [voorbereiding](#preparation) en [uitvoering](#execution) om uit te voeren van het voorbeeld.

### <a name="preparation"></a>Voorbereiding
1. De eerste twee stappen in [compileren en uitvoeren van een eenvoudig MS MPI-programma][msmpi_howto]. Dit voldoet aan de vereisten voor de volgende stap.
2. Bouwen een *Release* versie van de [MPIHelloWorld] [ helloworld_proj] MPI voorbeeldcode. Dit is het programma dat door de taak met meerdere instanties op rekenknooppunten worden uitgevoerd.
3. Maak een zip-bestand met `MPIHelloWorld.exe` (die u hebt gebouwd stap 2) en `MSMpiSetup.exe` (die u hebt gedownload stap 1). U hebt dit zipbestand uploaden als een toepassingspakket in de volgende stap.
4. Gebruik de [Azure-portal] [ portal] voor het maken van een Batch [toepassing](batch-application-packages.md) 'MPIHelloWorld' genoemd en geef het zip-bestand dat u in de vorige stap hebt gemaakt als "1.0" versie van de toepassingspakket. Zie [uploaden en beheren van toepassingen](batch-application-packages.md#upload-and-manage-applications) voor meer informatie.

> [!TIP]
> Bouwen een *Release* versie van `MPIHelloWorld.exe` zodat u hoeft extra afhankelijkheden zijn (bijvoorbeeld `msvcp140d.dll` of `vcruntime140d.dll`) in het toepassingspakket.
>
>

### <a name="execution"></a>Uitvoering
1. Download de [azure-batch-samples] [ github_samples_zip] vanuit GitHub.
2. Open de MultiInstanceTasks **oplossing** in Visual Studio 2015 of hoger. De `MultiInstanceTasks.sln` oplossingsbestand bevindt zich in:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Voer uw referenties in Batch- en Storage-account in `AccountSettings.settings` in de **Microsoft.Azure.Batch.Samples.Common** project.
4. **Bouwen en uitvoeren van** de MultiInstanceTasks oplossing voor het uitvoeren van de MPI-voorbeeldtoepassing op rekenknooppunten in een Batch-pool.
5. *Optionele*: Gebruik de [Azure-portal] [ portal] of [BatchLabs] [ batch_labs] om te onderzoeken van de voorbeeld-pool, Jobs en taak (' MultiInstanceSamplePool', 'MultiInstanceSampleJob', "MultiInstanceSampleTask") voordat u de resources verwijderen.

> [!TIP]
> U kunt downloaden [Visual Studio Community] [ visual_studio] gratis als u Visual Studio niet hebt.
>
>

De uitvoer van `MultiInstanceTasks.exe` is vergelijkbaar met het volgende:

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
* De Microsoft HPC & Azure Batch-Team blog besproken [MPI-ondersteuning voor Linux op Azure Batch][blog_mpi_linux], en bevat informatie over het gebruik van [OpenFOAM] [ openfoam] met Batch. Vindt u voorbeelden van Python-code voor de [OpenFOAM voorbeeld op GitHub][github_mpi].
* Meer informatie over hoe [maken van groepen van Linux-rekenknooppunten](batch-linux-nodes.md) voor gebruik in uw Azure Batch MPI-oplossingen.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchLabs/
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
