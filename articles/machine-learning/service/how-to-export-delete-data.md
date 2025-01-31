---
title: Exporteren of verwijderen van gegevens in de werkruimte
titleSuffix: Azure Machine Learning service
description: Informatie over het exporteren of verwijderen van uw werkruimte met de Azure portal, CLI, SDK en geverifieerde REST-API's.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: ph-com
ms.author: pahusban
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 3b55282adf56bedb0724eca578b8eaea00c26074
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65023979"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>Exporteren of uw gegevens in de werkruimte voor Machine Learning-service verwijderen 

U kunt in Azure Machine Learning, exporteren of verwijderen van uw gegevens in de werkruimte met de geverifieerde REST-API. In dit artikel leest u hoe.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>Uw gegevens in de werkruimte beheren
In het product gegevens die zijn opgeslagen door Azure Machine Learning Services is beschikbaar voor exporteren en verwijderen via de Azure portal, CLI, SDK, en geverifieerde REST-API's. Telemetriegegevens zijn toegankelijk via de Privacy van de Azure-portal. 

In Azure Machine Learning-Services bestaat persoonlijke gegevens uit gegevens van de gebruiker in de uitvoeringsgeschiedenis documenten en telemetrie-records van de interactie van sommige gebruikers met de service.

## <a name="delete-workspace-data-with-the-rest-api"></a>Verwijderen van gegevens in de werkruimte met de REST-API 

Als u wilt verwijderen van gegevens, kunnen de volgende API-aanroepen worden gemaakt met de bewerking HTTP DELETE. Deze zijn gemachtigd door een `Authorization: Bearer <arm-token>` header in de aanvraag, waar `<arm-token>` is het AAD-toegangstoken voor de `https://management.core.windows.net/` eindpunt.  

Zie voor meer informatie over dit token ophalen en Azure-eindpunten worden aangeroepen, [Azure REST API-documentatie](https://docs.microsoft.com/rest/api/azure/).  

Vervang de tekst in de volgende voorbeelden {} met de exemplaarnamen om te bepalen van de gekoppelde resource.

### <a name="delete-an-entire-workspace"></a>Een volledige-werkruimte verwijderen

Gebruik deze aanroep om te verwijderen van een hele werkruimte.  
> [!WARNING]
> Alle gegevens worden verwijderd en de werkruimte wordt niet langer worden gebruikt.

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2018-03-01-preview

### <a name="delete-models"></a>Verwijderen van modellen

Gebruik deze aanroep voor een lijst van modellen en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2018-03-01-preview

Afzonderlijke modellen kunnen worden verwijderd met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2018-03-01-preview

### <a name="delete-assets"></a>Assets verwijderen

Gebruik deze aanroep voor een lijst van assets en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2018-03-01-preview

Afzonderlijke elementen kunnen worden verwijderd met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2018-03-01-preview

### <a name="delete-images"></a>Verwijderen van installatiekopieën

Gebruik deze aanroep voor een lijst van afbeeldingen en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2018-03-01-preview

Afzonderlijke afbeeldingen kunnen worden verwijderd met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2018-03-01-preview

### <a name="delete-services"></a>Services verwijderen

Gebruik deze aanroep voor een lijst met services en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2018-03-01-preview

Afzonderlijke services kunnen worden verwijderd met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2018-03-01-preview

## <a name="export-service-data-with-the-rest-api"></a>Exporteren van de servicegegevens met de REST-API

Als u wilt exporteren, kunnen de volgende API-aanroepen worden gemaakt met de HTTP GET-bewerking. Deze zijn gemachtigd door een `Authorization: Bearer <arm-token>` header in de aanvraag, waar `<arm-token>` het AAD-toegangstoken voor het eindpunt is `https://management.core.windows.net/`  

Zie voor meer informatie over dit token ophalen en Azure-eindpunten worden aangeroepen, [Azure REST API-documentatie](https://docs.microsoft.com/rest/api/azure/).   

Vervang de tekst in de volgende voorbeelden {} met de exemplaarnamen om te bepalen van de gekoppelde resource.

### <a name="export-workspace-information"></a>Werkruimtegegevens exporteren

Gebruik deze aanroep voor een lijst van alle werkruimten:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces?api-version=2018-03-01-preview

Informatie over een afzonderlijke werkruimte kan worden verkregen door:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2018-03-01-preview

### <a name="export-compute-information"></a>Compute-gegevens exporteren

Alle compute-doelen die is gekoppeld aan een werkruimte kunnen worden verkregen door:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Informatie over een enkele compute-doel kan worden verkregen door:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{computeName}?api-version=2018-03-01-preview

### <a name="export-run-history-data"></a>Uitvoeringsgeschiedenisgegevens exporteren
Gebruik deze aanroep voor een lijst van alle experimenten en hun informatie:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments 

Alle uitvoeringen voor een bepaalde experiment kunnen worden verkregen door:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs 

Uitvoeringsgeschiedenis items kunnen worden verkregen door:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs/{runId} 

Alle uitvoeren metrische gegevens voor een experiment kan worden verkregen door:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics 

Één uitvoeren metrische waarde kan worden verkregen door:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics/{metricId}
    
### <a name="export-artifacts"></a>Exporteren van artefacten

Gebruik deze aanroep voor een lijst van artefacten en hun paden:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/artifacts/origins/ExperimentRun/containers/{runId}
    
### <a name="export-notifications"></a>Exporteren van meldingen

Gebruik deze aanroep voor een lijst met opgeslagen taken:     

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/tasks

Meldingen voor een enkele taak kunnen worden verkregen door:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}tasks/{taskId}

### <a name="export-data-stores"></a>Opgeslagen gegevens exporteren

Gebruik deze aanroep voor een lijst van gegevensarchieven:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores

Afzonderlijke gegevensarchieven kunnen worden verkregen door:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores/{name}

### <a name="export-models"></a>Modellen exporteren

Gebruik deze aanroep voor een lijst van modellen en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2018-03-01-preview

Afzonderlijke modellen kunnen worden verkregen door:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2018-03-01-preview

### <a name="export-assets"></a>Exporteren van assets

Gebruik deze aanroep voor een lijst van assets en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2018-03-01-preview

Afzonderlijke elementen kunnen worden verkregen door:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2018-03-01-preview

### <a name="export-images"></a>Exporteren van afbeeldingen

Gebruik deze aanroep voor een lijst van afbeeldingen en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2018-03-01-preview

Afzonderlijke afbeeldingen kunnen worden verkregen door:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2018-03-01-preview

### <a name="export-services"></a>Exporteren van services

Gebruik deze aanroep voor een lijst met services en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2018-03-01-preview

Afzonderlijke services kunnen worden verkregen door:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2018-03-01-preview

### <a name="export-pipeline-experiments"></a>Pijplijn experimenten exporteren

Afzonderlijke experimenten kunnen worden verkregen door:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Experiments/{experimentId}

### <a name="export-pipeline-graphs"></a>Pijplijn grafieken exporteren

Afzonderlijke grafieken kunnen worden verkregen door:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Graphs/{graphId}

### <a name="export-pipeline-modules"></a>Exporteren-Pijplijnmodules

Modules kunnen worden verkregen door:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Modules/{id}

### <a name="export-pipeline-templates"></a>Pijplijn sjablonen exporteren

Sjablonen kunnen worden verkregen door:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Templates/{templateId}

### <a name="export-pipeline-data-sources"></a>Exporteren van de pijplijn-gegevensbronnen

Gegevensbronnen kunnen worden verkregen door:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/DataSources/{id}

## <a name="delete-visual-interface-assets"></a>Visuele interface assets verwijderen

In de visuele interface waar u uw experiment hebt gemaakt, kunt u afzonderlijke assets verwijderen:

1. Aan de linkerkant, selecteer het type van de asset die u wilt verwijderen.

    ![Assets verwijderen](media/how-to-export-delete-data.md/delete-experiment.png)

1. In de lijst, selecteert u de afzonderlijke elementen te verwijderen.

1. Selecteer op de onderste **verwijderen**.

## <a name="export-visual-interface-data"></a>Visuele interface gegevens exporteren

In de visuele interface waar u uw experiment hebt gemaakt, exporteert u gegevens die u hebt toegevoegd:

1. Selecteer aan de linkerkant **gegevens**.

1. Selecteer op de bovenkant **mijn gegevenssets** of **voorbeelden** te vinden van de gegevens die u wilt exporteren.

    ![Gegevens downloaden](media/how-to-export-delete-data.md/download-data.png)

1. In de lijst, selecteert u de afzonderlijke gegevenssets te exporteren.

1. Selecteer op de onderste **downloaden**.