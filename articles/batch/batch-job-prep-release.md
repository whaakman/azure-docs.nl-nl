---
title: Als u wilt voorbereiden, taken en volledige taken op rekenknooppunten - Azure Batch-taken maken | Microsoft Docs
description: Gebruik van taakniveau Voorbereidingstaken om te beperken van de overdracht van gegevens naar Azure Batch-rekenknooppunten en vrijgevingstaken om op te schonen knooppunt op de taak is voltooid.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 517ac0f612b9e5fc5909a7f0fe2ce088c9b367d9
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548694"
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>Taak uitvoeren-jobvoorbereidingstaken en jobvrijgevingstaken op Batch-rekenknooppunten

 Een Azure Batch-taak moet vaak een vorm van setup voordat de taken worden uitgevoerd en onderhoud na taak wanneer de taken zijn voltooid. Mogelijk moet u algemene taak invoergegevens downloaden naar uw rekenknooppunten of de taak uitvoergegevens uploaden naar Azure Storage nadat de taak is voltooid. U kunt **jobvoorbereidingstaken** en **taak release** taken uit te voeren van deze bewerkingen.

## <a name="what-are-job-preparation-and-release-tasks"></a>Wat zijn de taakvoorbereidingstaak en vrijgevingstaken?
Voordat een job taken worden uitgevoerd, wordt de jobvoorbereidingstaak wordt uitgevoerd op alle rekenknooppunten die zijn gepland voor uitvoering van ten minste één taak. Zodra de taak is voltooid, wordt de jobvrijgevingstaak uitgevoerd op elk knooppunt in de pool dat ten minste één taak uitgevoerd. Net als bij normale Batch-taken, kunt u een opdrachtregel worden aangeroepen wanneer een voorbereidings- of versie van een taak wordt uitgevoerd.

Jobvoorbereidingstaken en release bieden vertrouwde Batch-taakfuncties, zoals het downloaden van bestand ([resourcebestanden][net_job_prep_resourcefiles]), met verhoogde bevoegdheden voor uitvoering, aangepaste omgevingsvariabelen, maximale uitvoeringsduur, probeer het opnieuw aantal en bewaartijd voor bestanden.

In de volgende gedeelten leert u hoe u gebruik van de [JobPreparationTask] [ net_job_prep] en [JobReleaseTask] [ net_job_release] klassen gevonden in de [ Batch .NET] [ api_net] bibliotheek.

> [!TIP]
> Jobvoorbereidingstaken en jobvrijgevingstaken taken zijn met name nuttig in omgevingen met 'gedeelde groep', waarin een pool van rekenknooppunten zich blijft voordoen tussen de taak wordt uitgevoerd en wordt gebruikt door vele jobs.
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Bij gebruik van de taakvoorbereidingstaak en vrijgevingstaken
Jobvoorbereidings- en jobvrijgevingstaken zijn zeer geschikt voor de volgende situaties:

**Algemene taakgegevens downloaden**

Batch-taken vereisen vaak een gemeenschappelijke set gegevens als invoer voor de taken van de job. In de dagelijkse risico analysis berekeningen is marketinggegevens bijvoorbeeld specifieke, nog gebruikelijk voor alle taken in de taak. Deze marktgegevens, vaak verschillende gigabyte groot, moet worden gedownload op elk knooppunt slechts één keer zodat elke taak die wordt uitgevoerd op het knooppunt kan worden gebruikt. Gebruik een **jobvoorbereidingstaak** voor het downloaden van deze gegevens aan elk knooppunt voordat de uitvoering van de taak's andere taken.

**Verwijderen van taken kunt uitvoeren**

In een omgeving 'gedeelde groep', waar van een pool-compute-knooppunten niet uit bedrijf genomen tussen taken zijn, moet u mogelijk verwijderen van taakgegevens wordt uitgevoerd. Mogelijk moet u beschikbare schijfruimte op de knooppunten of voldoen aan het beveiligingsbeleid van uw organisatie. Gebruik een **jobvrijgevingstaak** om gegevens die is gedownload door een jobvoorbereidingstaak of gegenereerd tijdens de uitvoering van de taak te verwijderen.

**Logboekbehoud**

Mogelijk wilt u Bewaar een kopie van de logboekbestanden die uw taken wordt gegenereerd, of misschien crashdump-bestanden die kunnen worden gegenereerd door de mislukte toepassingen. Gebruik een **jobvrijgevingstaak** in dergelijke gevallen voor het comprimeren en te uploaden van deze gegevens naar een [Azure Storage] [ azure_storage] account.

> [!TIP]
> Een andere manier om vast te leggen van Logboeken en andere taak en de taak uitvoer gegevens is het gebruik van de [Azure Batch File Conventions](batch-task-output.md) bibliotheek.
> 
> 

## <a name="job-preparation-task"></a>Taakvoorbereidingstaak
Batch wordt de jobvoorbereidingstaak voordat de uitvoering van de taken van een job uitgevoerd op elk knooppunt dat een taak is gepland. Standaard wacht de Batch-service voor de jobvoorbereidingstaak om te worden voltooid voordat de taken die zijn gepland om uit te voeren op het knooppunt worden uitgevoerd. U kunt echter de service niet te wachten. Als het knooppunt opnieuw wordt opgestart, wordt de jobvoorbereidingstaak wordt opnieuw uitgevoerd, maar u kunt dit gedrag ook uitschakelen.

De jobvoorbereidingstaak wordt uitgevoerd alleen op knooppunten die zijn gepland voor een taak uitvoert. Dit voorkomt dat de onnodige uitvoering van een voorbereidingstaak als een knooppunt niet aan een taak toegewezen is. Dit kan gebeuren wanneer het aantal taken voor een job kleiner dan het aantal knooppunten in een pool is. Dit geldt ook wanneer [uitvoering van gelijktijdige taken](batch-parallel-node-tasks.md) is ingeschakeld, waardoor sommige knooppunten niet-actieve als het aantal voor de taak is lager dan de totale mogelijk gelijktijdige taken. Door de jobvoorbereidingstaak wordt niet wordt uitgevoerd op niet-actieve knooppunten, kunt u minder geld te besteden aan kosten voor gegevensoverdracht.

> [!NOTE]
> [JobPreparationTask] [ net_job_prep_cloudjob] wijkt af van [CloudPool.StartTask] [ pool_starttask] in die JobPreparationTask aan het begin van elke taak wordt uitgevoerd terwijl StartTask wordt uitgevoerd alleen wanneer een rekenknooppunt eerst lid wordt van een groep of opnieuw wordt gestart.
> 
> 

## <a name="job-release-task"></a>Taakvrijgevingstaak
Wanneer een taak is gemarkeerd als voltooid, wordt de jobvrijgevingstaak uitgevoerd op elk knooppunt in de pool dat ten minste één taak uitgevoerd. U kunt een taak markeren als voltooid door het uitgeven van een aanvraag beëindigen. De Batch-service wordt de taakstatus van de vervolgens ingesteld op *beëindigen*, eventuele actieve of actieve taken die zijn gekoppeld aan de taak wordt beëindigd en wordt de jobvrijgevingstaak uitgevoerd. De taak wordt verplaatst naar de *voltooid* staat.

> [!NOTE]
> Taak verwijderen worden ook de jobvrijgevingstaak uitgevoerd. Echter, als een taak is al beëindigd, de release-taak wordt niet uitgevoerd een tweede keer als de taak wordt later verwijderd.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Job prep en de vrijgave van taken met Batch .NET
Voor het gebruik van een jobvoorbereidingstaak toewijzen een [JobPreparationTask] [ net_job_prep] object aan van de taak [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] eigenschap. Op deze manier worden geïnitialiseerd een [JobReleaseTask] [ net_job_release] en deze toewijzen aan van de taak [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] eigenschap van de taak instellen Release-taak.

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

Zoals eerder vermeld, wordt de release-taak wordt uitgevoerd wanneer een taak wordt beëindigd of verwijderd. Beëindigen van een taak met [JobOperations.TerminateJobAsync][net_job_terminate]. Een taak verwijderen met [JobOperations.DeleteJobAsync][net_job_delete]. U doorgaans beëindigen of een taak te verwijderen wanneer de taken zijn voltooid of wanneer een time-out die u hebt gedefinieerd is bereikt.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Codevoorbeeld op GitHub
Als u wilt zien taakvoorbereidingstaak en vrijgevingstaken in actie, bekijk de [JobPrepRelease] [ job_prep_release_sample] voorbeeldproject op GitHub. Deze consoletoepassing doet het volgende:

1. Hiermee maakt een pool met twee knooppunten.
2. Maakt een taak met de taakvoorbereidingstaak, versie en standard taken.
3. De jobvoorbereidingstaak, schrijft u eerst het knooppunt-ID naar een tekstbestand in van een knooppunt 'gedeeld' directory uitgevoerd.
4. Een taak uitvoert op elk knooppunt dat de taak-ID wordt geschreven naar het tekstbestand met dezelfde.
5. Wanneer alle taken zijn voltooid (of de time-out is bereikt), wordt u de inhoud van het tekstbestand van elk knooppunt in de console afgedrukt.
6. Wanneer de taak is voltooid, voert u de jobvrijgevingstaak als u wilt verwijderen van het bestand in het knooppunt.
7. De afsluitcodes van de taak jobvoorbereidingstaken en jobvrijgevingstaken taken voor elk knooppunt waarop ze uitgevoerd af te drukken.
8. De uitvoering van de pauzes waarmee de bevestiging van de taak en/of groep verwijderen.

Uitvoer van de voorbeeldtoepassing is vergelijkbaar met het volgende:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 nodes
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
> Vanwege de variabele maken en beginnen met de tijd van de knooppunten in een nieuwe pool (sommige knooppunten zijn gereed voor taken vóór andere), ziet u andere uitvoer. Specifiek, omdat de taken zijn voltooid snel, kan een van de knooppunten van de pool alles uitvoeren van de taken van. Als dit gebeurt, ziet u dat de taak voorbereiden en jobvrijgevingstaken bestaan niet voor het knooppunt dat er geen taken uitgevoerd.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Inspecteer jobvoorbereidings- en release-taken in Azure portal
Wanneer u de voorbeeldtoepassing uitvoert, kunt u de [Azure-portal] [ portal] weer te geven van de eigenschappen van de taak en de bijbehorende taken of zelfs downloaden van het gedeelde tekstbestand dat door de taken van de taak wordt gewijzigd.

De schermafbeelding hieronder toont de **voorbereiding taken blade** in Azure portal nadat een uitvoering van de voorbeeldtoepassing. Navigeer naar de *JobPrepReleaseSampleJob* eigenschappen nadat uw taken hebt voltooid (maar voordat de job en de pool wordt verwijderd) en klikt u op **Voorbereidingstaken** of **Vrijgevingstaken**om de eigenschappen weer te geven.

![Eigenschappen van de taak voorbereiden in Azure portal][1]

## <a name="next-steps"></a>Volgende stappen
### <a name="application-packages"></a>Toepassingspakketten
Naast de jobvoorbereidingstaak, kunt u ook gebruiken de [toepassingspakketten](batch-application-packages.md) functie van Batch rekenknooppunten voorbereiden voor uitvoering van de taak. Deze functie is vooral nuttig voor het implementeren van toepassingen die niet met een installatieprogramma, toepassingen die veel (100 en hoger) bestanden bevatten of toepassingen waarvoor strikte versiebeheer nodig hebben.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en faseert gegevens
Dit MSDN-forumbericht bevat een overzicht van de verschillende methoden voor het voorbereiden van de knooppunten voor het uitvoeren van taken:

[Installeren van toepassingen en faseert gegevens op Batch-rekenknooppunten][forum_post]

Geschreven door een van de Azure Batch-teamleden, hierin verschillende technieken die u gebruiken kunt om toepassingen en gegevens rekenknooppunten te implementeren.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
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
