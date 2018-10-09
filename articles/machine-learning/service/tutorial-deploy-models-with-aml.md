---
title: 'Zelfstudie: Een afbeeldingsclassificatiemodel implementeren in Azure Container Instance (ACI) met de Azure Machine Learning-service'
description: In deze zelfstudie ontdekt u hoe u de Azure Machine Learning-service kunt gebruiken om een afbeeldingsclassificatiemodel met scikit-learn in een Python Jupyter-notebook te implementeren.  Deze zelfstudie is deel 2 van een tweedelige reeks.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: eda4a1dabd20eee73cfbfa4dc0dd2b0eaa56788e
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031234"
---
# <a name="tutorial-2--deploy-an-image-classification-model-in-azure-container-instance-aci"></a>Zelfstudie 2: Een afbeeldingsclassificatiemodel implementeren in Azure Container Instance (ACI)

Deze zelfstudie is **deel twee van een tweedelige reeks**. In de [vorige zelfstudie](tutorial-train-models-with-aml.md) hebt u Machine Learning-modellen getraind en vervolgens een model geregistreerd in uw werkruimte in de cloud.  

Nu kunt u het model als webservice implementeren in [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/) (ACI). Een webservice is een afbeelding, in dit geval een Docker-installatiekopie, die bestaat uit de scorelogica en het model zelf. 

In dit gedeelte van de zelfstudie wordt uitgelegd hoe u de Azure Machine Learning-service (preview) gebruikt om de volgende bewerkingen uit te voeren:

> [!div class="checklist"]
> * Uw testomgeving instellen
> * Het model ophalen uit uw werkruimte
> * Het model lokaal testen
> * Het model implementeren in ACI
> * Het geïmplementeerde model testen

ACI is niet ideaal voor productie-implementaties, maar het is uiterst geschikt om de werkstroom te testen en om inzicht in de werkstroom te krijgen. Voor schaalbare productie-implementaties is het misschien beter om [Azure Kubernetes Service](how-to-deploy-to-aks.md) te gebruiken.

## <a name="get-the-notebook"></a>De notebook ophalen

Voor uw gemak is deze zelfstudie beschikbaar gemaakt als Jupyter-notebook. Gebruik een van deze methoden om de notebook `tutorials/02.deploy-models.ipynb` uit te voeren:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="prerequisites"></a>Vereisten

Voltooi de training van het model in de notebook in [Zelfstudie 1: Een afbeeldingsclassificatiemodel trainen met Azure Machine Learning](tutorial-train-models-with-aml.md).  


## <a name="set-up-the-environment"></a>De omgeving instellen

Begin met het instellen van een testomgeving.

### <a name="import-packages"></a>Pakketten importeren

Importeer de Python-pakketten die nodig zijn voor deze zelfstudie.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
 
import azureml
from azureml.core import Workspace, Run

# display the core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="retrieve-the-model"></a>Het model ophalen

Bij de vorige zelfstudie hebt u een model geregistreerd in uw werkruimte. Nu gaat u deze werkruimte laden en het model downloaden naar uw lokale map.


```python
from azureml.core import Workspace
from azureml.core.model import Model

ws = Workspace.from_config()
model=Model(ws, 'sklearn_mnist')
model.download(target_dir = '.')
import os 
# verify the downloaded model file
os.stat('./sklearn_mnist_model.pkl')
```

## <a name="test-model-locally"></a>Het model lokaal testen

Voordat u het model implementeert, moet u controleren of het lokaal werkt door:
* De testgegevens te laden
* De testgegevens te voorspellen
* De verwarringsmatrix te bestuderen

### <a name="load-test-data"></a>Testgegevens laden

Laad de testgegevens uit de map **. /data/**, die u hebt gemaakt tijdens de zelfstudie over het trainen van modellen.

```python
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the neural network converge faster

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

```

### <a name="predict-test-data"></a>Testgegevens voorspellen

Voer de testgegevensset in het model in om voorspellingen te krijgen.

```python
import pickle
from sklearn.externals import joblib

clf = joblib.load('./sklearn_mnist_model.pkl')
y_hat = clf.predict(X_test)
```

###  <a name="examine-the-confusion-matrix"></a>De verwarringsmatrix bestuderen

Genereer een verwarringsmatrix om te zien hoeveel voorbeelden uit de testset juist zijn geclassificeerd. Let op de verkeerd ingedeelde waarden voor de onjuiste voorspellingen. 

```python
from sklearn.metrics import confusion_matrix

conf_mx = confusion_matrix(y_test, y_hat)
print(conf_mx)
print('Overall accuracy:', np.average(y_hat == y_test))
```

In de uitvoer wordt de verwarringsmatrix weergegeven:

    [[ 960    0    1    2    1    5    6    3    1    1]
     [   0 1112    3    1    0    1    5    1   12    0]
     [   9    8  920   20   10    4   10   11   37    3]
     [   4    0   17  921    2   21    4   12   20    9]
     [   1    2    5    3  915    0   10    2    6   38]
     [  10    2    0   41   10  770   17    7   28    7]
     [   9    3    7    2    6   20  907    1    3    0]
     [   2    7   22    5    8    1    1  950    5   27]
     [  10   15    5   21   15   27    7   11  851   12]
     [   7    8    2   13   32   13    0   24   12  898]]
    Overall accuracy: 0.9204
   

Gebruik `matplotlib` om de verwarringsmatrix weer te geven in een grafiek. In deze grafiek staat de X-as voor de daadwerkelijke waarden en de Y-as voor de voorspelde waarden. De kleur van elke cel staat voor de foutfrequentie. Hoe lichter de kleur, hoe hoger de foutfrequentie. De 5 is vaak verkeerd ingedeeld als een 3. Daarom ziet u een lichtgekleurde cel bij (5,3).

```python
# normalize the diagnal cells so that they don't overpower the rest of the cells when visualized
row_sums = conf_mx.sum(axis=1, keepdims=True)
norm_conf_mx = conf_mx / row_sums
np.fill_diagonal(norm_conf_mx, 0)

fig = plt.figure(figsize=(8,5))
ax = fig.add_subplot(111)
cax = ax.matshow(norm_conf_mx, cmap=plt.cm.bone)
ticks = np.arange(0, 10, 1)
ax.set_xticks(ticks)
ax.set_yticks(ticks)
ax.set_xticklabels(ticks)
ax.set_yticklabels(ticks)
fig.colorbar(cax)
plt.ylabel('true labels', fontsize=14)
plt.xlabel('predicted values', fontsize=14)
plt.savefig('conf.png')
plt.show()
```

![verwarringsmatrix](./media/tutorial-deploy-models-with-aml/confusion.png)

## <a name="deploy-as-web-service"></a>Als webservice implementeren

Zodra u het model hebt getest en tevreden bent met de resultaten, kunt u het model implementeren als webservice die wordt gehost in ACI. 

Geef de volgende zaken op om de juiste omgeving te maken voor ACI:
* Een scoring-script dat aangeeft hoe het model moet worden gebruikt
* Een omgevingsbestand dat aangeeft welke pakketten moeten worden geïnstalleerd
* Een configuratiebestand om de ACI mee te bouwen
* Het model dat u eerder hebt getraind

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Scoring-script maken

Maak een scoring-script, met de naam score.py, dat door de aanroep van de webservice wordt gebruikt om aan te geven hoe het model moet worden gebruikt.

Er zijn twee functies die u verplicht in het scoring-script moet opnemen:
* De functie `init()`, die het model doorgaans in een algemeen object laadt. Deze functie wordt slechts één keer uitgevoerd wanneer de Docker-container wordt gestart. 

* De functie `run(input_data)` gebruikt het model om een waarde te voorspellen op basis van de invoergegevens. De in- en uitvoer van de uitvoerbewerking maken doorgaans gebruik van JSON voor serialisatie en deserialisatie, maar andere indelingen worden ook ondersteund.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

def init():
    global model
    # retreive the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

<a name="make-myenv"></a>

### <a name="create-environment-file"></a>Omgevingsbestand maken

Maak vervolgens een omgevingsbestand, met de naam myenv.yml, waarin alle pakketafhankelijkheden van het script zijn opgenomen. Dit bestand wordt gebruikt om ervoor te zorgen dat al die afhankelijkheden in de Docker-installatiekopie worden geïnstalleerd. Voor dit model zijn `scikit-learn` en `azureml-sdk` nodig.

```python
from azureml.core.conda_dependencies import CondaDependencies 

myenv = CondaDependencies()
myenv.add_conda_package("scikit-learn")

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
```
Controleer de inhoud van het bestand `myenv.yml`.

```python
with open("myenv.yml","r") as f:
    print(f.read())
```

### <a name="create-configuration-file"></a>Een configuratiebestand maken

Maak een configuratiebestand voor de implementatie en geef het aantal CPU's en het aantal gigabytes aan RAM-geheugen op dat nodig is voor uw ACI-container. Het verschilt per model, maar voor de meeste modellen is de standaardhoeveelheid van 1 kerngeheugen en 1 gigabyte aan RAM-geheugen wel voldoende. Als u er later meer nodig hebt, moet u de installatiekopie opnieuw maken en de service opnieuw implementeren.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "MNIST",  "method" : "sklearn"}, 
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-aci"></a>Implementeren in ACI
Geschatte duur: **ongeveer 7-8 minuten**

Configureer en implementeer de installatiekopie. De volgende code doorloopt de volgende stappen:

1. Bouw een installatiekopie met behulp van:
   * Het scoring-bestand (`score.py`)
   * Het omgevingsbestand (`myenv.yml`)
   * Het modelbestand
1. Registreer die installatiekopie in de werkruimte. 
1. Verzend de installatiekopie naar de ACI-container.
1. Start een container op in ACI met behulp van de installatiekopie.
1. Haal het HTTP-eindpunt van de webservice op.


```python
%%time
from azureml.core.webservice import Webservice
from azureml.core.image import ContainerImage

# configure the image
image_config = ContainerImage.image_configuration(execution_script="score.py", 
                                                  runtime="python", 
                                                  conda_file="myenv.yml")

service = Webservice.deploy_from_model(workspace=ws,
                                       name='sklearn-mnist-svc',
                                       deployment_config=aciconfig,
                                       models=[model],
                                       image_config=image_config)

service.wait_for_deployment(show_output=True)
```

Haal het HTTP-eindpunt van de scoring-webservice op, die REST-clientaanroepen accepteert. Dit eindpunt kan worden gedeeld met iedereen die de webservice wil testen of wil integreren in een toepassing. 

```python
print(service.scoring_uri)
```


## <a name="test-deployed-service"></a>Geïmplementeerde service testen

Eerder hebt u alle testgegevens gescoord met de lokale versie van het model. Nu kunt u het geïmplementeerde model testen met een steekproef van 30 afbeeldingen uit de testgegevens.  

De volgende code doorloopt de volgende stappen:
1. Verzend de gegevens als JSON-matrix naar de webservice die wordt gehost in ACI. 

1. Gebruik de API `run` van de SDK om de service aan te roepen. U kunt ook onbewerkte aanroepen doen met behulp van een HTTP-hulpprogramma zoals curl.

1. Druk de geretourneerde voorspellingen af en geef die weer samen met de invoerafbeeldingen. De verkeerd geclassificeerde voorbeelden worden aangegeven met een rood lettertype en inversie (wit op zwart). 

 Aangezien de nauwkeurigheid van het model hoog is, kan het nodig zijn om de volgende code een paar keer uit te voeren voordat u een voorbeeld van een onjuiste classificatie ziet.

```python
import json

# find 30 random samples from test set
n = 30
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
result = json.loads(service.run(input_data=test_samples))

# compare actual value vs. the predicted values:
i = 0
plt.figure(figsize = (20, 1))

for s in sample_indices:
    plt.subplot(1, n, i + 1)
    plt.axhline('')
    plt.axvline('')
    
    # use different color for misclassified sample
    font_color = 'red' if y_test[s] != result[i] else 'black'
    clr_map = plt.cm.gray if y_test[s] != result[i] else plt.cm.Greys
    
    plt.text(x=10, y =-10, s=result[i], fontsize=18, color=font_color)
    plt.imshow(X_test[s].reshape(28, 28), cmap=clr_map)
    
    i = i + 1
plt.show()
```

Dit is het resultaat van één steekproef van testafbeeldingen: ![resultaten](./media/tutorial-deploy-models-with-aml/results.png)

U kunt ook HTTP-aanvragen op basis van onbewerkte gegevens verzenden om de webservice te testen.

```python
import requests
import json

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

headers = {'Content-Type':'application/json'}

# for AKS deployment you'd need to the service key in the header as well
# api_key = service.get_key()
# headers = {'Content-Type':'application/json',  'Authorization':('Bearer '+ api_key)} 

resp = requests.post(service.scoring_uri, input_data, headers=headers)

print("POST to url", service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resourcegroep en werkruimte wilt bewaren voor andere zelfstudies en voor verkenning, kunt u deze API-aanroep gebruiken om alleen de ACI-implementatie te verwijderen:

```python
service.delete()
```

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie over Azure Machine Learning hebt u Python gebruikt om de volgende bewerkingen te doen:

> [!div class="checklist"]
> * Uw testomgeving instellen
> * Het model ophalen uit uw werkruimte
> * Het model lokaal testen
> * Het model implementeren in ACI
> * Het geïmplementeerde model testen
 
U kunt ook de zelfstudie [Automatische selectie van algoritmen]() uitproberen om te zien hoe Azure Machine Learning automatisch het beste algoritme voor uw model selecteert en het daarop afstemt, en hoe het dat model voor u bouwt.