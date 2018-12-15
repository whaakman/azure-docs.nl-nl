---
title: Migreren naar de bijgewerkte Azure Batch AI-API | Microsoft Docs
description: Azure Batch AI-code en scripts voor het gebruik van de werkruimte en experimenteren resources bijwerken
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 75a9a5e9bafd62b320397c00ef6574b7536d9e09
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407777"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Migreren naar de bijgewerkte Batch AI-API

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

In de Batch AI REST API-versie 2018-05-01 en gerelateerde Batch AI-SDK's en hulpprogramma's, zijn belangrijke wijzigingen en nieuwe functies geïntroduceerd.

Als u een eerdere versie van de Batch AI-API hebt gebruikt, is dit artikel wordt uitgelegd hoe u uw code en scripts die worden gebruikt met de nieuwe API wijzigt. 

## <a name="whats-changing"></a>Wat wordt gewijzigd?

In reactie op feedback van klanten, zijn we beperkingen met betrekking tot het aantal taken en waardoor het gemakkelijker voor het beheren van Batch AI-resources verwijderen. De nieuwe API bevat twee nieuwe resources, *werkruimte* en *experimenteren*. Verzamel gerelateerde trainingstaken onder een experiment, en alle gerelateerde Batch AI-resources (clusters, bestandsservers, experimenten, -taken) onder een werkruimte te organiseren.  

* **Werkruimte** -een verzameling op het hoogste niveau van alle typen resources van Batch AI. Clusters en bestandsservers zijn opgenomen in een werkruimte. Werkruimten meestal afzonderlijke werk die behoren tot verschillende groepen of projecten. Bijvoorbeeld, u mogelijk een ontwikkel- en een test-werkruimte. U moet waarschijnlijk slechts een beperkt aantal werkruimten per abonnement. 

* **Experiment** -een verzameling van gerelateerde taken die kunnen worden opgevraagd en beheerd samen. Een experiment bijvoorbeeld gebruiken voor het groeperen van alle taken die worden uitgevoerd als onderdeel van een hyper-afstemmen parameteropschoning. 

De volgende afbeelding toont een voorbeeld van de resource-hiërarchie. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>Controleren en beheren van bestaande resources
In elke resourcegroep waar u al hebt gemaakt Batch AI-clusters, taken of bestandsservers, maakt de Batch AI-service een werkruimte met de naam `migrated-<region>` (bijvoorbeeld `migrated-eastus`) en een experiment met de naam `migrated`. Voor toegang tot de eerder gemaakte taken, clusters of bestandsservers, moet u de gemigreerde werkruimte gebruiken en te experimenteren. 

### <a name="portal"></a>Portal 
Voor toegang tot eerder gemaakte taken, bestandsservers of clusters met behulp van de portal, selecteert u eerst de `migrated-<region>` werkruimte. Nadat u de werkruimte hebt geselecteerd, bewerkingen uitvoeren zoals vergroten of verkleinen of verwijderen van een cluster, en taakstatus en uitvoer weergeven. 

### <a name="sdks"></a>SDK's 
Voor toegang tot taken, clusters of bestandsservers die eerder zijn gemaakt via de Batch AI SDK's, geef de Werkruimtenaam en Experimenteer naam-parameters. 

Als u de Python SDK gebruikt, worden de relevante wijzigingen weergegeven in de volgende voorbeelden. Wijzigingen zijn vergelijkbaar in de andere Batch AI-SDK's. 


#### <a name="get-old-cluster"></a>Oude cluster ophalen 

```python
cluster = client.clusters.get(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="delete-old-cluster"></a>Oude cluster verwijderen 

```python
client.clusters.delete(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="get-old-file-server"></a>Oude bestandsserver ophalen 

```python
cluster = client.fileservers.get(resource_group_name, 'migrated-<region>', fileserver_name)
```

#### <a name="delete-old-file-server"></a>Oude bestandsserver verwijderen  

```python
client.fileservers.delete(resource_group_name, 'migrated-<region>', fileserver_name)
``` 


#### <a name="get-old-job"></a>Oude taak ophalen 

```python
cluster = client.jobs.get(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```

#### <a name="delete-old-job"></a>Oude taak verwijderen

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>Azure-CLI 
 
Wanneer u de Azure CLI-taken toegang eerder hebt gemaakt, clusters of bestandsservers, gebruiken de `-w` en `-e` parameters voor het leveren van de werkruimte en Experimenteer namen. 


#### <a name="get-old-cluster"></a>Oude cluster ophalen

```azurecli
az batchai cluster show -g resource-group-name -w migrated-<region> -n cluster-name
```


#### <a name="delete-old-cluster"></a>Oude cluster verwijderen 

```azurecli
az batchai cluster delete -g resource-group-name -w migrated-<region> -n cluster-name
```

#### <a name="get-old-file-server"></a>Oude bestandsserver ophalen

```azurecli
az batchai file-server show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Oude bestandsserver verwijderen 

```azurecli
az batchai file-server delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Oude taak ophalen

```azurecli
az batchai job show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Oude taak verwijderen 

```azurecli
az batchai job delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>Batch AI-resources maken 
 
Als u een van de bestaande Batch AI SDK's, kunt u echter ook doorgaan met het maken van Batch AI-resources (taken, clusters of bestandsservers) zonder dat code hoeft te wijzigen. Na de upgrade naar de nieuwe SDK, moet u de volgende wijzigingen aanbrengen.
 
### <a name="create-workspace"></a>Werkruimte maken 
Afhankelijk van uw scenario, kunt u een werkruimte handmatig eenmalig via de portal of de CLI maken. Als u de CLI, maakt u een werkruimte met de volgende opdracht: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Experiment maken 


Afhankelijk van uw scenario, kunt u een experiment handmatig eenmalig via de portal of de CLI maken. Als u de CLI, maakt u een experiment met de volgende opdracht: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Clusters, bestandsservers en taken maken
 
Als u de portal gebruiken om taken, clusters of bestandsservers te maken, leidt de portal u tijdens de creatieprocedure in en geef de Werkruimtenaam experimenteren naam-parameters.

Geef de parameter voor de werkruimte voor het maken van taken, clusters of bestandsservers via de bijgewerkte SDK. Als u de Python SDK gebruikt, worden de relevante wijzigingen weergegeven in de volgende voorbeelden. Vervang *werkruimte_naam* en *experiment_name* met de werkruimte en het experiment dat u eerder hebt gemaakt. Wijzigingen zijn vergelijkbaar in de andere Batch AI-SDK's. 


#### <a name="create-cluster"></a>Cluster maken 

```python
_ = client.clusters.create(resource_group_name, workspace_name, cluster_name, cluster_create_parameters).result()
```

#### <a name="create-file-server"></a>Bestandsserver maken 

```python
_ = client.fileservers.create(resource_group_name, workspace_name, fileserver_name, fileserver_create_parameters).result()
```

#### <a name="create-job"></a>Taak maken 

```python
_ = client.jobs.create(resource_group_name, workspace_name, experiment_name, job_name job_create_parameters).result()
```


## <a name="next-steps"></a>Volgende stappen

* Zie de Batch AI-API-verwijzing: [CLI](/cli/azure/batchai), [.NET](/dotnet/api/overview/azure/batchai), [Java](/java/api/overview/azure/batchai), [Node.js](/javascript/api/overview/azure/batchai), [Python](/python/api/overview/azure/batchai), en [REST](/rest/api/batchai)