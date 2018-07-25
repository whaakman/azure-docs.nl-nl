---
title: Efficiënte lijstquery's - Azure Batch ontwerpen | Microsoft Docs
description: De prestaties verbeteren door het filteren van uw query's bij het aanvragen van informatie over Batch-resources zoals pools, jobs, taken en rekenknooppunten.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 06/26/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6bc31e8541797930583e41fb6efbb6473cd4b894
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004452"
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a>Efficiënt query's naar de lijst met Batch-resources maken

Hier leert u hoe u uw Azure Batch-toepassing om prestaties te verhogen door te verminderen de hoeveelheid gegevens die worden geretourneerd door de service wanneer u taken, taken, rekenknooppunten en andere resources met een query de [Batch .NET] [ api_net] bibliotheek.

Bijna alle Batch-toepassingen moeten een soort van bewaking of een andere bewerking die in de Batch-service, vaak met regelmatige tussenpozen zoekt uitvoeren. Bijvoorbeeld, om te bepalen of er zijn taken in de wachtrij is nog in een taak, moet u gegevens ophalen voor elke taak in de taak. Om te bepalen van de status van knooppunten in uw pool, moet u gegevens ophalen op elk knooppunt in de groep. In dit artikel wordt uitgelegd hoe u dergelijke query's uitvoeren in de meest efficiënte manier.

> [!NOTE]
> De Batch-service biedt speciale API-ondersteuning voor de algemene scenario's van de taken in een taak worden geteld en tellen van rekenknooppunten in Batch-pool. In plaats van een lijst met query voor deze, roept u de [ophalen voor de taak wordt geteld] [ rest_get_task_counts] en [lijst Pool knooppunt telt] [ rest_get_node_counts] bewerkingen. Deze bewerkingen zijn efficiënter dan een lijst met query, maar meer beperkte informatie geretourneerd. Zie [aantal taken en rekenknooppunten per staat](batch-get-resource-counts.md). 


## <a name="meet-the-detaillevel"></a>Voldoen aan de DetailLevel
In een productie-Batch-toepassing kunnen entiteiten, zoals jobs, taken en rekenknooppunten nummer in duizenden wordt weergegeven. Wanneer u meer informatie over deze resources aanvraagt, moet een grote hoeveelheid gegevens 'cross-de kabel' van de Batch-service aan uw toepassing op elke query. Door het aantal items en het type informatie dat wordt geretourneerd door een query te beperken, kunt u de snelheid van uw query's, en daarom de prestaties van uw toepassing te verhogen.

Dit [Batch .NET] [ api_net] API codefragment lijsten *elke* taak die is gekoppeld aan een taak, samen met *alle* van de eigenschappen van elke taak:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

U kunt echter een lijstquery veel efficiënter uitvoeren door het toepassen van een 'detailniveau' aan de query. U dit doen door het leveren van een [ODATADetailLevel] [ odata] object toe aan de [JobOperations.ListTasks] [ net_list_tasks] methode. Dit codefragment retourneert alleen de ID, vanaf de opdrachtregel en compute-knooppunteigenschappen van voltooide taken:

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

In dit voorbeeldscenario, als er duizenden taken in de taak, de resultaten van de tweede query wordt doorgaans worden geretourneerd veel sneller dan de eerste. Meer informatie over het gebruik van ODATADetailLevel wanneer u objecten met de Batch .NET API is opgenomen [hieronder](#efficient-querying-in-batch-net).

> [!IMPORTANT]
> Is het raadzaam dat u *altijd* opgeven van een object ODATADetailLevel uw .NET-API-lijst-aanroepen om ervoor te zorgen voor maximale efficiëntie en prestaties van uw toepassing. U kunt een detailniveau opgeeft, lagere reactietijden voor Batch-service, het verbeteren van het netwerkgebruik en geheugengebruik minimaliseren door clienttoepassingen.
> 
> 

## <a name="filter-select-and-expand"></a>Filter selecteren en uitvouwen
De [Batch .NET] [ api_net] en [Batch REST] [ api_rest] API's bieden de mogelijkheid om te beperken van het aantal items dat wordt geretourneerd in een lijst, evenals de hoeveelheid informatie die wordt geretourneerd voor elk. U dit doen door op te geven **filter**, **Selecteer**, en **Vouw tekenreeksen** bij het uitvoeren van lijstquery's.

### <a name="filter"></a>Filteren
De filtertekenreeks is een expressie die vermindert het aantal items dat wordt geretourneerd. Bijvoorbeeld, een lijst met alleen de actieve taken voor een taak of lijst alleen rekenknooppunten die klaar zijn voor het uitvoeren van taken.

* De filtertekenreeks bestaat uit een of meer expressies met een expressie die uit een eigenschapsnaam, operator en waarde bestaat. De eigenschappen die kunnen worden opgegeven zijn specifiek voor elke entiteitstype dat u een query, zoals de operators die worden ondersteund voor elke eigenschap zijn.
* Meerdere expressies kunnen worden gecombineerd met behulp van de logische operators `and` en `or`.
* In dit voorbeeld filteren tekenreekslijsten alleen de die wordt uitgevoerd 'render' taken: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Selecteer
De tekenreeks die optie beperkt de waarden van eigenschappen die worden geretourneerd voor elk item. U geeft een lijst met namen van eigenschappen en alleen de eigenschapswaarden van deze voor de items in de queryresultaten worden geretourneerd.

* De optie tekenreeks bestaat uit een door komma's gescheiden lijst met namen van eigenschappen. U kunt een van de eigenschappen voor het entiteitstype die u wilt zoeken.
* In dit voorbeeld selecteren tekenreeks geeft aan dat alleen voor de drie waarden van eigenschappen voor elke taak moeten worden geretourneerd: `id, state, stateTransitionTime`.

### <a name="expand"></a>Uitvouwen
De tekenreeks uit te breiden vermindert het aantal API-aanroepen die nodig zijn om bepaalde informatie te verkrijgen. Wanneer u een tekenreeks uit te breiden, meer informatie over elk item kan worden verkregen met één API-aanroep. In plaats van de eerste het verkrijgen van de lijst met entiteiten, vervolgens aanvragen van informatie voor elk item in de lijst, kunt u een tekenreeks uit te breiden gebruiken om op te halen van dezelfde gegevens in één API-aanroep. Minder API-aanroepen betekent betere prestaties.

* Net als bij de optie tekenreeks, de tekenreeks uit te breiden bepaalt of bepaalde gegevens worden opgenomen in de lijst met query-resultaten.
* De tekenreeks uit te breiden wordt alleen ondersteund wanneer deze wordt gebruikt in de lijst van taken, taakschema's, taken en groepen. Op dit moment ondersteunt deze alleen statistische gegevens.
* Wanneer alle eigenschappen vereist zijn en er is geen optie tekenreeks is opgegeven, de tekenreeks uit te breiden *moet* worden gebruikt om op te halen van statistische gegevens. Als een optie tekenreeks wordt gebruikt om op te halen van een subset met eigenschappen, klikt u vervolgens `stats` kunnen worden opgegeven in de tekenreeks selecteren en de tekenreeks uit te breiden hoeft niet te worden opgegeven.
* In dit voorbeeld Vouw tekenreeks geeft aan dat de statistische gegevens voor elk item in de lijst moet worden geretourneerd: `stats`.

> [!NOTE]
> Bij het maken van een van de typen van de drie query-tekenreeks (filteren, selecteren en uitvouwen), moet u ervoor zorgen dat de namen van eigenschappen en de aanvraag overeenkomt met die van hun tegenhangers REST-API-element. Bijvoorbeeld, als u werkt met de .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) klasse, moet u **status** in plaats van **status**, zelfs als de eigenschap .NET [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Zie de onderstaande tabellen voor de eigenschaptoewijzingen tussen de .NET en REST-API's.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Regels voor filteren, selecteert u uit en vouw van tekenreeksen
* Namen van eigenschappen in worden gefilterd, selecteert u uit en vouw van tekenreeksen moeten worden weergegeven als in de [Batch REST] [ api_rest] API, zelfs wanneer u [Batch .NET] [ api_net]of een van de andere Batch-SDK's.
* Alle eigenschapnamen zijn hoofdlettergevoelig, maar eigenschapswaarden zijn niet hoofdlettergevoelig.
* Datum/tijd tekenreeksen kunnen twee verschillende indelingen, en moet worden voorafgegaan door `DateTime`.
  
  * Voorbeeld van W3C-DTF-indeling: `creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * Voorbeeld van RFC 1123-indeling: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Booleaanse tekenreeksen zijn `true` of `false`.
* Als een eigenschap is ongeldig of de operator is opgegeven, een `400 (Bad Request)` fout leidt.

## <a name="efficient-querying-in-batch-net"></a>Efficiënt uitvoeren van query's in Batch .NET
Binnen de [Batch .NET] [ api_net] API, de [ODATADetailLevel] [ odata] klasse wordt gebruikt voor het verstrekken van filter, selecteren en uitvouwen tekenreeksen om weer te geven bewerkingen. De klasse ODataDetailLevel heeft drie openbare tekenreekseigenschappen die kunnen worden opgegeven in de constructor, of rechtstreeks op het object is ingesteld. U vervolgens de ODataDetailLevel-object doorgeven als een parameter aan de verschillende bewerkingen na opvragen, zoals [ListPools][net_list_pools], [ListJobs][net_list_jobs], en [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[ FilterClause][odata_filter]: Beperk het aantal items dat wordt geretourneerd.
* [ODATADetailLevel][odata].[ SelectClause][odata_select]: opgeven welke eigenschapswaarden worden geretourneerd bij elk item.
* [ODATADetailLevel][odata].[ ExpandClause][odata_expand]: gegevens ophalen voor alle items in één API-aanroep in plaats van afzonderlijke aanroepen voor elk item.

Het volgende codefragment maakt gebruik van de Batch .NET API efficiënt query uitvoeren op de Batch-service voor de statistische gegevens van een specifieke set van toepassingen. In dit scenario heeft de gebruiker Batch pools voor test- en productieomgevingen. De test-groep-id's worden voorafgegaan door 'test' en de productie-groep-id's worden voorafgegaan door 'prod'. In het codefragment *myBatchClient* is een goed geïnitialiseerd exemplaar van de [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) klasse.

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
> Een exemplaar van [ODATADetailLevel] [ odata] die is geconfigureerd met bepaalde en uitbreiden van de EU kunnen ook worden doorgegeven aan de juiste Get-methoden, zoals [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), beperken van de hoeveelheid gegevens die wordt geretourneerd.
> 
> 

## <a name="batch-rest-to-net-api-mappings"></a>Batch REST, in .NET API-toewijzingen
Namen van eigenschappen in filter selecteren uit en vouw van tekenreeksen *moet* geven hun tegenhangers REST-API, zowel in de naam en de aanvraag. De onderstaande tabellen bevatten toewijzingen tussen de tegenhangers .NET en REST-API.

### <a name="mappings-for-filter-strings"></a>Toewijzingen voor tekenreeksen
* **.NET-lijst met methoden**: elk van de .NET API-methoden in deze kolom accepteert een [ODATADetailLevel] [ odata] object als parameter.
* **REST-aanvragen voor lijst**: elke REST-API-pagina die zijn gekoppeld aan de in deze kolom bevat een tabel met Hiermee geeft u de eigenschappen en bewerkingen die zijn toegestaan in *filter* tekenreeksen. U gebruikt deze namen van eigenschappen en bewerkingen wanneer u een [ODATADetailLevel.FilterClause] [ odata_filter] tekenreeks.

| .NET-lijst met methoden | REST-aanvragen voor lijst |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[De certificaten in een account weergeven][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[De bestanden die zijn gekoppeld aan een taak weergeven][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[De status van de taak jobvoorbereidingstaken en jobvrijgevingstaken voor een taak weergeven][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[Lijst van de taken in een account][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Lijst van de bestanden op een knooppunt][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[Een lijst met de taken die zijn gekoppeld aan een taak][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Overzicht van het taakschema's in een account][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[Lijst van de taken die zijn gekoppeld aan een jobplanning][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Lijst van de rekenknooppunten in een groep][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Lijst van de groepen in een account][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Toewijzingen voor tekenreeksen selecteren
* **Batch .NET-typen**: Batch .NET API-typen.
* **REST-API-entiteiten**: elke pagina in deze kolom bevat een of meer tabellen met de namen van de REST-API-eigenschappen voor het type. De namen van deze eigenschappen worden gebruikt wanneer u samenstelt *Selecteer* tekenreeksen. U gebruikt deze dezelfde namen van eigenschappen als u een [ODATADetailLevel.SelectClause] [ odata_select] tekenreeks.

| Batch .NET-typen | REST-API-entiteiten |
| --- | --- |
| [Certificaat][net_cert] |[Informatie over een certificaat ophalen][rest_get_cert] |
| [CloudJob][net_job] |[Informatie over een taak ophalen][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[Informatie over het schema van een taak ophalen][rest_get_schedule] |
| [ComputeNode][net_node] |[Informatie ophalen over een knooppunt][rest_get_node] |
| [CloudPool][net_pool] |[Informatie ophalen over een groep][rest_get_pool] |
| [CloudTask][net_task] |[Informatie over een taak ophalen][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Voorbeeld: een filtertekenreeks maken
Wanneer u een filtertekenreeks voor samenstelt [ODATADetailLevel.FilterClause][odata_filter], raadpleegt u de bovenstaande tabel onder "Toewijzingen voor tekenreeksen" om te zoeken naar de REST-API-documentatiepagina die overeenkomt met de lijst met de bewerking die u wilt uitvoeren. U vindt de filterbare eigenschappen en hun ondersteunde operators in de eerste multirow tabel op die pagina. Als u wilt ophalen van alle taken waarvan afsluitcode nul is, bijvoorbeeld deze rij op [een lijst met de taken die zijn gekoppeld aan een taak] [ rest_list_tasks] Hiermee geeft u de van toepassing op tekenreeks en toegestane operators:

| Eigenschap | Toegestane bewerkingen | Type |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

De filtertekenreeks voor het weergeven van alle taken met een andere afsluitcode dan dus:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Voorbeeld: een select tekenreeks maken
Om samen te stellen [ODATADetailLevel.SelectClause][odata_select], raadpleegt u de bovenstaande tabel onder "Toewijzingen voor select tekenreeksen" en navigeer naar de REST-API-pagina die overeenkomt met het type entiteit die u aanbiedt. U vindt de selecteerbare eigenschappen en hun ondersteunde operators in de eerste multirow tabel op die pagina. Als u ophalen van alleen de ID en vanaf de opdrachtregel voor elke taak in een lijst wilt, bijvoorbeeld, vindt u deze rijen in de tabel van toepassing op [informatie ophalen over een taak][rest_get_task]:

| Eigenschap | Type | Opmerkingen |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

Selecteer de string om alleen de ID en de opdrachtregel voor elke vermelde taak op te nemen zou vervolgens zijn:

`id, commandLine`

## <a name="code-samples"></a>Codevoorbeelden
### <a name="efficient-list-queries-code-sample"></a>Codevoorbeeld voor efficiënte lijstquery 's
Bekijk de [EfficientListQueries] [ efficient_query_sample] voorbeeldproject op GitHub om te zien hoe efficiënt lijst uitvoeren van query's kan invloed hebben op prestaties in een toepassing. Deze C#-consoletoepassing maakt en voegt een groot aantal taken aan een taak. Vervolgens wordt meerdere aanroepen naar de [JobOperations.ListTasks] [ net_list_tasks] methode en statuscontroles [ODATADetailLevel] [ odata] objecten die zijn geconfigureerd met verschillende eigenschapswaarden variëren van de hoeveelheid gegevens moeten worden geretourneerd. Het geeft een resultaat vergelijkbaar met het volgende:

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

Zoals weergegeven in de tijden die zijn verstreken, kunt u query responstijden aanzienlijk verlagen door het beperken van de eigenschappen en het aantal items dat wordt geretourneerd. U vindt deze en andere voorbeeldprojecten in de [azure-batch-samples] [ github_samples] -bibliotheek op GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics-bibliotheek en code-voorbeeld
Naast het EfficientListQueries bovenstaande codevoorbeeld, vindt u de [BatchMetrics] [ batch_metrics] project in de [azure-batch-samples] [ github_samples]GitHub-opslagplaats. Het voorbeeldproject BatchMetrics ziet u hoe u efficiënt taakvoortgang Azure Batch met behulp van de Batch-API.

De [BatchMetrics] [ batch_metrics] voorbeeld bevat een .NET-klassebibliotheekproject die u kunt opnemen in uw eigen projecten en een eenvoudig opdrachtregelprogramma programma om te oefenen en voorbeelden van het gebruik van de bibliotheek.

De voorbeeldtoepassing in het project ziet u de volgende bewerkingen:

1. Specifieke kenmerken selecteren om te downloaden van alleen de eigenschappen die u nodig hebt
2. Filteren op status overgang keer om te downloaden alleen wijzigingen sinds de laatste query

Bijvoorbeeld, de volgende methode wordt weergegeven in de bibliotheek BatchMetrics. Het resultaat een ODATADetailLevel dat Hiermee wordt aangegeven dat alleen de `id` en `state` eigenschappen moeten worden opgehaald voor de entiteiten die zijn opgevraagd. Ook wordt hiermee aangegeven dat alleen entiteiten waarvan u de status is gewijzigd sinds de opgegeven `DateTime` parameter moet worden geretourneerd.

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
### <a name="parallel-node-tasks"></a>Parallelle knooppunttaken
[Gebruik van Azure Batch compute-bronnen met gelijktijdige knooppunttaken maximaliseren](batch-parallel-node-tasks.md) is een ander artikel met betrekking tot de prestaties van de Batch-toepassingen. Bepaalde typen workloads kunnen profiteren van het uitvoeren van parallelle taken op groter, maar minder--rekenknooppunten. Bekijk de [voorbeeldscenario](batch-parallel-node-tasks.md#example-scenario) in het artikel voor meer informatie over dit scenario.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
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

[rest_get_task_counts]: /rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts