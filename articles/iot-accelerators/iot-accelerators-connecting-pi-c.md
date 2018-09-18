---
title: Inrichten van Raspberry Pi tot externe controle met c# - Azure | Microsoft Docs
description: Hierin wordt beschreven hoe u een Raspberry Pi-apparaat verbinden met de oplossingsversnellers bewaking op afstand met behulp van een toepassing die is geschreven in C.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: dobett
ms.openlocfilehash: add49aaf96190f782d2133e2a5f620a340f05eaf
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733828"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-c"></a>Uw Raspberry Pi-apparaat verbinden met de Remote Monitoring solution accelerator (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie leert u hoe u een fysiek apparaat verbinden met de oplossingsverbetering voor externe controle. Net als bij de meeste embedded-toepassingen die worden uitgevoerd op apparaten met beperkte, is de clientcode voor de toepassing Raspberry Pi-apparaat geschreven in C. In deze zelfstudie bouwt u de toepassing op een Raspberry Pi met de Raspbian OS.

### <a name="required-hardware"></a>Vereiste hardware

Een desktopcomputer waarmee u kunt extern verbinding maken met de opdrachtregel op de Raspberry Pi.

[Microsoft IoT Starter Kit voor Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) of equivalente onderdelen. In deze zelfstudie wordt de volgende items uit de kit:

- Raspberry Pi 3
- MicroSD-kaart (met NOOBS)
- Een Mini USB-kabel
- Een Ethernet-kabel

### <a name="required-desktop-software"></a>Vereiste bureaublad-software

SSH-client moet u op de computer waarmee u kunt voor externe toegang tot de opdrachtregel op de Raspberry Pi.

- Windows bevat geen een SSH-client. Wordt u aangeraden [PuTTY](http://www.putty.org/).
- De meeste Linux-distributies en Mac OS omvatten het SSH-opdrachtregelprogramma. Zie voor meer informatie, [SSH met behulp van Linux- of Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Vereiste software voor Raspberry Pi

In dit artikel wordt ervan uitgegaan dat u hebt geïnstalleerd dat de nieuwste versie van de [Raspbian besturingssysteem op uw Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/quickstart/).

De volgende stappen laten zien hoe u uw Raspberry Pi voorbereiden voor het bouwen van een toepassing C die verbinding met de oplossingsversnellers maakt:

1. Verbinding maken met uw Raspberry Pi met **ssh**. Zie voor meer informatie, [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) op de [Raspberry Pi website](https://www.raspberrypi.org/).

1. Gebruik de volgende opdracht uit om bij te werken uw Raspberry Pi:

    ```sh
    sudo apt-get update
    ```

1. Om uit te voeren de stappen in deze gebruiksaanwijzing Volg de stappen in [uw Linux-ontwikkelomgeving instellen](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) de vereiste ontwikkelprogramma's en bibliotheken toevoegen aan uw Raspberry Pi.

## <a name="view-the-code"></a>De code weergeven

De [voorbeeldcode](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring) gebruikt in deze handleiding is beschikbaar in de Azure IoT C SDK's GitHub-opslagplaats.

### <a name="download-the-source-code-and-prepare-the-project"></a>De broncode downloaden en voorbereiden van het project

Om voor te bereiden op het project, klonen of downloaden de [opslagplaats Azure IoT C SDKs](https://github.com/Azure/azure-iot-sdk-c) vanuit GitHub.

Het voorbeeld bevindt zich de **samples/oplossingen/remote_monitoring** map.

Open de **remote_monitoring.c** -bestand in de **samples/oplossingen/remote_monitoring** map in een teksteditor.

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
    ./samples/solutions/remote_monitoring/remote_monitoring_client
    ```

    De console worden berichten weergegeven:

    - De toepassing verzendt voorbeeld telemetrie naar de solution accelerator.
    - Reageert op methoden aangeroepen vanuit het dashboard van de oplossing.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
