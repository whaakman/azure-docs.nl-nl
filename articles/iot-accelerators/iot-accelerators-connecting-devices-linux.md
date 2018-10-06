---
title: Inrichten van Linux-apparaten voor externe controle in C - Azure | Microsoft Docs
description: Beschrijft hoe u een apparaat verbinden met de oplossingsversnellers bewaking op afstand met behulp van een toepassing die is geschreven in C op Linux uitgevoerd.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: dobett
ms.openlocfilehash: 5faa91f054e62e2b3d9d317efe57f2d3f659cee6
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829831"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-linux"></a>Uw apparaat aansluiten op de oplossingsversnellers bewaking op afstand (Linux)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie leert u hoe u een fysiek apparaat verbinden met de oplossingsverbetering voor externe controle.

Net als bij de meeste embedded-toepassingen die worden uitgevoerd op apparaten met beperkte, is de clientcode voor de apparaattoepassing geschreven in C. In deze zelfstudie bouwt u de toepassing op een machine met Ubuntu (Linux).

## <a name="prerequisites"></a>Vereiste onderdelen

Als u wilt de stappen in deze handleiding hebt voltooid, moet u een apparaat met Ubuntu versie 15.04 of hoger. Voordat u doorgaat, [uw Linux-ontwikkelomgeving instellen](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux).

## <a name="view-the-code"></a>De code weergeven

De [voorbeeldcode](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) gebruikt in deze handleiding is beschikbaar in de Azure IoT C SDK's GitHub-opslagplaats.

### <a name="download-the-source-code-and-prepare-the-project"></a>De broncode downloaden en voorbereiden van het project

Om voor te bereiden op het project, klonen of downloaden de [opslagplaats Azure IoT C SDKs](https://github.com/Azure/azure-iot-sdk-c) vanuit GitHub.

Het voorbeeld bevindt zich de **samples/oplossingen/remote_monitoring_client** map.

Open de **remote_monitoring.c** -bestand in de **samples/oplossingen/remote_monitoring_client** map in een teksteditor.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-application"></a>De toepassing bouwen en uitvoeren.

De volgende stappen wordt beschreven hoe u *CMake* om de clienttoepassing te bouwen. De clienttoepassing voor externe controle is gebouwd als onderdeel van het bouwproces voor de SDK.

1. Bewerk de **remote_monitoring.c** vervangen door het bestand `<connectionstring>` met de apparaatverbindingsreeks die u hebt genoteerd aan het begin van deze gebruiksaanwijzing wanneer u een apparaat toegevoegd aan de solution accelerator.

1. Ga naar de hoofdmap van de gekloonde kopie van de [opslagplaats Azure IoT C SDKs](https://github.com/Azure/azure-iot-sdk-c) opslagplaats en de volgende opdrachten om de clienttoepassing te bouwen:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Voer de clienttoepassing en verzenden van telemetrie naar IoT Hub:

    ```sh
    ./samples/solutions/remote_monitoring_client/remote_monitoring_client
    ```

    De console worden berichten weergegeven:

    - De toepassing verzendt voorbeeld telemetrie naar de solution accelerator.
    - Reageert op methoden aangeroepen vanuit het dashboard van de oplossing.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
