---
title: Registreren van een nieuw apparaat uit vanaf de opdrachtregel - Azure IoT Edge | Microsoft Docs
description: De IoT-extensie voor Azure CLI gebruiken om te registreren van een nieuwe IoT Edge-apparaat en de verbindingsreeks ophalen
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: de00c317483da9bcd93bb2e2505350d787385925
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66495398"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli"></a>Een nieuw Azure IoT Edge-apparaat registreren bij Azure CLI

Voordat u uw IoT-apparaten met Azure IoT Edge gebruiken kunt, moet u hen registreert bij uw IoT-hub. Wanneer u een apparaat hebt geregistreerd, ontvangt u een verbindingsreeks die kan worden gebruikt voor het instellen van uw apparaat voor IoT Edge-werkbelastingen.

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) is een open-source, cross-platform opdrachtregelprogramma voor het beheren van Azure-resources, zoals IoT Edge. Hiermee kunt u voor het beheren van Azure IoT Hub-resources, device provisioning service-exemplaren en gekoppelde hubs buiten het vak. Azure CLI verrijkt de nieuwe IoT-extensie met functies zoals Apparaatbeheer en de volledige functionaliteit van IoT Edge.

In dit artikel bevat informatie over het registreren van een nieuwe IoT Edge-apparaat met behulp van Azure CLI.

## <a name="prerequisites"></a>Vereisten

* Een [IoT-hub](../iot-hub/iot-hub-create-using-cli.md) in uw Azure-abonnement.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) in uw omgeving. Uw Azure CLI-versie moet ten minste 2.0.24 of hoger. Gebruik `az –-version` om de versie te valideren. In deze versie worden az-extensie-opdrachten ondersteund en is voor het eerst het Knack-opdrachtframework opgenomen.
* De [IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Een apparaat maken

Gebruik de [az iot hub apparaat-id maken](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) opdracht voor het maken van een nieuwe apparaat-id in uw IoT-hub. Bijvoorbeeld:

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Met deze opdracht worden drie parameters:

* **device-id**: Geef een beschrijvende naam die uniek is voor uw IoT-hub.

* **naam van de hub**: Geef de naam van uw IoT-hub.

* **edge-enabled**: Deze parameter worden gedeclareerd dat het apparaat voor gebruik met IoT Edge is.

   ![AZ iot hub device-identity maken uitvoer](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Alle apparaten weergeven

Gebruik de [az iot hub device-identity lijst](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-list) opdracht om alle apparaten in uw IoT-hub weer te geven. Bijvoorbeeld:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Een apparaat dat is geregistreerd als een IoT Edge-apparaat de eigenschap heeft **capabilities.iotEdge** ingesteld op **waar**.

## <a name="retrieve-the-connection-string"></a>De verbindingsreeks ophalen

Wanneer u klaar bent om uw apparaat instellen, moet u de verbindingsreeks die is gekoppeld aan uw fysieke apparaat met de identiteit van de IoT-hub. Gebruik de [apparaat-id in az iot hub-show-connection-string](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string) opdracht om de verbindingsreeks voor een enkel apparaat:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

De waarde voor de `device-id` parameter is hoofdlettergevoelig. Kopieer niet de aanhalingstekens rond de verbindingsreeks.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [modules implementeert op een apparaat met Azure CLI](how-to-deploy-modules-cli.md).
