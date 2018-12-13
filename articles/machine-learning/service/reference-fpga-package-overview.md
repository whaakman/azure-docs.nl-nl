---
title: FPGA-pakket voor hardwareversnelling
titleSuffix: Azure Machine Learning service
description: In dit artikel is afgeschaft.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 44c0818d7ddb418e34e6e124ddc0b27460bb24a0
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53262075"
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Azure Machine Learning-hardwareversnelling-pakket

>[!Note]
>**In dit artikel is afgeschaft.** Dit pakket FPGA is afgeschaft. Ondersteuning voor deze functionaliteit is toegevoegd aan de Azure ML-SDK. Ondersteuning voor dit pakket wordt stapsgewijs beëindigd. [Weergeven van de ondersteuningstijdlijn](overview-what-happened-to-workbench.md#timeline). Meer informatie over bijgewerkt [FPGA-ondersteuning](concept-accelerate-with-fpgas.md).

Het pakket hardwareversnelling van Azure Machine Learning is een Python-pip installeerbare-extensie voor Azure Machine Learning waarmee gegevenswetenschappers en AI-ontwikkelaars kunnen snel:

+ Parametriseer afbeeldingen met een quantized versie van ResNet 50

+ Classificaties op basis van deze functies trainen

+ Implementeren van modellen en [programmable gate arrays (FPGA) veld](concept-accelerate-with-fpgas.md) op Azure voor zeer lage latentie inferentietaken

## <a name="prerequisites"></a>Vereisten

>[!Note]
>Als u geen Azure-abonnement hebt, een gratis account maken voordat u begint. Probeer de [gratis of betaalde versie van Azure Machine Learning-service](http://aka.ms/AMLFree) vandaag nog.

1. Een Azure Machine Learning Modelbeheer-account. Zie voor meer informatie over het maken van het account de [installatie van Azure Machine Learning-Quickstart en Workbench](../desktop-workbench/quickstart-installation.md) document. 

1. Het pakket moet worden geïnstalleerd. 

 
## <a name="how-to-install-the-package"></a>Het pakket installeren

1. Download en installeer de nieuwste versie van [Git](https://git-scm.com/downloads).

2. Installeer [Anaconda (Python 3.6)](https://conda.io/miniconda.html)

   Voor het downloaden van een vooraf geconfigureerde Anaconda-omgeving, gebruikt u de volgende opdracht uit vanaf de Git-opdrachtprompt:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
1. Voor het maken van de omgeving, opent u een **Anaconda vragen** en gebruik de volgende opdracht:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

1. Gebruik de volgende opdracht voor het activeren van de omgeving:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Voorbeeldcode

Deze voorbeeldcode leidt u stapsgewijs de SDK door een model implementeren op een FPGA.

1. Het pakket importeren:
   ```python
   import amlrealtimeai
   from amlrealtimeai import resnet50
   ```

1. De installatiekopie van het vooraf verwerken:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Parametriseer afbeeldingen:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Een classificatie maken:
   ```python
   model.import_graph_def(include_featurizer=False)
   print(model.classifier_input)
   print(model.classifier_output)
   ```

1. De servicedefinitie van de maken:
   ```python
   from amlrealtimeai.pipeline import ServiceDefinition, TensorflowStage, BrainWaveStage
   save_path = os.path.expanduser('~/models/save')
   service_def_path = os.path.join(save_path, 'service_def.zip')

   service_def = ServiceDefinition()
   service_def.pipeline.append(TensorflowStage(tf.Session(), in_images, image_tensors))
   service_def.pipeline.append(BrainWaveStage(model))
   service_def.pipeline.append(TensorflowStage(tf.Session(), model.classifier_input, model.classifier_output))
   service_def.save(service_def_path)
   print(service_def_path)
   ```
 
1. Het voorbereiden van het model om uit te voeren op een FPGA:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Het model om uit te voeren op een FPGA implementeren:
   ```python
   service = deployment_client.get_service_by_name(service_name)
   model_id = deployment_client.register_model(model_name, service_def_path)

   if(service is None):
      service = deployment_client.create_service(service_name, model_id)    
   else:
      service = deployment_client.update_service(service.id, model_id)
   ```

1. De client maken:
    ```python
   from amlrealtimeai import PredictionClient
   client = PredictionClient(service.ipAddress, service.port)  
   ```

1. De API aanroepen:
   ```python
   image_file = R'C:\path_to_file\image.jpg'
   results = client.score_image(image_file)
   ```

## <a name="reporting-issues"></a>Problemen melden

Gebruik de [forum](https://aka.ms/aml-forum-service) voor het rapporteren van problemen die u ondervindt met het pakket.

## <a name="next-steps"></a>Volgende stappen

[Een model implementeren als een webservice op een FPGA](how-to-deploy-fpga-web-service.md)
