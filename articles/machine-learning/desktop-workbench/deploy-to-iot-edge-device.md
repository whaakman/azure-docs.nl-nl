---
title: Een Azure Machine Learning-model implementeert naar een Azure IoT Edge-apparaat | Microsoft Docs
description: Dit document wordt beschreven hoe Azure Machine Learning-modellen kunnen worden geïmplementeerd op Azure IoT Edge-apparaten.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: 792ac3f26bdea6c6ccb084d893925d60e6333edb
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852449"
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Een Azure Machine Learning-model implementeert naar een Azure IoT Edge-apparaat

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Kunnen App in een Azure Machine Learning-modellen worden container als Docker op basis van webservices. Azure IoT Edge kunt u containers naar apparaten op afstand kunt implementeren. Deze services samen gebruiken om uw modellen aan de rand uitvoeren voor snellere reactietijden en minder gegevens worden overgebracht. 

Extra scripts en instructies vindt u de [AI-werkset voor Azure IoT Edge](https://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Het model operationeel maken

Azure IoT Edge-modules zijn gebaseerd op containerinstallatiekopieën. Als u wilt uw Machine Learning-model implementeert naar een IoT Edge-apparaat, moet u een Docker-installatiekopie maken.

Uw model operationeel maken door de instructies in [Azure Machine Learning Web Service-implementatie Model Management](model-management-service-deploy.md) een Docker-installatiekopie maken met uw model.

## <a name="deploy-to-azure-iot-edge"></a>Implementeren naar Azure IoT Edge

Zodra u de installatiekopie van het model hebt, kunt u deze implementeren naar een Azure IoT Edge-apparaat. Alle Machine Learning-modellen kunnen uitvoeren op IoT Edge-apparaten. 

### <a name="set-up-an-iot-edge-device"></a>Een IoT Edge-apparaat instellen

Gebruik de Azure IoT Edge-documentatie om voor te bereiden van een apparaat. 

1. [Een apparaat te registreren bij Azure IoT Hub](../../iot-edge/how-to-register-device-portal.md). De uitvoer van deze processen is een verbindingsreeks die u gebruiken kunt om uw fysieke apparaat te configureren. 
2. IoT Edge-runtime installeren op uw fysieke apparaat en deze configureren met een verbindingsreeks. U kunt de runtime installeren op [Windows](../../iot-edge/how-to-install-iot-edge-windows-with-windows.md) of [Linux](../../iot-edge/how-to-install-iot-edge-linux.md) apparaten.  


### <a name="find-the-machine-learning-container-image-location"></a>De locatie van de Machine Learning-container-installatiekopie vinden
U moet de locatie van uw Machine Learning-container-installatiekopie. Zoek de container-installatiekopie-locatie:

1. Meld u aan bij de [Azure-portal](http://portal.azure.com/).
2. In de **Azure Container Registry**, selecteert u het register dat u wilt controleren.
3. Klik in het register op **opslagplaatsen** voor een overzicht van alle opslagplaatsen en afbeeldingen.

Als u geïnteresseerd bent in het containerregister in Azure portal, moet u container registerreferenties ophalen. Deze referenties moeten krijgen tot de IoT Edge-apparaat, zodat deze de installatiekopie uit het persoonlijke register ophalen kunt. 

1. Klik in het containerregister op **toegangssleutels**. 
2. **Schakel** de gebruiker met beheerdersrechten, als deze niet gebeurd is. 
3. Sla de waarden voor **aanmeldingsserver**, **gebruikersnaam**, en **wachtwoord**. 

### <a name="deploy-the-container-image-to-your-device"></a>De containerinstallatiekopie implementeert op uw apparaat

Met de container-installatiekopie en de container registry-referenties u kunt de machine learning-model op uw IoT Edge-apparaat wilt implementeren. 

Volg de instructies in [implementeren IoT Edge-modules van de Azure-portal](../../iot-edge/how-to-deploy-modules-portal.md) uw model op uw IoT Edge-apparaat te starten. 











