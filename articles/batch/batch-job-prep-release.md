---
title: Maak taken om voor te bereiden, taken en volledige taken op rekenknooppunten - Azure Batch | Microsoft Docs
description: Gebruik op jobniveau systeemvoorbereidingstaken om te beperken van overdracht van gegevens naar Azure Batch-rekenknooppunten en release van taken voor het opruimen van knooppunt bij Taakvoltooiing.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aecce83b4d4444f2651f48475b596fa76cb5f44a
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>Taak uitvoeren voorbereidings- en jobvrijgevingstaken voor Batch-rekenknooppunten

 Een Azure Batch-taak is een vorm van setup vaak vereist voordat de bijbehorende taken worden uitgevoerd en onderhoud na taak wanneer de taken zijn voltooid. Mogelijk moet u algemene taak invoergegevens downloaden naar uw rekenknooppunten, of de taak uitvoergegevens uploaden naar Azure Storage nadat de taak is voltooid. U kunt **taak voorbereiding** en **taak release** taken voor het uitvoeren van deze bewerkingen.

## <a name="what-are-job-preparation-and-release-tasks"></a>Wat zijn taakvoorbereidingstaak en release van taken?
Voordat een job taken worden uitgevoerd, is de jobvoorbereidingstaak wordt uitgevoerd op alle rekenknooppunten die ten minste één taak uitgevoerd. Zodra de taak is voltooid, wordt de jobvrijgevingstaak uitgevoerd op elk knooppunt in de pool dat ten minste één taak uitgevoerd. Net als bij normale Batch-taken, kunt u een opdrachtregel moet worden aangeroepen wanneer een taak voorbereidings- of release-taak wordt uitgevoerd.

Jobvoorbereidingstaken en jobvrijgevingstaken taken bieden vertrouwde Batch taakfuncties zoals bestand downloaden ([bronbestanden][net_job_prep_resourcefiles]), verhoogde uitvoering, aangepaste omgevingsvariabelen, maximale uitvoering duur, probeer het opnieuw aantal en bewaartijd voor bestanden.

In de volgende gedeelten leert u hoe u de [JobPreparationTask] [ net_job_prep] en [JobReleaseTask] [ net_job_release] klassen gevonden in de [ Batch .NET] [ api_net] bibliotheek.

> [!TIP]
> Jobvoorbereidingstaken en jobvrijgevingstaken taken zijn vooral handig in omgevingen 'van toepassingen wordt gedeeld', waarop een pool van rekenknooppunten zich blijft voordoen tussen de taak wordt uitgevoerd en wordt gebruikt door veel taken.
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Bij het gebruik van de taakvoorbereidingstaak en de vrijgave van taken
Taakvoorbereidings- en jobvrijgevingstaken zijn geschikt voor de volgende situaties:

**Algemene taakgegevens downloaden**

Batchtaken vereisen vaak een gemeenschappelijke set gegevens als invoer voor de taken van de taak. In de dagelijkse risico analysis berekeningen is marktgegevens bijvoorbeeld specifieke, nog gemeenschappelijk zijn voor alle taken in de taak. Deze marktgegevens, vaak verscheidene gigabytes groot, moet worden gedownload naar elk rekenknooppunt slechts één keer zodat elke taak die wordt uitgevoerd op het knooppunt kan worden gebruikt. Gebruik een **jobvoorbereidingstaak** te downloaden van deze gegevens voor elk knooppunt voordat de uitvoering van de taak's andere taken.

**Verwijderen van de taak en uitvoer**

In een omgeving 'van toepassingen wordt gedeeld', waarbij een groep rekenknooppunten zich niet buiten gebruik gestelde tussen taken, moet u wellicht verwijderen taakgegevens tussen wordt uitgevoerd. Mogelijk moet u beschikbare schijfruimte op de knooppunten of voldoen aan het beveiligingsbeleid van uw organisatie. Gebruik een **jobvrijgevingstaak** om gegevens die zijn gedownload door een jobvoorbereidingstaak of gegenereerd tijdens de uitvoering van de taak te verwijderen.

**Logboek bewaren**

Mogelijk wilt een kopie van de logboekbestanden die uw taken wordt gegenereerd of mogelijk crashdumpbestanden dat kan worden gegenereerd door de mislukte toepassingen. Gebruik een **jobvrijgevingstaak** in dergelijke gevallen comprimeren en het uploaden van deze gegevens naar een [Azure Storage] [ azure_storage] account.

> [!TIP]
> Een andere manier om vast te leggen logboeken en andere taak en uitvoer gegevens is het gebruik van de [Azure Batch-bestand conventies](batch-task-output.md) bibliotheek.
> 
> 

## <a name="job-preparation-task"></a>Jobvoorbereidingstaak
Batch: de jobvoorbereidingstaak voordat de uitvoering van een job taken uitvoeren op elk rekenknooppunt die is gepland voor een taak uitvoeren. Standaard wacht de Batch-service voor de jobvoorbereidingstaak om te worden voltooid voordat de taken die moeten worden uitgevoerd op het knooppunt wordt uitgevoerd. U kunt echter de service niet te wachten. Als het knooppunt opnieuw wordt opgestart, wordt de jobvoorbereidingstaak wordt opnieuw uitgevoerd, maar u kunt dit gedrag ook uitschakelen.

De jobvoorbereidingstaak wordt alleen uitgevoerd op knooppunten die zijn gepland voor een taak uitvoeren. Dit voorkomt dat de onnodige uitvoering van een jobvoorbereidingstaak als een knooppunt niet aan een taak toegewezen is. Dit kan gebeuren als het aantal taken voor een job kleiner dan het aantal knooppunten in een pool is. Dit geldt ook wanneer [uitvoering van gelijktijdige taken](batch-parallel-node-tasks.md) is ingeschakeld, waardoor sommige knooppunten inactief als het aantal taken lager is dan de totale mogelijke gelijktijdige taken. Door de jobvoorbereidingstaak niet wordt uitgevoerd op niet-actieve knooppunten, kunt u minder geld besteden aan gegevensoverdracht kosten.

> [!NOTE]
> [JobPreparationTask] [ net_job_prep_cloudjob] verschilt van [CloudPool.StartTask] [ pool_starttask] in dat JobPreparationTask aan het begin van elke taak wordt uitgevoerd terwijl StartTask voert alleen wanneer een rekenknooppunt eerst lid wordt van een groep of opnieuw wordt opgestart.
> 
> 

## <a name="job-release-task"></a>Jobvrijgevingstaak
Zodra een taak is gemarkeerd als voltooid, wordt de jobvrijgevingstaak uitgevoerd op elk knooppunt in de pool dat ten minste één taak uitgevoerd. U kunt een taak markeren als voltooid door uitgifte van een aanvraag beëindigen. De Batch-service wordt de taakstatus van de vervolgens ingesteld op *beëindigd*, alle actieve of actieve taken die zijn gekoppeld aan de taak wordt beëindigd en wordt de jobvrijgevingstaak uitgevoerd. De taak wordt verplaatst naar de *voltooid* status.

> [!NOTE]
> Verwijderen van een taak worden ook de jobvrijgevingstaak uitgevoerd. Echter, als er al een taak is beëindigd, de release-taak wordt niet uitgevoerd een tweede keer als de taak wordt later verwijderd.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Taak prep en de vrijgave van taken met Batch .NET
Voor het gebruik van een jobvoorbereidingstaak toewijzen een [JobPreparationTask] [ net_job_prep] object aan uw project [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] eigenschap. Op deze manier initialiseren een [JobReleaseTask] [ net_job_release] en toe te wijzen aan uw project [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] eigenschap van de taak ingesteld Release-taak.

In dit codefragment `myBatchClient` is een exemplaar van [BatchClient][net_batch_client], en `myPool` is een bestaande pool in de Batch-account.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobReleaseTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Zoals eerder vermeld, wordt de release-taak wordt uitgevoerd wanneer een taak is beëindigd of is verwijderd. Beëindigen van een taak met [JobOperations.TerminateJobAsync][net_job_terminate]. Verwijderen van een taak met [JobOperations.DeleteJobAsync][net_job_delete]. U doorgaans beëindigen of verwijderen van een taak wanneer de taken zijn voltooid of wanneer een time-out die u hebt gedefinieerd is bereikt.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Voorbeeld van code op GitHub
Als u wilt zien taakvoorbereidingstaak en taken in actie release, bekijk de [JobPrepRelease] [ job_prep_release_sample] voorbeeldproject op GitHub. Deze consoletoepassing doet het volgende:

1. Maakt een groep met twee knooppunten voor 'kleine'.
2. Maakt een taak met taak voorbereidings-, release- en standaardtaken.
3. De jobvoorbereidingstaak schrijft eerst de knooppunt-ID naar een tekstbestand in een knooppunt 'gedeeld' directory uitgevoerd.
4. Voert een taak op elk knooppunt dat de taak-ID naar hetzelfde tekstbestand schrijft.
5. Zodra alle taken zijn voltooid (of de time-out is bereikt), wordt de inhoud van elk knooppunt tekstbestand dat aan de console.
6. Wanneer de taak is voltooid, voert u de jobvrijgevingstaak om het bestand verwijderen uit het knooppunt.
7. Afdrukken van de afsluitcodes van de taak jobvoorbereidingstaken en jobvrijgevingstaken taken voor elk knooppunt waarop ze worden uitgevoerd.
8. De uitvoering van de onderbroken waarmee de bevestiging van de taak en/of de groep verwijderen.

De uitvoer van de voorbeeldtoepassing is vergelijkbaar met het volgende:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

> [!NOTE]
> Als gevolg van de variabele maken en start tijd van de knooppunten in een nieuwe pool (sommige knooppunten zijn gereed voor taken vóór andere), ziet u andere uitvoer. In het bijzonder omdat de taken snel kunnen worden uitgevoerd, kan een van de groep knooppunten uitvoeren alle taken van de taak. Als dit gebeurt, ziet u dat de taak voorbereiden en release taken bestaan niet voor het knooppunt dat geen taken uitgevoerd.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Inspecteer de taakvoorbereidingstaak en release-taken in de Azure portal
Wanneer u de voorbeeldtoepassing uitvoert, kunt u de [Azure-portal] [ portal] voor het weergeven van de eigenschappen van de taak en de bijbehorende taken of zelfs het gedeelde tekstbestand dat door de taken van de taak wordt gewijzigd te downloaden.

De schermafbeelding hieronder bevat de **voorbereiding taken blade** in de Azure portal na een uitvoering van de voorbeeldtoepassing. Navigeer naar de *JobPrepReleaseSampleJob* eigenschappen nadat uw taken hebt voltooid (maar voordat de job en de pool wordt verwijderd) en klik op **systeemvoorbereidingstaken** of **takenRelease**eigenschappen weergeven.

![Voorbereiding taakeigenschappen in Azure-portal][1]

## <a name="next-steps"></a>Volgende stappen
### <a name="application-packages"></a>Toepassingspakketten
Naast de jobvoorbereidingstaak ook kunt u de [toepassingspakketten](batch-application-packages.md) functie van Batch rekenknooppunten voorbereiden voor uitvoering van de taak. Deze functie is vooral handig voor het implementeren van toepassingen die niet met een installatieprogramma, toepassingen die veel (100 en hoger) bestanden bevatten of toepassingen waarvoor strikte versiebeheer nodig hebt.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en gegevens voor fasering
Dit bericht MSDN-forum biedt een overzicht van de verschillende methoden voor het voorbereiden van uw knooppunten om taken uit te voeren:

[Installeren van toepassingen en staging-gegevens op de Batch-rekenknooppunten][forum_post]

Geschreven door een van de Azure Batch-teamleden, besproken verschillende technieken die u gebruiken kunt voor het implementeren van toepassingen en gegevens rekenknooppunten.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
