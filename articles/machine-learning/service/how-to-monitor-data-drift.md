---
title: Gegevens drift (Preview) in AKS implementaties detecteren
titleSuffix: Azure Machine Learning service
description: Informatie over het detecteren van de afwijking van de gegevens in Azure Kubernetes Service geïmplementeerd modellen in Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 06/20/2019
ms.openlocfilehash: c446c8236ca64948f0bb6a8354a83579cc6ff24c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443942"
---
# <a name="detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service"></a>Gegevens drift (preview) in modellen die zijn geïmplementeerd in Azure Kubernetes Service detecteren
In dit artikel leert u hoe u voor het bewaken van gegevens van een geïmplementeerd model voor afwijking van gegevens tussen de gegevensset voor training en Deductie. 

## <a name="what-is-data-drift"></a>Wat is data drift?

Afwijking van gegevens, ook wel aangeduid als concept drift, is een van de belangrijkste redenen waar de nauwkeurigheid van model vermindert na verloop van tijd. Dit gebeurt wanneer gegevens geleverd met een model in de productieomgeving af van de gegevens die worden gebruikt wijkt voor het model te trainen. De Azure Machine Learning-service gegevens drift kunt bewaken en wanneer afwijking wordt gedetecteerd, de service een e-mailmelding kan verzenden naar u.  

> [!Note]
> Deze service is in (Preview) en zijn beperkt in de configuratie-opties. Raadpleeg onze [API-documentatie](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) en [opmerkingen bij de Release](azure-machine-learning-release-notes.md) voor meer informatie en updates. 

## <a name="what-can-i-monitor"></a>Wat kan ik controleren?
U kunt met Azure Machine Learning-service, de invoer voor een model dat is geïmplementeerd in AKS controleren en deze gegevens vergelijken met de gegevensset training voor het model. Op vaste intervallen worden de gegevens Deductie is [momentopname en geprofileerd](how-to-explore-prepare-data.md), vervolgens berekend op basis van de basislijn-gegevensset voor het produceren van een afwijking gegevensanalyse die: 

+ Hiermee wordt de omvang van de afwijking van gegevens, de coëfficiënt drift genoemd.
+ Metingen gegevens drift bijdrage per functie, welke functies veroorzaakt gegevens drift gemeld.
+ Metingen afstand metrische gegevens. Op dit moment worden Wasserstein en energie afstand berekend.
+ Metingen distributies van functies. Op dit moment kernel dichtheid schatting en histogrammen.
+ Waarschuwingen met gegevens drift per e-mail verzenden

Zie voor meer informatie over hoe deze metrische gegevens worden berekend, de [gegevens drift concept](concept-data-drift.md) artikel.

## <a name="prerequisites"></a>Vereisten

- Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.

- [Instellen van uw omgeving](how-to-configure-environment.md), en installeer vervolgens de gegevens drift SDK met behulp van de volgende opdracht uit:

    ```
    pip install azureml-contrib-datadrift
    ```

- Maak een [gegevensset](how-to-create-register-datasets.md) uit van het model trainen van gegevens.

- Geef de training-gegevensset als [registreren](concept-model-management-and-deployment.md) het model. Het volgende voorbeeld ziet u met behulp van de `datasets` parameter opgeven voor de gegevensset training:

    ```python
    model = Model.register(model_path=model_file,
                        model_name=model_name,
                        workspace=ws,
                        datasets=datasets)

    print(model_name, image_name, service_name, model)
    ```

- [Inschakelen van gegevensverzameling van model](how-to-enable-data-collection.md) verzamelen van gegevens uit de AKS-implementatie van het model en controleer of de gegevens worden verzameld de `modeldata` blob-container.

## <a name="import-dependencies"></a>Afhankelijkheden importeren 
Importeer de afhankelijkheden die worden gebruikt in deze handleiding:

```python
# Azure ML service packages 
from azureml.core import Experiment, Run, RunDetails
from azureml.contrib.datadrift import DataDriftDetector, AlertConfiguration
``` 

## <a name="configure-data-drift"></a>Afwijking van gegevens configureren 

Het volgende voorbeeld van Python ziet u het configureren van de `DataDriftDetector` object:

```python
# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

Zie voor meer informatie de `[DataDrift](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py)` referentiedocumentatie voor klasse.

## <a name="submit-a-datadriftdetector-run"></a>Een uitvoering DataDriftDetector verzenden

Met de `DataDriftDetector` -object is geconfigureerd, kunt u indienen een [gegevens drift uitvoeren](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector%28class%29?view=azure-ml-py#run-target-date--services--compute-target-name-none--create-compute-target-false--feature-list-none--drift-threshold-none-) op een bepaalde datum voor het model. 

```python
# adhoc run today
target_date = datetime.today()

# create a new compute - creates datadrift-server
run = datadrift.run(target_date, services, feature_list=feature_list, create_compute_target=True)

# or specify existing compute cluster
run = datadrift.run(target_date, services, feature_list=feature_list, compute_target='cpu-cluster')

# show details of the data drift run
exp = Experiment(ws, datadrift._id)
dd_run = Run(experiment=exp, run_id=run)
RunDetails(dd_run).show()
```

## <a name="visualize-drift-metrics"></a>Afwijking metrische gegevens visualiseren

Het volgende voorbeeld van Python ziet u hoe u relevante gegevens drift metrische gegevens tekenen. De geretourneerde metrische gegevens kunt u aangepaste visualisaties bouwen:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Zie gegevens drift gedetecteerd door Azure Machine Learning](media/how-to-monitor-data-drift/drift_show.png)

Zie voor meer informatie over de metrische gegevens die worden berekend, de [gegevens drift concept](concept-data-drift.md) artikel.

## <a name="schedule-data-drift-scans"></a>Planning gegevens drift scans 

Wanneer u gegevens afwijking detectie inschakelt, wordt een DataDriftDetector uitgevoerd met de opgegeven, geplande frequentie. Als de coëfficiënt drift hoger dan de opgegeven drempelwaarde is, wordt een e-mailbericht wordt verzonden. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

De configuratie van de gegevens drift detector kan worden weergegeven op de detailpagina van het model in Azure portal.

![Azure-portal gegevens Drift Config](media/how-to-monitor-data-drift/drift_config.png)

## <a name="view-results-in-azure-ml-workspace-ui"></a>Resultaten weergeven in de gebruikersinterface van Azure ML-werkruimte

Om resultaten te bekijken in de gebruikersinterface van Azure ML-werkruimte, gaat u naar de pagina met het model. Op het tabblad met details van het model, wordt de configuratie van de afwijking gegevens weergegeven. Een tabblad Data Drift (Preview) is nu beschikbaar die de gegevens drift metrische gegevens. 

![Azure-portal gegevens afwijking](media/how-to-monitor-data-drift/drift_ui.png)

## <a name="receiving-drift-alerts"></a>Ontvangende drift waarschuwingen

Door het instellen van de coëfficiënt drift drempelwaarde voor waarschuwingen en het geven van een e-mailadres, een [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) automatisch e-mailmelding wordt verzonden wanneer de coëfficiënt drift hoger dan de drempelwaarde is. Opdat u voor het instellen van aangepaste meldingen en acties worden alle gegevens drift metrische gegevens worden opgeslagen in de Application Insights-resource die is gemaakt, samen met de werkruimte van de Azure Machine Learning-service. U kunt de koppeling in de e-mailmelding volgen op de Application Insights-query.

![Gegevens Drift e-mailmelding](media/how-to-monitor-data-drift/drift_email.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor een compleet voorbeeld van het gebruik van gegevens drift de [Azure ML gegevens drift notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/data-drift/azure-ml-datadrift.ipynb). Deze Jupyter-Notebook ziet u hoe een [Azure Open gegevensset](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) trainen van een model om te voorspellen van het weer, implementeert u deze in AKS en bewaken voor gegevens drift. 

* We zouden aanzienlijk uw vragen, opmerkingen of suggesties waarderen als u gegevens drift verplaatst naar algemene beschikbaarheid. De product-feedbackknop hieronder gebruiken. 
