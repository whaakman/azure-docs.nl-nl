---
title: Logboekregistratie inschakelen in Azure Machine Learning-service
titleSuffix: Azure Machine Learning service
description: Informatie over het inschakelen van logboekregistratie in Azure Machine Learning-service met behulp van zowel de Python-pakket voor logboekregistratie, evenals met behulp van SDK-functionaliteit.
ms.author: trbye
author: trevorbye
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.date: 02/20/2019
ms.openlocfilehash: 8e39734a6f5fa13240d24eb35c431f746e540484
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57342119"
---
# <a name="enable-logging-in-azure-machine-learning-service"></a>Logboekregistratie inschakelen in Azure Machine Learning-service

De Python-SDK van Azure Machine Learning kunt u inschakelen van logboekregistratie met behulp van zowel de Standaardregistratie Python-pakket, evenals met behulp van SDK-functionaliteit voor lokale logboekregistratie en logboekregistratie naar uw werkruimte in de portal. Logboeken ontwikkelaars voorzien van realtime informatie over de status van de toepassing en kunnen helpen bij het oplossen van fouten of waarschuwingen hebben. In dit artikel leert u het inschakelen van logboekregistratie in de volgende gebieden op verschillende manieren:

> [!div class="checklist"]
> * Modellen voor training en compute-doelen
> * Het maken van installatiekopieën
> * Geïmplementeerde modellen
> * Python `logging` instellingen

Gebruik de [handleiding](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) voor het installeren van de SDK en [aan de slag](https://docs.microsoft.com/azure/machine-learning/service/quickstart-create-workspace-with-python) met de SDK voor het maken van een werkruimte in de Azure Portal.

## <a name="training-models-and-compute-target-logging"></a>Modellen voor training en registratie van de compute-doel

Er zijn meerdere manieren om in te schakelen logboekregistratie tijdens de training model en de voorbeelden die algemene ontwerppatronen wordt toegelicht. U kunt eenvoudig gegevens uitvoeren met betrekking tot aan uw werkruimte in de cloud vastleggen met behulp van de `start_logging` functioneren in de `Experiment` klasse.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Zie de referentiedocumentatie voor de [uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) klasse voor aanvullende logboekregistratiefuncties.

Om in te schakelen lokale logboekregistratie van de status van toepassing tijdens de training wordt uitgevoerd, gebruikt u de `show_output` parameter. Uitgebreide logboekregistratie inschakelen, kunt u informatie van de trainingsproces, evenals informatie over externe bronnen of compute-doelen. Gebruik de volgende code om in te schakelen van logboekregistratie van het experiment verzenden.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

U kunt ook de dezelfde parameter in de `wait_for_completion` functie voor de resulterende uitvoert.

```python
run.wait_for_completion(show_output=True)
```

De SDK biedt ook ondersteuning voor het gebruik van het standaard python-pakket logboekregistratie in bepaalde scenario's voor training. Het volgende voorbeeld wordt een niveau van logboekregistratie van `INFO` in een `AutoMLConfig` object.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task = 'regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

U kunt ook de `show_output` parameter bij het maken van een permanente compute-doel. Geef de parameter in de `wait_for_completion` functie voor het inschakelen van logboekregistratie tijdens het maken van de compute-doel.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-during-image-creation"></a>Logboekregistratie tijdens het maken van installatiekopieën

Inschakelen van logboekregistratie tijdens het maken van installatiekopieën kunt u fouten ziet tijdens het bouwproces. Stel de `show_output` param op de `wait_for_deployment()` functie.

```python
from azureml.core.webservice import Webservice

service = Webservice.deploy_from_image(deployment_config=your_config,
                                            image=image,
                                            name="example-image",
                                            workspace=ws)

service.wait_for_deployment(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Logboekregistratie voor geïmplementeerde modellen

Als u wilt Logboeken kunt ophalen uit een eerder geïmplementeerde webservice, de service te laden en gebruiken de `get_logs()` functie. De logboeken bevatten mogelijk gedetailleerde informatie over eventuele fouten die zijn opgetreden tijdens de implementatie.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

U kunt zich ook aanmelden aangepaste stack-traces voor uw web-service door in te schakelen van Application Insights, waardoor u kunnen monitor aanvraag/reactietijden en foutpercentages uitzonderingen. Roep de `update()` functie op een bestaande webservice om in te schakelen van Application Insights.

```python
service.update(enable_app_insights=True)
```

Zie de [procedures](how-to-enable-app-insights.md#enable-and-disable-in-the-portal) voor meer informatie over het werken met Application Insights in Azure portal.

## <a name="python-native-logging-settings"></a>Instellingen voor systeemeigen logboekregistratie van Python

Bepaalde Logboeken in de SDK bevatten mogelijk een foutbericht weergegeven dat Hiermee geeft u het niveau van logboekregistratie voor FOUTOPSPORING instellen. Om in te stellen op het niveau van logboekregistratie, voeg de volgende code naar uw script.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```