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
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 45f4de7e9ec880775f9ccf77b7d945766d465aa7
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
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

De volgende stappen ziet u hoe u uw Pi frambozen voorbereidt voor het bouwen van een toepassing C die verbinding met de vooraf geconfigureerde oplossing maakt:

1. Verbinding maken met uw frambozen Pi met `ssh`. Zie voor meer informatie [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) op de [frambozen Pi website](https://www.raspberrypi.org/).

1. Gebruik de volgende opdracht om bij te werken uw Pi frambozen:

    ```sh
    sudo apt-get update
    ```

1. Gebruik de volgende opdracht de vereiste ontwikkelingsprogramma's en -bibliotheken toevoegen aan uw Pi frambozen:

    ```sh
    sudo apt-get install g++ make cmake gcc git
    ```

1. Gebruik de volgende opdrachten voor het installeren van de clientbibliotheken IoT-Hub:

    ```sh
    grep -q -F 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
    grep -q -F 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA6A393E4C2257F
    sudo apt-get update
    sudo apt-get install -y azure-iot-sdk-c-dev cmake libcurl4-openssl-dev git-core
    ```

1. Kloon de parser Parson JSON naar uw frambozen Pi met de volgende opdrachten:

    ```sh
    cd ~
    git clone https://github.com/kgabis/parson.git
    ```

## <a name="create-a-project"></a>Een project maken

De volgende stappen uit met behulp van de `ssh` verbinding met uw Pi frambozen:

1. Maak een map `remote_monitoring` in uw basismap op de frambozen Pi. Navigeer naar deze map op de opdrachtregel:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Maken van de vier bestanden `main.c`, `remote_monitoring.c`, `remote_monitoring.h`, en `CMakeLists.txt` in de `remote_monitoring` map.

1. Maken van de map met de naam `parson` in de `remote_monitoring` map.

1. Kopieer de bestanden `parson.c` en `parson.h` uit het lokale exemplaar van de opslagplaats Parson in de `remote_monitoring/parson` map.

1. Open in een teksteditor, de `remote_monitoring.c` bestand. Op de Pi frambozen, kunt u ofwel de `nano` of `vi` teksteditor. Voeg de volgende `#include` instructies toe:

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

## <a name="add-code-to-run-the-app"></a>Code voor het uitvoeren van de app toevoegen

Open in een teksteditor, de `remote_monitoring.h` bestand. Voeg de volgende code toe:

```c
void remote_monitoring_run(void);
```

Open in een teksteditor, de `main.c` bestand. Voeg de volgende code toe:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

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

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
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
