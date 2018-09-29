---
title: Azure Machine Learning-Model Management Web Service-implementatie | Microsoft Docs
description: Dit document beschrijft de stappen voor het implementeren van een machine learning-model met behulp van Azure Machine Learning-model Management.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/03/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 8753463f90ae97d4b98d557eec5bd737b4853480
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433970"
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>Een Machine Learning-Model implementeren als een webservice

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Azure Machine Learning Modelbeheer biedt interfaces voor het implementeren van modellen als webservices op basis van Docker opgenomen in een container. U kunt u maken met behulp van frameworks zoals Spark, de Microsoft Cognitive Toolkit (CNTK), Keras, Tensorflow en Python modellen kunt implementeren. 

In dit document bevat informatie over de stappen voor het implementeren van uw modellen als webservices met behulp van de Azure Machine Learning Modelbeheer-CLI (opdrachtregelinterface).

## <a name="what-you-need-to-get-started"></a>Wat u nodig hebt om aan de slag

U optimaal gebruikmaken van deze handleiding, hebt u rechten voor bijdragers toegang tot een Azure-abonnement of resourcegroep die u kunt uw modellen te implementeren.
De CLI is vooraf geïnstalleerd op de Azure Machine Learning Workbench en klik op [Azure-Dsvm](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).  Het kan ook worden geïnstalleerd als een zelfstandige pakket.

Bovendien moet een model management-account- en implementatieomgeving al zijn ingesteld.  Zie voor meer informatie over het instellen van uw Modelbeheer-account en de omgeving voor lokale en de implementatie van het cluster [Modelbeheer-configuratie](deployment-setup-configuration.md).

## <a name="deploying-web-services"></a>Implementeren van webservices
Met behulp van de CLI's gebruikt, kunt u webservices om uit te voeren op de lokale computer of op een cluster implementeren.

U wordt aangeraden beginnen met een lokale implementatie. U eerst valideren dat uw model en de code, klikt u vervolgens werkt de webservice implementeren in een cluster voor gebruik van de productie-schaal.

Hier volgen de implementatiestappen:
1. Uw opgeslagen, getraind, Machine Learning-model gebruiken
2. Maak een schema voor de webservice-invoer en uitvoer
3. Een op basis van een Docker-containerinstallatiekopie maken
4. Maken en implementeren van de webservice

### <a name="1-save-your-model"></a>1. Uw model opslaan
Beginnen met het bestand opgeslagen getrainde model, bijvoorbeeld mymodel.pkl. De bestandsextensie is afhankelijk van het platform dat u gebruikt om te trainen en slaat u het model. 

Als u bijvoorbeeld kunt u Python van Pickle-bibliotheek een getraind model opslaan in een bestand. Hier volgt een [voorbeeld](http://scikit-learn.org/stable/modules/model_persistence.html) met behulp van de Iris-gegevensset:

```python
import pickle
from sklearn import datasets
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf = linear_model.LogisticRegression()
clf.fit(X, y)  
saved_model = pickle.dumps(clf)
```

### <a name="2-create-a-schemajson-file"></a>2. Maak een bestand schema.json

Schema genereren is optioneel, moet het is raadzaam om de aanvraag- en invoer variabele indeling voor het verwerken van beter te definiëren.

Maak een schema voor het automatisch valideren van de invoer en uitvoer van de webservice. Het schema de CLI ook gebruiken voor het genereren van een Swagger-document voor uw webservice.

Importeer de volgende bibliotheken voor het maken van het schema:

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
Vervolgens definieert u de invoervariabelen, zoals een Spark dataframe, Pandas dataframe of NumPy matrix. Het volgende voorbeeld wordt een matrix Numpy:

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```
Het volgende voorbeeld wordt een Spark-gegevensframe:

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

Het volgende voorbeeld wordt een PANDAS dataframe:

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

Het volgende voorbeeld wordt een generieke JSON-indeling:

```python
inputs = {"input_json": SampleDefinition(DataTypes.STANDARD, yourinputjson)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. Maak een bestand score.py
U opgeven een bestand score.py, dat het model wordt geladen en retourneert de voorspelling resultaten met behulp van het model.

Het bestand moet bevatten twee functies: init en uit te voeren.

Voeg de volgende code aan de bovenkant van het bestand score.py om in te schakelen van de functionaliteit voor modelgegevensverzameling waarmee invoer- en voorspellingsgegevens modelgegevens verzamelen

```python
from azureml.datacollector import ModelDataCollector
```

Controleer [modelgegevensverzamelings](how-to-use-model-data-collection.md) sectie voor meer informatie over hoe u deze functie wilt gebruiken.

#### <a name="init-function"></a>Init-functie
De init-functie gebruiken om te laden van de opgeslagen model.

Voorbeeld van een eenvoudige init-functie die het laden van het model:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>Functie uitvoeren
De run-functie maakt gebruik van het model en de invoergegevens om terug te keren een voorspelling.

Voorbeeld van een eenvoudige functie verwerking van de invoer en retourneert het resultaat voorspelling uitvoeren:

```python
def run(input_df):
    # clf2 is the same model as clf1, but loaded from the model.pkl file
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. Een model registreren
Volgende opdracht wordt gebruikt voor het registreren van een model dat is gemaakt in stap 1 hierboven

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. Maak het manifest.
Opdracht volgen helpt u een manifest voor het model maken

```
az ml manifest create --manifest-name [your new manifest name] -f [path to score file] -r [runtime for the image, e.g. spark-py]
```
U kunt een eerder geregistreerde model toevoegen aan het manifest met behulp van argument `--model-id` of `-i` in de bovenstaande opdracht. Meerdere modellen kunnen worden opgegeven met extra -i argumenten.

### <a name="6-create-an-image"></a>6. Een installatiekopie maken 
U kunt een installatiekopie maken met de optie van de manifest voordat hebben gemaakt. 

```
az ml image create -n [image name] --manifest-id [the manifest ID]
```

>[!NOTE] 
>U kunt slechts één opdracht ook gebruiken om uit te voeren van de registratie, het manifest en het model maken van het model. Gebruik -h met de service maakt een opdracht voor meer informatie.

Als alternatief, er is een één opdracht voor het registreren van een model, een manifest maken en een installatiekopie maken (maar niet maken en de webservice implementeert, nog) als een stap als volgt.

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime e.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>Type -h aan het einde van de opdracht bijvoorbeeld az ml-afbeelding maken -h voor meer informatie over de opdrachtparameters.


### <a name="7-create-and-deploy-the-web-service"></a>7. Maken en implementeren van de webservice
Implementeer de service met behulp van de volgende opdracht uit:

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>U kunt slechts één opdracht ook gebruiken om uit te voeren van alle voorgaande 4 stappen. Gebruik -h met de service maakt een opdracht voor meer informatie.

Als alternatief is er een enkele opdracht een model te registreren, een manifest te maken, een installatiekopie maken, evenals, maken en implementeren van de webservice als één stap als volgt.

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```


### <a name="8-test-the-service"></a>8. De service testen
Gebruik de volgende opdracht om informatie op over het aanroepen van de service te halen:

```
az ml service usage realtime -i <service id>
```

Roept de service met behulp van de functie uitvoeren vanaf de opdrachtprompt:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

Het volgende voorbeeld wordt een voorbeeld van een Iris-webservice:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw web-service om uit te voeren lokaal hebt getest, kunt u deze kunt implementeren in een cluster voor grootschalige gebruik. Zie voor meer informatie over het instellen van een cluster voor de implementatie van de webservice [Model Management Configuration](deployment-setup-configuration.md). 
