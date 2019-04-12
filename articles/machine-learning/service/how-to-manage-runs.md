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
ms.openlocfilehash: 82df2258116ce55fa440b67ec0a66b106d0d72c7
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59494044"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Starten, bewaken en annuleren van trainingsuitvoeringen in Python

De [Azure Machine Learning-SDK voor Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) biedt verschillende methoden voor het bewaken, organiseren en beheren van uw uitvoerbewerkingen voor training en experimenten.

Deze procedure ziet u voorbeelden van de volgende taken:

* [Monitor uitvoeren](#monitor)
* [Annuleert of niet wordt uitgevoerd](#cancel)
* [Onderliggende maken wordt uitgevoerd](#children)
* [Labelen en uitvoeringen zoeken](#tag)

## <a name="prerequisites"></a>Vereisten

Hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een werkruimte van Azure Machine Learning-service. Zie [maken van een werkruimte van Azure Machine Learning-service](setup-create-workspace.md).

* De Azure Machine Learning-SDK voor Python geïnstalleerd (versie 1.0.21 of hoger). Als u wilt installeren of bijwerken naar de nieuwste versie van de SDK, gaat u naar de [installeren/bijwerken van de SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) pagina.

    Gebruik de volgende code om te controleren of uw versie van de SDK van Azure Machine Learning.

    ```Python
    print(azureml.core.VERSION)
    ```

<a name="monitor"></a>

## <a name="start-a-run-and-set-its-status"></a>Een uitvoering starten en de status ervan instellen

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

Haal de status van de uitvoering met [ `get_status()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--).

```Python
print(notebook_run.get_status())
```

Voor aanvullende informatie over het gebruik van uitvoeren [ `get_details()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--).

```Python
notebook_run.get_details()
```

Wanneer uw uitvoering voltooid is, gebruikt u de [ `complete()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) methode om deze te markeren als voltooid.

```Python
notebook_run.complete()
print(notebook_run.get_status())
```

U kunt ook de Python `with...as` patroon. In deze context bevindt worden de uitvoering automatisch gemarkeerd zelf als voltooid wanneer de uitvoering buiten het bereik valt. Op deze manier niet moet u handmatig de uitvoering markeren als voltooid.

```Python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print("Is it still running?",notebook_run.get_status())

print("Has it completed?",notebook_run.get_status())
```

<a name="cancel"></a>

## <a name="cancel-or-fail-runs"></a>Annuleert of niet wordt uitgevoerd

 Als er een fout of uw uitvoering lijkt te duren wilt voltooien, gebruikt u de [ `cancel()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) methode om te stoppen als de uitvoering is voltooid en markeer deze als geannuleerd.

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Als uw uitvoering is voltooid, maar bevat een fout opgetreden, zoals, het trainingsscript onjuist is gebruikt, kunt u de [ `fail()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#fail-error-details-none---set-status-true-) methode om deze te markeren als mislukt.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

<a name="children"></a>

## <a name="create-child-runs"></a>Onderliggende maken wordt uitgevoerd

Onderliggende uitgevoerd om te groeperen gerelateerde wordt uitgevoerd, zoals voor verschillende hyperparameter afstemmen iteraties maken.

Dit codevoorbeeld maakt gebruik van het script hello_with_children.py te maken van een batch van vijf onderliggende uitvoeringen van binnen een ingediende uitvoeren met de [ `child_run()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) methode.

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
> Onderliggende voltooid wordt automatisch uitgevoerd als ze niet binnen het bereik worden verplaatst.

U kunt ook onderliggende uitvoeringen, één voor één starten, maar omdat elke maken in een netwerkaanroep resulteert, is minder efficiënt dan het indienen van een batch wordt uitgevoerd.

Gebruik de [ `get_children()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) methode naar de onderliggende query wordt uitgevoerd van een specifieke bovenliggende.

```Python
list(parent_run.get_children())
```

<a name="tag"></a>

## <a name="tag-and-find-runs"></a>Labelen en uitvoeringen zoeken

U kunt in Azure Machine Learning-service, eigenschappen en tags gebruiken om te organiseren en query uitvoeren op uw uitvoerbewerkingen voor belangrijke informatie.

### <a name="add-properties-and-tags"></a>Eigenschappen en tags toevoegen

Gebruik de [ `add_properties()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) doorzoekbare metagegevens toevoegen aan uw wordt uitgevoerd. De volgende code wordt bijvoorbeeld de eigenschap 'auteur' toegevoegd aan de uitvoering.

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Eigenschappen zijn onveranderbare, dit is handig als een permanente record voor controledoeleinden. Het volgende codevoorbeeld resulteert in een fout, omdat er al 'azureml-gebruiker' als de eigenschap 'auteur' in de bovenstaande code hebt toegevoegd.

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Tags, zijn echter wel worden. Gebruik [ `tag()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) doorzoekbaar en duidelijke informatie voor consumenten van uw experiment toevoegen.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

U kunt ook een eenvoudige tekenreeks-tag toevoegen. Deze wordt weergegeven in de tag-woordenlijst met de waarde van `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

### <a name="query-properties-and-tags"></a>Query-eigenschappen en tags

U kunt een query wordt uitgevoerd in een experiment die overeenkomen met specifieke eigenschappen en tags.

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

## <a name="example-notebooks"></a>Voorbeeld-laptops

De volgende notebooks illustratie van concepten in dit artikel:

* Zie voor meer informatie over het vastleggen van API's, de [logboekregistratie API notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Zie voor meer informatie over het beheren van wordt uitgevoerd met de SDK van Azure Machine Learning, de [beheren uitvoeringen notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Volgende stappen

* Als u wilt weten hoe u zich aanmeldt metrische gegevens voor uw experimenten, Zie de [Meld u metrische gegevens tijdens de trainingsuitvoeringen](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiment) artikel.