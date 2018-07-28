---
title: Registreer een nieuwe Azure IoT Edge-apparaat (CLI) | Microsoft Docs
description: De IoT-extensie voor Azure CLI 2.0 gebruiken om een nieuwe IoT Edge-apparaat te registreren
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 451f4df31cd1c520b14227829923f72fe80c38c3
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325493"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Een nieuw Azure IoT Edge-apparaat registreren bij Azure CLI 2.0

Voordat u uw IoT-apparaten met Azure IoT Edge gebruiken kunt, moet u hen registreert bij uw IoT-hub. Wanneer u een apparaat hebt geregistreerd, ontvangt u een verbindingsreeks die kan worden gebruikt voor het instellen van uw apparaat voor Edge-werkbelastingen. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) is een open-source, cross-platform opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT Edge. Hiermee kunt u voor het beheren van Azure IoT Hub-resources, device provisioning service-exemplaren en gekoppelde hubs buiten het vak. De nieuwe IoT-extensie voorziet Azure CLI 2.0 van extra functies zoals Apparaatbeheer en alle functionaliteit van IoT Edge.

In dit artikel bevat informatie over het registreren van een nieuwe IoT Edge-apparaat met behulp van Azure CLI 2.0.

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement. 
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw versie van Azure CLI 2.0 moet minimaal versie 2.0.24 of hoger zijn. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen. 
* De [IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Maken van een apparaat

Gebruik de volgende opdracht om te maken van een nieuwe apparaat-id in uw IoT-hub: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Met deze opdracht worden drie parameters:
* **apparaat-id**: Geef een beschrijvende naam die uniek is voor uw IoT-hub.
* **naam van de hub**: Geef de naam van uw IoT-hub.
* **Edge-functionaliteit**: deze parameter geeft aan dat het apparaat voor gebruik met IoT Edge.

   ![Een IoT Edge-apparaat maken](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Alle apparaten weergeven

Gebruik de volgende opdracht om alle apparaten in uw IoT-hub weer te geven:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Een apparaat dat is geregistreerd als een IoT Edge-apparaat de eigenschap heeft **capabilities.iotEdge** ingesteld op **waar**. 

## <a name="retrieve-the-connection-string"></a>De verbindingsreeks ophalen

Wanneer u klaar bent om uw apparaat instellen, moet u de verbindingsreeks die is gekoppeld aan uw fysieke apparaat met de identiteit van de IoT-hub. Gebruik de volgende opdracht uit om te retourneren van de verbindingsreeks voor een enkel apparaat:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

De apparaat-id-parameter is hoofdlettergevoelig. Kopieer niet de aanhalingstekens rond de verbindingsreeks. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [modules implementeert op een apparaat met Azure CLI 2.0](how-to-deploy-modules-cli.md)