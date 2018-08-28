---
title: Indienen van een groot aantal taken - Azure Batch | Microsoft Docs
description: Het efficiënt indienen van een zeer groot aantal taken in een enkele Azure Batch-taak
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 08/24/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 3c683b24db2899ee680988c7bedc760d6bb8ec73
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053487"
---
# <a name="submit-a-large-number-of-tasks-to-a-batch-job"></a>Een groot aantal taken aan een Batch-taak verzenden

Wanneer u grootschalige Azure Batch-workloads uitvoert, is het raadzaam om in te dienen tienduizenden, honderden of duizenden of zelfs nog meer taken aan een enkele taak. 

Dit artikel bevat richtlijnen en enkele codevoorbeelden in te dienen een groot aantal taken met een aanzienlijk betere doorvoer naar een één Batch-taak. Nadat taken zijn ingediend, geef ze de Batch-wachtrij voor verwerking op de groep die u voor de taak opgeeft.

## <a name="use-task-collections"></a>Taak verzamelingen gebruiken

De Batch-API's bieden methoden voor het efficiënt taken toevoegen aan een taak als een *verzameling*, naast een op een tijdstip. Als u een groot aantal taken toevoegt, moet u de juiste methoden of overloads taken toevoegen als een verzameling. U over het algemeen maken een verzameling taak met het definiëren van taken als u een set van invoerbestanden of parameters voor uw taak herhalen.

De maximale grootte van de taak-verzameling die u in één aanroep kunt toevoegen, is afhankelijk van de Batch-API die u gebruikt:

* De volgende Batch-API's de verzameling te beperken **100 taken**. De limiet kan niet kleiner zijn, afhankelijk van de grootte van de taken - bijvoorbeeld: als de taken een groot aantal bestanden of omgevingsvariabelen hebben.

    * [REST API](/rest/api/batchservice/task/addcollection)
    * [Python-API](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python#azure_batch_operations_TaskOperations_add_collection)
    * [Node.js-API](/javascript/api/azure-batch/task?view=azure-node-latest#addcollection)

  Wanneer u deze API's gebruikt, moet u opgeven van logica voor het aantal taken om te voldoen aan de limiet voor de verzameling, en voor het afhandelen van fouten en nieuwe pogingen als mislukt het toevoegen van taken te verdelen. Als een verzameling van de taak te groot om toe te voegen is, wordt de aanvraag een fout gegeneerd en weer opnieuw moet worden uitgevoerd met minder taken.

* De volgende API's ondersteunen veel grotere verzamelingen van taak: alleen beperkt door de beschikbaarheid van de RAM-geheugen op de client verzenden. Deze API's worden transparant verwerkt delen van de verzameling taak in 'segmenten' voor de op lager niveau API's en nieuwe pogingen als mislukt het toevoegen van taken.

    * [.NET API](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync?view=azure-dotnet)
    * [Java-API](/java/api/com.microsoft.azure.batch.protocol._tasks.addcollectionasync?view=azure-java-stable)
    * [Azure Batch CLI-extensie](batch-cli-templates.md) met Batch CLI-sjablonen
    * [Python-SDK-extensie](https://pypi.org/project/azure-batch-extensions/)

## <a name="increase-throughput-of-task-submission"></a>Betere doorvoer van de taak verzenden

Het kan enige tijd een groot aantal taken bijvoorbeeld toevoegen aan een job - duren, van op 1 minuut 20.000 toevoegen taken via de .NET API. Afhankelijk van de Batch-API en uw werkbelasting, kunt u de doorvoer van de taak kunt verbeteren door het wijzigen van een of meer van de volgende opties:

* **Grootte van de taak** -toe te voegen omvangrijke taken langer duurt dan kleinere toe te voegen. Verklein de grootte van elke taak in een verzameling, kunt u vereenvoudigen van de opdrachtregel van de taak, het verminderen van het aantal omgevingsvariabelen of efficiënter afhandelen van de vereisten voor de uitvoering van de taak. Bijvoorbeeld, in plaats van een groot aantal bestanden, installeert afhankelijkheden van de taak met behulp van een [begintaak](batch-api-basics.md#start-task) op de groep van toepassingen of gebruik een [toepassingspakket](batch-application-packages.md) of [Docker-container](batch-docker-container-workloads.md).

* **Het aantal parallelle bewerkingen** - afhankelijk van de Batch-API, toename van de doorvoer door het maximum aantal gelijktijdige bewerkingen door de Batch-client. Configureer deze instellen met behulp van de [BatchClientParallelOptions.MaxDegreeOfParallelism](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) eigenschap in de .NET-API of de `threads` parameter van methoden, zoals [TaskOperations.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python#add-collection)in de Batch Python SDK-extensie. (Deze eigenschap is niet beschikbaar in de systeemeigen Batch Python-SDK.) Standaard is deze eigenschap ingesteld op 1, maar stelt hogere voor het verbeteren van doorvoer van bewerkingen. U wisselwerking tussen verhoogde doorvoer netwerkbandbreedte in beslag nemen en sommige CPU-prestaties. Doorvoer van de taak wordt verhoogd met tot 100 keer de `MaxDegreeOfParallelism` of `threads`. In de praktijk moet u het aantal gelijktijdige bewerkingen lager dan 100 ingesteld. 
 
  De Azure Batch CLI-extensie met de Batch-sjablonen verhoogt het aantal gelijktijdige bewerkingen automatisch op basis van het aantal beschikbare cores, maar deze eigenschap kan niet worden geconfigureerd in de CLI. 

* **Limieten voor de HTTP-verbindingen** -het aantal gelijktijdige HTTP-verbindingen kan de prestaties van de Batch-client worden beperkt wanneer dit is het toevoegen van een groot aantal taken. Het aantal HTTP-verbindingen is beperkt met bepaalde API's. Bij het ontwikkelen met de .NET-API, bijvoorbeeld de [ServicePointManager.DefaultConnectionLimit](/dotnet/api/system.net.servicepointmanager.defaultconnectionlimit) eigenschap is standaard ingesteld op 2. Het is raadzaam om de waarde moet een getal van parallelle bewerkingen dicht bij of groter zijn dan het aantal te verhogen.

## <a name="example-batch-net"></a>Voorbeeld: Batch .NET

De volgende C#-codefragmenten weergeven-instellingen configureren bij het toevoegen van een groot aantal taken met behulp van de Batch .NET API.

Voor een betere doorvoer van de taak, verhoogt u de waarde van de [MaxDegreeofParallelism](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) eigenschap van de [BatchClient](/dotnet/api/microsoft.azure.batch.batchclient?view=azure-dotnet). Bijvoorbeeld:

```csharp
BatchClientParallelOptions parallelOptions = new BatchClientParallelOptions()
  {
    MaxDegreeOfParallelism = 15
  };
...
```
Een verzameling taken toevoegen aan de taak met behulp van de juiste overbelasting van de [AddTaskAsync](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync?view=azure-dotnet) of [AddTask](/dotnet/api/microsoft.azure.batch.cloudjob.addtask?view=azure-dotnet
) methode. Bijvoorbeeld:

```csharp
// Add a list of tasks as a collection
List<CloudTask> tasksToAdd = new List<CloudTask>(); // Populate with your tasks
...
await batchClient.JobOperations.AddTaskAsync(jobId, tasksToAdd, parallelOptions);
```


## <a name="example-batch-cli-extension"></a>Voorbeeld: Batch CLI-extensie

Met behulp van de Azure Batch CLI-extensies met [Batch CLI sjablonen](batch-cli-templates.md), maak een JSON-bestand voor taak-sjabloon bevat een [taak factory](https://github.com/Azure/azure-batch-cli-extensions/blob/master/doc/taskFactories.md). De factory taak configureert u een verzameling verwante taken voor een taak uit de definitie van een enkele taak.  

Hier volgt een voorbeeld-taaksjabloon voor een taak eendimensionale parametrische met een groot aantal taken - in dit geval 250.000. De opdrachtregel van de taak is een eenvoudige `echo` opdracht.

```json
{
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "myjob",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "mypool"
            },
            "taskFactory": {
                "type": "parametricSweep",
                "parameterSets": [
                    {
                        "start": 1,
                        "end": 250000,
                        "step": 1
                    }
                ],
                "repeatTask": {
                    "commandLine": "/bin/bash -c 'echo Hello world from task {0}'",
                    "constraints": {
                        "retentionTime":"PT1H"
                    }
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```
Zie voor informatie over het uitvoeren van een taak met de sjabloon [met Azure Batch CLI sjablonen en bestandsoverdracht](batch-cli-templates.md).

## <a name="example-batch-python-sdk-extension"></a>Voorbeeld: Batch Python SDK-extensie

Voor het gebruik van de Azure Batch Python SDK-extensie, moet u eerst de Python-SDK en de extensie installeren:

```
pip install azure-batch
pip install azure-batch-extensions
```

Instellen van een `BatchExtensionsClient` die gebruikmaakt van de SDK-extensie:

```python

client = batch.BatchExtensionsClient(base_url=BATCH_ACCOUNT_URL, resource_group=RESOURCE_GROUP_NAME, batch_account=BATCH_ACCOUNT_NAME)
...
```

Een verzameling van taken toe te voegen aan een taak maken. Bijvoorbeeld:


```python
tasks=list()
# Populate the list with your tasks
...

```

Toevoegen van de taak verzameling met [task.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python#add-collection). Stel de `threads` parameter voor het verhogen van het aantal gelijktijdige bewerkingen:

```python
try:
    client.task.add_collection(job_id, threads=100)
except Exception as e:
    raise e
```

De extensie van de Batch Python-SDK biedt ook ondersteuning toe te voegen taakparameters aan de taak met behulp van een JSON-specificatie voor de factory voor een taak. Bijvoorbeeld configureren taakparameters voor een die vergelijkbaar is met de in de voorgaande parametrische [Batch CLI sjabloon](#example-batch-cli-template) voorbeeld:

```python
parameter_sweep = {
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "myjob",
            "poolInfo": {
                "poolId": "mypool"
            },
            "taskFactory": {
                "type": "parametricSweep",
                "parameterSets": [
                    {
                        "start": 1,
                        "end": 250000,
                        "step": 1
                    }
                ],
                "repeatTask": {
                    "commandLine": "/bin/bash -c 'echo Hello world from task {0}'",
                    "constraints": {
                        "retentionTime":"PT1H"
                    }
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
...
job_json = client.job.expand_template(parameter_sweep)
job_parameter = client.job.jobparameter_from_json(job_json)
```

De taakparameters toevoegen aan de taak. Stel de `threads` parameter voor het verhogen van het aantal gelijktijdige bewerkingen:

```python
try:
    client.job.add(job_parameter, threads=50)
except Exception as e:
    raise e
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het gebruik van de Azure Batch CLI-extensie met [Batch CLI sjablonen](batch-cli-templates.md).
* Meer informatie over de [Batch Python SDK-extensie](https://pypi.org/project/azure-batch-extensions/).