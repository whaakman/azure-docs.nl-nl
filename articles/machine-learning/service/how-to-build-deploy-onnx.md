---
title: U kunt ONNX- en Azure Machine Learning | Maken en implementeren van modellen
description: Meer informatie over ONNX en het gebruik van Azure Machine Learning maken en implementeren van ONNX-modellen
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 09/24/2018
ms.openlocfilehash: 190b7fff24c9d6b3dee86471b56ad68c962e51ce
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116875"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>U kunt ONNX- en Azure Machine Learning: maken en interoperabele AI-modellen implementeren

De [Neural Network-Exchange Open](http://onnx.ai) (ONNX)-indeling is een open standaard voor de vertegenwoordiging van machine learning-modellen. U kunt ONNX wordt ondersteund door een [community van partners](http://onnx.ai/supported-tools), waaronder Microsoft, die compatibel frameworks en programma's maken. Microsoft hecht aan open en interoperabele AI dat data scientists en ontwikkelaars kunnen:

+ Gebruik van het kader van hun keuze te maken en trainen van modellen
+ Modellen voor meerdere platforms met minimale integratie work implementeren

Microsoft biedt ondersteuning voor ONNX voor de producten, waaronder Azure en Windows, zodat u kunt deze doelen kan bereiken.  

## <a name="why-choose-onnx"></a>Waarom kiezen voor ONNX?
De interoperabiliteit waarover u met ONNX beschikt maakt het mogelijk is om op te halen van de briljante ideeën sneller naar productie. Gegevenswetenschappers kunnen ONNX, hun favoriete framework voor de taak kiezen. Ontwikkelaars kunnen op dezelfde manier besteed minder tijd aan modellen gereed voor productie aan en implementeer in de cloud en edge.  

U kunt ONNX-modellen maken van veel frameworks, waaronder PyTorch, Chainer, Microsoft Cognitive Toolkit (CNTK), MXNet, ML.Net, TensorFlow, Keras, SciKit meer en meer.

Er is ook een ecosysteem van hulpprogramma's voor het visualiseren en aan het verkorten van ONNX-modellen. Een aantal vooraf getrainde ONNX-modellen zijn ook beschikbaar voor algemene scenario's.

[U kunt ONNX-modellen kunnen worden geïmplementeerd](#deploy) naar de cloud met behulp van Azure Machine Learning en de ONNX-Runtime. Ze kunnen ook worden geïmplementeerd op Windows 10-apparaten met [Windows ML](https://docs.microsoft.com/windows/ai/). Ze kunnen ook worden geïmplementeerd op andere platformen met behulp van conversieprogramma's die beschikbaar via de ONNX-community zijn. 

[ ![ONNX flow diagram van training, conversieprogramma's en implementatie](media/concept-onnx/onnx.png) ] (. / media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>U kunt ONNX-modellen

U kunt ONNX-modellen op verschillende manieren verkrijgen:
+ Ophalen van een vooraf getrainde ONNX-model van de [ONNX Model Zoo](https://github.com/onnx/models) (Zie het voorbeeld onder aan dit artikel)
+ Genereren van een aangepaste ONNX-model van [Azure Custom Vision service](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) 
+ Bestaande model vanuit een andere indeling converteren naar ONNX (Zie het voorbeeld onder aan dit artikel) 
+ Traint u een nieuwe ONNX-model in Azure Machine Learning-service (Zie het voorbeeld onder aan dit artikel)

## <a name="saveconvert-your-models-to-onnx"></a>Uw ONNX-modellen opslaan/converteren

U kunt bestaande modellen converteren naar ONNX of opslaat als ONNX aan het einde van uw training.

|Framework voor model|Conversievoorbeeld van de of hulpprogramma|
|-----|-------|
|PyTorch|[Jupyter-notebook](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb)|
|Microsoft&nbsp;Cognitive&nbsp;Toolkit&nbsp;(CNTK)|[Jupyter-notebook](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)|
|TensorFlow|[tensorflow-onnx-converter](https://github.com/onnx/tensorflow-onnx)|
|Chainer|[Jupyter-notebook](https://github.com/onnx/tutorials/blob/master/tutorials/ChainerOnnxExport.ipynb)|
|MXNet|[Jupyter-notebook](https://github.com/onnx/tutorials/blob/master/tutorials/MXNetONNXExport.ipynb)|
|Keras, ScitKit meer CoreML<br/>XGBoost en libSVM|[WinMLTools](https://docs.microsoft.com/windows/ai/convert-model-winmltools)|

U vindt de meest recente lijst met ondersteunde frameworks en conversieprogramma's op de [ONNX-zelfstudies site](https://github.com/onnx/tutorials).

<a name="deploy"></a>

## <a name="deploy-onnx-models-in-azure"></a>U kunt ONNX-modellen in Azure implementeren

Met Azure Machine Learning-service, kunt u implementeren, beheren en controleren van uw ONNX-modellen. Met behulp van de standaard [implementatiewerkstroom](concept-model-management-and-deployment.md) en de ONNX-Runtime, kunt u een REST-eindpunt gehost in de cloud maken. Zie een compleet voorbeeld Jupyter-notebook aan het einde van dit artikel om te proberen voor uzelf. 

### <a name="install-and-configure-the-onnx-runtime"></a>Installeren en configureren van de ONNX-Runtime

De ONNX-Runtime is een engine met hoge prestaties Deductie voor ONNX-modellen. Het wordt geleverd met een Python-API en biedt hardwareversnelling in CPU- en GPU. Op dit moment ondersteunt 1.2 ONNX-modellen en op Ubuntu 16.04 Linux wordt uitgevoerd. Beide [CPU](https://pypi.org/project/onnxruntime) en [GPU](https://pypi.org/project/onnxruntime-gpu) pakketten zijn beschikbaar op [PyPi.org](https://pypi.org).

Voor het installeren van de ONNX-Runtime gebruiken:
```python
pip install onnxruntime
```

Voor het aanroepen van de Runtime ONNX in uw Python-script, gebruikt u:
```python
import onnxruntime

session = onnxruntime.InferenceSession("path to model")
```

De documentatie bij het model meestal ziet u de invoer en uitvoer voor het gebruik van het model. U kunt ook een hulpprogramma voor gegevensvisualisatie zoals gebruiken [Netron](https://github.com/lutzroeder/Netron) om het model weer te geven. U kunt ONNX-Runtime kunt u ook query metagegevens van het model, invoer en uitvoer:
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

Naar Deductie uw model, gebruik `run` en door te geven in de lijst van de uitvoer geretourneerd (laat leeg als u wilt dat ze allemaal) en een overzicht van de invoerwaarden. Het resultaat is een lijst van de uitvoer.
```python
results = session.run(["output1", "output2"], {"input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

Zie voor de volledige API-verwijzing de [ONNX-Runtime-referentiedocumenten](https://aka.ms/onnxruntime-python).

### <a name="example-deployment-steps"></a>Voorbeeld van de implementatiestappen

Hier volgt een voorbeeld voor het implementeren van een ONNX-model:

1. Initialiseren van de werkruimte van uw Azure Machine Learning-service. Als u een nog geen hebt, informatie over het maken van een werkruimte in [in deze Quick Start](quickstart-get-started.md).

   ```python
   from azureml.core import Workspace

   ws = Workspace.from_config()
   print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
   ```

2. Registreer het model met Azure Machine Learning.

   ```python
   from azureml.core.model import Model

   model = Model.register(model_path = "model.onnx",
                          model_name = "MyONNXmodel",
                          tags = ["onnx"],
                          description = "test",
                          workspace = ws)
   ```

3. Een installatiekopie maken met het model en eventuele afhankelijkheden.

   ```python
   from azureml.core.image import ContainerImage
   
   image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                     runtime = "python",
                                                     conda_file = "myenv.yml",
                                                     description = "test",
                                                     tags = ["onnx"]
                                                    )

   image = ContainerImage.create(name = "myonnxmodelimage",
                                 # this is the model object
                                 models = [model],
                                 image_config = image_config,
                                 workspace = ws)

   image.wait_for_creation(show_output = True)
   ```

   Het bestand `score.py` bevat de logica scoring en moet worden opgenomen in de installatiekopie. Dit bestand wordt gebruikt om uit te voeren van het model in de afbeelding. Raadpleeg deze [zelfstudie](tutorial-deploy-models-with-aml.md#create-scoring-script) voor instructies over het maken van een scoring-script. Een voorbeeld van bestand voor een ONNX-model wordt hieronder weergegeven:

   ```python
   import onnxruntime
   import json
   import numpy as np
   import sys
   from azureml.core.model import Model

   def init():
       global model_path
       model_path = Model.get_model_path(model_name = 'MyONNXmodel')

   def run(raw_data):
       try:
           data = json.loads(raw_data)['data']
           data = np.array(data)
        
           sess = onnxruntime.InferenceSession(model_path)
           result = sess.run(["outY"], {"inX": data})
        
           return json.dumps({"result": result.tolist()})
       except Exception as e:
           result = str(e)
           return json.dumps({"error": result})
   ```

   Het bestand `myenv.yml` beschrijft de afhankelijkheden die nodig zijn voor de installatiekopie. Raadpleeg deze [zelfstudie](tutorial-deploy-models-with-aml.md#create-environment-file) voor instructies over het maken van een omgevingsbestand, zoals dit voorbeeldbestand:

   ```python
   from azureml.core.conda_dependencies import CondaDependencies 

   myenv = CondaDependencies()
   myenv.add_pip_package("numpy")
   myenv.add_pip_package("azureml-core")
   myenv.add_pip_package("onnxruntime")

   with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
   ```

4. Uw ONNX-model met Azure Machine Learning om te implementeren:
   + Azure Container Instances (ACI): [informatie over hoe...](how-to-deploy-to-aci.md)

   + Azure Kubernetes Service (AKS): [informatie over hoe...](how-to-deploy-to-aks.md)


## <a name="examples"></a>Voorbeelden
 
De volgende notebooks laten zien hoe u kunt ONNX-modellen maken en deze implementeren met Azure Machine Learning: 
+ `/onnx/onnx-modelzoo-aml-deploy-resnet50.ipynb` 
+ `/onnx/onnx-convert-aml-deploy-tinyyolo.ipynb`
+ `/onnx/onnx-train-pytorch-aml-deploy-mnist.ipynb`

De volgende notebooks laten zien hoe u bestaande ONNX-modellen met Azure Machine Learning implementeren: 
+ [u kunt onnx/onnx-Deductie-mnist.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-mnist.ipynb) 
+ [u kunt onnx/onnx-Deductie-emotion-recognition.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-emotion-recognition.ipynb)
 
Deze laptops ophalen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Meer informatie

Meer informatie over ONNX of bij te dragen aan het project:
+ [U kunt ONNX-Projectwebsite](http://onnx.ai)

+ [U kunt ONNX-code op GitHub](https://github.com/onnx/onnx)
