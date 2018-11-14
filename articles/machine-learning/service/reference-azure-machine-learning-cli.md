---
title: Het gebruik van de Azure Machine Learning CLI-extensie
description: Meer informatie over de Azure Machine Learning CLI-extensie voor de Azure CLI. De Azure-CLI is een opdrachtregelprogramma voor meerdere platformen die u kunt werken met resources in de Azure-cloud. De Machine Learning-extensie kunt u werken met de Azure Machine Learning-Service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: f5c74055747cacbede479e12397bbb66ac74d10e
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615633"
---
# <a name="use-the-azure-machine-learning-cli-extension"></a>De Azure Machine Learning CLI-extensie gebruiken

De CLI van Azure Machine Learning is een uitbreiding van de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), een platformoverschrijdende opdrachtregelinterface voor het Azure-platform. Deze extensie bevat opdrachten voor het werken met de Azure Machine Learning-service vanaf de opdrachtregel. Hiermee kunt u scripts maken die uw machine learning-werkstromen automatiseren. Bijvoorbeeld, kunt u scripts maken die de volgende acties uitvoeren:

+ Uitvoeren van experimenten voor het maken van machine learning-modellen

+ Machine learning-modellen voor het gebruik van de klant registreren

+ Verpakken, implementeren en bijhouden van de levenscyclus van machine learning-modellen

De CLI is geen vervanging voor de SDK van Azure Machine Learning. Het is een aanvullende hulpprogramma dat is geoptimaliseerd voor het afhandelen van maximaal geparameteriseerde taken, zoals:

* Het maken van compute-resources

* met parameters experiment verzenden

* Registratie van model

* Het maken van installatiekopieën

* Service-implementatie

## <a name="prerequisites"></a>Vereisten

* De [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

> [!NOTE]
> Voor het gebruik van de CLI, moet u een Azure-abonnement hebben. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="install-the-extension"></a>De extensie installeren

Gebruik de volgende opdracht voor het installeren van de Machine Learning CLI-extensie:

```azurecli-interactive
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.68-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

Wanneer u hierom wordt gevraagd, selecteert u `y` voor het installeren van de extensie.

Om te controleren of de extensie is geïnstalleerd, gebruikt u de volgende opdracht uit om een lijst met ML-specifieke subopdrachten weer te geven:

```azurecli-interactive
az ml -h
```

> [!TIP]
> De extensie u moet bijwerken __verwijderen__ , en vervolgens __installeren__ deze. Hiermee installeert u de meest recente versie.

## <a name="remove-the-extension"></a>Verwijder de extensie

Als u wilt verwijderen van de CLI-extensie, gebruik de volgende opdracht:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Resourcebeheer

De volgende opdrachten laten zien hoe u de CLI gebruiken voor het beheren van resources die worden gebruikt door Azure Machine Learning.


+ Maak een Azure Machine Learning-service-werkruimte:

   ```azurecli-interactive
   az ml workspace create -n myworkspace -g myresourcegroup
   ```

+ Stel een standaardwerkruimte:

   ```azurecli-interactive
   az configure --defaults aml_workspace=myworkspace group=myresourcegroup
   ```

+ Maak een DSVM (data science VM). U kunt ook maken BatchAI clusters voor een gedistribueerde training of AKS-clusters voor implementatie.


  ```azurecli-interactive
  az ml computetarget setup dsvm -n mydsvm
  ```

## <a name="experiments"></a>Experimenten

De volgende opdrachten laten zien hoe u de CLI gebruiken om te werken met experimenten:

* Toevoegen aan een project (configuratie uitvoeren) voordat u een experiment verzendt:

    ```azurecli-interactive
    az ml project attach --experiment-name myhistory
    ```

* Start een uitvoering van uw experiment. Wanneer u deze opdracht gebruikt, Geef een compute-doel. In dit voorbeeld `local` maakt gebruik van de lokale computer met het trainen van het model door de `train.py` script:

    ```azurecli-interactive
    az ml run submit -c local train.py
    ```

* Een lijst met ingediende experimenten bekijken:

    ```azurecli-interactive
    az ml history list
    ```

## <a name="model-registration-image-creation--deployment"></a>Model registratie, het maken van installatiekopieën en implementatie

De volgende opdrachten laten zien hoe u een getraind model registreert en implementeert u deze vervolgens als een productieservice:

+ Registreer een model met Azure Machine Learning:

  ```azurecli-interactive
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Maken van een installatiekopie die uw machine learning-model en de afhankelijkheden bevat: 

  ```azurecli-interactive
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Een installatiekopie implementeren naar een compute-doel:

  ```azurecli-interactive
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
