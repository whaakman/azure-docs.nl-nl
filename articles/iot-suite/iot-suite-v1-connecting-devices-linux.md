---
title: Verbinding maken met een apparaat met C op Linux | Microsoft Docs
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
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: a5768041a13d5ddc355c054dc85ba651b0752aba
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Verbind het apparaat met de vooraf geconfigureerde oplossing voor externe controle (Linux)
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Bouwen en uitvoeren van een voorbeeld van C client Linux
De volgende stappen ziet u het maken van een clienttoepassing die communiceert met de vooraf geconfigureerde oplossing voor externe controle. Deze toepassing is geschreven in C en gebaseerd en op Ubuntu Linux wordt uitgevoerd.

Deze stappen uit te voeren, moet u een apparaat met Ubuntu versie 15.04 of 15.10. Voordat u doorgaat, de vereiste pakketten te installeren op uw Ubuntu-apparaat met de volgende opdracht:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>De clientbibliotheken op uw apparaat installeert
De clientbibliotheken van Azure IoT Hub zijn beschikbaar als een pakket dat u op uw Ubuntu apparaten met installeren kunt de **apt get-** opdracht. De volgende stappen voor het installeren van het pakket met de IoT Hub-bibliotheek en header-clientbestanden op uw computer Ubuntu:

1. In een shell toevoegen de opslagplaats AzureIoT op uw computer:
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. Installeer het azure-iot-sdk-c-dev-pakket
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-the-parson-json-parser"></a>Installeer de parser Parson JSON
De clientbibliotheken IoT Hub gebruiken de parser Parson JSON bericht nettoladingen parseren. Kloon de Parson GitHub-opslagplaats met de volgende opdracht in een geschikte map op uw computer:

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a>Voorbereiden van uw project
Maak een map op uw machine Ubuntu **externe\_bewaking**. In de **externe\_bewaking** map:

- Maken van de vier bestanden **main.c**, **externe\_monitoring.c**, **externe\_monitoring.h**, en **CMakeLists.txt**.
- Maken van de map met de naam **parson**.

Kopieer de bestanden **parson.c** en **parson.h** uit het lokale exemplaar van de opslagplaats Parson in de **externe\_bewaking/parson** map.

Open in een teksteditor, de **externe\_monitoring.c** bestand. Voeg de volgende `#include` instructies toe:
   
```
#include "iothubtransportmqtt.h"
#include "schemalib.h"
#include "iothub_client.h"
#include "serializer_devicetwin.h"
#include "schemaserializer.h"
#include "azure_c_shared_utility/threadapi.h"
#include "azure_c_shared_utility/platform.h"
#include "parson.h"
```

[!INCLUDE [iot-suite-v1-connecting-code](../../includes/iot-suite-v1-connecting-code.md)]

## <a name="call-the-remotemonitoringrun-function"></a>De externe aanroep\_bewaking\_functie uitvoeren
Open in een teksteditor, de **remote_monitoring.h** bestand. Voeg de volgende code toe:

```
void remote_monitoring_run(void);
```

Open in een teksteditor, de **main.c** bestand. Voeg de volgende code toe:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-the-application"></a>De toepassing bouwen en uitvoeren.
De volgende stappen wordt beschreven hoe u *CMake* voor het bouwen van uw client-toepassing.

1. Open in een teksteditor, de **CMakeLists.txt** bestand de **remote_monitoring** map.

1. De volgende instructies voor het definiëren van het bouwen van uw client-toepassing toevoegen:
   
    ```
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
1. In de **remote_monitoring** map, maak een map voor het opslaan van de *zorg* bestanden die CMake genereert en voer vervolgens de **cmake** en **zorg** opdrachten als volgt:
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Voer de clienttoepassing en verzenden van telemetrie naar IoT Hub:
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

