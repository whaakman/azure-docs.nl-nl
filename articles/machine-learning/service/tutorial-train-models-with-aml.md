---
title: 'Zelfstudie over classificatie van afbeeldingen: Modellen trainen'
titleSuffix: Azure Machine Learning service
description: In deze zelfstudie leert u hoe u met de service Azure Machine Learning een model voor het classificeren van afbeeldingen traint met scikit-learn in een Python Jupyter-notebook. Deze zelfstudie is deel één van een serie van twee.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 117934c83d54cb5454f476ffb3b1a1437c0fd30b
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811474"
---
# <a name="tutorial-train-an-image-classification-model-with-azure-machine-learning-service"></a>Zelfstudie: een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning Service

In deze zelfstudie gaat u een machine learning-model zowel lokaal als op externe rekenresources trainen. U gebruikt de werkstroom voor training en implementatie voor de Azure Machine Learning-service in een Python Jupyter-notebook. Vervolgens kunt u het notebook gebruiken als een sjabloon voor het trainen van uw eigen machine learning-model met uw eigen gegevens. Deze zelfstudie is **deel één van een serie van twee**.  

In deze zelfstudie traint u een eenvoudig logistiek regressiemodel met de gegevensset [MNIST](https://yann.lecun.com/exdb/mnist/) en [scikit-learn](https://scikit-learn.org) met behulp van Azure Machine Learning Service. MNIST is een populaire gegevensset die bestaat uit 70.000 afbeeldingen in grijstinten. Elke afbeelding is een handgeschreven cijfer van 28 x 28 pixels, dat een getal tussen 0-9 vertegenwoordigt. Het doel is om een classificatiemechanisme met meerdere klassen te maken om het cijfer te identificeren dat een bepaalde afbeelding vertegenwoordigt. 

U leert hoe u de volgende acties uitvoert:

> [!div class="checklist"]
> * De ontwikkelomgeving instellen.
> * De gegevens downloaden en controleren.
> * Een eenvoudig logistiek regressiemodel lokaal trainen met behulp van de populaire bibliotheek voor machine learning scikit-learn. 
> * Meerdere modellen trainen op een extern cluster.
> * Trainingsresultaten bekijken en het beste model registreren.

In [deel twee van deze zelfstudie](tutorial-deploy-models-with-aml.md) leert u hoe u een model selecteert en dit implementeert. 

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](http://aka.ms/AMLFree).

>[!NOTE]
> Code in dit artikel is getest met Azure Machine Learning SDK-versie 1.0.2.

## <a name="get-the-notebook"></a>De notebook ophalen

Voor uw gemak is deze zelfstudie beschikbaar gemaakt als een [Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb). Voer het `tutorials/img-classification-part1-training.ipynb`-notebook uit in Azure Notebooks of op uw eigen Jupyter-notebookserver.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="set-up-your-development-environment"></a>De ontwikkelomgeving instellen

De configuratie van uw ontwikkelomgeving kan worden uitgevoerd met een Python-notebook. De configuratie bestaat uit de volgende acties:

* Python-pakketten importeren.
* Verbinding maken met een werkruimte, zodat uw lokale computer met externe bronnen kan communiceren.
* Een experiment maken om alle uitvoeringen (runs) bij te houden.
* Een extern rekendoel voor training maken.

### <a name="import-packages"></a>Pakketten importeren

Importeer de Python-pakketten die u nodig hebt in deze sessie. Controleer ook wat de versie is van de SDK voor Azure Machine Learning:

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-a-workspace"></a>Verbinding maken met een werkruimte

Maak een werkruimte-object van de bestaande werkruimte. `Workspace.from_config()` leest het bestand **config.json** en laadt de gegevens in een object met de naam `ws`:

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-an-experiment"></a>Een experiment maken

Maak een experiment voor het volgen van de runs in uw werkruimte. Een werkruimte kan meerdere experimenten bevatten: 

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-or-attach-an-existing-amlcompute"></a>Een bestaande AMlCompute maken of koppelen

Met Azure Machine Learning Compute (AmlCompute), een beheerde service, kunnen gegevenswetenschappers Machine Learning-modellen trainen op clusters met virtuele Azure-machines. Voorbeelden hiervan zijn virtuele machines met GPU-ondersteuning. In deze zelfstudie stelt u AmlCompute in als uw trainingsomgeving. Met deze code wordt het rekencluster voor u gemaakt als dat nog niet in uw werkruimte bestaat.

 **Het maken van het rekencluster duurt ongeveer vijf minuten.** Als het rekencluster al aanwezig is in de werkruimte, wordt het cluster gebruikt door deze code en wordt er geen nieuw cluster gemaakt:


```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpucluster")
compute_min_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("AML_COMPUTE_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use the 'status' property    
    print(compute_target.status.serialize())
```

U beschikt nu over de vereiste pakketten en rekenresources voor het trainen van een model in de cloud. 

## <a name="explore-data"></a>Gegevens verkennen

Voordat u een model gaat trainen, is het belangrijk dat u de gegevens begrijpt die u gebruikt voor het trainen. Bovendien moet u de gegevens naar de cloud kopiëren. Deze kunnen vervolgens worden geopend door uw cloudomgeving voor de training. In deze sectie leert u hoe u de volgende acties uitvoert:

* De MNIST-gegevensset downloaden.
* Enkele voorbeeldafbeeldingen weergeven.
* Gegevens uploaden naar de cloud.

### <a name="download-the-mnist-dataset"></a>De MNIST-gegevensset downloaden

Download de MNIST-gegevensset en sla de bestanden op in een lokale map `data`. Er worden afbeeldingen en labels gedownload voor trainings- en testdoeleinden:


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>Enkele voorbeeldafbeeldingen weergeven

Laad de gecomprimeerde bestanden in `numpy`-matrices. Gebruik vervolgens `matplotlib` om 30 willekeurige afbeeldingen uit de gegevensset te tekenen, met de bijbehorende labels erboven. Voor deze stap hebt u een `load_data`-functie nodig die is opgenomen in een `util.py`-bestand. Dit bestand staat in de map met voorbeelden. Zorg ervoor dat u het bestand in de map met dit notebook zet. Met de `load_data`-functie parseert u de gecomprimeerde bestanden eenvoudig in numpy-matrices:



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the training set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Er wordt een steekproef van afbeeldingen weergegeven:

![Steekproef van afbeeldingen](./media/tutorial-train-models-with-aml/digits.png)

U hebt nu een beter beeld van hoe deze afbeeldingen eruit zien en wat u voor resultaat kunt verwachten van de voorspelling.

### <a name="upload-data-to-the-cloud"></a>Gegevens uploaden naar de cloud

U kunt de gegevens nu op afstand toegankelijk maken door ze vanaf uw lokale computer te uploaden naar Azure. Hierdoor kunnen de gegevens worden gebruikt voor externe training. Het gegevensarchief is een handige structuur die is gekoppeld aan uw werkruimte voor het uploaden/downloaden van gegevens. Hier is tevens interactie mogelijk met de gegevens vanaf uw externe rekendoelen. Het archief is toegankelijk met een Azure Blob Storage-account.

De MNIST-bestanden worden geüpload naar een map met de naam `mnist` in de hoofdmap van het gegevensarchief:

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
U hebt nu alles wat u nodig hebt om een model te gaan trainen. 

## <a name="train-a-local-model"></a>Een lokaal model trainen

U gaat een eenvoudig logistiek regressiemodel lokaal trainen met scikit-learn.

**Lokaal trainen duurt een minuut of twee**, afhankelijk van de configuratie van uw computer:

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Vervolgens doet u voorspellingen aan de hand van de testset en berekent u de nauwkeurigheid: 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

De nauwkeurigheid van het lokale model wordt weergegeven:

`0.9202`

Met slechts een paar regels code hebt u een nauwkeurigheid van 92 procent.

## <a name="train-on-a-remote-cluster"></a>Trainen op een extern cluster

U kunt dit eenvoudige model uitbreiden door een model met een andere regularisatiefrequentie te bouwen. Dit keer traint u het model op een externe resource.  

Voor deze taak verstuurt u de taak naar het cluster voor externe training dat u eerder hebt ingesteld. Voer de volgende stappen uit om een taak te verzenden:
* Maak een map.
* Maak een trainingsscript.
* Maak een estimator-object.
* Verzend de taak.

### <a name="create-a-directory"></a>Een map maken

Maak een map om de benodigde code vanaf uw computer aan te bieden aan de externe resource:

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Een trainingsscript maken

Als u de taak naar het cluster wilt verzenden, moet u eerst een trainingsscript maken. Voer de volgende code uit om in de map die u hebt gemaakt een trainingsscript te maken met de naam `train.py`. In deze training wordt een regularisatiefrequentie toegevoegd aan het trainingsalgoritme. Hierdoor is het model iets anders dan de lokale versie:

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_context()

print('Train a logistic regression model with regularization rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

U ziet hoe met het script gegevens worden opgehaald en modellen worden opgeslagen:

+ Met het trainingsscript leest u een argument om de map met de gegevens te vinden. Als u de taak later verzendt, wijst u naar het gegevensarchief voor dit argument: `parser.add_argument('--data-folder', type=str, dest='data_folder', help='data directory mounting point')`.

+ Met het trainingsscript slaat u uw model op in een map met de naam **outputs**: <br/>
`joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')`.<br/>
Alles gegevens die naar deze map worden geschreven, worden automatisch geüpload naar uw werkruimte. Verderop in de zelfstudie gaat u dit model openen vanuit deze map.
Vanuit het trainingsscript wordt verwezen naar het bestand `utils.py` om de gegevensset juist te laden. Kopieer dit script naar de scriptmap, zodat het samen met het trainingsscript vanaf de externe resource kan worden geopend.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Een estimator maken

Er wordt een estimator-object gebruikt om de run te verzenden. Maak de estimator door de volgende code uit te voeren en deze items te definiëren:

* De naam van het estimator-object, `est`.
* De map met uw scripts. Alle bestanden in deze map worden naar de clusterknooppunten geüpload voor uitvoering. 
* Het rekendoel. In dit geval gebruikt u het Azure Machine Learning-rekencluster dat u hebt gemaakt.
* De naam van het trainingsscript, **train.py**.
* Vereiste parameters uit het trainingsscript. 
* Python-pakketten die nodig zijn voor de training.

In deze zelfstudie bestaat dit doel uit AmlCompute. Alle bestanden in de scriptmap worden naar de clusterknooppunten geüpload om te worden uitgevoerd. De **data_folder** is ingesteld voor gebruik van het gegevensarchief, `ds.as_mount()`:

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>De taak verzenden naar het cluster

Voer het experiment uit door het estimator-object in te dienen:

```python
run = exp.submit(config=est)
run
```

Aangezien de aanroep asynchroon is, retourneert deze een status **Preparing** of **Running** zodra de taak is gestart.

## <a name="monitor-a-remote-run"></a>Een externe run controleren

In totaal duurt de eerste run **ongeveer tien minuten**. Voor latere runs wordt dezelfde afbeelding echter opnieuw gebruikt indien de scriptafhankelijkheden niet zijn gewijzigd. Hierdoor is de opstarttijd van de container veel korter.

Terwijl u wacht, gebeurt het volgende:

- **Afbeelding maken**: Er wordt een Docker-afbeelding gemaakt die overeenkomt met de Python-omgeving die is opgegeven met de estimator. De afbeelding wordt naar de werkruimte geüpload. Het maken en uploaden van de afbeelding duurt **circa vijf minuten**. 

  Deze fase vindt eenmaal plaats voor elke Python-omgeving, aangezien de container voor volgende runs in de cache wordt opgeslagen. Tijdens het maken van de afbeelding, worden er logboeken gestreamd naar de uitvoeringsgeschiedenis. U kunt de voortgang van het maken van afbeeldingen volgen aan de hand van deze logboeken.

- **Schalen**: Als voor het externe cluster meer knooppunten zijn vereist voor het uitvoeren van de run dan er momenteel beschikbaar zijn, worden er automatisch extra knooppunten toegevoegd. Het schalen duurt meestal **ongeveer vijf minuten.**

- **Uitvoeren**: In deze fase worden de noodzakelijke scripts en bestanden verzonden naar het rekendoel. Vervolgens worden gegevensarchieven gekoppeld of gekopieerd. Ten slotte wordt het **entry_script** uitgevoerd. Terwijl de taak wordt uitgevoerd, worden **stdout** en de map **./logs** naar de uitvoeringsgeschiedenis gestreamd. U kunt de voortgang van de run volgen aan de hand van deze logboeken.

- **Nabewerken**: De map **./outputs** van de run wordt naar de uitvoeringsgeschiedenis in uw werkruimte gekopieerd, zodat u deze resultaten kunt bekijken.


U kunt de voortgang van een actieve taak op verschillende manieren controleren. In deze zelfstudie wordt gebruikgemaakt van een Jupyter-widget en de methode `wait_for_completion`. 

### <a name="jupyter-widget"></a>Jupyter-widget

U kunt de voortgang van de rum volgen met een Jupyter-widget. Net als het indienen van de run, is de widget asynchroon en biedt deze elke 10 tot 15 seconden live updates totdat de taak is voltooid:


```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

Deze momentopname is de widget die aan het einde van de training wordt weergegeven:

![Notebook-widget](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Resultaten van logboeken weergeven bij voltooiing

Het trainen en controleren van het model gebeurt op de achtergrond. Wacht totdat het trainen van het model is voltooid voordat u andere code uitvoert. Gebruik `wait_for_completion` om weer te geven wanneer het trainen van het model is voltooid: 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Resultaten van run weergeven

U hebt nu een model dat is getraind op een extern cluster. De nauwkeurigheid van het model opvragen:

```python
print(run.get_metrics())
```
De uitvoer laat zien dat het externe model een nauwkeurigheid heeft die iets hoger is dan die van het lokale model vanwege het toevoegen van de regularisatiefrequentie tijdens het trainen:  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

In de volgende zelfstudie wordt dit model in meer detail besproken.

## <a name="register-model"></a>Model registreren

Met de laatste stap in het trainingsscript wordt het bestand `outputs/sklearn_mnist_model.pkl` weggeschreven naar een map met de naam `outputs` op de VM van het cluster waarop de taak wordt uitgevoerd. `outputs` is een speciale map omdat alle inhoud in deze map automatisch wordt geüpload naar uw werkruimte. Deze inhoud wordt opgenomen in de uitvoeringsrecord in het experiment, onder uw werkruimte. Hierdoor is het modelbestand nu ook beschikbaar in uw werkruimte.

U kunt zien welke bestanden aan die run zijn gekoppeld:

```python
print(run.get_file_names())
```

Registreer het model in de werkruimte, zodat u of anderen query's op het model kunnen uitvoeren, of het model kunnen onderzoeken en implementeren:

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

U kunt ook gewoon alleen het Azure Machine Learning Compute-cluster verwijderen. Automatische schaalaanpassing is ingeschakeld en het clusterminimum is 0. Hierdoor worden er voor deze specifieke resource geen extra rekenkosten in rekening gebracht wanneer deze niet in gebruik is:


```python
# optionally, delete the Azure Machine Learning Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie over de Azure Machine Learning-service hebt u Python gebruikt voor de volgende taken:

> [!div class="checklist"]
> * De ontwikkelomgeving instellen.
> * De gegevens downloaden en controleren.
> * Een eenvoudig logistiek regressiemodel lokaal trainen met behulp van de populaire bibliotheek voor machine learning scikit-learn.
> * Meerdere modellen trainen op een extern cluster.
> * Trainingsgegevens bekijken en het beste model registreren.

U kunt dit geregistreerde model nu implementeren aan de hand van de instructies in het volgende deel van de reeks zelfstudies:

> [!div class="nextstepaction"]
> [Zelfstudie 2: Modellen implementeren](tutorial-deploy-models-with-aml.md)
