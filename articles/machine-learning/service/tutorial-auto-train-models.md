---
title: 'Zelfstudie voor een regressiemodel: Automatisch modellen trainen'
titleSuffix: Azure Machine Learning service
description: Leer hoe u een ML-model genereert met behulp van geautomatiseerde Machine Learning.  Azure Machine Learning kan voorbewerking van gegevens, selectie van algoritme en selectie van hyperparameters automatisch voor u uitvoeren. Het uiteindelijke model kan vervolgens geïmplementeerd met de Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6bbc2d44ab128aec032ead29bf247cd834f932b6
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315193"
---
# <a name="tutorial-use-automated-machine-learning-to-build-your-regression-model"></a>Zelfstudie: Geautomatiseerde machine learning gebruiken om uw regressiemodel te bouwen

Deze zelfstudie is **deel twee van een tweedelige reeks**. In de vorige zelfstudie hebt u [de NYC-taxigegevens voorbereid voor regressiemodellering](tutorial-data-prep.md).

Nu bent u er klaar voor om een model te bouwen met behulp van de Azure Machine Learning-service. In dit gedeelte van de zelfstudie gebruikt u de voorbereide gegevens en genereert u automatisch een regressiemodel om de prijzen voor taxiritten te voorspellen. Met behulp van de geautomatiseerde ML-mogelijkheden van de service definieert u uw Machine Learning-doelen en -beperkingen en lanceert u het geautomatiseerde Machine Learning-proces. Vervolgens laat u het selecteren van een algoritme en het afstemmen van hyperparameters voor u uitvoeren. De techniek van geautomatiseerde ML doorloopt of itereert allerlei combinaties van algoritmen en hyperparameters totdat het beste model wordt gevonden op basis van uw criterium.

![stroomdiagram](./media/tutorial-auto-train-models/flow2.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Python-omgeving instellen en de SDK-pakketten importeren
> * Een werkruimte configureren in de Azure Machine Learning-service
> * Een regressiemodel automatisch trainen
> * Het model lokaal uitvoeren met aangepaste parameters
> * De resultaten verkennen
> * Het beste model registreren

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](http://aka.ms/AMLFree).

>[!NOTE]
> Code in dit artikel is getest met Azure Machine Learning SDK-versie 1.0.0

## <a name="prerequisites"></a>Vereisten

> * [Voer de zelfstudie voor gegevensvoorbereiding uit](tutorial-data-prep.md).
> * Geautomatiseerde omgevingen die met Machine Learning zijn geconfigureerd, bijvoorbeeld Azure-notebooks, lokale Python-omgeving of Data Science Virtual Machine. [Installeer](samples-notebooks.md) geautomatiseerde Machine Learning.

## <a name="get-the-notebook"></a>De notebook ophalen

Voor uw gemak is deze zelfstudie beschikbaar gemaakt als een [Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/regression-part2-automated-ml.ipynb). Voer het `regression-part2-automated-ml.ipynb`-notebook uit in Azure Notebooks of op uw eigen Jupyter-notebookserver.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

## <a name="import-packages"></a>Pakketten importeren
Importeer de Python-pakketten die u nodig hebt in deze zelfstudie.


```python
import azureml.core
import pandas as pd
from azureml.core.workspace import Workspace
from azureml.train.automl.run import AutoMLRun
import time
import logging
import os
```

## <a name="configure-workspace"></a>Werkruimte configureren

Maak een werkruimte-object van de bestaande werkruimte. Een `Workspace` is een klasse die uw Azure-abonnement en resourcegegevens accepteert, en waarmee een cloudresource wordt gemaakt om de uitvoeringen van het model te controleren en bij te houden. `Workspace.from_config()` leest het bestand **aml_config/config.json** en laadt de gegevens in een object met de naam `ws`.  `ws` wordt gebruikt in de rest van de code in deze zelfstudie.

Zodra u beschikt over een werkruimte-object, geeft u een naam op voor het experiment en maakt en registreert u een lokale map in de werkruimte. De geschiedenis van alle uitvoeringen wordt vastgelegd onder het opgegeven experiment en in de [Azure-portal](https://portal.azure.com).


```python
ws = Workspace.from_config()
# choose a name for the run history container in the workspace
experiment_name = 'automated-ml-regression'
# project folder
project_folder = './automated-ml-regression'

output = {}
output['SDK version'] = azureml.core.VERSION
output['Subscription ID'] = ws.subscription_id
output['Workspace'] = ws.name
output['Resource Group'] = ws.resource_group
output['Location'] = ws.location
output['Project Directory'] = project_folder
pd.set_option('display.max_colwidth', -1)
pd.DataFrame(data=output, index=['']).T
```

## <a name="explore-data"></a>Gegevens verkennen

Gebruik het gegevensstroomobject dat u hebt gemaakt in de vorige zelfstudie. Open de gegevensstroom, voer deze uit en beoordeel de resultaten.


```python
import azureml.dataprep as dprep

file_path = os.path.join(os.getcwd(), "dflows.dprep")

package_saved = dprep.Package.open(file_path)
dflow_prepared = package_saved.dataflows[0]
dflow_prepared.get_profile()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Min.</th>
      <th>Max.</th>
      <th>Count</th>
      <th>Ontbrekend aantal</th>
      <th>Niet-ontbrekend aantal</th>
      <th>Ontbrekend percentage</th>
      <th>Aantal fouten</th>
      <th>Leeg aantal</th>
      <th>0,1%-kwantiel</th>
      <th>1%-kwantiel</th>
      <th>5%-kwantiel</th>
      <th>25%-kwantiel</th>
      <th>50%-kwantiel</th>
      <th>75%-kwantiel</th>
      <th>95%-kwantiel</th>
      <th>99%-kwantiel</th>
      <th>99,9%-kwantiel</th>
      <th>Gemiddelde</th>
      <th>Standaardafwijking</th>
      <th>Variantie</th>
      <th>Asymmetrie</th>
      <th>Kurtosis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>leverancier</th>
      <td>FieldType.STRING</td>
      <td>1</td>
      <td>VTS</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_weekday</th>
      <td>FieldType.STRING</td>
      <td>Vrijdag</td>
      <td>Woensdag</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>2,90047</td>
      <td>2,69355</td>
      <td>9,72889</td>
      <td>16</td>
      <td>19,3713</td>
      <td>22,6974</td>
      <td>23</td>
      <td>23</td>
      <td>14,2731</td>
      <td>6,59242</td>
      <td>43,46</td>
      <td>-0,693723</td>
      <td>-0,570403</td>
    </tr>
    <tr>
      <th>pickup_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>4,99701</td>
      <td>4,95833</td>
      <td>14,1528</td>
      <td>29,3832</td>
      <td>44,6825</td>
      <td>56,4444</td>
      <td>58,9909</td>
      <td>59</td>
      <td>29,427</td>
      <td>17,4333</td>
      <td>303,921</td>
      <td>0,0120999</td>
      <td>-1,20981</td>
    </tr>
    <tr>
      <th>pickup_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>5,28131</td>
      <td>5</td>
      <td>14,7832</td>
      <td>29,9293</td>
      <td>44,725</td>
      <td>56,7573</td>
      <td>59</td>
      <td>59</td>
      <td>29,7443</td>
      <td>17,3595</td>
      <td>301,351</td>
      <td>-0,0252399</td>
      <td>-1,19616</td>
    </tr>
    <tr>
      <th>dropoff_weekday</th>
      <td>FieldType.STRING</td>
      <td>Vrijdag</td>
      <td>Woensdag</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>dropoff_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>2,57153</td>
      <td>2</td>
      <td>9,58795</td>
      <td>15,9994</td>
      <td>19,6184</td>
      <td>22,8317</td>
      <td>23</td>
      <td>23</td>
      <td>14,2105</td>
      <td>6,71093</td>
      <td>45,0365</td>
      <td>-0,687292</td>
      <td>-0,61951</td>
    </tr>
    <tr>
      <th>dropoff_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>5,44383</td>
      <td>4,84694</td>
      <td>14,1036</td>
      <td>28,8365</td>
      <td>44,3102</td>
      <td>56,6892</td>
      <td>59</td>
      <td>59</td>
      <td>29,2907</td>
      <td>17,4108</td>
      <td>303,136</td>
      <td>0,0222514</td>
      <td>-1,2181</td>
    </tr>
    <tr>
      <th>dropoff_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>5,07801</td>
      <td>5</td>
      <td>14,5751</td>
      <td>29,5972</td>
      <td>45,4649</td>
      <td>56,2729</td>
      <td>59</td>
      <td>59</td>
      <td>29,772</td>
      <td>17,5337</td>
      <td>307,429</td>
      <td>-0,0212575</td>
      <td>-1,226</td>
    </tr>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>J</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74,0781</td>
      <td>-73,7459</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>-74,0578</td>
      <td>-73,9639</td>
      <td>-73,9656</td>
      <td>-73,9508</td>
      <td>-73,9255</td>
      <td>-73,8529</td>
      <td>-73,8302</td>
      <td>-73,8238</td>
      <td>-73,7697</td>
      <td>-73,9123</td>
      <td>0,0503757</td>
      <td>0,00253771</td>
      <td>0,352172</td>
      <td>-0,923743</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40,5755</td>
      <td>40,8799</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>40,632</td>
      <td>40,7117</td>
      <td>40,7115</td>
      <td>40,7213</td>
      <td>40,7565</td>
      <td>40,8058</td>
      <td>40,8478</td>
      <td>40,8676</td>
      <td>40,8778</td>
      <td>40,7649</td>
      <td>0,0494674</td>
      <td>0,00244702</td>
      <td>0,205972</td>
      <td>-0,777945</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74,0857</td>
      <td>-73,7209</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>-74,0775</td>
      <td>-73,9875</td>
      <td>-73,9882</td>
      <td>-73,9638</td>
      <td>-73,935</td>
      <td>-73,8755</td>
      <td>-73,8125</td>
      <td>-73,7759</td>
      <td>-73,7327</td>
      <td>-73,9202</td>
      <td>0,0584627</td>
      <td>0,00341789</td>
      <td>0,623622</td>
      <td>-0,262603</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40,5835</td>
      <td>40,8797</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>40,5973</td>
      <td>40,6928</td>
      <td>40,6911</td>
      <td>40,7226</td>
      <td>40,7567</td>
      <td>40,7918</td>
      <td>40,8495</td>
      <td>40,868</td>
      <td>40,8787</td>
      <td>40,7583</td>
      <td>0,0517399</td>
      <td>0,00267701</td>
      <td>0,0390404</td>
      <td>-0,203525</td>
    </tr>
    <tr>
      <th>passagiers</th>
      <td>FieldType.DECIMAL</td>
      <td>1</td>
      <td>6</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>6</td>
      <td>2,39249</td>
      <td>1,83197</td>
      <td>3,3561</td>
      <td>0,763144</td>
      <td>-1,23467</td>
    </tr>
    <tr>
      <th>afstand</th>
      <td>FieldType.DECIMAL</td>
      <td>0,01</td>
      <td>32,34</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0108744</td>
      <td>0,743898</td>
      <td>0,738194</td>
      <td>1,243</td>
      <td>2,40168</td>
      <td>4,74478</td>
      <td>10,5136</td>
      <td>14,9011</td>
      <td>21,8035</td>
      <td>3,5447</td>
      <td>3,2943</td>
      <td>10,8524</td>
      <td>1,91556</td>
      <td>4,99898</td>
    </tr>
    <tr>
      <th>kosten</th>
      <td>FieldType.DECIMAL</td>
      <td>0.1</td>
      <td>88</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>6148,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>2,33837</td>
      <td>5,00491</td>
      <td>5</td>
      <td>6,93129</td>
      <td>10,524</td>
      <td>17,4811</td>
      <td>33,2343</td>
      <td>50,0093</td>
      <td>63,1753</td>
      <td>13,6843</td>
      <td>9,66571</td>
      <td>93,426</td>
      <td>1,78518</td>
      <td>4,13972</td>
    </tr>
  </tbody>
</table>

U bereidt het model voor het experiment voor door kolommen toe te voegen aan `dflow_x` die functies worden bij het maken van het model. U definieert `dflow_y` als voorspellingswaarde; kosten.

```python
dflow_X = dflow_prepared.keep_columns(['pickup_weekday','pickup_hour', 'distance','passengers', 'vendor'])
dflow_y = dflow_prepared.keep_columns('cost')
```

### <a name="split-data-into-train-and-test-sets"></a>Gegevens splitsen in sets voor trainen en voor testen

Nu splitst u de gegevens in trainings- en testsets met behulp van de functie `train_test_split` in de `sklearn`-bibliotheek. Met deze functie worden de gegevens verdeeld in de gegevensset x (functies) voor het trainen van het model, en de gegevensset y (te voorspellen waarden) voor testen. Met de parameter `test_size` wordt het percentage gegevens bepaald dat moet worden toegewezen aan testen. Met de parameter `random_state` wordt de willekeurige generator ingesteld, zodat de verdeling tussen trainen en testen altijd deterministisch is.

```python
from sklearn.model_selection import train_test_split

x_df = dflow_X.to_pandas_dataframe()
y_df = dflow_y.to_pandas_dataframe()

x_train, x_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=223)
# flatten y_train to 1d array
y_train.values.flatten()
```

U beschikt nu over de vereiste pakketten en gegevens voor automatische training van uw model.

## <a name="automatically-train-a-model"></a>Automatisch een model trainen

Ga als volgt te werk om automatisch een model te trainen:
1. Definieer de instellingen voor het uitvoeren van het experiment
1. Verzend het experiment om het model af te stemmen

### <a name="define-settings-for-autogeneration-and-tuning"></a>Definieer de instellingen voor automatisch genereren en afstemmen

Definieer de experimentparameters en modelinstellingen voor automatisch genereren en afstemmen. Bekijk de volledige lijst met [instellingen](how-to-configure-auto-train.md).


|Eigenschap| Waarde in deze zelfstudie |Beschrijving|
|----|----|---|
|**iteration_timeout_minutes**|10|Tijdslimiet in minuten voor elke iteratie|
|**iterations**|30|Aantal iteraties. Met elke iteratie wordt het model getraind met de gegevens met een specifieke pijplijn.|
|**primary_metric**| spearman_correlation | De metrische gegevens die u wilt optimaliseren.|
|**preprocess**| True | True stelt het experiment in staat voorbewerking toe te passen op de invoer.|
|**uitgebreidheid**| logging.INFO | Hiermee bepaalt u het niveau van logboekregistratie.|
|**n_cross_validationss**|5|Aantal cross-validatie splitsingen.



```python
automl_settings = {
    "iteration_timeout_minutes" : 10,
    "iterations" : 30,
    "primary_metric" : 'spearman_correlation',
    "preprocess" : True,
    "verbosity" : logging.INFO,
    "n_cross_validations": 5
}
```


```python
from azureml.train.automl import AutoMLConfig

# local compute
automated_ml_config = AutoMLConfig(task = 'regression',
                             debug_log = 'automated_ml_errors.log',
                             path = project_folder,
                             X = x_train.values,
                             y = y_train.values.flatten(),
                             **automl_settings)
```

### <a name="train-the-automatic-regression-model"></a>Het automatische regressiemodel trainen

Start het experiment voor lokale uitvoering. Geef het gedefinieerde `automated_ml_config`-object door aan het experiment, en stel de uitvoer in op `True` om de voortgang weer te geven tijdens het experiment.


```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, experiment_name)
local_run = experiment.submit(automated_ml_config, show_output=True)
```

    Parent Run ID: AutoML_02778de3-3696-46e9-a71b-521c8fca0651
    *******************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    *******************************************************************************************
    
     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler ExtremeRandomTrees                0:00:08       0.9447    0.9447
             1   StandardScalerWrapper GradientBoosting         0:00:09       0.9536    0.9536
             2   StandardScalerWrapper ExtremeRandomTrees       0:00:09       0.8580    0.9536
             3   StandardScalerWrapper RandomForest             0:00:08       0.9147    0.9536
             4   StandardScalerWrapper ExtremeRandomTrees       0:00:45       0.9398    0.9536
             5   MaxAbsScaler LightGBM                          0:00:08       0.9562    0.9562
             6   StandardScalerWrapper ExtremeRandomTrees       0:00:27       0.8282    0.9562
             7   StandardScalerWrapper LightGBM                 0:00:07       0.9421    0.9562
             8   MaxAbsScaler DecisionTree                      0:00:08       0.9526    0.9562
             9   MaxAbsScaler RandomForest                      0:00:09       0.9355    0.9562
            10   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            11   MaxAbsScaler LightGBM                          0:00:09       0.9553    0.9602
            12   MaxAbsScaler DecisionTree                      0:00:07       0.9484    0.9602
            13   MaxAbsScaler LightGBM                          0:00:08       0.9540    0.9602
            14   MaxAbsScaler RandomForest                      0:00:10       0.9365    0.9602
            15   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            16   StandardScalerWrapper ExtremeRandomTrees       0:00:49       0.9171    0.9602
            17   SparseNormalizer LightGBM                      0:00:08       0.9191    0.9602
            18   MaxAbsScaler DecisionTree                      0:00:08       0.9402    0.9602
            19   StandardScalerWrapper ElasticNet               0:00:08       0.9603    0.9603
            20   MaxAbsScaler DecisionTree                      0:00:08       0.9513    0.9603
            21   MaxAbsScaler SGD                               0:00:08       0.9603    0.9603
            22   MaxAbsScaler SGD                               0:00:10       0.9602    0.9603
            23   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            24   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            25   MaxAbsScaler SGD                               0:00:09       0.9603    0.9603
            26   TruncatedSVDWrapper ElasticNet                 0:00:09       0.9602    0.9603
            27   MaxAbsScaler SGD                               0:00:12       0.9413    0.9603
            28   StandardScalerWrapper ElasticNet               0:00:07       0.9603    0.9603
            29    Ensemble                                      0:00:38       0.9622    0.9622

## <a name="explore-the-results"></a>De resultaten verkennen

Bekijk de resultaten van de automatische training met een Jupyter-widget of door de geschiedenis van het experiment te controleren.

### <a name="option-1-add-a-jupyter-widget-to-see-results"></a>Optie 1: Een Jupyter-widget toevoegen om de resultaten te bekijken

Als u gebruikmaakt van een Juypter-notebook, gebruikt u deze Juypter-widget om een grafiek en een tabel met alle resultaten te zien.


```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Uitvoeringsdetails van Jupyter-widget](./media/tutorial-auto-train-models/automl-dash-output.png)
![Tekengebied Jupyter-widget](./media/tutorial-auto-train-models/automl-chart-output.png)

### <a name="option-2-get-and-examine-all-run-iterations-in-python"></a>Optie 2: Alle uitgevoerde herhalingen ophalen en onderzoeken in Python

U kunt er ook voor kiezen om de geschiedenis van elk experiment op te halen en de afzonderlijke metrische gegevens voor elke uitgevoerde herhaling te verkennen.

```python
children = list(local_run.get_children())
metricslist = {}
for run in children:
    properties = run.get_properties()
    metrics = {k: v for k, v in run.get_metrics().items() if isinstance(v, float)}
    metricslist[int(properties['iteration'])] = metrics

import pandas as pd
rundata = pd.DataFrame(metricslist).sort_index(1)
rundata
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>explained_variance</th>
      <td>0,811037</td>
      <td>0,880553</td>
      <td>0,398582</td>
      <td>0,776040</td>
      <td>0,663869</td>
      <td>0,875911</td>
      <td>0,115632</td>
      <td>0,586905</td>
      <td>0,851911</td>
      <td>0,793964</td>
      <td>...</td>
      <td>0,850023</td>
      <td>0,883603</td>
      <td>0,883704</td>
      <td>0,880797</td>
      <td>0,881564</td>
      <td>0,883708</td>
      <td>0,881826</td>
      <td>0,585377</td>
      <td>0,883123</td>
      <td>0,886817</td>
    </tr>
    <tr>
      <th>mean_absolute_error</th>
      <td>2,189444</td>
      <td>1,500412</td>
      <td>5,480531</td>
      <td>2,626316</td>
      <td>2,973026</td>
      <td>1,550199</td>
      <td>6,383868</td>
      <td>4,414241</td>
      <td>1,743328</td>
      <td>2,294601</td>
      <td>...</td>
      <td>1,797402</td>
      <td>1,415815</td>
      <td>1,418167</td>
      <td>1,578617</td>
      <td>1,559427</td>
      <td>1,413042</td>
      <td>1,551698</td>
      <td>4,069196</td>
      <td>1,505795</td>
      <td>1,430957</td>
    </tr>
    <tr>
      <th>median_absolute_error</th>
      <td>1,438417</td>
      <td>0,850899</td>
      <td>4,579662</td>
      <td>1,765210</td>
      <td>1,594600</td>
      <td>0,869883</td>
      <td>4,266450</td>
      <td>3,627355</td>
      <td>0,954992</td>
      <td>1,361014</td>
      <td>...</td>
      <td>0,973634</td>
      <td>0,774814</td>
      <td>0,797269</td>
      <td>1,147234</td>
      <td>1,116424</td>
      <td>0,783958</td>
      <td>1,098464</td>
      <td>2,709027</td>
      <td>1,003728</td>
      <td>0,851724</td>
    </tr>
    <tr>
      <th>normalized_mean_absolute_error</th>
      <td>0,024908</td>
      <td>0,017070</td>
      <td>0,062350</td>
      <td>0,029878</td>
      <td>0,033823</td>
      <td>0,017636</td>
      <td>0,072626</td>
      <td>0,050219</td>
      <td>0,019833</td>
      <td>0,026105</td>
      <td>...</td>
      <td>0,020448</td>
      <td>0,016107</td>
      <td>0,016134</td>
      <td>0,017959</td>
      <td>0,017741</td>
      <td>0,016076</td>
      <td>0,017653</td>
      <td>0,046293</td>
      <td>0,017131</td>
      <td>0,016279</td>
    </tr>
    <tr>
      <th>normalized_median_absolute_error</th>
      <td>0,016364</td>
      <td>0,009680</td>
      <td>0,052101</td>
      <td>0,020082</td>
      <td>0,018141</td>
      <td>0,009896</td>
      <td>0,048538</td>
      <td>0,041267</td>
      <td>0,010865</td>
      <td>0,015484</td>
      <td>...</td>
      <td>0,011077</td>
      <td>0,008815</td>
      <td>0,009070</td>
      <td>0,013052</td>
      <td>0,012701</td>
      <td>0,008919</td>
      <td>0,012497</td>
      <td>0,030819</td>
      <td>0,011419</td>
      <td>0,009690</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_error</th>
      <td>0,047968</td>
      <td>0,037882</td>
      <td>0,085572</td>
      <td>0,052282</td>
      <td>0,065809</td>
      <td>0,038664</td>
      <td>0,109401</td>
      <td>0,071104</td>
      <td>0,042294</td>
      <td>0,049967</td>
      <td>...</td>
      <td>0,042565</td>
      <td>0,037685</td>
      <td>0,037557</td>
      <td>0,037643</td>
      <td>0,037513</td>
      <td>0,037560</td>
      <td>0,037465</td>
      <td>0,072077</td>
      <td>0,037249</td>
      <td>0,036716</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_log_error</th>
      <td>0,055353</td>
      <td>0,045000</td>
      <td>0,110219</td>
      <td>0,065633</td>
      <td>0,063589</td>
      <td>0,044412</td>
      <td>0,123433</td>
      <td>0,092312</td>
      <td>0,046130</td>
      <td>0,055243</td>
      <td>...</td>
      <td>0,046540</td>
      <td>0,041804</td>
      <td>0,041771</td>
      <td>0,045175</td>
      <td>0,044628</td>
      <td>0,041617</td>
      <td>0,044405</td>
      <td>0,079651</td>
      <td>0,042799</td>
      <td>0,041530</td>
    </tr>
    <tr>
      <th>r2_score</th>
      <td>0,810900</td>
      <td>0,880328</td>
      <td>0,398076</td>
      <td>0,775957</td>
      <td>0,642812</td>
      <td>0,875719</td>
      <td>0,021603</td>
      <td>0,586514</td>
      <td>0,851767</td>
      <td>0,793671</td>
      <td>...</td>
      <td>0,849809</td>
      <td>0,880142</td>
      <td>0,880952</td>
      <td>0,880586</td>
      <td>0,881347</td>
      <td>0,880887</td>
      <td>0,881613</td>
      <td>0,548121</td>
      <td>0,882883</td>
      <td>0,886321</td>
    </tr>
    <tr>
      <th>root_mean_squared_error</th>
      <td>4,216362</td>
      <td>3,329810</td>
      <td>7,521765</td>
      <td>4,595604</td>
      <td>5,784601</td>
      <td>3,398540</td>
      <td>9,616354</td>
      <td>6,250011</td>
      <td>3,717661</td>
      <td>4,392072</td>
      <td>...</td>
      <td>3,741447</td>
      <td>3,312533</td>
      <td>3,301242</td>
      <td>3,308795</td>
      <td>3,297389</td>
      <td>3,301485</td>
      <td>3,293182</td>
      <td>6,335581</td>
      <td>3,274209</td>
      <td>3,227365</td>
    </tr>
    <tr>
      <th>root_mean_squared_log_error</th>
      <td>0,243184</td>
      <td>0,197702</td>
      <td>0,484227</td>
      <td>0,288349</td>
      <td>0,279367</td>
      <td>0,195116</td>
      <td>0,542281</td>
      <td>0,405559</td>
      <td>0,202666</td>
      <td>0,242702</td>
      <td>...</td>
      <td>0,204464</td>
      <td>0,183658</td>
      <td>0,183514</td>
      <td>0,198468</td>
      <td>0,196067</td>
      <td>0,182836</td>
      <td>0,195087</td>
      <td>0,349935</td>
      <td>0,188031</td>
      <td>0,182455</td>
    </tr>
    <tr>
      <th>spearman_correlation</th>
      <td>0,944743</td>
      <td>0,953618</td>
      <td>0,857965</td>
      <td>0,914703</td>
      <td>0,939846</td>
      <td>0,956159</td>
      <td>0,828187</td>
      <td>0,942069</td>
      <td>0,952581</td>
      <td>0,935477</td>
      <td>...</td>
      <td>0,951287</td>
      <td>0,960335</td>
      <td>0,960195</td>
      <td>0,960279</td>
      <td>0,960288</td>
      <td>0,960323</td>
      <td>0,960161</td>
      <td>0,941254</td>
      <td>0,960293</td>
      <td>0,962158</td>
    </tr>
    <tr>
      <th>spearman_correlation_max</th>
      <td>0,944743</td>
      <td>0,953618</td>
      <td>0,953618</td>
      <td>0,953618</td>
      <td>0,953618</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>...</td>
      <td>0,960303</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,962158</td>
    </tr>
  </tbody>
</table>
<p>12 rijen × 30 kolommen</p>
</div>

## <a name="retrieve-the-best-model"></a>Het beste model ophalen

Selecteer uit de herhalingen de beste pijplijn. Met de methode `get_output` in `automl_classifier` worden de beste uitvoering en het aangepaste model voor de laatste aangepaste aanroep geretourneerd. `get_output` heeft overloads die u in staat stellen om de beste uitvoering en het aangepaste model op te halen voor alle geregistreerde metrische gegevens of voor een bepaalde herhaling.

```python
best_run, fitted_model = local_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="register-the-model"></a>Het model registreren

Registreer het model in uw werkruimte in de Azure Machine Learning-service.


```python
description = 'Automated Machine Learning Model'
tags = None
local_run.register_model(description=description, tags=tags)
local_run.model_id # Use this id to deploy the model as a web service in Azure
```

## <a name="test-the-best-model-accuracy"></a>Nauwkeurigheid van het beste model testen

Gebruik het beste model om voorspellingen uit te voeren voor de testgegevensset. De functie `predict` maakt gebruik van het beste model. Met deze functie worden ook de waarden van y (reiskosten) uit de gegevensset `x_test` voorspeld. Druk de eerste 10 voorspelde kostenwaarden uit `y_predict` af.

```python
y_predict = fitted_model.predict(x_test.values)
print(y_predict[:10])
```

Maak een spreidingsdiagram om de voorspelde kostenwaarden in vergelijking met de daadwerkelijke kostenwaarden te visualiseren. De volgende code gebruikt de functie `distance` als x-as en de reis `cost` als y-as. De eerste 100 voorspelde en daadwerkelijke kostenwaarden worden in afzonderlijke reeksen gemaakt om de variantie van de voorspelde kosten van elke reisafstandswaarde. Als u de grafiek bekijkt, ziet u dat de afstand/kosten-relatie bijna lineair is en dat de voorspelde kostenwaarden in de meeste gevallen dicht bij de daadwerkelijke kostenwaarden voor dezelfde reisafstand liggen.

```python
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(14, 10))
ax1 = fig.add_subplot(111)

distance_vals = [x[4] for x in x_test.values]
y_actual = y_test.values.flatten().tolist()

ax1.scatter(distance_vals[:100], y_predict[:100], s=18, c='b', marker="s", label='Predicted')
ax1.scatter(distance_vals[:100], y_actual[:100], s=18, c='r', marker="o", label='Actual')

ax1.set_xlabel('distance (mi)')
ax1.set_title('Predicted and Actual Cost/Distance')
ax1.set_ylabel('Cost ($)')

plt.legend(loc='upper left', prop={'size': 12})
plt.rcParams.update({'font.size': 14})
plt.show()
```

![Spreidingsdiagram voor voorspelling](./media/tutorial-auto-train-models/automl-scatter-plot.png)

Bereken de `root mean squared error` van de resultaten. Gebruik het gegevensframe `y_test` en converteer dit naar een lijst om de voorspelde waarden te vergelijken. Met de functie `mean_squared_error` wordt de gemiddelde gekwadrateerde fout berekend tussen twee matrices met waarden. De vierkantswortel van het resultaat resulteert in een fout in dezelfde eenheden als de y-variabele (kosten) en geeft ongeveer aan hoe ver uw voorspellingen van de werkelijke waarde liggen.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

    3.2204936862688798

Voer de volgende code uit om MAPE te berekenen (gemiddelde absolute foutpercentage) met behulp van de volledige gegevenssets `y_actual` en `y_predict`. Met deze metriek worden de verschillen berekend tussen elke voorspelde en werkelijke waarde, worden de verschillen opgeteld, en wordt deze som vervolgens uitgedrukt als een percentage van de totale werkelijke waarden.

```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

    Model MAPE:
    0.10545153869569586

    Model Accuracy:
    0.8945484613043041

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze geautomatiseerde Machine Learning-zelfstudie, hebt u:

> [!div class="checklist"]
> * Een werkruimte geconfigureerd en gegevens voorbereid voor een experiment
> * Getraind met behulp van een lokaal geautomatiseerd regressiemodel met aangepaste parameters
> * Trainingsresultaten verkend en gecontroleerd
> * Het beste model geregistreerd

[Uw model implementeren](tutorial-deploy-models-with-aml.md) met Azure Machine Learning.
