---
title: Modellen implementeren op laptop-Vm's
titleSuffix: Azure Machine Learning service
description: Meer informatie over het implementeren van uw Azure Machine Learning-service modellen als een webservice met behulp van een notebook-Vm's.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 08/08/2019
ms.openlocfilehash: d6b26dfe1eb8ea65dd7c751a148c599123b0f6db
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947758"
---
# <a name="deploy-a-model-to-notebook-vms"></a>Een model implementeren op laptop-Vm's

Informatie over het gebruik van de Azure Machine Learning-service voor het implementeren van een model als een webservice op uw notebook-VM. Gebruik laptop-Vm's als aan een van de volgende voor waarden wordt voldaan:

- U moet sneller te implementeren en valideren van uw model.
- U test een model dat is in ontwikkeling.

> [!TIP]
> Het implementeren van een model van een Jupyter Notebook op een notebook-VM is een _lokale implementatie_op een webservice op dezelfde VM. In dit geval is de ' lokale ' computer de VM van de notebook. Zie [modellen implementeren met Azure machine learning service](how-to-deploy-and-where.md)voor meer informatie over implementaties.

## <a name="prerequisites"></a>Vereisten

- Een Azure Machine Learning Services-werk ruimte met een notebook-VM die wordt uitgevoerd. Zie [Setup Environment and Workspace](tutorial-1st-experiment-sdk-setup.md)(Engelstalig) voor meer informatie.

## <a name="deploy-to-the-notebook-vms"></a>Implementeren op de laptop-Vm's

Een voor beeld van een notebook dat lokale implementaties bevat, is opgenomen op de VM van uw notebook. Gebruik de volgende stappen om het notitie blok te laden en het model te implementeren als een webservice op de VM:

1. Selecteer uw Azure Machine Learning-laptop-Vm's in het [Azure Portal](https://portal.azure.com).

1. Open de `samples-*` submap en open `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`. Als het notitie blok eenmaal is geopend, voert u het uit.

    ![Scherm afbeelding van de actieve lokale service op notebook](media/how-to-deploy-local-container-notebookvm/deploy-local-service.png)

1. In het notitie blok worden de URL en de poort weer gegeven waarop de service wordt uitgevoerd. Bijvoorbeeld `https://localhost:6789`. U kunt ook de cel met `print('Local service port: {}'.format(local_service.port))` om de poort weer te geven.

    ![Scherm afbeelding van de actieve lokale service poort](media/how-to-deploy-local-container-notebookvm/deploy-local-service-port.png)

1. Als u de service wilt testen vanaf de VM van het `https://localhost:<local_service.port>` notebook, gebruikt u de URL. Als u wilt testen vanaf een externe client, haalt u de open bare URL op van de service die wordt uitgevoerd op de notebook-VM. de open bare URL kan worden bepaald met behulp van de volgende formule: `https://<notebookvm_name>-<local_service_port>.<azure_region_of_notebook>.notebooks.azureml.net/score`. Bijvoorbeeld `https://mynotebookvm-6789.eastus2.notebooks.azureml.net/score`.

## <a name="test-the-service"></a>Test de service

Gebruik de volgende code om voorbeeld gegevens naar de actieve service te verzenden. Vervang de waarde van `service_url` door de URL van de vorige stap:

```python
import requests
import json
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')
access_token = "your bearer token"
headers = {'Content-Type':'application/json', 'Authorization': 'Bearer ' + access_token}
service_url = "https://mynotebookvm-6789.eastus2.notebooks.azureml.net/score"
resp = requests.post(service_url, test_sample, headers=headers)
print("prediction:", resp.text)
```

## <a name="next-steps"></a>Volgende stappen

* [Een model implementeren met behulp van een aangepaste docker-installatie kopie](how-to-deploy-custom-docker-image.md)
* [Problemen met implementatie oplossen](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning-webservices met SSL beveiligde](how-to-secure-web-service.md)
* [Een ML-Model dat is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
* [Uw Azure Machine Learning modellen bewaken met Application Insights](how-to-enable-app-insights.md)
* [Gegevens verzamelen voor modellen in productie](how-to-enable-data-collection.md)