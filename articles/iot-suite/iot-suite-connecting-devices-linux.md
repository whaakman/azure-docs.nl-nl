---
title: Linux-apparaten voor externe controle in de C - Azure inrichten | Microsoft Docs
description: Beschrijft hoe een apparaat aansluit op de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle met behulp van een toepassing die is geschreven in C op Linux wordt uitgevoerd.
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0c7c8039-0bbf-4bb5-9e79-ed8cff433629
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: e4ed662c4dbe6da05582426f56e087020c05c6b8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Verbind het apparaat met de vooraf geconfigureerde oplossing voor externe controle (Linux)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie laat zien hoe een fysiek apparaat verbindt met de vooraf geconfigureerde oplossing voor externe controle.

## <a name="create-a-c-client-project-on-linux"></a>Maken van een project C-client op Linux

Net als bij meest embedded-toepassingen die worden uitgevoerd op een beperkte apparaten, is de clientcode voor de apparaattoepassing geschreven in c In deze zelfstudie bouwt u de toepassing op een machine met Ubuntu (Linux).

Deze stappen uit te voeren, moet u een apparaat met Ubuntu versie 15.04 of hoger. Voordat u doorgaat, de vereiste pakketten te installeren op uw Ubuntu-apparaat met de volgende opdracht:

```sh
sudo apt-get install cmake gcc g++
```

### <a name="install-the-client-libraries-on-your-device"></a>De clientbibliotheken op uw apparaat installeert

De clientbibliotheken van Azure IoT Hub zijn beschikbaar als een pakket dat u op uw Ubuntu apparaten met installeren kunt de **apt get-** opdracht. De volgende stappen voor het installeren van het pakket met de IoT Hub-bibliotheek en header-clientbestanden op uw computer Ubuntu:

1. In een shell toevoegen de opslagplaats AzureIoT op uw computer:

    ```sh
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

1. Installeer het azure-iot-sdk-c-dev-pakket

    ```sh
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

### <a name="install-the-parson-json-parser"></a>Installeer de parser Parson JSON

De clientbibliotheken IoT Hub gebruiken de parser Parson JSON bericht nettoladingen parseren. Kloon de Parson GitHub-opslagplaats met de volgende opdracht in een geschikte map op uw computer:

```sh
git clone https://github.com/kgabis/parson.git
```

### <a name="prepare-your-project"></a>Voorbereiden van uw project

Maak een map op uw machine Ubuntu `remote_monitoring`. In de `remote_monitoring` map:

- Maken van de vier bestanden `main.c`, `remote_monitoring.c`, `remote_monitoring.h`, en `CMakeLists.txt`.
- Maken van de map met de naam `parson`.

Kopieer de bestanden `parson.c` en `parson.h` uit het lokale exemplaar van de opslagplaats Parson in de `remote_monitoring/parson` map.

Open in een teksteditor, de `remote_monitoring.c` bestand. Voeg de volgende `#include` instructies toe:

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
