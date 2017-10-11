---
title: "Lijst met efficiënt query's, Azure Batch ontwerpen | Microsoft Docs"
description: De prestaties verbeteren door het filteren van uw query's bij het aanvragen van informatie over Batch-resources zoals pools, jobs, taken en rekenknooppunten.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 08/02/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a80b207f591bd888d4749287527013c5e554fb6e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a>Query's op de lijst met Batch-resources efficiënt maken

Hier leert u hoe u de prestaties van uw Azure Batch-toepassingen door te verminderen de hoeveelheid gegevens die door de service worden geretourneerd wanneer u query jobs, taken en met rekenknooppunten verhoogt de [Batch .NET] [ api_net] bibliotheek.

Bijna alle Batch-toepassingen hoeft uit te voeren van een type controle of een andere bewerking vaak query van de Batch-service met regelmatige tussenpozen. Om te bepalen of er in een taak resterende taken in de wachtrij zijn, kunt u gegevens moet ophalen voor elke taak in de taak. Om te bepalen van de status van knooppunten in uw pool, moet u gegevens ophalen op elk knooppunt in de groep. In dit artikel wordt uitgelegd hoe u dergelijke query's uitvoeren in de meest efficiënte manier.

> [!NOTE]
> De Batch-service biedt speciale API-ondersteuning voor de gangbare scenario van taken in een taak worden geteld. In plaats van een lijst met query om deze, roept u de [ophalen taak telt] [ rest_get_task_counts] bewerking. Aantallen voor GET-taak geeft aan hoeveel taken in behandeling is, zijn uitgevoerd of voltooid en hoeveel taken hebt voltooid of mislukt. Get-taak telt is efficiënter dan een lijst met query. Zie voor meer informatie [aantal taken voor een taak op status (Preview)](batch-get-task-counts.md). 
>
> De bewerking taak telt ophalen is niet beschikbaar in Batch-versies eerder dan 2017-06-01.5.1. Als u van een oudere versie van de service gebruikmaakt, gebruikt u een lijst met query voor het tellen van taken in een job in plaats daarvan.
>
> 

## <a name="meet-the-detaillevel"></a>Voldoen aan de DetailLevel
In een productie-Batch-toepassing kunnen entiteiten, zoals jobs, taken en rekenknooppunten in duizendtallen nummeren. Wanneer u informatie over deze bronnen aanvraagt, moet een grote hoeveelheid gegevens 'kruislingse de kabel' van de Batch-service voor uw toepassing op elke query. Door te beperken het aantal items en type informatie dat wordt geretourneerd door een query, verhoogt u de snelheid van uw query's en daarom op de prestaties van uw toepassing.

Dit [Batch .NET] [ api_net] API code codefragment lijsten *elke* taak die is gekoppeld aan een taak, samen met *alle* van de eigenschappen van elke taak:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

U kunt echter een veel efficiënter lijstquery uitvoeren door het toepassen van een 'detailniveau' aan de query. Dit doet u door het leveren van een [ODATADetailLevel] [ odata] object toe aan de [JobOperations.ListTasks] [ net_list_tasks] methode. In dit fragment retourneert alleen de ID, de opdrachtregel en de compute knooppunt informatie eigenschappen van voltooide taken:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

In dit voorbeeldscenario als er duizenden taken in de taak, de resultaten van de tweede query wordt doorgaans geretourneerd veel sneller dan de eerste. Meer informatie over het gebruik van ODATADetailLevel wanneer u objecten met de Batch .NET API is opgenomen [hieronder](#efficient-querying-in-batch-net).

> [!IMPORTANT]
> Ten zeerste aangeraden dat u *altijd* opgeven van een object ODATADetailLevel op uw lijst .NET API aanroepen van maximale efficiëntie en prestaties van uw toepassing. Door te geven een detailniveau, kunt u helpen te verlagen reactietijden van de Batch-service, netwerkgebruik verbeteren en geheugengebruik minimaliseren door clienttoepassingen.
> 
> 

## <a name="filter-select-and-expand"></a>Filteren, selecteert en vouw
De [Batch .NET] [ api_net] en [Batch REST] [ api_rest] API's bieden de mogelijkheid om te beperken van zowel het aantal items dat wordt geretourneerd in een lijst, evenals de hoeveelheid gegevens die voor elke wordt geretourneerd. U dit doen door op te geven **filter**, **Selecteer**, en **Vouw tekenreeksen** bij het uitvoeren van de lijst met query's.

### <a name="filter"></a>Filteren
De filtertekenreeks is een expressie die vermindert het aantal items dat wordt geretourneerd. Bijvoorbeeld, lijst met actieve taken voor een taak of lijst alleen rekenknooppunten die gereed zijn om de taken uitvoeren.

* De filtertekenreeks bestaat uit een of meer expressies met een expressie die uit een eigenschapsnaam, een operator en een waarde bestaat. De eigenschappen die kunnen worden opgegeven zijn specifiek voor elk entiteitstype waarmee u een query uitvoeren, zoals de operators die worden ondersteund voor elke eigenschap zijn.
* Meerdere expressies kunnen worden gecombineerd met behulp van de logische operators `and` en `or`.
* In dit voorbeeld filteren tekenreekslijsten alleen de wordt uitgevoerd 'weergeven' taken: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Selecteer
De select-tekenreeks beperkt de waarden van de eigenschappen die worden geretourneerd voor elk item. U geeft een lijst met namen van eigenschappen en alleen de eigenschapswaarden van deze voor de items in de queryresultaten worden geretourneerd.

* De select-tekenreeks bestaat uit een door komma's gescheiden lijst met namen van eigenschappen. U kunt opgeven dat geen van de eigenschappen voor het entiteitstype die u wilt zoeken.
* In dit voorbeeld selecteert tekenreeks dat slechts drie eigenschapswaarden voor elke taak moeten worden geretourneerd: `id, state, stateTransitionTime`.

### <a name="expand"></a>Uitvouwen
De tekenreeks uit te breiden vermindert het aantal API-aanroepen die nodig zijn om bepaalde informatie te verkrijgen. Wanneer u een tekenreeks uit te breiden, meer informatie over elk item kan worden verkregen met één API-aanroep. In plaats van de eerste verkrijgen van de lijst met entiteiten, die vervolgens vraagt informatie voor elk item in de lijst met een tekenreeks uit te breiden kunt u dezelfde gegevens in één API-aanroep verkrijgen. Minder API-aanroepen betekent betere prestaties.

* Net als bij de optie tekenreeks, de expand-reeks bepaalt of bepaalde gegevens worden opgenomen in de lijst met queryresultaten.
* De tekenreeks uit te breiden wordt alleen ondersteund wanneer het wordt gebruikt in de lijst taken, taakschema's, taken en groepen. Op dit moment wordt alleen ondersteund statistische gegevens.
* Wanneer alle eigenschappen vereist zijn en er is geen tekenreeks select is opgegeven, de tekenreeks uit te breiden *moet* worden gebruikt voor statistische gegevens ophalen. Als een select-tekenreeks is gebruikt voor het verkrijgen van een subset van eigenschappen, klikt u vervolgens `stats` kunnen worden opgegeven in de select-tekenreeks en de tekenreeks uit te breiden niet hoeft te worden opgegeven.
* In dit voorbeeld Vouw tekenreeks geeft aan dat statistische gegevens voor elk item in de lijst moet worden geretourneerd: `stats`.

> [!NOTE]
> Bij het maken van een van de drie querytypen tekenreeks (filteren, selecteert en vouw), moet u ervoor zorgen dat de namen van eigenschappen en de aanvraag overeenkomen met die van hun collega's REST-API-element. Bijvoorbeeld, als u werkt met de .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) klasse, moet u **status** in plaats van **status**, zelfs als de eigenschap .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Zie de tabellen hieronder voor de eigenschaptoewijzingen tussen .NET en REST-API's.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Regels voor filteren, selecteert en breidt tekenreeksen
* Namen van eigenschappen in het filter, selecteert en breidt tekenreeksen moeten worden weergegeven als in de [Batch REST] [ api_rest] API--zelfs wanneer u [Batch .NET] [ api_net] of een van de andere Batch-SDK's.
* Alle namen van eigenschappen zijn hoofdlettergevoelig, maar eigenschapswaarden zijn niet hoofdlettergevoelig.
* Datum/tijd tekenreeksen kunnen twee verschillende indelingen, en moet worden voorafgegaan door `DateTime`.
  
  * Voorbeeld van W3C-DTF-indeling:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * Voorbeeld van RFC 1123 indeling:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Booleaanse tekenreeksen zijn `true` of `false`.
* Als een eigenschap is ongeldig of de operator is opgegeven, een `400 (Bad Request)` fout resulteert.

## <a name="efficient-querying-in-batch-net"></a>Efficiënter uitvoeren van query's in Batch .NET
Binnen de [Batch .NET] [ api_net] API, de [ODATADetailLevel] [ odata] klasse wordt gebruikt voor het verstrekken van filter, en selecteer tekenreeksen worden uitgebreid tot bewerkingen na opvragen. De klasse ODataDetailLevel heeft drie openbare eigenschappen die kunnen worden opgegeven in de constructor of rechtstreeks op het object is ingesteld. U geeft het ODataDetailLevel-object als parameter voor de verschillende bewerkingen na opvragen, zoals [ListPools][net_list_pools], [ListJobs][net_list_jobs], en [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[ FilterClause][odata_filter]: Beperk het aantal items dat wordt geretourneerd.
* [ODATADetailLevel][odata].[ SelectClause][odata_select]: Geef op welke eigenschapswaarden worden geretourneerd bij elk item.
* [ODATADetailLevel][odata].[ ExpandClause][odata_expand]: gegevens ophalen voor alle artikelen in één API-aanroep in plaats van afzonderlijke aanroepen voor elk item.

Het volgende codefragment gebruikt de Batch .NET API om efficiënt query de Batch-service voor de statistieken van een specifieke set met groepen. In dit scenario heeft de gebruiker Batch pools test- en productie. De id's van de test-toepassingen worden voorafgegaan door 'test' en de productiepool-id's worden voorafgegaan door 'prod'. In het codefragment *myBatchClient* is een goed geïnitialiseerd exemplaar van de [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) klasse.

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> Een exemplaar van [ODATADetailLevel] [ odata] die is geconfigureerd met selecteren en uitvouwen componenten kunnen ook worden doorgegeven aan de juiste Get-methoden, zoals [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), te beperken van de hoeveelheid gegevens die wordt geretourneerd.
> 
> 

## <a name="batch-rest-to-net-api-mappings"></a>Batch REST in .NET API toewijzingen
Namen van eigenschappen in het filter, selecteert en breidt tekenreeksen *moet* overeenstemming met de REST-API collega's, zowel in de naam en het geval is. De onderstaande tabellen bevatten toewijzingen tussen de collega .NET en REST-API's.

### <a name="mappings-for-filter-strings"></a>Toewijzingen voor tekenreeksen
* **.NET-lijst methoden**: elk van de .NET API-methoden in deze kolom accepteert een [ODATADetailLevel] [ odata] -object als parameter.
* **Aanvragen voor REST-lijst**: elke REST-API-pagina is gekoppeld aan in deze kolom bevat een tabel waarin de eigenschappen en bewerkingen die zijn toegestaan in *filter* tekenreeksen. U gebruikt deze eigenschapnamen en bewerkingen wanneer u samenstellen een [ODATADetailLevel.FilterClause] [ odata_filter] tekenreeks.

| Methoden voor .NET-lijst | Aanvragen voor REST-lijst |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[Lijst van de certificaten in een account][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[De bestanden die zijn gekoppeld aan een taak weergeven][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[De status van de taakvoorbereidingstaak en jobvrijgevingstaken voor een taak weergeven][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[Lijst van de taken in een account][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Lijst van de bestanden op een knooppunt][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[Lijst van de taken die zijn gekoppeld aan een taak][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Lijst van de taakschema's in een account][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[Lijst van de taken die zijn gekoppeld aan een jobplanning][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Lijst van de rekenknooppunten in een groep][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[De toepassingen in een account weergeven][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Toewijzingen voor Selecteer tekenreeksen
* **Batch .NET-typen**: typen Batch .NET API.
* **REST-API-entiteiten**: elke pagina in deze kolom bevat een of meer tabellen met de namen van de REST-API-eigenschappen voor het type. De namen van deze eigenschappen worden gebruikt wanneer u samenstellen *Selecteer* tekenreeksen. U gebruikt deze dezelfde eigenschapnamen als u een [ODATADetailLevel.SelectClause] [ odata_select] tekenreeks.

| Batch .NET-typen | REST-API-entiteiten |
| --- | --- |
| [Certificaat][net_cert] |[Informatie ophalen over een certificaat][rest_get_cert] |
| [CloudJob][net_job] |[Informatie over een taak ophalen][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[Informatie over het schema van een taak ophalen][rest_get_schedule] |
| [ComputeNode][net_node] |[Informatie ophalen over een knooppunt][rest_get_node] |
| [CloudPool][net_pool] |[Informatie ophalen over een groep][rest_get_pool] |
| [CloudTask][net_task] |[Informatie over een taak ophalen][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Voorbeeld: een filtertekenreeks maken
Wanneer u een filtertekenreeks voor samenstellen [ODATADetailLevel.FilterClause][odata_filter], raadpleegt u de bovenstaande tabel onder '-toewijzingen voor filtertekenreeksen' om te zoeken naar de REST-API-documentatiepagina die overeenkomt met de lijstbewerking die u wilt uitvoeren. U vindt de Filterbaar eigenschappen en hun ondersteunde operators in de eerste multirow tabel op die pagina. Als u wilt ophalen van alle taken waarvan afsluitcode niet nul was, bijvoorbeeld dit rij op [lijst van de taken die zijn gekoppeld aan een taak] [ rest_list_tasks] stelt de toepasselijke eigenschap tekenreeks en de toegestane operators:

| Eigenschap | Bewerkingen die zijn toegestaan | Type |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Daarom zou de filtertekenreeks voor het weergeven van alle taken met een andere waarde dan nul afsluitcode:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Voorbeeld: een select tekenreeks maken
Om samen te stellen [ODATADetailLevel.SelectClause][odata_select], raadpleegt u de bovenstaande tabel onder '-toewijzingen voor Selecteer tekenreeksen' en navigeer naar de pagina REST-API die overeenkomt met het type entiteit die u weergeeft. U vindt de selecteerbare eigenschappen en hun ondersteunde operators in de eerste multirow tabel op die pagina. Als u wilt dat alleen de ID en de opdrachtregel voor elke taak in een lijst ophalen, bijvoorbeeld, vindt u deze rijen in de tabel van toepassing op [informatie ophalen over een taak][rest_get_task]:

| Eigenschap | Type | Opmerkingen |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

Selecteer de string voor het opnemen van alleen de ID en de opdrachtregel voor elke vermelde taak zou vervolgens zijn:

`id, commandLine`

## <a name="code-samples"></a>Codevoorbeelden
### <a name="efficient-list-queries-code-sample"></a>Voorbeeld van code voor efficiënte lijst met query 's
Bekijk de [EfficientListQueries] [ efficient_query_sample] voorbeeldproject op GitHub om te zien hoe efficiënt lijst uitvoeren van query's kan invloed hebben op prestaties in een toepassing. Deze C#-consoletoepassing maakt en voegt u een groot aantal taken toe aan een job. Klik op deze manier meerdere aanroepen naar de [JobOperations.ListTasks] [ net_list_tasks] methode en geeft [ODATADetailLevel] [ odata] objecten die zijn geconfigureerd met verschillende eigenschapwaarden variëren van de hoeveelheid gegevens moeten worden geretourneerd. Het wordt ongeveer de volgende uitvoer gegenereerd:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Zoals u in de verstreken tijd, kunt u de reactietijden van de query aanzienlijk verlagen door het beperken van de eigenschappen en het aantal items dat wordt geretourneerd. U vindt deze en andere voorbeeldprojecten in de [azure-batch-samples] [ github_samples] opslagplaats op GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics-bibliotheek en code-voorbeeld
Naast het codevoorbeeld EfficientListQueries bovenstaande vindt u de [BatchMetrics] [ batch_metrics] project in de [azure-batch-samples] [ github_samples] GitHub-opslagplaats. Het voorbeeldproject BatchMetrics laat zien hoe efficiënt voortgang Azure Batch-taak met de Batch-API.

De [BatchMetrics] [ batch_metrics] voorbeeld bevat een bibliotheekproject voor .NET-klasse die u kunt opnemen in uw eigen projecten en een eenvoudig opdrachtregelprogramma uitoefenen en demonstreren van het gebruik van de bibliotheek.

De voorbeeldtoepassing in het project ziet u de volgende bewerkingen:

1. Specifieke kenmerken selecteren om te downloaden van alleen de eigenschappen die u nodig hebt
2. Filteren op status overgang keren om te kunnen downloaden van alleen wijzigingen sinds de laatste query

Bijvoorbeeld, verschijnt de volgende methode in de bibliotheek BatchMetrics. Deze retourneert een ODATADetailLevel die geeft aan dat alleen de `id` en `state` eigenschappen moeten worden opgehaald voor de entiteiten die zijn opgevraagd. Ook wordt hiermee aangegeven dat alleen entiteiten waarvan de status is gewijzigd sinds de opgegeven `DateTime` parameter moet worden geretourneerd.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Volgende stappen
### <a name="parallel-node-tasks"></a>Knooppunt parallelle taken
[Azure Batch compute Resourcegebruik met gelijktijdige knooppunt taken maximaliseren](batch-parallel-node-tasks.md) een ander artikel betrekking heeft op de prestaties van de Batch-toepassingen. Bepaalde typen werkbelastingen kunnen profiteren van parallelle taken worden uitgevoerd op grotere-- maar minder--rekenknooppunten. Bekijk de [voorbeeldscenario](batch-parallel-node-tasks.md#example-scenario) in het artikel voor meer informatie over dit scenario.

### <a name="batch-forum"></a>Batch-Forum
De [Azure Batch-Forum] [ forum] is een goede plaats om te bespreken Batch en vragen over de service op MSDN. Kop op via voor nuttige 'een tijdelijke' berichten, en stel uw vragen wanneer deze zich voordoen tijdens het bouwen van uw Batch-oplossingen.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

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

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job