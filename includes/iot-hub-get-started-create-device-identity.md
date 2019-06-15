---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 09/07/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b2bce9788006a564def9bd8c1375a85dc4184b67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814830"
---
## <a name="create-a-device-identity"></a>Een apparaat-id maken

In deze sectie maakt u de Azure CLI gebruiken om te maken van een apparaat-id voor deze zelfstudie. De Azure CLI vooraf is geïnstalleerd in de [Azure Cloud Shell](~/articles/cloud-shell/overview.md), of u kunt [Azure CLI lokaal installeren](/cli/azure/install-azure-cli). Apparaat-id's zijn hoofdlettergevoelig.

1. Voer de volgende opdracht in de opdrachtregel omgeving waarbij u gebruikmaakt van de Azure CLI voor het installeren van de IoT-extensie:

    ```cmd/sh
    az extension add --name azure-cli-iot-ext
    ```

1. Als u de Azure CLI lokaal uitvoert, gebruikt u de volgende opdracht uit om aan te melden bij uw Azure-account (als u de Cloud Shell gebruikt, u automatisch bent aangemeld en u hoeft deze opdracht uit te voeren):

    ```cmd/sh
    az login
    ```

1. Maak ten slotte een nieuwe apparaat-id met de naam `myDeviceId` en op te halen van de verbindingsreeks met de volgende opdrachten:

    ```cmd/sh
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Noteer de apparaatverbindingsreeks van het resultaat. Deze verbindingsreeks van het apparaat wordt gebruikt door de apparaat-app verbinden met uw IoT-Hub als apparaat.

<!-- images and links -->
