---
title: MLflow gebruiken met Azure Machine Learning-service
titleSuffix: Azure Machine Learning service
description: Leer hoe u metrische gegevens en-artefacten met behulp van MLflow bibliotheek met Azure Machine Learning-service
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: 934e9b705ab5f399d29f24c915b4c60a3b06138b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082571"
---
# <a name="how-to-use-mlflow-with-azure-machine-learning-service-preview"></a>MLflow gebruiken met Azure Machine Learning-service (Preview)

In dit artikel ziet u hoe u van MLflow URI tracering en logboekregistratie API, gezamenlijk ook wel bekend als MLflow bijhouden, met Azure Machine Learning-service bij te houden en uw experiment metrische gegevens en-artefacten in uw [Azure Machine Learning werkruimte-service](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace). Als u al MLflow bijhouden voor uw experimenten, biedt de werkruimte een gecentraliseerde, veilige en schaalbare locatie voor het opslaan van uw metrische gegevens over training en -modellen.

[MLflow](https://www.mlflow.org) is een open-source-bibliotheek voor het beheren van de levenscyclus van uw machine learning-experimenten. [Bijhouden van MLFlow](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api) is een onderdeel van MLflow die zich aanmeldt en uw training uitvoeren van metrische gegevens worden bijgehouden en model artefacten, of uw experimenten lokaal worden uitgevoerd op een virtuele machine of op een externe compute cluster.
![mlflow met azure machine learning-diagram](media/how-to-use-mlflow/mlflow-diagram.png)

## <a name="compare-mlflow-and-azure-machine-learning-service-clients"></a>Vergelijk MLflow en Azure Machine Learning-service-clients

 De onderstaande tabel bevat een overzicht van de verschillende clients die van Azure Machine Learning-service en de mogelijkheden van hun respectieve functie gebruikmaken.

 Bijhouden van MLflow biedt artefact en metrische gegevens vastleggen in een opslag-functies die anders is alleen beschikbaar via zijn de [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

| | MLflow bijhouden | Azure Machine Learning <br> Python-SDK |  Azure Machine Learning <br> CLI | Azure Portal|
|-|-|-|-|-|
| -Werkruimte beheren |   | ✓ |  ✓ | ✓  |
| Opgeslagen gegevens gebruiken  |   | ✓ |  ✓ |    |
| Logboek metrische gegevens      | ✓ | ✓ |    |    |
| Artefacten uploaden | ✓ | ✓ |    |    |
| Metrische gegevens bekijken     | ✓ | ✓ | ✓  | ✓ |
| Compute beheren   |   | ✓ | ✓  | ✓ |
| Modellen implementeren    |   | ✓ |   ✓ | ✓ |

## <a name="prerequisites"></a>Vereisten

* [MLflow installeren.](https://mlflow.org/docs/latest/quickstart.html)
* [De Azure Machine Learning Python SDK installeren op uw lokale computer en een Azure Machine Learning-werkruimte maken](setup-create-workspace.md#sdk). De SDK biedt de connectiviteit voor MLflow voor toegang tot uw werkruimte.

## <a name="track-local-runs"></a>Bijhouden van lokaal wordt uitgevoerd

Installeer de `azureml-contrib-run` pakket MLflow bijhouden met Azure Machine Learning op uw experimenten die lokaal worden uitgevoerd in een Jupyter-Notebook of code-editor gebruiken.

```shell
pip install azureml-contrib-run
```

>[!NOTE]
>De naamruimte azureml.contrib regelmatig wordt gewijzigd, omdat we werken om de service te verbeteren. Als zodanig moet in deze naamruimte worden beschouwd als een Preview-versie, en niet volledig ondersteund door Microsoft.

Importeren van de `mlflow` en [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) klassen voor toegang tot MLflow bijhouden van de URI en configureren uw werkruimte.

In de volgende code wordt de `get_mlflow_tracking_uri()` methode wordt een unieke bijhouden URI-adres toegewezen aan de werkruimte `ws`, en `set_tracking_uri()` verwijst de MLflow URI naar dat adres bijhouden.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>De tracerings-URI is geldig tot een uur of minder. Als u het script opnieuw na enige tijd niet actief, gebruikt u de get_mlflow_tracking_uri API om een nieuwe URI.

Stel de naam van het experiment MLflow met `set_experiment()` en start uw training uitvoeren met `start_run()`. Gebruik vervolgens `log_metric()` voor het activeren van de API MLflow logboekregistratie en begint met het registreren van uw training metrische gegevens worden uitgevoerd.

```Python
experiment_name = "experiment_with_mlflow"
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Uitvoeringen van externe bijhouden

Externe wordt uitgevoerd, kunt u met het trainen van uw modellen op krachtige berekeningen, zoals virtuele machines met GPU of Machine Learning-Computing-clusters. Zie [instellen van compute-doelen voor modeltraining](how-to-set-up-training-targets.md) voor meer informatie over andere compute-opties.

Configureer uw compute- en uitvoeren-trainingsomgeving met de [ `Environment` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) klasse. Opnemen `mlflow` en `azure-contrib-run` pip-pakketten in de omgeving [ `CondaDependencies` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) sectie. Vervolgens maken [ `ScriptRunConfig` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) met uw externe compute als de compute-doel.

```Python

from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = "my_workspace",
                 name = "my_experiment")

mlflow_env = Environment(name="mlflow-env")

cd = CondaDependencies.create(pip_packages = ["mlflow", "azureml-contrib-run"])

mlflow_env.python.conda_dependencies=cd

src = ScriptRunConfig(source_directory="./my_script_location", script="my_training_script.py")

src.run_config.target = "my-remote-compute-compute"
src.run_config.environment = mlflow_env
```

Importeren in uw trainingsscript `mlflow` de MLflow logboekregistratie van API's en registratie van uw uitvoeren metrische gegevens starten.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric("example", 1.23)
```

Met deze reken- en configuratie van de training uitvoeren, gebruiken de `Experiment.submit("train.py")` methode voor het indienen van een uitvoering. Dit wordt automatisch Hiermee stelt u de MLflow URI bijhouden en zorgt ervoor dat de logboekregistratie van MLflow aan uw werkruimte.

```Python
run = exp.submit(src)
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Metrische gegevens weergeven en artefacten in uw werkruimte

De metrische gegevens en de artefacten van MLflow logboekregistratie worden opgeslagen in uw werkruimte in de [Azure-portal](https://portal.azure.com). Als u wilt ze elk gewenst moment weergeven, gaat u naar uw werkruimte en het experiment zoeken op naam.

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet van plan het gebruik van de geregistreerde metrische gegevens en de artefacten in uw werkruimte, is de mogelijkheid om te ze afzonderlijk verwijdert momenteel niet beschikbaar. In plaats daarvan verwijdert u de resourcegroep met de storage-account en de werkruimte, zodat u geen kosten:

1. Selecteer **Resourcegroepen** links in Azure Portal.

   ![Verwijderen in Azure Portal](media/how-to-use-mlflow/delete-resources.png)

1. Selecteer de resourcegroep die u eerder hebt gemaakt uit de lijst.

1. Selecteer **Resourcegroep verwijderen**.

1. Voer de naam van de resourcegroup in. Selecteer vervolgens **Verwijderen**.

## <a name="example-notebooks"></a>Voorbeeld-laptops

De [MLflow met Azure ML-laptops](https://github.com/Azure/MachineLearningNotebooks/blob/master/contrib/mlflow) concepten in dit artikel laat zien.

## <a name="next-steps"></a>Volgende stappen

* [Over het implementeren van een model](how-to-deploy-and-where.md).