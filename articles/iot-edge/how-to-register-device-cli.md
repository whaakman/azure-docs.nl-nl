---
title: Registreren van een nieuwe Azure IoT randapparaat (CLI) | Microsoft Docs
description: Gebruik de IoT-extensie voor Azure CLI 2.0 voor het registreren van een nieuwe IoT-randapparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/30/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a6935dfe5b32364e402017cd3005ab17969ce1ce
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035914"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Een nieuwe Azure IoT Edge-apparaat registreren bij Azure CLI 2.0

Voordat u uw IoT-apparaten met Azure IoT rand gebruiken kunt, moet u deze registreren met uw IoT-hub. Wanneer u een apparaat registreert, krijgt u een verbindingsreeks die kan worden gebruikt voor het instellen van uw apparaat voor werkbelastingen van de rand. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) is een open-source, cross-platform opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT Edge. Hiermee kunt u voor het beheren van resources, apparaat inrichting service-exemplaren en gekoppelde hubs gebruiksklaar Azure IoT Hub. De nieuwe IoT-extensie voorziet Azure CLI 2.0 van extra functies zoals Apparaatbeheer en alle functionaliteit van IoT Edge.

In dit artikel laat zien hoe u een nieuwe IoT Edge-apparaat met Azure CLI 2.0 registreren.

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Een apparaat maken

Gebruik de volgende opdracht voor het maken van een nieuw apparaat-id in uw IoT-hub: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Met deze opdracht bevat drie parameters:
* **apparaat-id**: Geef een beschrijvende naam die uniek is voor uw IoT-hub.
* **naam van de hub**: Geef de naam van uw IoT-hub.
* **rand ingeschakeld**: deze parameter wordt aangegeven dat het apparaat voor gebruik met IoT rand is.

   ![Een IoT Edge-apparaat maken](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Alle apparaten weergeven

Gebruik de volgende opdracht om weer te geven van alle apparaten in uw IoT-hub:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Elk apparaat dat is geregistreerd als een Edge van de IoT-apparaat de eigenschap heeft **capabilities.iotEdge** ingesteld op **true**. 

## <a name="retrieve-the-connection-string"></a>De verbindingsreeks ophalen

Wanneer u klaar bent voor het instellen van uw apparaat, moet u de verbindingsreeks die is gekoppeld aan uw fysieke apparaat met de identiteit van de IoT-hub. Gebruik de volgende opdracht om te retourneren van de verbindingsreeks voor één apparaat:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

De apparaat-id-parameter is hoofdlettergevoelig. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [modules implementeert op een apparaat met Azure CLI 2.0](how-to-deploy-modules-cli.md)