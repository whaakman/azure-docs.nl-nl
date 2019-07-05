---
title: Machine learning-CLI-extensie
titleSuffix: Azure Machine Learning service
description: Meer informatie over de Azure Machine Learning CLI-extensie voor de Azure CLI. De Azure-CLI is een opdrachtregelprogramma voor meerdere platformen die u kunt werken met resources in de Azure-cloud. De Machine Learning-extensie kunt u werken met de Azure Machine Learning-Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 601a6139b81e45fa5005b7510189eac594c29fb0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67475985"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Gebruik de CLI-extensie voor Azure Machine Learning-service

De CLI van Azure Machine Learning is een uitbreiding van de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), een platformoverschrijdende opdrachtregelinterface voor het Azure-platform. Deze extensie bevat opdrachten voor het werken met de Azure Machine Learning-service. Hiermee kunt u uw machine learning-activiteiten automatiseren. De volgende lijst bevat enkele voorbeeld-acties die u met de CLI-extensie kunt doen:

+ Uitvoeren van experimenten voor het maken van machine learning-modellen

+ Machine learning-modellen voor het gebruik van de klant registreren

+ Verpakken, implementeren en bijhouden van de levenscyclus van machine learning-modellen

De CLI is geen vervanging voor de SDK van Azure Machine Learning. Het is een aanvullende hulpprogramma dat is geoptimaliseerd voor het afhandelen van maximaal geparameteriseerde taken die aan zichzelf goed voor automation.

## <a name="prerequisites"></a>Vereisten

* Voor het gebruik van de CLI, moet u een Azure-abonnement hebben. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* De [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="full-reference-docs"></a>Volledige naslaginformatie docs

Zoek de [referentiedocumenten voor de azure-cli-ml-extensie van Azure CLI volledige](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/?view=azure-cli-latest).

## <a name="install-the-extension"></a>De extensie installeren

Gebruik de volgende opdracht voor het installeren van de Machine Learning CLI-extensie:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Van de voorbeeldbestanden die u met de onderstaande opdrachten gebruiken kunt vindt [hier](https://aka.ms/azml-deploy-cloud).

Wanneer u hierom wordt gevraagd, selecteert u `y` voor het installeren van de extensie.

Om te controleren of de extensie is geïnstalleerd, gebruikt u de volgende opdracht uit om een lijst met ML-specifieke subopdrachten weer te geven:

```azurecli-interactive
az ml -h
```

## <a name="update-the-extension"></a>De extensie bijwerken

Gebruik de volgende opdracht voor het bijwerken van de Machine Learning CLI-extensie:

```azurecli-interactive
az extension update -n azure-cli-ml
```


## <a name="remove-the-extension"></a>Verwijder de extensie

Als u wilt verwijderen van de CLI-extensie, gebruik de volgende opdracht:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Resourcebeheer

De volgende opdrachten laten zien hoe u de CLI gebruiken voor het beheren van resources die worden gebruikt door Azure Machine Learning.

+ Als u nog geen een, moet u een resourcegroep maken:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Maak een Azure Machine Learning-service-werkruimte:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    Zie voor meer informatie, [az ml-werkruimte maken](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create).

+ Een configuratie van de werkruimte koppelen aan een map om in te schakelen contextuele awareness CLI.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Deze opdracht maakt u een `.azureml` submap dat voorbeeld runconfig en conda-omgeving bevat. Het bevat ook een `config.json` -bestand dat wordt gebruikt om te communiceren met uw Azure Machine Learning-werkruimte.

    Zie voor meer informatie, [az ml-map koppelen](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

+ Een Azure blob-container als gegevensopslag koppelen.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Zie voor meer informatie, [az ml gegevensopslag koppelen-blob](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-attach-blob).

    
+ Een AKS-cluster koppelen als een Compute-doel.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Zie voor meer informatie, [az ml computetarget aks koppelen](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks)

+ Maak een nieuwe AMLcompute doel.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```

    Zie voor meer informatie, [az ml computetarget maken amlcompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute).

## <a id="experiments"></a>Voer experimenten

* Start een uitvoering van uw experiment. Wanneer u deze opdracht gebruikt, geef de naam van het bestand runconfig (de tekst vóór \*.runconfig als u uw bestandssysteem wilt) op basis van de parameter - c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > De `az ml folder attach` opdracht maakt u een `.azureml` submap, die twee voorbeeld runconfig bestanden bevat. 
    >
    > Als u een Python-script waarmee u een configuratieobject uitvoeren via een programma maakt hebt, kunt u [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) als een runconfig opslaan.
    >
    > Zie voor meer voorbeeld runconfig bestanden, [ https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml ](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Zie voor meer informatie, [az ml verzenden-script uitvoeren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

* Een lijst van experimenten bekijken:

    ```azurecli-interactive
    az ml experiment list
    ```

    Zie voor meer informatie, [az ml experimenten lijst](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

## <a name="model-registration-profiling-deployment"></a>Model-registratie, profilering, implementatie

De volgende opdrachten laten zien hoe u een getraind model registreert en implementeert u deze vervolgens als een productieservice:

+ Registreer een model met Azure Machine Learning:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Zie voor meer informatie, [az ml-model registreren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register).

+ **OPTIONELE** uw model voor een optimale CPU en geheugen waarden ophalen voor de implementatie van het profiel.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Zie voor meer informatie, [az ml-model profiel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

+ Uw model implementeert naar AKS
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Hier volgt een voorbeeld `inferenceconfig.json` document:
    ```json
    {
    "entryScript": "score.py",
    "runtime": "python",
    "condaFile": "myenv.yml",
    "extraDockerfileSteps": null,
    "sourceDirectory": null,
    "enableGpu": false,
    "baseImage": null,
    "baseImageRegistry": null
    }
    ```
    Hier volgt een voorbeeld van het document 'deploymentconfig.json':
    ```json
    {
    "computeType": "aks",
    "ComputeTarget": "akscomputetarget"
    }
    ```

    Zie voor meer informatie, [az ml-model implementeren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).


## <a name="next-steps"></a>Volgende stappen

* [Opdrachten voor de Machine Learning CLI-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest).

* [Trainen en implementeren van machine learning-modellen met behulp van Azure-pijplijnen](/azure/devops/pipelines/targets/azure-machine-learning)
