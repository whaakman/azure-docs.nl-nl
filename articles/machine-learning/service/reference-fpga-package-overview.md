---
title: FPGA-pakket voor hardwareversnelling voor Azure Machine Learning
description: Meer informatie over de python-pakketten beschikbaar zijn voor Azure Machine Learning-gebruikers.
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: routlaw
author: rloutlaw
ms.date: 05/07/2018
ms.openlocfilehash: e680ef34be1d5dae2942c432de5e81fe620bbdc4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832975"
---
# <a name="azure-machine-learning-hardware-acceleration-package"></a>Azure Machine Learning-hardwareversnelling-pakket

Het pakket hardwareversnelling voor Azure Machine Learning is een pip installeerbare Python-extensie voor Azure Machine Learning waarmee gegevenswetenschappers en ontwikkelaars een AI snel:

+ Featurize afbeeldingen met een quantized versie ResNet 50

+ Op basis van deze functies classificaties trainen

+ Implementeren van modellen [programmeerbare gate matrices (FPGA) veld](concept-accelerate-with-fpgas.md) op Azure voor inferencing zeer lage latentie

## <a name="prerequisites"></a>Vereisten

1. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

1. U moet een Azure Machine Learning-Model Management-account maken. Zie voor meer informatie over het maken van het account de [Quick Start Azure Machine Learning en de Workbench](../service/quickstart-installation.md) document. 

1. Het pakket moet worden geïnstalleerd. 

 
## <a name="how-to-install-the-package"></a>Het installeren van het pakket

1. Download en installeer de nieuwste versie van [Git](https://git-scm.com/downloads).

2. Installeer [Anaconda (Python 3.6)](https://conda.io/miniconda.html)

3. Gebruik de volgende opdracht vanaf de Git-prompt voor het downloaden van een vooraf geconfigureerde Anaconda-omgeving:

    ```
    git clone https://aka.ms/aml-real-time-ai
    ```
5. Voor het maken van de omgeving, opent u een **Anaconda vragen** en gebruik de volgende opdracht:

    ```
    conda env create -f aml-real-time-ai/environment.yml
    ```

6. Gebruik de volgende opdracht voor het activeren van de omgeving:

    ```
    conda activate amlrealtimeai
    ```

## <a name="sample-code"></a>Voorbeeldcode

Deze voorbeeldcode begeleidt u bij het implementeren van een model voor een FPGA met behulp van de SDK.

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

1. Featurize de afbeeldingen:
   ```python 
   from amlrealtimeai.resnet50.model import LocalQuantizedResNet50
   model_path = os.path.expanduser('~/models')
   model = LocalQuantizedResNet50(model_path)
   print(model.version)
   ```

1. Maak een classificatie:
   ```python
   model.import_graph_def(include_featurizer=False)
   print(model.classifier_input)
   print(model.classifier_output)
   ```

1. De servicedefinitie maken:
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
 
1. Het model om te worden uitgevoerd op een FPGA voorbereiden:
   ```python
   from amlrealtimeai import DeploymentClient

   subscription_id = "<Your Azure Subscription ID>"
   resource_group = "<Your Azure Resource Group Name>"
   model_management_account = "<Your AzureML Model Management Account Name>"

   model_name = "resnet50-model"
   service_name = "quickstart-service"

   deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account)
   ```

1. Het model uit te voeren op een FPGA implementeren:
   ```python
   service = deployment_client.get_service_by_name(service_name)
   model_id = deployment_client.register_model(model_name, service_def_path)

   if(service is None):
      service = deployment_client.create_service(service_name, model_id)    
   else:
      service = deployment_client.update_service(service.id, model_id)
   ```

1. Maken van de client:
    ```python
   from amlrealtimeai import PredictionClient
   client = PredictionClient(service.ipAddress, service.port)  
   ```

1. De API niet aanroepen:
   ```python
   image_file = R'C:\path_to_file\image.jpg'
   results = client.score_image(image_file)
   ```

## <a name="reporting-issues"></a>Melden van problemen

Gebruik de [forum](https://aka.ms/aml-forum) voor het rapporteren van problemen die u tegenkomt met het pakket.

## <a name="next-steps"></a>Volgende stappen

[Een model als een webservice op een FPGA implementeren](how-to-deploy-fpga-web-service.md)