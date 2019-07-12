---
title: Gegevens drift (Preview) in AKS implementaties detecteren
titleSuffix: Azure Machine Learning service
description: Afwijking van de gegevens in Azure Kubernetes Service geïmplementeerd modellen in Azure Machine Learning-service worden gedetecteerd.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 07/08/2019
ms.openlocfilehash: 3b8152bde8b7e44dde1b0b9c82216333778f83da
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806011"
---
# <a name="detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service-aks"></a>Gegevens drift (preview) in modellen die zijn geïmplementeerd op Azure Kubernetes Service (AKS) detecteren

In dit artikel leert u hoe u voor het bewaken van gegevens van een geïmplementeerd model voor afwijking van gegevens tussen de gegevensset voor training en Deductie. Getrainde machine learning-modellen kan voorspelling van verminderde prestaties optreden vanwege drift in de context van machine learning. Met de Azure Machine Learning-service, kunt u de afwijking van gegevens controleren en de service kan een e-mailmelding verzenden naar u wanneer afwijking wordt gedetecteerd.

## <a name="what-is-data-drift"></a>Wat is data drift?

Gegevens drift gebeurt wanneer de gegevens die worden geleverd met een model in de productieomgeving wijkt af van de gegevens die worden gebruikt voor het model te trainen. Het is een van de belangrijkste redenen waar de nauwkeurigheid van model vermindert na verloop van tijd, dus bewakingsgegevens drift helpt model prestatieproblemen detecteren. 

## <a name="what-can-i-monitor"></a>Wat kan ik controleren?

U kunt met Azure Machine Learning-service, de invoer voor een model dat is geïmplementeerd in AKS controleren en deze gegevens vergelijken met de gegevensset training voor het model. Op vaste intervallen worden de gegevens Deductie is [momentopname en geprofileerd](how-to-explore-prepare-data.md), vervolgens berekend op basis van de basislijn-gegevensset voor het produceren van een afwijking gegevensanalyse die: 

+ Hiermee wordt de omvang van de afwijking van gegevens, de coëfficiënt drift genoemd.
+ Metingen gegevens drift bijdrage per functie, welke functies veroorzaakt gegevens drift gemeld.
+ Metingen afstand metrische gegevens. Op dit moment worden Wasserstein en energie afstand berekend.
+ Metingen distributies van functies. Op dit moment kernel dichtheid schatting en histogrammen.
+ Waarschuwingen met gegevens drift per e-mail verzenden

> [!Note]
> Deze service is in (Preview) en zijn beperkt in de configuratie-opties. Raadpleeg onze [API-documentatie](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) en [opmerkingen bij de Release](azure-machine-learning-release-notes.md) voor meer informatie en updates. 

### <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Hoe gegevens drift in Azure Machine Learning-service wordt bewaakt

Met behulp van Azure Machine Learning-service, wordt gegevens afwijking bewaakt met gegevenssets of implementaties. Om te controleren op gegevens drift, is een basislijn-gegevensset - meestal de training gegevensset voor een model - opgegeven. Een tweede gegevensset - meestal model invoergegevens die zijn verzameld van een implementatie - is getest op basis van de basislijn-gegevensset. Beide gegevenssets zijn [geprofileerd](how-to-explore-prepare-data.md#explore-with-summary-statistics) en invoer voor de gegevens beginnen af te wijken monitoring-service. Een machine learning-model wordt getraind voor het detecteren van de verschillen tussen de twee gegevenssets. Prestaties van het model wordt geconverteerd naar de coëfficiënt drift, waarvan de omvang van de afwijking tussen de twee gegevenssets maatregelen. Met behulp van [interpretability model](machine-learning-interpretability-explainability.md), de functies die aan de coëfficiënt drift bijdragen worden berekend. Van het profiel gegevensset wordt statistische informatie over elke functie bijgehouden. 

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u niet hebt, een gratis account maken voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Volg de instructies op [maken van een werkruimte van Azure Machine Learning-service](setup-create-workspace.md#sdk) het volgende doen:

    - Een Miniconda-omgeving maken
    - De Azure Machine Learning-SDK voor Python installeren
    - Een werkruimte maken
    - Schrijf een configuratiebestand werkruimte (aml_config/config.json).

- De gegevens drift SDK met behulp van de volgende opdracht installeren:

    ```shell
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

## <a name="configure-data-drift"></a>Afwijking van gegevens configureren
Als u wilt configureren afwijking van de gegevens voor uw experiment, afhankelijkheden te importeren zoals te zien is in het volgende voorbeeld van Python. 

In dit voorbeeld ziet u het configureren van de [ `DataDriftDetector` ](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector.datadriftdetector?view=azure-ml-py) object:

```python
# Import Azure ML packages
from azureml.core import Experiment, Run, RunDetails
from azureml.contrib.datadrift import DataDriftDetector, AlertConfiguration

# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

## <a name="submit-a-datadriftdetector-run"></a>Een uitvoering DataDriftDetector verzenden

Met de `DataDriftDetector` -object is geconfigureerd, kunt u indienen een [gegevens drift uitvoeren](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector%28class%29?view=azure-ml-py#run-target-date--services--compute-target-name-none--create-compute-target-false--feature-list-none--drift-threshold-none-) op een bepaalde datum voor het model. Inschakelen als onderdeel van de uitvoering, DataDriftDetector waarschuwingen door in te stellen de `drift_threshold` parameter. Als de [datadrift_coefficient](#metrics) hoger is dan de opgegeven `drift_threshold`, een e-mailbericht wordt verzonden.

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

<a name="metrics"></a>

Nadat u uw DataDriftDetector uitvoeren verzonden, zijn u de afwijking metrische gegevens die zijn opgeslagen in elke iteratie uitvoeren voor een taak van de afwijking gegevens zien:


|Gegevens|Description|
--|--|
wasserstein_distance|Statistische afstand gedefinieerd voor eendimensionale numerieke distributie.|
energy_distance|Statistische afstand gedefinieerd voor eendimensionale numerieke distributie.|
datadrift_coefficient|Op dezelfde manier als de Matthew correlatiecoëfficiënt berekend, maar deze uitvoer is een reëel getal tussen 0 en 1. 0 geeft aan dat er geen afwijking en 1 geeft aan dat de maximale drift in de context van de afwijking.|
datadrift_contribution|De urgentie van de functie van de functies die bijdragen aan het drift.|

Er zijn meerdere manieren om drift metrische gegevens weer te geven:

* Gebruik de widget Jupyter.
* Gebruik de [ `get_metrics()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#get-metrics-name-none--recursive-false--run-type-none--populate-false-) functie op een `datadrift` object uitvoeren.
* De metrische gegevens weergeven in de Azure-portal voor uw model

Het volgende voorbeeld van Python ziet u hoe u relevante gegevens drift metrische gegevens tekenen. De geretourneerde metrische gegevens kunt u aangepaste visualisaties bouwen:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Zie gegevens drift gedetecteerd door Azure Machine Learning](media/how-to-monitor-data-drift/drift_show.png)


## <a name="schedule-data-drift-scans"></a>Planning gegevens drift scans 

Wanneer u gegevens afwijking detectie inschakelt, wordt een DataDriftDetector uitgevoerd met de opgegeven, geplande frequentie. Als de datadrift_coefficient bereikt de opgegeven `drift_threshold`, een e-mailbericht wordt verzonden met elke geplande uitvoering. 

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

Door het instellen van de coëfficiënt drift drempelwaarde voor waarschuwingen en het geven van een e-mailadres, een [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) automatisch e-mailmelding wordt verzonden wanneer de coëfficiënt drift hoger dan de drempelwaarde is. 

In de volgorde voor het instellen van aangepaste meldingen en acties, alle gegevens drift metrische gegevens worden opgeslagen in de [Application Insights](how-to-enable-app-insights.md) resource die is gemaakt, samen met de werkruimte van de Azure Machine Learning-service. U kunt de koppeling in de e-mailmelding volgen op de Application Insights-query.

![Gegevens Drift e-mailmelding](media/how-to-monitor-data-drift/drift_email.png)

## <a name="retrain-your-model-after-drift"></a>Opnieuw trainen van uw model na afwijking

Wanneer gegevens drift negatieve gevolgen heeft voor de prestaties van uw geïmplementeerde model, is het tijd om het opnieuw trainen het model. De volgende [ `diff()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#diff-rhs-dataset--compute-target-none--columns-none-
) methode biedt u een eerste idee van wat er gewijzigd tussen de oude en nieuwe training-gegevenssets. 

```python
from azureml.core import Dataset

old_training_dataset.diff(new_training_dataset)
```

Op basis van de uitvoer van de vorige code, kunt u uw model te trainen. Om dit te doen, gaat u verder met de volgende stappen uit.

* Onderzoek de verzamelde gegevens en voorbereiden van gegevens naar het nieuwe model te trainen.
* Deze opsplitsen in gegevens van de trein en testen.
* Trainen het model opnieuw met de nieuwe gegevens.
* Prestaties van het zojuist gegenereerde model evalueren.
* Nieuw model implementeren als de prestaties beter dan de productiemodel.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een compleet voorbeeld van het gebruik van gegevens drift de [Azure ML gegevens drift notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/data-drift/azure-ml-datadrift.ipynb). Deze Jupyter-Notebook ziet u hoe een [Azure Open gegevensset](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) trainen van een model om te voorspellen van het weer, implementeert u deze in AKS en bewaken voor gegevens drift. 

* We zouden aanzienlijk uw vragen, opmerkingen of suggesties waarderen als u gegevens drift verplaatst naar algemene beschikbaarheid. De product-feedbackknop hieronder gebruiken. 
