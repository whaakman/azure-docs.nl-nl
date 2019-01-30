---
title: Geautomatiseerde ML externe compute-doelen
titleSuffix: Azure Machine Learning service
description: Informatie over het bouwen van modellen met behulp van geautomatiseerde machine learning op een Data Science Virtual machine (DSVM) externe compute-doel met Azure Machine Learning-service
services: machine-learning
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 95c495bf3a8ad7b82c42d4071899d045cb49f27b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247541"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Trainen van modellen met geautomatiseerde machine learning in de cloud

In Azure Machine Learning, door uw model op verschillende soorten compute-resources die u beheert te trainen. De compute-doel wordt mogelijk een lokale computer of een computer in de cloud.

U kunt eenvoudig omhoog of uw machine learning-experiment uitbreiden door aanvullende compute-doelen toe te voegen. Opties voor COMPUTE-doel zijn op basis van een Ubuntu Data Science Virtual Machine (DSVM) of Azure Machine Learning-Computing. De DSVM is een aangepaste VM-installatiekopie op van Microsoft Azure-cloud speciaal gebouwd voor gegevenswetenschap. Bevat veel populaire gegevenswetenschap- en andere hulpprogramma's vooraf geïnstalleerd en geconfigureerd.  

In dit artikel leert u over het bouwen van een model met behulp van geautomatiseerde ML op de DSVM.

## <a name="how-does-remote-differ-from-local"></a>Hoe verschilt afstand van lokale?

De zelfstudie '[een classificatie-model met geautomatiseerde machine learning te trainen](tutorial-auto-train-models.md)"leert u hoe u een lokale computer voor het model met geautomatiseerde ML te trainen.  De werkstroom bij het trainen van lokaal ook van toepassing is ook externe doelen. Echter met externe compute geautomatiseerde ML-iteraties uitgevoerd asynchroon. Deze functie kunt u een bepaalde iteratie annuleren, bekijk de status van de uitvoering of blijven werken van andere cellen in de Jupyter-notebook. Als u wilt trainen op afstand, moet u eerst een externe compute-doel, zoals een Azure-DSVM maken.  Vervolgens de externe bron te configureren en verzenden van uw code er.

In dit artikel bevat de extra stappen die nodig zijn voor een geautomatiseerde ML-experiment uitvoeren op een externe DSVM.  Een object in de werkruimte `ws`, uit de zelfstudie wordt gebruikt in de code hier.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Bron maken

De DSVM maakt in uw werkruimte (`ws`) als deze nog niet bestaat. Als de DSVM is eerder hebt gemaakt, deze code wordt overgeslagen het proces voor het maken en laadt de details van de bestaande resource in de `dsvm_compute` object.  

**Geschatte tijd**: Het maken van de virtuele machine duurt ongeveer 5 minuten.

```python
from azureml.core.compute import DsvmCompute

dsvm_name = 'mydsvm' #Name your DSVM
try:
    dsvm_compute = DsvmCompute(ws, dsvm_name)
    print('found existing dsvm.')
except:
    print('creating new dsvm.')
    # Below is using a VM of SKU Standard_D2_v2 which is 2 core machine. You can check Azure virtual machines documentation for additional SKUs of VMs.
    dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
    dsvm_compute = DsvmCompute.create(ws, name = dsvm_name, provisioning_configuration = dsvm_config)
    dsvm_compute.wait_for_completion(show_output = True)
```

U kunt nu de `dsvm_compute` object als de externe compute-doel.

Beperkingen op basis van DSVM zijn onder andere:
+ Moet korter zijn dan 64 tekens lang zijn.  
+ De volgende tekens niet bevatten: `\` ~! @ # $ % ^ & * () = + [] {} van _ \\ \\ |;: \' \\', in combinatie /?. `

>[!Warning]
>Als het maken is mislukt met een bericht over Marketplace-aankoop in aanmerking komen:
>    1. Ga naar [Azure Portal](https://portal.azure.com)
>    1. Beginnen met het maken van een DSVM 
>    1. Selecteer "wilt maken via een programma' programmatische maken inschakelen
>    1. Afsluiten zonder de virtuele machine te maken
>    1. Voer de code voor het maken

Deze code maakt geen gebruikersnaam of wachtwoord voor de DSVM die is ingericht. Als u rechtstreeks verbinding maken met de virtuele machine wilt, gaat u naar de [Azure-portal](https://portal.azure.com) referenties maken.  

### <a name="attach-existing-linux-dsvm"></a>Bestaande Linux-DSVM koppelen

U kunt ook een bestaande Linux-DSVM koppelen als de compute-doel. In dit voorbeeld maakt gebruik van een bestaande DSVM, maar een nieuwe resource niet maken.

> [!NOTE]
>
> De volgende code gebruikt de `RemoteCompute` target-klasse om te koppelen van een bestaande virtuele machine als uw compute-doel.
> De `DsvmCompute` klasse worden afgeschaft in toekomstige releases en vervangen door dit ontwerppatroon.

Voer de volgende code voor het maken van de compute-doel van een bestaande Linux-DSVM.

```python
from azureml.core.compute import ComputeTarget, RemoteCompute 

attach_config = RemoteCompute.attach_configuration(username='<username>',
                                                   address='<ip_address_or_fqdn>',
                                                   ssh_port=22,
                                                   private_key_file='./.ssh/id_rsa')
compute_target = ComputeTarget.attach(workspace=ws,
                                      name='attached_vm',
                                      attach_configuration=attach_config)

compute_target.wait_for_completion(show_output=True)
```

U kunt nu de `compute_target` object als de externe compute-doel.

## <a name="access-data-using-getdata-file"></a>Toegang tot gegevens met behulp van get_data bestand

Geef de externe resource toegang tot uw trainingsgegevens. Voor geautomatiseerde machine learning-experimenten die worden uitgevoerd op externe compute, de gegevens moeten worden opgehaald met behulp van een `get_data()` functie.  

Om toegang te bieden, moet u:
+ Maak een get_data.py bestand met een `get_data()` functie 
* Dat bestand in een directory toegankelijk als een absoluut pad plaatsen 

U kunt de code voor het lezen van gegevens uit een blob-opslag of lokale schijf in het bestand get_data.py bevatten. In het volgende codevoorbeeld wordt de gegevens zijn afkomstig uit het pakket sklearn.

>[!Warning]
>Als u van externe compute gebruikmaakt, dan moet u `get_data()` waar uw gegevenstransformaties worden uitgevoerd. Als u meer bibliotheken voor gegevenstransformaties installeren als onderdeel van get_data() wilt, zijn er extra stappen worden gevolgd. Raadpleeg de [auto-ml-dataprep-voorbeeld-notebook](https://aka.ms/aml-auto-ml-data-prep ) voor meer informatie.


```python
# Create a project_folder if it doesn't exist
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

#Write the get_data file.
%%writefile $project_folder/get_data.py

from sklearn import datasets
from scipy import sparse
import numpy as np

def get_data():
    
    digits = datasets.load_digits()
    X_digits = digits.data[10:,:]
    y_digits = digits.target[10:]

    return { "X" : X_digits, "y" : y_digits }
```

## <a name="configure-experiment"></a>Configureren van experiment

Geef de instellingen voor `AutoMLConfig`.  (Zie een [volledige lijst met parameters](how-to-configure-auto-train.md#configure-experiment) en hun mogelijke waarden.)

In de instellingen voor `run_configuration` is ingesteld op de `run_config` object, dat de instellingen en configuratie voor de DSVM bevat.  

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "iteration_timeout_minutes": 10,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "max_concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                            )
```

### <a name="enable-model-explanations"></a>Model uitleg inschakelen

Stel de optionele `model_explainability` parameter in de `AutoMLConfig` constructor. Bovendien een dataframe validatieobject moet worden doorgegeven als parameter `X_valid` om het model explainability-functie te gebruiken.

```python
automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                             model_explainability=True,
                             X_valid = X_test
                            )
```

## <a name="submit-training-experiment"></a>Trainingsexperiment verzenden

Nu de configuratie voor het automatisch selecteren van het algoritme, de hyper-parameters indienen en het model te trainen.

```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Hier ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************
    
     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Resultaten verkennen

U kunt de dezelfde Jupyter-widget gebruiken als het account dat in [de zelfstudie](tutorial-auto-train-models.md#explore-the-results) om te zien in een grafiek en tabel met resultaten.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```
Hier ziet u een statische afbeelding van de widget.  In het notitieblok, kunt u klikken op elke regel in de tabel om te zien van de eigenschappen voor de uitvoerbewerking en uitvoer van Logboeken voor die worden uitgevoerd.   U kunt ook de vervolgkeuzelijst boven de grafiek gebruiken om een grafiek van elke beschikbare metrische gegevens voor elke herhaling van weer te geven.

![tabel van widget](./media/how-to-auto-train-remote/table.png)
![grafiek van widget](./media/how-to-auto-train-remote/plot.png)

De widget wordt weergegeven een URL die u gebruiken kunt om te zien en de details uitvoering van afzonderlijke verkennen.
 
### <a name="view-logs"></a>Logboeken weergeven

Logboeken zoeken op de DSVM onder `/tmp/azureml_run/{iterationid}/azureml-logs`.

## <a name="best-model-explanation"></a>Beste model uitleg

Uitleg bij modelgegevens ophalen, kunt u gedetailleerde informatie over de implementatiemodellen transparantie in wat wordt uitgevoerd op de back-end te vergroten. In dit voorbeeld moet u uitleg over model alleen voor de beste passend model uitvoeren. Als u voor alle modellen in de pijplijn uitvoert, wordt dit aanzienlijke uitvoeringstijd leiden. Modelgegevens uitleg bevat:

* shap_values: De uitleg over gegevens die worden gegenereerd door de shapegegevens lib
* expected_values: De verwachte waarde van het model voor het instellen van X_train gegevens toegepast.
* overall_summary: Het model level functie belang waarden in aflopende volgorde gesorteerd
* overall: De functienamen van de in dezelfde volgorde als in overall_summary gesorteerd
* per_class_summary: De klasse niveau functie belang waarden in aflopende volgorde gesorteerd. Alleen beschikbaar voor het geval van classificatie
* per_class_imp: De functienamen in dezelfde volgorde als in per_class_summary gesorteerd. Alleen beschikbaar voor het geval van classificatie

Gebruik de volgende code om te selecteren van de beste pijplijn uit uw iteraties. De `get_output` methode retourneert de beste uitvoering en het model voor de laatste aanroep past.

```python
best_run, fitted_model = remote_run.get_output()
```

Importeren van de `retrieve_model_explanation` functioneren en worden uitgevoerd op het beste model.

```python
from azureml.train.automl.automlexplainer import retrieve_model_explanation

shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
    retrieve_model_explanation(best_run)
```

Afdrukken van resultaten voor de `best_run` uitleg variabelen die u wilt weergeven.

```python
print(overall_summary)
print(overall_imp)
print(per_class_summary)
print(per_class_imp)
```

Afdrukken via de `best_run` uitleg samenvatting variabelen resultaten in de volgende uitvoer.

![Model explainability console-uitvoer](./media/how-to-auto-train-remote/expl-print.png)

Urgentie van de functie via de gebruikersinterface van de widget, evenals de web-UI in Azure portal kunt u ook visualiseren in uw werkruimte.

![Model explainability UI](./media/how-to-auto-train-remote/model-exp.png)

## <a name="example"></a>Voorbeeld

De [how-to-use-azureml/automated-machine-learning/remote-execution/auto-ml-remote-execution.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-execution/auto-ml-remote-execution.ipynb) notebook concepten in dit artikel laat zien. 

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

Informatie over [over het configureren van instellingen voor automatische training](how-to-configure-auto-train.md).
