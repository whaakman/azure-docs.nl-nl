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
ms.openlocfilehash: a67ac07c26063b380bda2b8cb2b6a02677e7f816
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67656188"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Starten, bewaken en annuleren van trainingsuitvoeringen in Python

De [Azure Machine Learning-SDK voor Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) en [Machine Learning CLI](reference-azure-machine-learning-cli.md) bieden verschillende methoden om te controleren, organiseren en beheren van uw uitvoerbewerkingen voor training en experimenten.

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

* De [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) en [CLI-extensie voor Azure Machine Learning-service](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Een uitvoering en wordt de logboekregistratie gestart

### <a name="using-the-sdk"></a>De SDK gebruiken

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

### <a name="using-the-cli"></a>Met behulp van de CLI

Voor het starten van een uitvoering van uw experiment, gebruikt u de volgende stappen uit:

1. Vanuit een shell of opdrachtprompt, gebruikt u de Azure CLI voor verificatie bij uw Azure-abonnement:

    ```azurecli-interactive
    az login
    ```

1. Een configuratie van de werkruimte koppelen aan de map waarin het trainingsscript. Vervang `myworkspace` met uw werkruimte van Azure Machine Learning-service. Vervang `myresourcegroup` met de Azure-resourcegroep waarin uw werkruimte:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Deze opdracht maakt u een `.azureml` submap dat voorbeeld runconfig en conda-omgeving bevat. Het bevat ook een `config.json` -bestand dat wordt gebruikt om te communiceren met uw Azure Machine Learning-werkruimte.

    Zie voor meer informatie, [az ml-map koppelen](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Gebruik de volgende opdracht voor het starten van de uitvoering. Wanneer u deze opdracht gebruikt, geef de naam van het bestand runconfig (de tekst vóór \*.runconfig als u uw bestandssysteem wilt) op basis van de parameter - c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > De `az ml folder attach` opdracht gemaakt een `.azureml` submap, die twee voorbeeld runconfig bestanden bevat. 
    >
    > Als u een Python-script waarmee u een configuratieobject uitvoeren via een programma maakt hebt, kunt u [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) als een runconfig opslaan.
    >
    > Zie voor meer voorbeeld runconfig bestanden, [ https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml ](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Zie voor meer informatie, [az ml verzenden-script uitvoeren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

## <a name="monitor-the-status-of-a-run"></a>De status van een uitvoering controleren

### <a name="using-the-sdk"></a>De SDK gebruiken

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

### <a name="using-the-cli"></a>Met behulp van de CLI

1. Als u wilt een lijst met uitvoeringen voor uw experiment weergeven, de volgende opdracht te gebruiken. Vervang `experiment` met de naam van uw experiment:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Met deze opdracht retourneert een JSON-document met een lijst met informatie over uitvoeringen voor dit experiment.

    Zie voor meer informatie, [az ml experimenten lijst](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Als u informatie op een specifieke uitvoering, gebruik de volgende opdracht. Vervang `runid` met de ID van de uitvoering:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Met deze opdracht retourneert een JSON-document met een lijst met informatie over de uitvoering.

    Zie voor meer informatie, [az ml uitvoeren weergeven](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).

## <a name="cancel-or-fail-runs"></a>Annuleert of niet wordt uitgevoerd

Als er een fout of als uw uitvoering te lang om te voltooien duurt, kunt u de uitvoering annuleren.

### <a name="using-the-sdk"></a>De SDK gebruiken

Als u wilt annuleren van een uitvoering met de SDK, gebruiken de [ `cancel()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) methode:

```Python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')

local_script_run = exp.submit(run_config)
print("Did the run start?",local_script_run.get_status())

local_script_run.cancel()
print("Did the run cancel?",local_script_run.get_status())
```

Als uw uitvoering is voltooid, maar deze een fout opgetreden bevat (bijvoorbeeld de onjuiste trainingsscript is gebruikt), kunt u de [ `fail()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) methode om deze te markeren als mislukt.

```Python
local_script_run = exp.submit(run_config)
local_script_run.fail()

print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>Met behulp van de CLI

Als u wilt een uitvoeren met behulp van de CLI annuleren, gebruik de volgende opdracht. Vervang `runid` met de ID van de uitvoering

```azurecli-interactive
az ml run cancel -r runid
```

Zie voor meer informatie, [az ml uitvoeren annuleren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

## <a name="create-child-runs"></a>Onderliggende maken wordt uitgevoerd

Onderliggende uitgevoerd om te groeperen gerelateerde wordt uitgevoerd, zoals voor verschillende iteraties hyperparameter afstemmen maken.

> [!NOTE]
> Onderliggende uitgevoerd kunnen alleen worden gemaakt met behulp van de SDK.

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

#### <a name="using-the-sdk"></a>De SDK gebruiken

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

#### <a name="using-the-cli"></a>Met behulp van de CLI

> [!NOTE]
> Met behulp van de CLI, kunt u alleen toevoegen of bijwerken van tags.

Als u wilt toevoegen of bijwerken van een tag, gebruik de volgende opdracht:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Zie voor meer informatie, [az ml-update uitvoeren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Query-eigenschappen en tags

U kunt een query wordt uitgevoerd in een experiment om terug te keren een lijst met uitvoeringen die overeenkomen met specifieke eigenschappen en tags.

#### <a name="using-the-sdk"></a>De SDK gebruiken

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>Met behulp van de CLI

De Azure CLI ondersteunt [JMESPath](http://jmespath.org) query's, die kunnen worden gebruikt om te filteren op basis van eigenschappen en tags wordt uitgevoerd. Geef voor het gebruik van een JMESPath-query met de Azure CLI, met de `--query` parameter. De volgende voorbeelden ziet eenvoudige query's met eigenschappen en tags:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Zie voor meer informatie over het opvragen van Azure CLI-resultaten [Query Azure CLI-opdrachtuitvoer](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

## <a name="example-notebooks"></a>Voorbeeld-laptops

De volgende notebooks illustratie van de concepten in dit artikel:

* Zie voor meer informatie over de logboekregistratie API's, de [logboekregistratie API notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api/logging-api.ipynb).

* Zie voor meer informatie over het beheren van wordt uitgevoerd met de SDK van Azure Machine Learning, de [beheren uitvoeringen notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/manage-runs).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie hoe u zich aanmeldt metrische gegevens voor uw experimenten, [Meld u metrische gegevens tijdens de trainingsuitvoeringen](how-to-track-experiments.md).
