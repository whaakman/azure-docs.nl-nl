---
title: Visualiseer experimenten met TensorBoard en Azure Machine Learning-service
description: Start TensorBoard om te visualiseren van de geschiedenis van het experiment uitvoert en Identificeer potentiële probleemgebieden voor hyperparameter afstemmen en opnieuw trainen.
services: machine-learning
author: maxluk
ms.author: maxluk
ms.reviewer: nibaccam
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: babd4cdf8b7ed9e04b4bd975d840688b27439c4f
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560857"
---
# <a name="visualize-experiment-runs-and-metrics-with-tensorboard-and-azure-machine-learning"></a>Visualiseer en metrische gegevens met TensorBoard en Azure Machine Learning-experimenten

In dit artikel leert u hoe u uw experimenten en metrische gegevens weergeven in met behulp van TensorBoard [de `tensorboard` pakket](https://docs.microsoft.com/python/api/azureml-tensorboard/?view=azure-ml-py) in de belangrijkste Azure Machine Learning-service SDK. Nadat u uw experimenten hebt bekeken, kunt u beter af en opnieuw trainen van uw machine learning-modellen.

[TensorBoard](https://www.tensorflow.org/tensorboard/r1/overview) is een suite van web-apps voor het inspecteren en inzicht krijgen in uw experiment structuur en de prestaties.

Hoe u TensorBoard starten met Azure Machine Learning-experimenten, is afhankelijk van het type experiment:
+ Als uw experiment levert systeemeigen logboekbestanden die gebruikt door TensorBoard, zoals PyTorch, Chainer en TensorFlow experimenten worden, dan u kunt [TensorBoard rechtstreeks starten](#direct) geschiedenis van het experiment uitvoeren. 

+ Gebruik voor experimenten die geen systeemeigen TensorBoard analyserapporten, uitvoerbestanden zoals zoals Scikit-informatie of Azure Machine Learning-experimenten, [de `export_to_tensorboard()` methode](#export) de uitvoeringsgeschiedenis exporteren als TensorBoard logboeken en starten TensorBoard van daaruit. 

## <a name="prerequisites"></a>Vereisten

* Als u wilt starten TensorBoard en bekijk uw experiment uitvoeringsgeschiedenis, moeten uw experimenten logboekregistratie voor het bijhouden van de metrische gegevens en prestaties eerder hebt ingeschakeld.  

* De code in deze instructies kan worden uitgevoerd in een van de volgende omgevingen: 

    * Azure Machine Learning-Notebook VM - geen downloads of installatie nodig

        * Voltooid de [cloud-gebaseerde notebook snelstartgids](quickstart-run-cloud-notebook.md#create-notebook) maken een toegewezen notebookserver vooraf geladen met de SDK en de opslagplaats met voorbeelden.

        * Zoeken in de map samples op de notebookserver, twee voltooid en laptops uitgebreid door te navigeren naar deze map: **How-to-naar-gebruik-azureml > training met deep learning**.
        * export-Run-history-to-Run-History.ipynb
        * tensorboard.ipynb

    * Uw eigen Juptyer notebook-server
      * Gebruik de [maken van een artikel werkruimte](setup-create-workspace.md) naar
          * [Installeer de Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) met de `tensorboard` extra
          * Een werkruimte en het configuratiebestand (config.json) maken
  
<a name="direct"></a>
## <a name="option-1-directly-view-run-history-in-tensorboard"></a>Optie 1: Rechtstreeks uitvoeringsgeschiedenis weergeven in TensorBoard

Deze optie werkt voor experimenten dat systeemeigen uitvoer verbruikbare door TensorBoard, zoals PyTorch, Chainer logboekbestanden en TensorFlow-experimenten. Als dat niet het geval van uw experiment, gebruikt u [de `export_to_tensorboard()` methode](#export) in plaats daarvan.

De volgende voorbeeldcode maakt gebruik van de [MNIST demo experiment](https://raw.githubusercontent.com/tensorflow/tensorflow/r1.8/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py) vanuit TensorFlow van-opslagplaats in een externe compute-doel, Azure Machine Learning-Computing. Vervolgens we trainen het model met de SDK aangepaste [TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py), en start vervolgens TensorBoard op basis van dit experiment TensorFlow, dat wil zeggen, een experiment die systeemeigen TensorBoard gebeurtenisbestanden uitvoert.

### <a name="set-experiment-name-and-create-project-folder"></a>Stel de naam van het experiment en projectmap maken

Hier wordt het experiment een naam en maak de map. 
 
```python
from os import path, makedirs
experiment_name = 'tensorboard-demo'

# experiment folder
exp_dir = './sample_projects/' + experiment_name

if not path.exists(exp_dir):
    makedirs(exp_dir)

```

### <a name="download-tensorflow-demo-experiment-code"></a>TensorFlow-demo experiment code downloaden

TensorFlow van-opslagplaats is een demo MNIST met uitgebreide TensorBoard instrumentatie. We dit niet doet, noch moet, wijzigen van deze demo-code voor het werken met de Azure Machine Learning-service. In de volgende code wordt de MNIST-code downloaden en opslaan in de map van de zojuist gemaakte experiment.

```python
import requests
import os

tf_code = requests.get("https://raw.githubusercontent.com/tensorflow/tensorflow/r1.8/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py")
with open(os.path.join(exp_dir, "mnist_with_summaries.py"), "w") as file:
    file.write(tf_code.text)
```
In het hele bestand code MNIST mnist_with_summaries.py, Let op: Er zijn regels die aanroep `tf.summary.scalar()`, `tf.summary.histogram()`, `tf.summary.FileWriter()` enzovoort. Deze methoden groep, het logboek en tag belangrijke metrische gegevens van uw experimenten in de uitvoeringsgeschiedenis. De `tf.summary.FileWriter()` is vooral belangrijk als deze de gegevens uit uw geregistreerde experiment metrische gegevens, waardoor TensorBoard voor het genereren van visualisaties uit deze serialiseert.

 ### <a name="configure-experiment"></a>Configureren van experiment

In de volgende we onze experiment configureren en instellen van mappen voor logboeken en gegevens. Deze logboeken worden geüpload naar de artefact-Service, die TensorBoard toegang heeft tot later opnieuw.

>[!Note]
> Voor dit voorbeeld TensorFlow moet u TensorFlow installeren op uw lokale computer. Bovendien is moet de module TensorBoard (dat wil zeggen, het account dat is opgenomen in TensorFlow) toegankelijk zijn voor van dit notitieblok kernel, omdat de lokale machine wat TensorBoard wordt uitgevoerd.

```Python
import azureml.core
from azureml.core import Workspace
from azureml.core import Experiment

ws = Workspace.from_config()

# create directories for experiment logs and dataset
logs_dir = os.path.join(os.curdir, "logs")
data_dir = os.path.abspath(os.path.join(os.curdir, "mnist_data"))

if not path.exists(data_dir):
    makedirs(data_dir)

os.environ["TEST_TMPDIR"] = data_dir

# Writing logs to ./logs results in their being uploaded to Artifact Service,
# and thus, made accessible to our TensorBoard instance.
arguments_list = ["--log_dir", logs_dir]

# Create an experiment
exp = Experiment(ws, experiment_name)
```

### <a name="create-a-cluster-for-your-experiment"></a>Een cluster maken voor uw experiment
We maken een cluster AmlCompute voor dit experiment, maar uw experimenten in elke omgeving kunnen worden gemaakt en u kunt nog steeds TensorBoard starten op basis van het experiment uitvoeringsgeschiedenis. 

```Python
from azureml.core.compute import ComputeTarget, AmlCompute

cluster_name = "cpucluster"

cts = ws.compute_targets
found = False
if cluster_name in cts and cts[cluster_name].type == 'AmlCompute':
   found = True
   print('Found existing compute target.')
   compute_target = cts[cluster_name]
if not found:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

compute_target.wait_for_completion(show_output=True, min_node_count=None)

# use get_status() to get a detailed status for the current cluster. 
# print(compute_target.get_status().serialize())
```

### <a name="submit-run-with-tensorflow-estimator"></a>Uitvoeren met TensorFlow estimator verzenden

De estimator TensorFlow biedt een eenvoudige manier om een taak TensorFlow-training op een compute-doel starten. Deze geïmplementeerd via de algemene [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) klasse, die kan worden gebruikt voor de ondersteuning van elk gewenst framework. Zie voor meer informatie over het trainen van modellen met behulp van de algemene estimator [trainen van modellen met Azure Machine Learning met behulp van estimator](how-to-train-ml-models.md)

```Python
from azureml.train.dnn import TensorFlow
script_params = {"--log_dir": "./logs"}

tf_estimator = TensorFlow(source_directory=exp_dir,
                          compute_target=compute_target,
                          entry_script='mnist_with_summaries.py',
                          script_params=script_params)

run = exp.submit(tf_estimator)
```

### <a name="launch-tensorboard"></a>TensorBoard starten

Tijdens de uitvoering of nadat deze is voltooid, kunt u TensorBoard starten. In het volgende, maken we een exemplaar van het object TensorBoard `tb`, duurt het experiment uitvoeringsgeschiedenis geladen de `run`, en daarna wordt gestart TensorBoard met de `start()` methode. 
  
De [TensorBoard constructor](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py) neemt een matrix van wordt uitgevoerd, dus zorg ervoor dat en geven deze in als een matrix met één element.

```python
from azureml.tensorboard import Tensorboard

tb = Tensorboard([run])

# If successful, start() returns a string with the URI of the instance.
tb.start()

# After your job completes, be sure to stop() the streaming otherwise it will continue to run. 
tb.stop()
```

<a name="export"></a>

## <a name="option-2-export-history-as-log-to-view-in-tensorboard"></a>Optie 2: Geschiedenis exporteren als aan te melden in TensorBoard weergeven

De volgende code stelt u een voorbeeldexperiment, begint het proces voor logboekregistratie met behulp van de uitvoeringsgeschiedenis API's van Azure Machine Learning en exporteert u het experiment uitvoeringsgeschiedenis in Logboeken verbruikbare door TensorBoard voor visualisatie. 

### <a name="set-up-experiment"></a>Instellen van experiment

De volgende code stelt u een nieuw experiment en namen van de map uitvoeren `root_run`. 

```python
from azureml.core import Workspace, Experiment
import azuremml.core

# set experiment name and run name
ws = Workspace.from_config()
experiment_name = 'export-to-tensorboard'
exp = Experiment(ws, experiment_name)
root_run = exp.start_logging()
```

Hier laden we de gegevensset diabetes--een ingebouwde kleine gegevensset is die wordt geleverd met scikit-informatie, en deze opsplitsen in test- en training van sets.

```Python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
X, y = load_diabetes(return_X_y=True)
columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
x_train, x_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
data = {
    "train":{"x":x_train, "y":y_train},        
    "test":{"x":x_test, "y":y_test}
}
```

### <a name="run-experiment-and-log-metrics"></a>Experiment uit te voeren en meld u metrische gegevens

Voor deze code wordt een lineair regressiemodel trainen en meld u belangrijke metrische gegevens, de alpha coëfficiënt `alpha` en mean squared error, `mse`, in de uitvoeringsgeschiedenis.

```Python
from tqdm import tqdm
alphas = [.1, .2, .3, .4, .5, .6 , .7]
# try a bunch of alpha values in a Linear Regression (aka Ridge regression) mode
for alpha in tqdm(alphas):
  # create child runs and fit lines for the resulting models
  with root_run.child_run("alpha" + str(alpha)) as run
 
   reg = Ridge(alpha=alpha)
   reg.fit(data["train"]["x"], data["train"]["y"])    
 
   preds = reg.predict(data["test"]["x"])
   mse = mean_squared_error(preds, data["test"]["y"])
   # End train and eval

# log alpha, mean_squared_error and feature names in run history
   root_run.log("alpha", alpha)
   root_run.log("mse", mse)
```

### <a name="export-runs-to-tensorboard"></a>Exporteren wordt uitgevoerd naar TensorBoard

Met de SDK [export_to_tensorboard()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.export?view=azure-ml-py) methode, kunnen we de uitvoeringsgeschiedenis van onze Azure machine learning-experiment in TensorBoard Logboeken, exporteren, zodat we ze via TensorBoard kunt bekijken.  

In de volgende code, maken we de map `logdir` in onze huidige werkmap. Deze map is waar we ons experiment uitvoeringsgeschiedenis worden geëxporteerd en logboeken van `root_run` en vervolgens die worden uitgevoerd als voltooid te markeren. 

```Python
from azureml.tensorboard.export import export_to_tensorboard
import os

logdir = 'exportedTBlogs'
log_path = os.path.join(os.getcwd(), logdir)
try:
    os.stat(log_path)
except os.error:
    os.mkdir(log_path)
print(logdir)

# export run history for the project
export_to_tensorboard(root_run, logdir)

root_run.complete()
```

>[!Note]
 U kunt ook een bepaalde uitvoering exporteren naar TensorBoard door de naam van de uitvoering op te geven  `export_to_tensorboard(run_name, logdir)`

Starten en stoppen TensorBoard eenmaal onze uitvoeringsgeschiedenis voor dit experiment is uitgevoerd, kunnen we starten TensorBoard met de [start()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py#start-start-browser-false-) methode. 

```Python
from azureml.tensorboard import Tensorboard

# The TensorBoard constructor takes an array of runs, so be sure and pass it in as a single-element array here
tb = Tensorboard([], local_root=logdir, port=6006)

# If successful, start() returns a string with the URI of the instance.
tb.start()
```

Wanneer u klaar bent, zorg ervoor dat aan te roepen de [stop()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py#stop--) methode van het object TensorBoard. Anders blijft TensorBoard actief totdat u de kernel van de notebook afsluiten. 

```python
tb.stop()
```

## <a name="next-steps"></a>Volgende stappen

In deze procedures voor u gemaakt twee experimenten en hebt geleerd hoe u TensorBoard starten op basis van hun uitvoeringsgeschiedenis gebieden voor mogelijke afstemmen en opnieuw trainen vast te stellen. 

* Als u tevreden met het model bent, Ga naar onze [over het implementeren van een model](how-to-deploy-and-where.md) artikel. 
* Meer informatie over [hyperparameter afstemmen](how-to-tune-hyperparameters.md). 
