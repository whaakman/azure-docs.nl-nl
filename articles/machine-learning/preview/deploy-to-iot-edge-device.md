---
title: Een Azure Machine Learning-model implementeren op een Azure-IoT-randapparaat | Microsoft Docs
description: Dit document wordt beschreven hoe Azure Machine Learning-modellen kunnen worden toegepast op Azure IoT Edge-apparaten.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/10/2017
ms.openlocfilehash: f83e4b17a3209adeb655723362e6882e38bfaf49
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Een Azure Machine Learning-model implementeren op een Azure-IoT-randapparaat

Alle Azure Machine Learning-modellen als webservices op basis van Docker beperkte kunnen ook uitvoeren op Azure IoT Edge-apparaten. Extra scripts en instructies vindt u in de [AI Toolkit voor Azure IoT rand](http://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Het model operationeel maken
Het model operationeel door de instructies in [Azure Machine Learning-Model Management Web Service-implementatie](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy) een Docker-installatiekopie maken met het model.

## <a name="deploy-to-azure-iot-edge"></a>Implementeren naar Azure IoT-rand
Azure IoT-rand wordt verplaatst van cloud analytics en aangepaste bedrijfsregels naar apparaten. Alle Machine Learning-modellen kunnen uitvoeren op de IoT Edge-apparaten. De documentatie van een IoT-randapparaat instellen en maakt u een implementatie kan worden gevonden op [aka.ms/azure-iot-edge-doc](https://aka.ms/azure-iot-edge-doc).

Hieronder vindt u aanvullende dingen om aan te geven.

### <a name="add-registry-credentials-to-the-edge-runtime-on-your-edge-device"></a>Voeg register referenties toe aan de rand runtime op uw apparaat van rand
Toevoegen op de computer waarop u IoT-rand wordt uitgevoerd, de referenties van het register in, zodat de runtime toegang krijgen kan tot de container pull.

Voor Windows, moet u de volgende opdracht uitvoeren:
```cmd/sh
iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```
Voer de volgende opdracht voor Linux:
```cmd/sh
sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```

### <a name="find-the-machine-learning-container-image-location"></a>Zoeken naar de locatie van Machine Learning container installatiekopie
U moet de locatie van de installatiekopie van uw Machine Learning-container. De afbeeldingslocatie container zoeken:

1. Meld u aan bij de [Azure-portal](http://portal.azure.com/).
2. In de **Azure Container register**, selecteert u het register die u wilt onderzoeken.
3. Klik in het register op **opslagplaatsen** om een lijst weer van alle opslagplaatsen en hun afbeeldingen.













