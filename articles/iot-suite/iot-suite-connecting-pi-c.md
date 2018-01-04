---
title: Frambozen Pi voor externe controle met C - Azure inrichten | Microsoft Docs
description: Hierin wordt beschreven hoe een frambozen Pi apparaat aansluit op de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle met behulp van een toepassing die is geschreven in c
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.author: dobett
ms.openlocfilehash: 7cfa6dd93c6db7477e03ff966b2ac8af15de3614
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-c"></a>Verbind het apparaat frambozen Pi met de vooraf geconfigureerde oplossing voor externe controle (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie laat zien hoe een fysiek apparaat verbindt met de vooraf geconfigureerde oplossing voor externe controle. Net als bij meest embedded-toepassingen die worden uitgevoerd op een beperkte apparaten, is de clientcode voor de toepassing frambozen Pi-apparaat geschreven in c In deze zelfstudie bouwt u de toepassing op een Pi frambozen het Raspbian-besturingssysteem.

### <a name="required-hardware"></a>Vereiste hardware

Een desktopcomputer zodat u kunt extern verbinding maken met de opdrachtregel op de frambozen Pi.

[Microsoft IoT Starter Kit voor frambozen Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) of gelijkwaardige onderdelen. Deze zelfstudie maakt gebruik van de volgende items van de kit:

- Raspberry Pi 3
- MicroSD-kaart (met NOOBS)
- Een Mini USB-kabel
- Een Ethernet-kabel

### <a name="required-desktop-software"></a>Vereiste bureaublad software

SSH-client moet u op de computer waarmee u kunt extern toegang tot de opdrachtregel op de frambozen Pi.

- Windows bevat geen een SSH-client. Wordt u aangeraden [PuTTY](http://www.putty.org/).
- De meeste Linux-distributies en Mac OS omvatten het SSH-opdrachtregelprogramma. Zie voor meer informatie [SSH met behulp van Linux- of Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Vereiste frambozen Pi software

In dit artikel wordt ervan uitgegaan dat u hebt geïnstalleerd dat de nieuwste versie van de [Raspbian OS op uw Pi frambozen](https://www.raspberrypi.org/learning/software-guide/quickstart/).

De volgende stappen ziet u hoe u uw Pi frambozen voorbereidt voor het bouwen van een toepassing C die verbinding met de vooraf geconfigureerde oplossing maakt:

1. Verbinding maken met uw frambozen Pi met **ssh**. Zie voor meer informatie [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) op de [frambozen Pi website](https://www.raspberrypi.org/).

1. Gebruik de volgende opdracht om bij te werken uw Pi frambozen:

    ```sh
    sudo apt-get update
    ```

1. Gebruik de volgende opdracht de vereiste ontwikkelingsprogramma's en -bibliotheken toevoegen aan uw Pi frambozen:

    ```sh
    sudo apt-get purge libssl-dev
    sudo apt-get install g++ make cmake gcc git libssl1.0-dev build-essential curl libcurl4-openssl-dev uuid-dev
    ```

1. Gebruik de volgende opdrachten om te downloaden, bouwen en de clientbibliotheken IoT-Hub installeren op uw Pi frambozen:

    ```sh
    cd ~
    git clone --recursive https://github.com/azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c/build_all/linux
    ./build.sh --no-make
    cd ../../cmake/iotsdk_linux
    make
    sudo make install
    ```

## <a name="create-a-project"></a>Een project maken

De volgende stappen uit met behulp van de **ssh** verbinding met uw Pi frambozen:

1. Maak een map `remote_monitoring` in uw basismap op de frambozen Pi. Navigeer naar deze map in uw shell:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Maken van de vier bestanden **main.c**, **remote_monitoring.c**, **remote_monitoring.h**, en **CMakeLists.txt** in de `remote_monitoring` de map.

1. Open in een teksteditor, de **remote_monitoring.c** bestand. Op de Pi frambozen, kunt u ofwel de **nano** of **vi** teksteditor. Voeg de volgende `#include` instructies toe:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

Sla de **remote_monitoring.c** bestands- en sluit de editor af.

## <a name="add-code-to-run-the-app"></a>Code voor het uitvoeren van de app toevoegen

Open in een teksteditor, de **remote_monitoring.h** bestand. Voeg de volgende code toe:

```c
void remote_monitoring_run(void);
```

Sla de **remote_monitoring.h** bestands- en sluit de editor af.

Open in een teksteditor, de **main.c** bestand. Voeg de volgende code toe:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

Sla de **main.c** bestands- en sluit de editor af.

## <a name="build-and-run-the-application"></a>De toepassing bouwen en uitvoeren.

De volgende stappen wordt beschreven hoe u *CMake* voor het bouwen van uw client-toepassing.

1. Open in een teksteditor, de **CMakeLists.txt** bestand de `remote_monitoring` map.

1. De volgende instructies voor het definiëren van het bouwen van uw client-toepassing toevoegen:

    ```cmake
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "/usr/local/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```

1. Sla de **CMakeLists.txt** bestands- en sluit de editor af.

1. In de `remote_monitoring` map, maak een map voor het opslaan van de *zorg* bestanden die CMake genereert. Voer vervolgens de **cmake** en **zorg** opdrachten als volgt:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Voer de clienttoepassing en verzenden van telemetrie naar IoT Hub:

    ```sh
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
