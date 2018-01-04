---
title: Azure Machine Learning-Model Management Web Service-implementatie | Microsoft Docs
description: Dit document beschrijft de stappen voor het implementeren van een machine learning-model met Azure Machine Learning-model Management.
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: 965e33f3c7d050dca8f6c4e92d75cb7c7a8fa60d
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>Implementatie van een Machine Learning-Model als een webservice

Azure Machine Learning-Model Management biedt interfaces voor het implementeren van modellen zoals beperkte Docker-webservices. U kunt implementeren modellen die u maakt met behulp van frameworks zoals Spark, de Microsoft cognitieve Toolkit (CNTK), Keras, Tensorflow en Python. 

Dit document bevat informatie over de stappen voor het implementeren van uw modellen als webservices met behulp van de Azure Machine Learning-Model Management-opdrachtregelinterface (CLI).

## <a name="deploying-web-services"></a>Webservices implementeren
De CLIs kunt u web-services worden uitgevoerd op de lokale computer of op een cluster implementeren.

U kunt het beste beginnen met een lokale implementatie. U eerst controleren of uw model en code, klikt u vervolgens werkt de webservice implementeren naar een cluster voor gebruik in productie schaal. Zie voor meer informatie over het instellen van uw omgeving voor Clusterimplementatie [Model Management configuration](deployment-setup-configuration.md). 

Hier volgen de implementatiestappen:
1. Uw opgeslagen, getraind, Machine Learning-model gebruiken
2. Maak een schema voor uw webservice invoer- en uitvoergegevens
3. Een installatiekopie op basis van Docker-container maken
4. Maken en implementeren van de webservice

### <a name="1-save-your-model"></a>1. Sla uw model
Beginnen met uw opgeslagen getraind model-bestand bijvoorbeeld mymodel.pkl. De bestandsextensie hangt af van het platform dat u gebruikt om te trainen en sla het model. 

Als u bijvoorbeeld kunt u de Python Pickle-bibliotheek een getraind model opslaan in een bestand. Hier volgt een [voorbeeld](http://scikit-learn.org/stable/modules/model_persistence.html) met behulp van de gegevensset Iris:

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
Deze stap is optioneel. 

Maak een schema voor het valideren van automatisch de invoer en uitvoer van uw web-service. Het schema de CLIs ook gebruiken voor het genereren van een Swagger-document voor uw webservice.

Importeer de volgende bibliotheken voor het maken van het schema:

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
Definieer vervolgens de invoervariabelen zoals een Spark-dataframe, Pandas dataframe of NumPy matrix. Het volgende voorbeeld wordt een matrix Numpy:

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```
Het volgende voorbeeld wordt een Spark-dataframe:

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

Het volgende voorbeeld wordt een dataframe PANDAS:

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. Maak een bestand score.py
U opgeven een bestand score.py, dat het model geladen en retourneert de resultaten van de voorspelling met het model.

Het bestand moet bevatten twee functies: init en uitvoeren.

Plaats de volgende code aan de bovenkant van het bestand score.py gegevens verzameling functionaliteit waarmee model invoer- en voorspelling gegevens verzamelen in te schakelen

```python
from azureml.datacollector import ModelDataCollector
```

Controleer [model gegevensverzameling](how-to-use-model-data-collection.md) sectie voor meer informatie over het gebruik van deze functie.

#### <a name="init-function"></a>Init-functie
De init-functie gebruiken om het opgeslagen model niet laden.

Voorbeeld van een eenvoudige init-functie laden van het model:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>Functie uitvoeren
Het uitvoeren van de functie maakt gebruik van het model en de invoergegevens om te retourneren van een voorspelling.

Voorbeeld van een eenvoudige functie voor de verwerking van de invoer en retourneert het resultaat van de voorspelling uitvoert:

```python
def run(input_df):
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. Registreren van een model
Na de opdracht wordt gebruikt voor het registreren van een model dat is gemaakt in stap 1 hierboven

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. Maak het manifest.
Opdracht volgen helpt u een manifest voor het model maken

```
az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]
```
U kunt een eerder geregistreerde model toevoegen aan het manifest met argument `--model-id` of `-i` in de bovenstaande opdracht. Meerdere modellen kunnen worden opgegeven met aanvullende -i argumenten.

### <a name="6-create-an-image"></a>6. Een installatiekopie maken 
U kunt een installatiekopie maken met de optie van de manifest voordat hebben gemaakt. 

```
az ml image create -n [image name] -manifest-id [the manifest ID]
```

Of u kunt maken van het manifest en de installatiekopie met één opdracht. 

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>Voor meer informatie over de parameters van de maken type -h aan het einde van de opdracht bijvoorbeeld az ml afbeelding -h.


### <a name="7-create-and-deploy-the-web-service"></a>7. Maken en implementeren van de webservice
Implementeer de service met de volgende opdracht:

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>U kunt ook één opdracht gebruiken om uit te voeren van de vorige stappen 4. Gebruik -h met de service maken voor meer informatie.

### <a name="8-test-the-service"></a>8. De service testen
Gebruik de volgende opdracht om meer informatie over de service aanroepen:

```
az ml service usage realtime -i <service id>
```

Aanroepen van de service met de functie uitvoeren vanaf de opdrachtprompt:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

Het volgende voorbeeld wordt een voorbeeld van een Iris-webservice:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw webservice om uit te voeren, lokaal hebt getest, kunt u deze kunt implementeren naar een cluster voor grootschalige gebruik. Zie voor meer informatie over het instellen van een cluster voor web service-implementatie [Model Management Configuration](deployment-setup-configuration.md). 
