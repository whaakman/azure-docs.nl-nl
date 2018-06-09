---
title: Migreren naar de bijgewerkte Azure Batch AI-API | Microsoft Docs
description: Het bijwerken van Azure Batch AI-codes en scripts werkruimte gebruiken en Experimenteer resources
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
ms.openlocfilehash: 94db54f35b7871407368b174536dc6454775779c
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249287"
---
# <a name="migrate-to-the-updated-batch-ai-api"></a>Migreren naar de bijgewerkte Batch AI-API

In de komende Batch AI REST-API versie 2018-05-01 en verwante Batch AI-SDK's en hulpprogramma's, zijn belangrijke wijzigingen en nieuwe functies worden geïntroduceerd.

Als u een eerdere versie van de API van Batch AI hebt gebruikt, is dit artikel wordt uitgelegd hoe u uw code en scripts voor gebruik met de nieuwe API wijzigen. U kunt deze wijzigingen pas nadat de nieuwe API beschikbaar is.

## <a name="whats-changing"></a>Wat wordt gewijzigd?

In reactie op feedback van klanten bent we verwijderen van grenzen van het aantal taken en waardoor het gemakkelijker om Batch AI-resources te beheren. De nieuwe API bevat twee nieuwe bronnen, *werkruimte* en *experimenteren*. Verzamelen van gerelateerde training taken onder een experiment en alle gerelateerde Batch AI resources (clusters, bestandsservers, experimenten, -taken) onder een werkruimte te organiseren.  

* **Werkruimte** -een verzameling op het hoogste niveau van alle soorten AI Batch-resources. Clusters en bestandsservers zijn opgenomen in een werkruimte. Werkruimten meestal afzonderlijke werk die horen bij verschillende groepen of projecten. Bijvoorbeeld, wellicht u een dev en een test-werkruimte. Moet u waarschijnlijk slechts een beperkt aantal werkruimten per abonnement. 

* **Experiment** -een verzameling van gerelateerde taken die kunnen worden opgevraagd en samen beheerd. Een experiment bijvoorbeeld gebruiken voor het groeperen van alle taken die worden uitgevoerd als onderdeel van een afstemmen vegen hyper-parameter. 

De volgende afbeelding toont een voorbeeld van de resource-hiërarchie. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

## <a name="monitor-and-manage-existing-resources"></a>Bewaken en beheren van bestaande resources
Binnen elke resourcegroep waar u al Batch AI-clusters, taken of bestandsservers gemaakt, maakt de Batch AI-service een werkruimte met de naam `migrated-<region>` (bijvoorbeeld `migrated-eastus`) en een experiment met de naam `migrated`. Voor toegang tot de eerder gemaakte taken, clusters of bestandsservers, moet u de gemigreerde werkruimte gebruiken en Experimenteer. 

### <a name="portal"></a>Portal 
Voor toegang tot eerder gemaakte taken, clusters of bestandsservers met behulp van de portal, selecteert u eerst de `migrated-<region>` werkruimte. Nadat u de werkruimte selecteert, voert u bewerkingen zoals het vergroten of verkleinen of verwijderen van een cluster en de status van taak- en uitvoer weergeven. 

### <a name="sdks"></a>SDK's 
Voor toegang tot taken, clusters of bestandsservers eerder hebt gemaakt via de Batch AI-SDK's, moet u de Werkruimtenaam opgeven en Experimenteer naam parameters. 

Als u de SDK voor Python gebruikt, worden de relevante wijzigingen weergegeven in de volgende voorbeelden. Wijzigingen zijn in de andere partij AI-SDK's vergelijkbaar. 


#### <a name="get-old-cluster"></a>Ophalen van de oude cluster 

```python
cluster = client.clusters.get(resource_group_name, 'migrated-<region>', cluster_name)
```

#### <a name="delete-old-cluster"></a>Verwijderen van oude cluster 

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

#### <a name="delete-old-job"></a>Verwijderen van oude taak

```python
client.jobs.delete(resource_group_name, 'migrated-<region>', 'migrated', job_name)
```
 
### <a name="azure-cli"></a>Azure-CLI 
 
Wanneer u de Azure CLI taken toegang eerder hebt gemaakt, clusters of bestandsservers, gebruiken de `-w` en `-e` parameters voor de werkruimte op te geven en Experimenteer namen. 


#### <a name="get-old-cluster"></a>Ophalen van de oude cluster

```azurecli
az batchai cluster show -g resource-group-name -w migrated-<region> -n cluster-name
```


#### <a name="delete-old-cluster"></a>Verwijderen van oude cluster 

```azurecli
az batchai cluster delete -g resource-group-name -w migrated-<region> -n cluster-name
```

#### <a name="get-old-file-server"></a>Oude bestandsserver ophalen

```azurecli
az batchai fileserver show -g resource-group-name -w migrated-<region> -n fileserver-name
```


#### <a name="delete-old-file-server"></a>Oude bestandsserver verwijderen 

```azurecli
az batchai fileserver delete -g resource-group-name -w migrated-<region> -n fileserver-name
``` 


#### <a name="get-old-job"></a>Oude taak ophalen

```azurecli
az batchai fileserver show -g resource-group-name -w migrated-<region> -e migrated -n job-name
```


#### <a name="delete-old-job"></a>Verwijderen van oude taak 

```azurecli
az batchai fileserver delete -g resource-group-name -w migrated-<region> -e migrated -n job-name
``` 

## <a name="create-batch-ai-resources"></a>AI Batch-resources maken 
 
Als u een van de bestaande Batch AI-SDK's, kunt u blijven AI Batch-resources (taken, clusters of bestandsservers) maken zonder codewijzigingen. Na de upgrade naar de nieuwe SDK, moet u de volgende wijzigingen aanbrengen.
 
### <a name="create-workspace"></a>Werkruimte maken 
Afhankelijk van uw scenario kunt u een werkruimte handmatig eenmalig via de portal of de CLI. Als u de CLI, maakt u een werkruimte met de volgende opdracht: 

```azurecli
az batchai workspace create -g resource-group-name -n workspace-name
```

### <a name="create-experiment"></a>Experiment maken 


Afhankelijk van uw scenario kunt u een experiment handmatig eenmalig via de portal of de CLI. Als u de CLI, maakt u een experiment met de volgende opdracht: 

```azurecli
az batchai experiment create -g resource-group-name -w workspace-name -n experiment-name

```

### <a name="create-clusters-file-servers-and-jobs"></a>Maken van clusters, bestandsservers en taken
 
Als u de portal gebruiken om taken, clusters of bestandsservers te maken, leidt de portal u tijdens de ervaring maken uit te geven de naam van de werkruimte en Experimenteer naam parameters.

Geef de parameter voor de werkruimte voor het maken van taken, clusters of bestandsservers via de bijgewerkte SDK. Als u de SDK voor Python gebruikt, worden de relevante wijzigingen weergegeven in de volgende voorbeelden. Vervang *werkruimte_naam* en *experiment_name* met de werkruimte en experimenten die u eerder hebt gemaakt. Wijzigingen zijn in de andere partij AI-SDK's vergelijkbaar. 


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