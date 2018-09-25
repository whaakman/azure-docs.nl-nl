---
title: Exporteren of verwijderen van experimenten of model management-gegevens - Azure Machine Learning | Microsoft Docs
description: U kunt in Azure Machine Learning, exporteren of verwijderen van uw accountgegevens met betrekking tot beheer van experimenten of het model met de Azure portal, CLI, SDK en geverifieerde REST-API's. In dit artikel ziet u hoe u.
services: machine-learning
author: cjgronlund
ms.author: cgronlun
manager: haining
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 05/22/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 7daa3bdf9fb51fee6b0e190625f07e0d14c3d1f4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995139"
---
# <a name="export-or-delete-your-experimentation-or-model-management-data-in-machine-learning"></a>Exporteren of verwijderen van uw experimenten of beheergegevens model in Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

U kunt in Azure Machine Learning, exporteren of verwijderen van uw accountgegevens met betrekking tot beheer van experimenten of het model met de geverifieerde REST-API. In dit artikel leest u hoe.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-account-data"></a>De accountgegevens van uw beheren
In het product-gegevens die zijn opgeslagen door Azure Machine Learning experimenten en Modelbeheer-management is beschikbaar voor exporteren en verwijderen via de Azure portal, CLI, SDK en geverifieerde REST-API's. Telemetriegegevens zijn toegankelijk via de Privacy van de Azure-portal. 

In Azure Machine Learning bestaat persoonlijke gegevens uit gegevens van de gebruiker in de uitvoeringsgeschiedenis documenten en telemetrie-records van de interactie van sommige gebruikers met de service.

## <a name="delete-account-data-with-the-rest-api"></a>Verwijderen van gegevens met de REST-API 

Als u wilt verwijderen van gegevens, kunnen de volgende API-aanroepen worden gemaakt met de bewerking HTTP DELETE. Deze zijn gemachtigd door een `Authorization: Bearer <arm-token>` header in de aanvraag, waar `<arm-token>` het AAD-toegangstoken voor het eindpunt is `https://management.core.windows.net/` eindpunt.  

Zie voor meer informatie over dit token ophalen en Azure-eindpunten worden aangeroepen, [Azure REST API-documentatie](https://docs.microsoft.com/rest/api/azure/).  

Vervang de tekst in de volgende voorbeelden {} met de exemplaarnamen om te bepalen van de gekoppelde resource.

## <a name="delete-from-a-hosting-account"></a>Verwijderen uit een hosting-account

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningModelManagement/accounts/{account-name}?api-version=2017-09-01-preview      

## <a name="delete-from-the-model-management-service"></a>Verwijderen van de model-management-service

### <a name="model-document"></a>Modeldocument
Gebruik deze aanroep voor een lijst van modellen en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Afzonderlijke modellen kunnen worden verwijderd met:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview

### <a name="manifest-document"></a>Manifest van document
Gebruik deze aanroep voor een lijst van alle manifesten en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Afzonderlijke manifesten kunnen worden verwijderd met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="service-documents"></a>Service-documenten
Gebruik deze aanroep voor een lijst van alle services en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Afzonderlijke services kunnen worden verwijderd met:    

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="image-document"></a>Document-afbeelding
Gebruik deze aanroep voor een lijst van alle installatiekopieën en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Afzonderlijke afbeeldingen kunnen worden verwijderd met:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview

## <a name="delete-run-history-artifact-and-notification-data"></a>Voer geschiedenis en artefact melding gegevens verwijderen
Voer geschiedenis en artefact melding winkels voor een project zijn verwijderd na het verwijderen van de bijbehorende project-document:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview
    
## <a name="delete-from-experimentation-account-resource-provider"></a>Verwijderen van de resourceprovider van experimenten-account
Project-documenten zijn verwijderd met:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview

Documenten in de werkruimte worden verwijderd met:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}?api-version=2017-05-01-preview

Het hele experimenten-account is verwijderd:
    
    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}?api-version=2017-05-01-preview


## <a name="export-service-data-with-the-rest-api"></a>Exporteren van de servicegegevens met de REST-API
Als u wilt exporteren, kunnen de volgende API-aanroepen worden gemaakt met de HTTP GET-bewerking. Deze zijn gemachtigd door een `Authorization: Bearer <arm-token>` header in de aanvraag, waar `<arm-token>` het AAD-toegangstoken voor het eindpunt is `https://management.core.windows.net/`  

Zie voor meer informatie over dit token ophalen en Azure-eindpunten worden aangeroepen, [Azure REST API-documentatie](https://docs.microsoft.com/rest/api/azure/).   

Vervang de tekst in de volgende voorbeelden {} met de exemplaarnamen om te bepalen van de gekoppelde resource.

## <a name="export-hosting-account-data"></a>Hosting accountgegevens exporteren

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningModelManagement/accounts/{accountName}?api-version=2017-09-01-preview     

## <a name="export-model-management-service-data"></a>Model management-servicegegevens exporteren
### <a name="model-document"></a>Modeldocument

Gebruik deze aanroep voor een lijst van modellen en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Afzonderlijke modellen kunnen worden verkregen door:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview 

### <a name="manifests"></a>Manifesten
Gebruik deze aanroep voor een lijst van alle manifesten en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Afzonderlijke manifesten kunnen worden verkregen door:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="services"></a>Services
Gebruik deze aanroep voor een lijst van alle services en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Afzonderlijke services kunnen worden verkregen door: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="images"></a>Installatiekopieën
Gebruik deze aanroep voor een lijst van alle installatiekopieën en hun-id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Afzonderlijke services kunnen worden verkregen door: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview     

## <a name="export-compute-data"></a>Compute-gegevens exporteren
### <a name="compute-clusters"></a>Rekenclusters
Gebruik deze aanroep voor een lijst van alle rekenclusters en hun namen:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Afzonderlijke clusters kunnen worden verkregen door:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{compute-name}?api-version=2018-03-01-preview

### <a name="operationalization-clusters"></a>Uitoefening clusters
Gebruik deze aanroep voor een lijst van alle clusters en hun namen:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters?api-version=2017-06-01-preview

Afzonderlijke clusters kunnen worden verkregen door:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters/{clusterName}?api-version=2017-06-01-preview

## <a name="export-run-history-data"></a>Uitvoeringsgeschiedenisgegevens exporteren
Gebruik deze aanroep voor een lijst van alle uitvoeringen en hun-id's:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs

Gebruik deze aanroep voor een lijst van alle experimenten en hun-id's:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments

Uitvoeringsgeschiedenis items kunnen worden verkregen door:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}

Metrische gegevens over items kunnen worden verkregen door uitvoeren:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runmetrics

Voer experimenten kunnen worden verkregen door:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments/{experimentId}    

Geschiedenis-artefacten worden uitgevoerd:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts

Geschiedenis van artefacten URI's worden uitgevoerd:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacturi?name={artifactName}

## <a name="export-artifacts"></a>Exporteren van artefacten
Gebruik deze aanroep voor een lijst van assets en hun namen:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets

Gebruik deze aanroep voor een lijst van artefacten en hun paden:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/origins/{origin}/containers/{runId}
        
        
### <a name="artifact-contents"></a>De inhoud van het artefact

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/contentinfo/ExperimentRun/{runId}/{artifactPath}

### <a name="artifact-documents"></a>Artefact documenten

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts
        
### <a name="assets"></a>Assets

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets/name/{name}

## <a name="export-notifications"></a>Exporteren van meldingen

1. Ga naar de [sectie van de gebruikers van de Azure-portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/), en selecteer vervolgens een gebruiker vanuit de **naam** kolom. 
2. Houd er rekening mee de **Object-ID**, en deze gebruiken in de volgende oproep verzenden:     

        https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/users/{objectId}/jobs

## <a name="export-experimentation-account-information"></a>Exporteren van gegevens over experimenten-account
### <a name="experimentation-account-information"></a>Gegevens over experimenten-account

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}?api-version=2017-05-01-preview
        
### <a name="workspace-information"></a>Werkruimtegegevens

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}?api-version=2017-05-01-preview  

### <a name="project-information"></a>Projectgegevens

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}?api-version=2017-05-01-preview
