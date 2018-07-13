---
title: Inrichten van Raspberry Pi tot externe controle met c# - Azure | Microsoft Docs
description: Hierin wordt beschreven hoe u een Raspberry Pi-apparaat verbinden met de oplossingsversnellers bewaking op afstand met behulp van een toepassing die is geschreven in C.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 23e84a8d577bb1c4950de3acd76b0f8528551ae0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611438"
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

1. Gebruik de volgende opdracht de vereiste ontwikkelprogramma's en bibliotheken toevoegen aan uw Raspberry Pi:

    ```sh
    sudo apt-get install g++ make cmake gcc git libssl1.0-dev build-essential curl libcurl4-openssl-dev uuid-dev
    ```

1. Gebruik de volgende opdrachten om te downloaden en installeren van de IoT Hub-clientbibliotheken op uw Raspberry Pi bouwen:

    ```sh
    cd ~
    git clone --recursive https://github.com/azure/azure-iot-sdk-c.git
    mkdir cmake
    cd cmake
    cmake ..
    make
    sudo make install
    ```

## <a name="create-a-project"></a>Een project maken

De volgende stappen met behulp van de **ssh** verbinding met uw Raspberry Pi:

1. Maak een map genaamd `remote_monitoring` in de basismap van de Raspberry Pi. Navigeer naar deze map in uw shell:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Maken van de vier bestanden **main.c**, **remote_monitoring.c**, **remote_monitoring.h**, en **CMakeLists.txt** in de `remote_monitoring` de map.

1. Open in een teksteditor en de **remote_monitoring.c** bestand. Aan de Raspberry Pi, kunt u ofwel de **nano** of **vi** teksteditor. Voeg de volgende `#include` instructies toe:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include <string.h>
    ```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

Sla de **remote_monitoring.c** -bestand en de editor te sluiten.

## <a name="add-code-to-run-the-app"></a>Code voor het uitvoeren van de app toevoegen

Open in een teksteditor en de **remote_monitoring.h** bestand. Voeg de volgende code toe:

```c
void remote_monitoring_run(void);
```

Sla de **remote_monitoring.h** -bestand en de editor te sluiten.

Open in een teksteditor en de **main.c** bestand. Voeg de volgende code toe:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

Sla de **main.c** -bestand en de editor te sluiten.

## <a name="build-and-run-the-application"></a>De toepassing bouwen en uitvoeren.

De volgende stappen wordt beschreven hoe u *CMake* om uw clienttoepassing te bouwen.

1. Open in een teksteditor en de **CMakeLists.txt** -bestand in de `remote_monitoring` map.

1. Voeg de volgende instructies voor het definiëren van het bouwen van uw clienttoepassing:

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
      iothub_client_mqtt_transport
      umqtt
      iothub_client
      aziotsharedutil
      parson
      pthread
      curl
      ssl
      crypto
      m
    )
    ```

1. Sla de **CMakeLists.txt** -bestand en de editor te sluiten.

1. In de `remote_monitoring` map, maakt u een map voor het opslaan van de *maken* bestanden die CMake genereert. Voer vervolgens de **cmake** en **maken** opdrachten als volgt:

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
