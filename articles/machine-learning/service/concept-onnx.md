---
title: Hoge prestaties, cross-platform Deductie met ONNX
titleSuffix: Azure Machine Learning service
description: Meer informatie over ONNX en de ONNX-Runtime voor het versnellen van modellen
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 04/24/2019
ms.custom: seodec18
ms.openlocfilehash: f1eca5bdd81a384efe04f769ebd12be9d91fc78a
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849729"
---
# <a name="onnx-and-azure-machine-learning-create-and-accelerate-ml-models"></a>U kunt ONNX en Azure Machine Learning: Maken en ML-modellen te versnellen

Informatie over hoe met de [Open Neural Network-Exchange](https://onnx.ai) (ONNX) de Deductie van uw machine learning-model kunt optimaliseren. Deductie of het model scoren, is de fase waarin het gedistribueerde model wordt gebruikt voor voorspellingen, meestal op productiegegevens. 

Optimaliseren machine learning-modellen voor Deductie (of het model scoren) is moeilijk, omdat u het model en de Deductie-bibliotheek moet om optimaal te benutten de hardwaremogelijkheden af te stemmen. Het probleem wordt zeer moeilijk is als u wilt om de optimale prestaties op verschillende soorten platforms (cloud/edge, CPU/GPU, enzovoort), aangezien elk een heeft verschillende mogelijkheden en -kenmerken. Als u modellen van tal van frameworks die nodig zijn om uit te voeren op diverse platformen, wordt de complexiteit verhoogd. Het is zeer veel tijd in beslag om te optimaliseren van de verschillende combinaties van frameworks en hardware. Er is een oplossing voor het trainen één keer in uw favoriete framework en overal worden uitgevoerd op de cloud of edge nodig. Dit is waar u kunt ONNX is beschikbaar in.

Microsoft en een community van partners die zijn ONNX gemaakt als een open standaard voor de vertegenwoordiging van machine learning-modellen. Modellen vanuit [veel frameworks](https://onnx.ai/supported-tools) waaronder TensorFlow, PyTorch SciKit-informatie, Keras, Chainer, MXNet en MATLAB kan worden geëxporteerd of geconverteerd naar de standard ONNX-indeling. Zodra de modellen in de ONNX-indeling, kunnen ze worden uitgevoerd op verschillende platforms en apparaten.

[U kunt ONNX-Runtime](https://github.com/Microsoft/onnxruntime) is een krachtige Deductie-engine voor het implementeren van ONNX-modellen voor productie. Het geoptimaliseerd voor zowel de cloud en edge en werkt op Linux, Windows en Mac. Die zijn geschreven in C++, heeft ook C, Python, en C# API's. U kunt ONNX Runtime biedt ondersteuning voor alle van de ONNX-ML-specificatie en kan ook worden geïntegreerd met accelerators op andere hardware zoals TensorRT op NVidia GPU's.

De ONNX-Runtime wordt gebruikt op grote schaal Microsoft-services zoals Bing, Office en Cognitive Services. Prestaties zijn afhankelijk van een aantal factoren, maar deze Microsoft-services hebben gezien een __2 x prestatieverbetering te bereiken op CPU gemiddeld__. U kunt ONNX-Runtime wordt ook gebruikt als onderdeel van Windows-ML op honderden miljoenen apparaten. Met Azure Machine Learning-services kunt u de runtime. Met behulp van ONNX-Runtime, kunt u profiteren van de uitgebreide productiekwaliteit optimalisaties, testen en de voortdurende verbeteringen.

[![U kunt ONNX flow diagram van training, conversieprogramma's en -implementatie](media/concept-onnx/onnx.png)](./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>U kunt ONNX-modellen

U kunt ONNX-modellen op verschillende manieren verkrijgen:
+ Traint u een nieuwe ONNX-model in Azure Machine Learning-service (Zie voorbeelden aan de onderkant van dit artikel)
+ Bestaande model vanuit een andere indeling converteren naar ONNX (Zie de [zelfstudies](https://github.com/onnx/tutorials)) 
+ Ophalen van een vooraf getrainde ONNX-model van de [ONNX Model Zoo](https://github.com/onnx/models) (Zie voorbeelden aan de onderkant van dit artikel)
+ Genereren van een aangepaste ONNX-model van [Azure Custom Vision service](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) 

Veel modellen, met inbegrip van de classificatie van afbeeldingen, objectdetectie van het en verwerking van de tekst kunnen worden weergegeven als ONNX-modellen. Evenwel sommige modellen mogelijk niet kunnen worden geconverteerd. Als u deze situatie, vul u een probleem in de GitHub van de respectieve conversieprogramma's die u hebt gebruikt. U kunt met behulp van uw bestaande indeling model totdat het probleem is verholpen.

## <a name="deploy-onnx-models-in-azure"></a>U kunt ONNX-modellen in Azure implementeren

Met Azure Machine Learning-service, kunt u implementeren, beheren en controleren van uw ONNX-modellen. Met behulp van de standaard [implementatiewerkstroom](concept-model-management-and-deployment.md) en u kunt ONNX-Runtime, kunt u een REST-eindpunt gehost in de cloud maken. Zie voorbeeld Jupyter notebooks onder aan het einde van dit artikel om te proberen voor uzelf. 

### <a name="install-and-use-onnx-runtime-with-python"></a>Installeren en u kunt ONNX-Runtime gebruiken met Python

Python-pakketten voor ONNX-Runtime zijn beschikbaar op [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)). Lees [systeemvereisten](https://github.com/Microsoft/onnxruntime#system-requirements) voordat de installatie. 

 U kunt ONNX-Runtime voor Python wilt installeren, gebruikt u een van de volgende opdrachten: 
```python   
pip install onnxruntime       # CPU build
pip install onnxruntime-gpu   # GPU build
``` 

Om aan te roepen ONNX-Runtime in uw Python-script, gebruikt u:    
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

Zie voor de volledige Python API-verwijzing de [ONNX-Runtime-referentiedocumenten](https://aka.ms/onnxruntime-python).    

## <a name="examples"></a>Voorbeelden

Zie [How-to-naar-gebruik-azureml/implementatie/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) bijvoorbeeld laptops die maakt en implementeert kunt ONNX-modellen.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Meer informatie

Meer informatie over ONNX of bij te dragen aan het project:
+ [U kunt ONNX-Projectwebsite](https://onnx.ai)
+ [U kunt ONNX-code op GitHub](https://github.com/onnx/onnx)

Meer informatie over ONNX-Runtime of bij te dragen aan het project:
+ [U kunt ONNX Runtime GitHub-opslagplaats](https://github.com/Microsoft/onnxruntime)


