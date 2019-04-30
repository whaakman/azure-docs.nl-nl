---
title: Starten, bewaken en annuleren van trainingsuitvoeringen in Python
titleSuffix: Azure Machine Learning service
description: Informatie over het starten, zet de status van de tag en organiseren van uw machine learning-experimenten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/05/2019
ms.openlocfilehash: c0c1c1353b12944fa913dfb0789192917b99f234
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819206"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Starten, bewaken en annuleren van trainingsuitvoeringen in Python

De [Azure Machine Learning-SDK voor Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) biedt verschillende methoden voor het bewaken, organiseren en beheren van uw uitvoerbewerkingen voor training en experimenten.

In dit artikel ziet u voorbeelden van de volgende taken:

* Monitor uitvoeren.
* Annuleert of niet wordt uitgevoerd.
* Maken van onderliggende object wordt uitgevoerd.
* Labelen en zoeken wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

U moet de volgende items:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een [werkruimte van Azure Machine Learning-service](setup-create-workspace.md).

* De Azure Machine Learning-SDK voor Python (versie 1.0.21 of hoger). Als u wilt installeren of bijwerken naar de nieuwste versie van de SDK, Zie [installeren of bijwerken van de SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Om te controleren of uw versie van de SDK van Azure Machine Learning, gebruik de volgende code:

    ```Python
    print(azureml.core.VERSION)
    ```

## <a name="start-a-run-and-its-logging-process"></a>Een uitvoering en wordt de logboekregistratie gestart

Instellen van uw experiment door het importeren van de [werkruimte](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [experimenteren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py), en [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) klassen van de [azureml.core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) pakket.

```Python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Een uitvoering en wordt de logboekregistratie gestart met de [ `start_logging()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) methode.

```Python
notebook_run = exp.start_logging()

notebook_run.log(name="message", value="Hello from run!")
```

## <a name="monitor-the-status-of-a-run"></a>De status van een uitvoering controleren

De status van een uitvoering met de [ `get_status()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) methode.

```Python
print(notebook_run.get_status())
```

Voor meer informatie over de uitvoering, gebruikt de [ `get_details()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) methode.

```Python
notebook_run.get_details()
```

Wanneer uw uitvoering voltooid is, gebruikt u de [ `complete()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) methode om deze te markeren als voltooid.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

Als u de Python `with...as` patroon, de uitvoering, worden automatisch gemarkeerd zelf als voltooid wanneer de uitvoering buiten het bereik valt. U hoeft niet handmatig de uitvoering markeren als voltooid.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

## <a name="cancel-or-fail-runs"></a>Annuleert of niet wordt uitgevoerd

 Als u een fout ziet of als uw uitvoering te lang om te voltooien duurt, gebruikt u de [ `cancel()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) methode om te stoppen als de uitvoering is voltooid en markeer deze als geannuleerd.

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Als uw uitvoering is voltooid, maar deze een fout opgetreden bevat (bijvoorbeeld de onjuiste trainingsscript is gebruikt), kunt u de [ `fail()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-) methode om deze te markeren als mislukt.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

## <a name="create-child-runs"></a>Onderliggende maken wordt uitgevoerd

Onderliggende uitgevoerd om te groeperen gerelateerde wordt uitgevoerd, zoals voor verschillende iteraties hyperparameter afstemmen maken.

Dit codevoorbeeld maakt gebruik van de `hello_with_children.py` script voor het maken van een batch van vijf onderliggende uitgevoerd op een ingediende uitvoeren met behulp van de [ `child_run()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) methode:

```Python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Als ze niet binnen het bereik worden verplaatst, worden de onderliggende wordt uitgevoerd automatisch gemarkeerd als voltooid.

U kunt ook onderliggende uitvoeringen, één voor één starten, maar omdat elke maken in een netwerkaanroep resulteert, is minder efficiënt dan het indienen van een batch wordt uitgevoerd.

Om te vragen de onderliggende wordt uitgevoerd van een specifieke bovenliggende, gebruikt u de [ `get_children()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) methode.

```Python
list(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Labelen en uitvoeringen zoeken

U kunt in Azure Machine Learning-service, eigenschappen en tags gebruiken om te organiseren en query uitvoeren op uw uitvoerbewerkingen voor belangrijke informatie.

### <a name="add-properties-and-tags"></a>Eigenschappen en tags toevoegen

Doorzoekbare metagegevens toevoegen aan uw wordt uitgevoerd, gebruikt u de [ `add_properties()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) methode. Voegt bijvoorbeeld de volgende code toe de `"author"` eigenschap aan het run:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Eigenschappen zijn onveranderbare, zodat ze een permanente record maken voor controledoeleinden. De volgende code voorbeeld resulteert in een fout, omdat er al toegevoegd `"azureml-user"` als de `"author"` waarde van de eigenschap in de bovenstaande code:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

In tegenstelling tot de eigenschappen van zijn tags mag worden gewijzigd. U doorzoekbare en duidelijke informatie voor consumenten van uw experiment toevoegen met de [ `tag()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) methode.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

U kunt ook eenvoudige tekenreeks labels toevoegen. Wanneer deze labels worden weergegeven in de woordenlijst tag, ze hebben een waarde van `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

### <a name="query-properties-and-tags"></a>Query-eigenschappen en tags

U kunt een query wordt uitgevoerd in een experiment om terug te keren een lijst met uitvoeringen die overeenkomen met specifieke eigenschappen en tags.

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

## <a name="example-notebooks"></a>Voorbeeld-laptops

De volgende notebooks illustratie van de concepten in dit artikel:

* Zie voor meer informatie over de logboekregistratie API's, de [logboekregistratie API notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Zie voor meer informatie over het beheren van wordt uitgevoerd met de SDK van Azure Machine Learning, de [beheren uitvoeringen notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie hoe u zich aanmeldt metrische gegevens voor uw experimenten, [Meld u metrische gegevens tijdens de trainingsuitvoeringen](how-to-track-experiments.md).