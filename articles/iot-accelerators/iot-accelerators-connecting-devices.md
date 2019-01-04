---
title: Inrichten van Windows-apparaten voor externe controle in C - Azure | Microsoft Docs
description: Beschrijft hoe u een apparaat verbinden met de oplossingsversnellers bewaking op afstand met behulp van een toepassing die is geschreven in C die worden uitgevoerd op Windows.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: dobett
ms.openlocfilehash: 729ba19153eeb9767961d099e7a37c10a38b1286
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53634708"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Uw apparaat aansluiten op de Remote Monitoring solution accelerator (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie leert u hoe u een echt apparaat verbinden met de oplossingsverbetering voor externe controle.

Net als bij de meeste embedded-toepassingen die worden uitgevoerd op apparaten met beperkte, is de clientcode voor de apparaattoepassing geschreven in C. In deze zelfstudie bouwt u de apparaat-clienttoepassing op een computer met Windows.

Als u liever een apparaat simuleren, Zie [maken en test een nieuw gesimuleerd apparaat](iot-accelerators-remote-monitoring-create-simulated-device.md).

## <a name="prerequisites"></a>Vereisten

Om uit te voeren de stappen in deze gebruiksaanwijzing Volg de stappen in [instellen van uw Windows-ontwikkelomgeving](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) de vereiste ontwikkelprogramma's en bibliotheken toevoegen aan uw Windows-machine.

## <a name="view-the-code"></a>De code weergeven

De [voorbeeldcode](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) gebruikt in deze handleiding is beschikbaar in de Azure IoT C SDK's GitHub-opslagplaats.

### <a name="download-the-source-code-and-prepare-the-project"></a>De broncode downloaden en voorbereiden van het project

Voorbereiden van het project, [kloon de opslagplaats van de Azure IoT C SDKs](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) vanuit GitHub.

Het voorbeeld bevindt zich de **samples/oplossingen/remote_monitoring_client** map.

Open de **remote_monitoring.c** -bestand in de **samples/oplossingen/remote_monitoring_client** map in een teksteditor.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Bewerk de **remote_monitoring.c** vervangen door het bestand `<connectionstring>` met de apparaatverbindingsreeks die u hebt genoteerd aan het begin van deze gebruiksaanwijzing wanneer u een apparaat toegevoegd aan de solution accelerator.

1. Volg de stappen in [bouw de SDK voor C in Windows](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#build-the-c-sdk-in-windows) om de SDK en de clienttoepassing voor externe controle te bouwen.

1. Achter de opdrachtprompt-u hebt gebruikt om de oplossing te bouwen, voert u de volgende uit:

    ```cmd
    samples\solutions\remote_monitoring_client\Release\remote_monitoring_client.exe
    ```

    De console worden berichten weergegeven:

    - De toepassing verzendt voorbeeld telemetrie naar de solution accelerator.
    - Reageert op methoden aangeroepen vanuit het dashboard van de oplossing.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
