---
title: 'Zelfstudie 2: Een regressiemodel trainen met geautomatiseerde Machine Learning - Azure Machine Learning-service'
description: Leer hoe u een ML-model genereert met behulp van geautomatiseerde Machine Learning.  Azure Machine Learning kan voorbewerking van gegevens, selectie van algoritme en selectie van hyperparameters automatisch voor u uitvoeren. Het uiteindelijke model kan vervolgens geïmplementeerd met de Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.openlocfilehash: 0c4b9c31c4ae8a6a7a7044887c9af051966c745e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879528"
---
# <a name="tutorial-2-train-a-regression-model-with-automated-machine-learning"></a>Zelfstudie 2: Een regressiemodel trainen met geautomatiseerde Machine Learning

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

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://aka.ms/AMLfree) aan voordat u begint.

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

import os

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
package_saved = dprep.Package.open(".\dflow")
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
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
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
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
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
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>3,57523</td>
      <td>3</td>
      <td>9,91106</td>
      <td>15,9327</td>
      <td>19</td>
      <td>22,0225</td>
      <td>23</td>
      <td>23</td>
      <td>14,2326</td>
      <td>6,34926</td>
      <td>40,3131</td>
      <td>-0,693335</td>
      <td>-0,459336</td>
    </tr>
    <tr>
      <th>pickup_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>5,32313</td>
      <td>4,92308</td>
      <td>14,2214</td>
      <td>29,5244</td>
      <td>44,6436</td>
      <td>56,3767</td>
      <td>58,9798</td>
      <td>59</td>
      <td>29,4635</td>
      <td>17,4396</td>
      <td>304,14</td>
      <td>0,00440324</td>
      <td>-1,20458</td>
    </tr>
    <tr>
      <th>pickup_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>4,99286</td>
      <td>4,91954</td>
      <td>14,6121</td>
      <td>29,9239</td>
      <td>44,5221</td>
      <td>56,6792</td>
      <td>59</td>
      <td>59</td>
      <td>29,6225</td>
      <td>17,3868</td>
      <td>302,302</td>
      <td>-0,0227466</td>
      <td>-1,19409</td>
    </tr>
    <tr>
      <th>dropoff_weekday</th>
      <td>FieldType.STRING</td>
      <td>Vrijdag</td>
      <td>Woensdag</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
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
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>3,23217</td>
      <td>2,93333</td>
      <td>9,92334</td>
      <td>15,9135</td>
      <td>19</td>
      <td>22,2739</td>
      <td>23</td>
      <td>23</td>
      <td>14,1815</td>
      <td>6,45578</td>
      <td>41,677</td>
      <td>-0,691001</td>
      <td>-0,500215</td>
    </tr>
    <tr>
      <th>dropoff_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>5,1064</td>
      <td>5</td>
      <td>14,2051</td>
      <td>29,079</td>
      <td>44,2937</td>
      <td>56,6338</td>
      <td>58,9984</td>
      <td>59</td>
      <td>29,353</td>
      <td>17,4241</td>
      <td>303,598</td>
      <td>0,0142562</td>
      <td>1,21531</td>
    </tr>
    <tr>
      <th>dropoff_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>5,03373</td>
      <td>5</td>
      <td>14,7471</td>
      <td>29,598</td>
      <td>45,3216</td>
      <td>56,1044</td>
      <td>58,9728</td>
      <td>59</td>
      <td>29,7923</td>
      <td>17,481</td>
      <td>305,585</td>
      <td>0,0281313</td>
      <td>1,21965</td>
    </tr>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>J</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
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
      <td>-74,0782</td>
      <td>-73,7365</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>74,0763</td>
      <td>73,9625</td>
      <td>73,9629</td>
      <td>73,949</td>
      <td>73,949</td>
      <td>-73,8667</td>
      <td>-73,8304</td>
      <td>-73,8232</td>
      <td>-73,7698</td>
      <td>-73,9139</td>
      <td>0,0487111</td>
      <td>0,00237277</td>
      <td>0,402697</td>
      <td>-0,613516</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40,5755</td>
      <td>40,8799</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>40,6329</td>
      <td>40,7131</td>
      <td>40,7116</td>
      <td>40,7214</td>
      <td>40,7581</td>
      <td>40,8051</td>
      <td>40,8489</td>
      <td>40,8676</td>
      <td>40,8777</td>
      <td>40,7652</td>
      <td>0,0483485</td>
      <td>0,00233758</td>
      <td>0,228088</td>
      <td>-0,598862</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74,0857</td>
      <td>-73,7209</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>-74,0788</td>
      <td>-73,9856</td>
      <td>-73,9858</td>
      <td>-73,959</td>
      <td>-73,9367</td>
      <td>-73,8848</td>
      <td>-73,8155</td>
      <td>-73,7767</td>
      <td>-73,7335</td>
      <td>-73,9207</td>
      <td>0,055961</td>
      <td>0,00313163</td>
      <td>0,648649</td>
      <td>0,0229141</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40,5835</td>
      <td>40,8797</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>40,5977</td>
      <td>40,6954</td>
      <td>40,6951</td>
      <td>40,7275</td>
      <td>40,7582</td>
      <td>40,7884</td>
      <td>40,8504</td>
      <td>40,868</td>
      <td>40,8786</td>
      <td>40,7595</td>
      <td>0,0504621</td>
      <td>0,00254642</td>
      <td>0,0484179</td>
      <td>0,0368799</td>
    </tr>
    <tr>
      <th>passagiers</th>
      <td>FieldType.DECIMAL</td>
      <td>1</td>
      <td>6</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
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
      <td>2,32979</td>
      <td>1,79978</td>
      <td>3,2392</td>
      <td>0,834099</td>
      <td>1,11111</td>
    </tr>
    <tr>
      <th>kosten</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>444</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>7059,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0,0</td>
      <td>0</td>
      <td>3,01808</td>
      <td>3,0125</td>
      <td>5,91545</td>
      <td>9,49055</td>
      <td>16,5816</td>
      <td>33,5638</td>
      <td>51,9924</td>
      <td>81,1368</td>
      <td>12,9112</td>
      <td>11,6447</td>
      <td>135,599</td>
      <td>8,6842</td>
      <td>269,818</td>
    </tr>
  </tbody>
</table>

U bereidt het model voor het experiment voor door kolommen toe te voegen aan `dflow_x` die functies worden bij het maken van het model. U definieert `dflow_y` als voorspellingswaarde; kosten.

```python
dflow_X = dflow_prepared.keep_columns(['pickup_weekday', 'dropoff_latitude', 'dropoff_longitude','pickup_hour','pickup_longitude','pickup_latitude','passengers'])
dflow_y = dflow_prepared.keep_columns('cost')
```

### <a name="split-data-into-train-and-test-sets"></a>Gegevens splitsen in sets voor trainen en voor testen

Nu splitst u de gegevens in trainings- en testsets met behulp van de functie `train_test_split` in de `sklearn`-bibliotheek. Met deze functie worden de gegevens verdeeld in de gegevensset x (functies) voor het trainen van het model, en de gegevensset y (te voorspellen waarden) voor testen. Met de parameter `test_size` wordt het percentage gegevens bepaald dat moet worden toegewezen aan testen. Met de parameter `random_state` wordt de willekeurige generator ingesteld, zodat de verdeling tussen trainen en testen altijd deterministisch is.

```python
from sklearn.model_selection import train_test_split

x_df = dflow_X.to_pandas_dataframe()
y_df = dflow_y.to_pandas_dataframe()

x_train, x_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=123)
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

Start het experiment voor lokale uitvoering. Geef het gedefinieerde `automated_ml_config`-object door aan het experiment, en stel de uitvoer in op `true` om de voortgang weer te geven tijdens het experiment.


```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, experiment_name)
local_run = experiment.submit(automated_ml_config, show_output=True)
```

    Parent Run ID: AutoML_83117da4-07e3-473a-b83e-99471bfa9e09
    *******************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    *******************************************************************************************
    
     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler ExtremeRandomTrees                0:00:21       0.6498    0.6498
             1   MinMaxScaler GradientBoosting                  0:00:22       0.6624    0.6624
             2   StandardScalerWrapper KNN                      0:00:18       0.7267    0.7267
             3   StandardScalerWrapper GradientBoosting         0:00:18       0.5003    0.7267
             4    Ensemble                                      0:00:38       0.6659    0.7267
    

## <a name="explore-the-results"></a>De resultaten verkennen

Bekijk de resultaten van de automatische training met een Jupyter-widget of door de geschiedenis van het experiment te controleren.

### <a name="option-1-add-a-jupyter-widget-to-see-results"></a>Optie 1: een Jupyter-widget toevoegen om de resultaten te zien

Als u gebruikmaakt van een Juypter-notebook, gebruikt u deze Juypter-widget om een grafiek en een tabel met alle resultaten te zien.


```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Details van de uitvoering van de Jupyter-widget](./media/tutorial-auto-train-models/jup-widget-auto.png)

### <a name="option-2-get-and-examine-all-run-iterations-in-python"></a>Optie 2: alle uitgevoerde herhalingen ophalen en onderzoeken in Python

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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>explained_variance</th>
      <td>0,113810</td>
      <td>0,093514</td>
      <td>-0,010248</td>
      <td>0,005867</td>
      <td>0,108187</td>
    </tr>
    <tr>
      <th>mean_absolute_error</th>
      <td>7,004893</td>
      <td>6,348354</td>
      <td>6,493000</td>
      <td>7,045597</td>
      <td>6,646850</td>
    </tr>
    <tr>
      <th>median_absolute_error</th>
      <td>4,834063</td>
      <td>3,503244</td>
      <td>3,321553</td>
      <td>4,349547</td>
      <td>4,389995</td>
    </tr>
    <tr>
      <th>normalized_mean_absolute_error</th>
      <td>0,077832</td>
      <td>0,070537</td>
      <td>0,072144</td>
      <td>0,078284</td>
      <td>0,073854</td>
    </tr>
    <tr>
      <th>normalized_median_absolute_error</th>
      <td>0,053712</td>
      <td>0,038925</td>
      <td>0,036906</td>
      <td>0,048328</td>
      <td>0,048778</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_error</th>
      <td>0,117819</td>
      <td>0,120518</td>
      <td>0,126141</td>
      <td>0,124289</td>
      <td>0,118340</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_log_error</th>
      <td>0,177689</td>
      <td>0,163360</td>
      <td>0,168101</td>
      <td>0,178250</td>
      <td>0,168685</td>
    </tr>
    <tr>
      <th>r2_score</th>
      <td>0,104661</td>
      <td>0,064075</td>
      <td>-0,036158</td>
      <td>-0,004403</td>
      <td>0,096976</td>
    </tr>
    <tr>
      <th>root_mean_squared_error</th>
      <td>10,603744</td>
      <td>10,846632</td>
      <td>11,352731</td>
      <td>11,185972</td>
      <td>10,650593</td>
    </tr>
    <tr>
      <th>root_mean_squared_log_error</th>
      <td>0,801531</td>
      <td>0,736896</td>
      <td>0,758279</td>
      <td>0,804062</td>
      <td>0,760913</td>
    </tr>
    <tr>
      <th>spearman_correlation</th>
      <td>0,549825</td>
      <td>0,562435</td>
      <td>0,526702</td>
      <td>0,500302</td>
      <td>0,565857</td>
    </tr>
    <tr>
      <th>spearman_correlation_max</th>
      <td>0,549825</td>
      <td>0,562435</td>
      <td>0,562435</td>
      <td>0,562435</td>
      <td>0,565857</td>
    </tr>
  </tbody>
</table>
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

Vergelijk de voorspelde kostenwaarden met de werkelijke kostenwaarden. Gebruik het gegevensframe `y_test` en converteer dit naar een lijst om de voorspelde waarden te vergelijken. Met de functie `mean_absolute_error` wordt de gemiddelde absolute waardefout berekend tussen twee matrices met waarden. In dit voorbeeld betekent een absolute fout van 3,5 dat de kostenvoorspelling van het model gemiddeld binnen plusminus 3,5 van de werkelijke waarde valt.

```python
from sklearn.metrics import mean_absolute_error

y_actual = y_test.values.flatten().tolist()
mean_absolute_error(y_actual, y_predict)
```

    [ 3.16213051 ]

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
    0.22424976634422172

    Model Accuracy:
    0.7757502336557782

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
