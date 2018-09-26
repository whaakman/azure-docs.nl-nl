---
title: Over de Azure Machine Learning CLI-extensie
description: Meer informatie over de machine learning CLI-extensie voor Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 09/24/2018
ms.openlocfilehash: f8dae6de835173181430a98c19c7dd1fb3ebaa9f
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47158900"
---
# <a name="what-is-the-azure-machine-learning-cli"></a>Wat is Azure Machine Learning CLI?

De extensie Azure Machine Learning-opdrachtregelinterface (CLI) is voor data scientists en ontwikkelaars die werken met Azure Machine Learning-service. Hiermee kunt u snel machine learning-werkstromen automatiseren en plaats u ze in productie nemen, zoals:
+ Uitvoeren van experimenten voor het maken van machine learning-modellen

+ Machine learning-modellen voor het gebruik van de klant registreren

+ Verpakken, implementeren en bijhouden van de levenscyclus van machine learning-modellen

Deze machine learning-CLI is een uitbreiding van [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) en is gebaseerd op de op Python gebaseerde <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> voor Azure Machine Learning-service.

> [!NOTE]
> De CLI is momenteel in een vroege preview en wordt bijgewerkt.

## <a name="installing-and-uninstalling"></a>Installeren en verwijderen

U kunt de CLI met de volgende opdracht uit ons voorbeeld PyPi index installeren:
```AzureCLI
az extension add -s https://azuremlsdktestpypi.blob.core.windows.net/wheels/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1/azure_cli_ml-0.1.50-py2.py3-none-any.whl --pip-extra-index-urls  https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
```

U kunt de CLI met de volgende opdracht verwijderen:
```AzureCLI
az extension remove -n azure-cli-ml
```

U kunt bijwerken met de CLI met de **verwijderen** en **toevoegen** bovenstaande stappen.

## <a name="using-the-cli-vs-the-sdk"></a>Met behulp van de CLI vergeleken met de SDK
De CLI is beter geschikt voor automation op een dev ops-persona of als onderdeel van een continue integratie en levering pijplijn. Deze is geoptimaliseerd voor het afhandelen van niet-frequente en maximaal geparameteriseerde taken. 

Voorbeelden zijn:
- COMPUTE inrichten
- met parameters experiment verzenden
- model-registratie, het maken van installatiekopieën
- Service-implementatie

Gegevenswetenschappers worden aanbevolen om het gebruik van de Azure ML-SDK.

## <a name="common-machine-learning-cli-commands"></a>Reguliere machine learning CLI-opdrachten
> [!NOTE]
> Voorbeeld van bestanden die u gebruiken kunt om uit te voeren is de onderstaande opdrachten vindt u [hier.](https://github.com/Azure/MachineLearningNotebooks/tree/cli/cli)

Gebruik de uitgebreide set `az ml` interactie van de service in een opdrachtregelomgeving, met inbegrip van Azure-portal cloudshell-opdrachten.

Hier volgt een voorbeeld van veelgebruikte opdrachten:

### <a name="workspace-creation--compute-setup"></a>Werkruimte maken en compute instellen

+ Maak een Azure Machine Learning-werkruimte en de resource op het hoogste niveau voor machine learning.
   ```AzureCLI
   az ml workspace create -n myworkspace -g myresourcegroup
   ```

+ Hiermee stelt u de CLI te maken standaard gebruik van deze werkruimte.
   ```AzureCLI
   az configure --defaults aml_workspace=myworkspace group=myresourcegroup
   ```

+ Maak een DSVM (data science VM). U kunt ook maken BatchAI clusters voor een gedistribueerde training of AKS-clusters voor implementatie.
  ```AzureCLI
  az ml computetarget setup dsvm -n mydsvm
  ```

### <a name="experiment-submission"></a>Verzending van experiment
+ Koppelen aan een project (configuratie uitvoeren) voor het indienen van een experiment. Dit wordt gebruikt voor het bijhouden van uw experimenten.
  ```AzureCLI
  az ml project attach --experiment-name myhistory
  ```

+ Dien een experiment op basis van de Azure Machine Learning-service op de compute-doel van uw keuze. In dit voorbeeld wordt uitgevoerd op basis van de lokale compute-omgeving. Zorg ervoor dat uw bestand conda-omgeving vastlegt uw python-afhankelijkheden.

  ```AzureCLI
  az ml run submit -c local train.py
  ```

+ Bekijk een lijst met ingediende experimenten.
```AzureCLI
az ml history list
```

### <a name="model-registration-image-ceation--deployment"></a>Model registratie, afbeelding ceation en implementatie

+ Registreer een model met Azure Machine Learning.
  ```AzureCLI
  az ml model register -n mymodel -m sklearn_regression_model.pkl
  ```

+ Een installatiekopie bevat uw machine learning-model en de afhankelijkheden maken. 
  ```AzureCLI
  az ml image create container -n myimage -r python -m mymodel:1 -f score.py -c myenv.yml
  ```

+ Uw App-pakket model implementeert naar doelen, met inbegrip van ACI en AKS.
  ```AzureCLI
  az ml service create aci -n myaciservice --image-id myimage:1
  ```
    
## <a name="full-command-list"></a>Volledige lijst met
U vindt de volledige lijst met opdrachten voor de CLI-extensie (en de bijbehorende ondersteunde parameters) door te voeren ```az ml COMMANDNAME -h```. 
