---
title: Exporteren of verwijder experimenteren of model in beheergegevens - Azure Machine Learning | Microsoft Docs
description: U kunt in Azure Machine Learning exporteren of verwijderen van uw accountgegevens met betrekking tot experimenteren of het model management met de Azure-portal, CLI, SDK en geverifieerde REST-API's. Dit artikel ziet u hoe.
services: machine-learning
author: cjgronlund
ms.author: cgronlun
manager: haining
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 7db37865c99908e0fd44be3ec04a8493d190e941
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833509"
---
# <a name="export-or-delete-your-experimentation-or-model-management-data-in-machine-learning"></a>Exporteren of verwijder uw experimenteren of beheergegevens model in Machine Learning

U kunt in Azure Machine Learning exporteren of verwijderen van uw accountgegevens met betrekking tot experimenteren of het model management met de geverifieerde REST-API. In dit artikel leest u hoe.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-account-data"></a>De gegevens van uw account beheren
Product-gegevens die zijn opgeslagen door Azure Machine Learning-model te experimenteren en management is beschikbaar voor exporteren en verwijderen via de Azure-portal, CLI, SDK en geverifieerde REST-API's. Telemetriegegevens zijn toegankelijk via de Privacy van de Azure-portal. 

In Azure Machine Learning, worden persoonlijke gegevens van gebruikersgegevens in uitvoeringsgeschiedenis documenten en telemetrie records van de interactie van sommige gebruikers met de service bestaat.

## <a name="delete-account-data-with-the-rest-api"></a>Verouderde accountgegevens van een met de REST-API 

Om gegevens verwijdert, kunnen de volgende API-aanroepen worden gemaakt met de term HTTP DELETE. Deze worden geautoriseerd door een `Authorization: Bearer <arm-token>` header in de aanvraag waar `<arm-token>` het AAD-toegangstoken voor het eindpunt is `https://management.core.windows.net/` eindpunt.  

Zie voor meer informatie over dit token ophalen en roept u Azure-eindpunten, [Azure REST API-documentatie](https://docs.microsoft.com/rest/api/azure/).  

Vervang de tekst in de volgende voorbeelden {} met de instantienamen om te bepalen van de bijbehorende resource.

## <a name="delete-from-a-hosting-account"></a>Verwijderen van een hosting-account

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningModelManagement/accounts/{account-name}?api-version=2017-09-01-preview      

## <a name="delete-from-the-model-management-service"></a>Verwijderen van de service model-management

### <a name="model-document"></a>Modeldocument
Gebruik deze aanroep om een lijst met modellen en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Afzonderlijke modellen kunnen worden verwijderd met:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview

### <a name="manifest-document"></a>Manifestdocument
Gebruik deze aanroep om een lijst met alle manifesten en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Afzonderlijke manifesten kunnen worden verwijderd met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="service-documents"></a>Service-documenten
Gebruik deze aanroep om een lijst met alle services en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Afzonderlijke services kunnen worden verwijderd met:    

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="image-document"></a>Document-afbeelding
Gebruik deze aanroep om een lijst met alle installatiekopieën en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Afzonderlijke afbeeldingen kunnen worden verwijderd met:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview

## <a name="delete-run-history-artifact-and-notification-data"></a>Voer geschiedenis, artefacten en notification-gegevens verwijderen
Voer geschiedenis, artefacten en notification winkels voor een project zijn verwijderd na het verwijderen van het bijbehorende projectdocument:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview
    
## <a name="delete-from-experimentation-account-resource-provider"></a>Verwijderen van de resourceprovider experimenteren-account
Project-documenten zijn verwijderd met:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview

Documenten in de werkruimte zijn verwijderd met:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}?api-version=2017-05-01-preview

De account volledige experimenteren is met verwijderd:
    
    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}?api-version=2017-05-01-preview


## <a name="export-service-data-with-the-rest-api"></a>Exporteren van de servicegegevens met de REST-API
Als u wilt exporteren, kunnen de volgende API-aanroepen worden gemaakt met de term HTTP GET. Deze worden geautoriseerd door een `Authorization: Bearer <arm-token>` header in de aanvraag waar `<arm-token>` is het AAD-toegangstoken voor het eindpunt `https://management.core.windows.net/`  

Zie voor meer informatie over dit token ophalen en roept u Azure-eindpunten, [Azure REST API-documentatie](https://docs.microsoft.com/rest/api/azure/).   

Vervang de tekst in de volgende voorbeelden {} met de instantienamen om te bepalen van de bijbehorende resource.

## <a name="export-hosting-account-data"></a>Hosting accountgegevens exporteren

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningModelManagement/accounts/{accountName}?api-version=2017-09-01-preview     

## <a name="export-model-management-service-data"></a>Model management-servicegegevens exporteren
### <a name="model-document"></a>Modeldocument

Gebruik deze aanroep om een lijst met modellen en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Afzonderlijke modellen kunnen worden verkregen met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview 

### <a name="manifests"></a>Manifesten
Gebruik deze aanroep om een lijst met alle manifesten en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Afzonderlijke manifesten kunnen worden verkregen met:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="services"></a>Services
Gebruik deze aanroep om een lijst met alle services en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Afzonderlijke services kunnen worden verkregen met: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="images"></a>Installatiekopieën
Gebruik deze aanroep om een lijst met alle installatiekopieën en de id's:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Afzonderlijke services kunnen worden verkregen met: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview     

## <a name="export-compute-data"></a>Compute gegevens exporteren
### <a name="compute-clusters"></a>Rekenclusters
Gebruik deze aanroep om een lijst met alle computerclusters en hun namen:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Afzonderlijke clusters kunnen worden verkregen met:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{compute-name}?api-version=2018-03-01-preview

### <a name="operationalization-clusters"></a>Uitoefening clusters
Gebruik deze aanroep om een lijst met alle clusters en hun namen:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters?api-version=2017-06-01-preview

Afzonderlijke clusters kunnen worden verkregen met:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters/{clusterName}?api-version=2017-06-01-preview

## <a name="export-run-history-data"></a>Export geschiedenisgegevens worden uitgevoerd
Gebruik deze aanroep om een lijst met alle wordt uitgevoerd en de id's:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs

Gebruik deze aanroep om een lijst met alle experimenten en de id's:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments

Geschiedenis items kunnen worden verkregen met uitvoeren:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}

Voer items kunnen worden verkregen met metrische gegevens:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runmetrics

Voer experimenten kunnen worden verkregen met:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments/{experimentId}    

Geschiedenis van artefacten uitvoeren:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts

Geschiedenis van artefacten URI's worden uitgevoerd:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacturi?name={artifactName}

## <a name="export-artifacts"></a>Exporteren van artefacten
Gebruik deze aanroep om een lijst met activa en hun namen:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets

Gebruik deze aanroep om een lijst met artefacten en hun paden:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/origins/{origin}/containers/{runId}
        
        
### <a name="artifact-contents"></a>Inhoud van artefacten

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/contentinfo/ExperimentRun/{runId}/{artifactPath}

### <a name="artifact-documents"></a>Artefacten documenten

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts
        
### <a name="assets"></a>Assets

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets/name/{name}

## <a name="export-notifications"></a>Meldingen exporteren

1. Ga naar de [sectie gebruikers van de Azure portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/), en selecteer vervolgens een gebruiker vanuit de **naam** kolom. 
2. Opmerking de **Object-ID**, en deze gebruiken in de volgende oproep verzenden:     

        https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/users/{objectId}/jobs

## <a name="export-experimentation-account-information"></a>Accountgegevens experimenteren exporteren
### <a name="experimentation-account-information"></a>Accountgegevens experimenteren

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}?api-version=2017-05-01-preview
        
### <a name="workspace-information"></a>Werkruimtegegevens

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}?api-version=2017-05-01-preview  

### <a name="project-information"></a>Projectgegevens

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}?api-version=2017-05-01-preview
