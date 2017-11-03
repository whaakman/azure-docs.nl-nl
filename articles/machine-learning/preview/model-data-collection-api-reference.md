---
title: Naslaginformatie over Azure Machine Learning-Model gegevens verzameling API | Microsoft Docs
description: Naslaginformatie over Azure Machine Learning-Model gegevens verzameling API.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 7a0fda8a44d13bcaba84b4124d9b693c05874154
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Naslaginformatie over Azure Machine Learning-Model gegevens verzameling API

Gegevensverzameling model kunt u bij de archivering van model ingangen en de voorspellingen van een machine learning-webservice. Zie de [model gegevens verzameling how-to handleiding](how-to-use-model-data-collection.md) om te begrijpen hoe installeren `azureml.datacollector` op uw Windows- en Linux-machine.

In deze handleiding API gebruiken we een stapsgewijze benadering voor het model ingangen en voorspellingen verzamelen van een machine learning-webservice.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Model verzamelen in Azure ML-Workbench omgeving inschakelen

 Zoek naar conda\_dependencies.yml-bestand in uw project onder de map aml_config en hebben uw conda\_dependencies.yml bestand omvatten de module azureml.datacollector onder de sectie pip zoals hieronder wordt weergegeven. Houd er rekening mee dat dit alleen een subsectie van een volledige conda is\_dependencies.yml bestand:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>Op dit moment kunt u de data Collection module in Azure ML-Workbench door te voeren in de docker-modus. Lokale modus werkt mogelijk niet voor alle omgevingen.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Verzamelen in het bestand score model inschakelen

In het score-bestand dat wordt gebruikt voor uitoefening data collector-module en ModelDataCollector klasse te importeren:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Model data collector instantiëring
Een nieuwe instantie van een ModelDataCollector:

DC = ModelDataCollector (modelnaam, id = 'default', feature_names = geen model_management_account_id = 'Onbekend', webservice_name = 'Onbekend', model_id = 'Onbekend', model_version = 'Onbekend')

Zie de details van klasse en de Parameter:

### <a name="class"></a>Klasse
| Naam | Beschrijving |
|--------------------|--------------------|
| ModelDataCollector | Een klasse in azureml.datacollector naamruimte. Een exemplaar van deze klasse wordt gebruikt voor het verzamelen van modelgegevens. Een enkel bestand scoreprofiel kan meerdere ModelDataCollectors bevatten. Elke instantie moet worden gebruikt voor het verzamelen van gegevens op één discrete locatie in het score-bestand zodat het schema van de verzamelde gegevens consistent blijft (dat wil zeggen, invoer en voorspelling)|


### <a name="parameters"></a>Parameters

| Naam | Type | Beschrijving |
|-------------|------------|-------------------------|
| modelnaam | Tekenreeks | de naam van het model voor welke gegevens worden verzameld |
| ID | Tekenreeks | de locatie in de code die deze gegevens, dat wil zeggen identificeert 'RawInput' of 'Prediction' |
| feature_names | lijst met tekenreeksen | een lijst met onderdelen die de csv-koptekst als opgegeven geworden |
| model_management_account_id | Tekenreeks | de id voor het account van de model-beheerserver waarop dit model is opgeslagen. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |
| webservice_name | Tekenreeks | de naam van de webservice waarnaar dit model wordt geïmplementeerd. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |
| model_id | Tekenreeks | De unieke id voor dit model in de context van een model-management-account. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |
| model_version | Tekenreeks | het versienummer van dit model in de context van een model-management-account. Dit wordt automatisch gevuld wanneer modellen zijn geoperationaliseerd via AML |



 

## <a name="collecting-the-model-data"></a>De modelgegevens verzamelen

U kunt de gegevens van het model met een exemplaar van de hierboven gemaakte ModelDataCollector verzamelen.

    dc.collect(input_data, user_correlation_id="")

Zie de details van methode en de Parameter:

### <a name="method"></a>Methode
| Naam | Beschrijving |
|--------------------|--------------------|
| verzamelen | Gebruikt de gegevens voor een model invoer of de voorspelling te verzamelen|


### <a name="parameters"></a>Parameters

| Naam | Type | Beschrijving |
|-------------|------------|-------------------------|
| input_data | meerdere typen | de gegevens te verzamelen (momenteel de lijst met bestandstypen, numpy.array, pandas accepteert. DataFrame, pyspark.sql.DataFrame). Voor dataframe typen, als er een header met functienamen bestaat, deze informatie is opgenomen in de doel-gegevens (zonder expliciet onderdeelnamen in de constructor ModelDataCollector) |
| user_correlation_id | Tekenreeks | een optionele correlatie-id, die kan worden opgegeven door de gebruiker die deze voorspelling correleren |

