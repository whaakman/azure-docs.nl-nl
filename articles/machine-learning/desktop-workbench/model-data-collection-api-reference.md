---
title: Azure Machine Learning-Model gegevens verzameling API-naslaginformatie | Microsoft Docs
description: Azure Machine Learning-Model gegevens verzameling API-verwijzing.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: d9fee56d7748cdfd34f982fe79467f7d61c54926
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643667"
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Azure Machine Learning-Model gegevens verzameling API-naslaginformatie

Gegevensverzameling van model kunt u om modelinvoer en voorspellingen op basis van een machine learning-webservice te archiveren. Zie de [model gegevens verzameling gebruiksaanwijzing](how-to-use-model-data-collection.md) wilt weten hoe u voor het installeren van `azureml.datacollector` op uw Windows- en Linux-computer.

In deze API-referentiegids gebruiken we een stapsgewijze benadering voor het verzamelen van modelinvoer en voorspellingen van een machine learning-webservice.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Inschakelen van gegevensverzameling van model in Azure ML Workbench-omgeving

 Zoek naar conda\_dependencies.yml-bestand in uw project onder de map aml_config en hebt u uw conda\_dependencies.yml bestand omvatten de module azureml.datacollector onder de sectie pip zoals hieronder wordt weergegeven. Houd er rekening mee dat dit alleen een subsectie van een volledige conda is\_dependencies.yml bestand:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>U kunt op dit moment de data collector-module gebruiken in Azure ML Workbench door te voeren in de docker-modus. Lokale modus werkt mogelijk niet voor alle omgevingen.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Inschakelen van gegevensverzameling van model in het scoring-bestand

In het scoring-bestand dat wordt gebruikt voor uitoefening, data collector-module en de klasse ModelDataCollector te importeren:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Model data collector instantiëring
Een nieuwe instantie van een ModelDataCollector:

    dc = ModelDataCollector(model_name, identifier='default', feature_names=None, model_management_account_id='unknown', webservice_name='unknown', model_id='unknown', model_version='unknown')

Zie de details van de klasse en de Parameter:

### <a name="class"></a>Klasse
| Naam | Beschrijving |
|--------------------|--------------------|
| ModelDataCollector | Een klasse in azureml.datacollector naamruimte. Een exemplaar van deze klasse wordt gebruikt voor het verzamelen van gegevens van model. Een enkele scoring-bestand kan meerdere ModelDataCollectors bevatten. Elk exemplaar moet worden gebruikt voor het verzamelen van gegevens in een afzonderlijke locatie in het scoring-bestand zodat het schema van de verzamelde gegevens consistent blijft (dat wil zeggen, invoer en voorspelling)|


### <a name="parameters"></a>Parameters

| Naam | Type | Beschrijving |
|-------------|------------|-------------------------|
| model_name | tekenreeks | de naam van het model voor welke gegevens worden verzameld |
| ID | tekenreeks | de locatie in de code die deze gegevens, dat wil zeggen identificeert 'RawInput' of 'Voorspelling' |
| feature_names | lijst met tekenreeksen | een lijst van de onderdeelnamen die de csv-koptekst worden als opgegeven |
| model_management_account_id | tekenreeks | de id voor de Modelbeheer-account waar dit model is opgeslagen. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |
| webservice_name | tekenreeks | de naam van de webservice waarnaar dit model wordt geïmplementeerd. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |
| model_id | tekenreeks | De unieke id voor dit model in de context van een Modelbeheer-account. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |
| model_version | tekenreeks | het versienummer van dit model in de context van een Modelbeheer-account. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |



 

## <a name="collecting-the-model-data"></a>De modelgegevens verzamelen

U kunt de gegevens van het model met behulp van een exemplaar van de hierboven gemaakte ModelDataCollector verzamelen.

    dc.collect(input_data, user_correlation_id="")

Methode en de Parameter details bekijken:

### <a name="method"></a>Methode
| Naam | Beschrijving |
|--------------------|--------------------|
| verzamelen | Gebruikt voor het verzamelen van de gegevens voor een model invoer of de voorspelling|


### <a name="parameters"></a>Parameters

| Naam | Type | Beschrijving |
|-------------|------------|-------------------------|
| input_data | meerdere typen | de gegevens te verzamelen (momenteel de lijst met bestandstypen, numpy.array, pandas accepteert. DataFrame, pyspark.sql.DataFrame). Voor dataframe typen, als een header met de functienamen, bestaat is deze informatie opgenomen in de Gegevensdoel-(zonder expliciet onderdeelnamen doorgeven in de constructor ModelDataCollector) |
| user_correlation_id | tekenreeks | een optionele correlatie-id, die kan worden opgegeven door de gebruiker deze voorspelling correleren |

