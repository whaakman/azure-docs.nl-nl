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
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 2992ec9f43aac9e0d80c5e42873d26ac3a9c3fd1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64916982"
---
# <a name="use-the-cli-extension-for-azure-machine-learning-service"></a>Gebruik de CLI-extensie voor Azure Machine Learning-service

De CLI van Azure Machine Learning is een uitbreiding van de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), een platformoverschrijdende opdrachtregelinterface voor het Azure-platform. Deze extensie bevat opdrachten voor het werken met de Azure Machine Learning-service vanaf de opdrachtregel. Hiermee kunt u uw machine learning-werkstromen automatiseren. U kunt bijvoorbeeld de volgende acties uitvoeren:

+ Uitvoeren van experimenten voor het maken van machine learning-modellen

+ Machine learning-modellen voor het gebruik van de klant registreren

+ Verpakken, implementeren en bijhouden van de levenscyclus van machine learning-modellen

De CLI is geen vervanging voor de SDK van Azure Machine Learning. Het is een aanvullende hulpprogramma dat is geoptimaliseerd voor het afhandelen van maximaal geparameteriseerde taken die aan zichzelf goed voor automation.

## <a name="prerequisites"></a>Vereisten

* Voor het gebruik van de CLI, moet u een Azure-abonnement hebben. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* De [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="install-the-extension"></a>De extensie installeren

Gebruik de volgende opdracht voor het installeren van de Machine Learning CLI-extensie:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Van de voorbeeldbestanden die u met de onderstaande opdrachten gebruiken kunt vindt [hier](http://aka.ms/azml-deploy-cloud).

Wanneer u hierom wordt gevraagd, selecteert u `y` voor het installeren van de extensie.

Om te controleren of de extensie is geïnstalleerd, gebruikt u de volgende opdracht uit om een lijst met ML-specifieke subopdrachten weer te geven:

```azurecli-interactive
az ml -h
```

## <a name="remove-the-extension"></a>De extensie verwijderen

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

+ Een configuratie van de werkruimte koppelen aan een map om in te schakelen contextuele awareness CLI.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

+ Een Azure blob-container als gegevensopslag koppelen.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

+ Een AKS-cluster koppelen als een Compute-doel.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myrg -w myworkspace
    ```

+ Maak een nieuwe AMLcompute-doel

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```
    
## <a id="experiments"></a>Voer experimenten

+ Een configuratie van de werkruimte koppelen aan een map om in te schakelen contextuele awareness CLI.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

* Start een uitvoering van uw experiment. Wanneer u deze opdracht gebruikt, geef de naam van het bestand runconfig (de tekst vóór \*.runconfig als u uw bestandssysteem wilt) op basis van de parameter - c.

    ```azurecli-interactive
    az ml run submit-script -c local -e testexperiment train.py
    ```

* Een lijst van experimenten bekijken:

    ```azurecli-interactive
    az ml experiment list
    ```

## <a name="model-registration-profiling-deployment"></a>Model-registratie, profilering, implementatie

De volgende opdrachten laten zien hoe u een getraind model registreert en implementeert u deze vervolgens als een productieservice:

+ Registreer een model met Azure Machine Learning:

  ```azurecli-interactive
  az ml model register -n mymodel -p sklearn_regression_model.pkl
  ```

+ Uw model implementeert naar AKS

  ```azurecli-interactive
  az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
  ```
