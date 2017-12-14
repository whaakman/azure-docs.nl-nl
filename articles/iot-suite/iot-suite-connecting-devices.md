---
title: Windows-apparaten voor externe controle in de C - Azure inrichten | Microsoft Docs
description: Beschrijft hoe een apparaat aansluit op de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle met behulp van een toepassing die is geschreven in C met Windows.
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: a6453c8d84b7025f1f510db5e675cde8685c23ed
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Verbind het apparaat met de vooraf geconfigureerde oplossing voor externe controle (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie laat zien hoe een fysiek apparaat verbindt met de vooraf geconfigureerde oplossing voor externe controle.

## <a name="create-a-c-client-solution-on-windows"></a>Een C client-oplossing maken in Windows

Net als bij meest embedded-toepassingen die worden uitgevoerd op een beperkte apparaten, is de clientcode voor de apparaattoepassing geschreven in c In deze zelfstudie bouwt u de toepassing op een computer waarop Windows wordt uitgevoerd.

### <a name="create-the-starter-project"></a>Het starter-project maken

Maak een starter-project in Visual Studio 2017 en de IoT Hub apparaat-client NuGet-pakketten toevoegen:

1. Maak in Visual Studio een C-consoletoepassing met Visual C++ **Windows-consoletoepassing** sjabloon. Noem het project **RMDevice**.

    ![Visual C++-Windows-consoletoepassing maken](media/iot-suite-connecting-devices/visualstudio01.png)

1. In **Solution Explorer**, verwijdert u de bestanden `stdafx.h`, `targetver.h`, en `stdafx.cpp`.

1. In **Solution Explorer**, wijzig de bestandsnaam `RMDevice.cpp` naar `RMDevice.c`.

    ![Solution Explorer tonen hernoemd RMDevice.c bestand](media/iot-suite-connecting-devices/visualstudio02.png)

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en klik vervolgens op **beheren NuGet-pakketten**. Kies **Bladeren**, zoek vervolgens naar en installeren van de volgende NuGet-pakketten:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![NuGet-Pakketbeheer toont geïnstalleerde Microsoft.Azure.IoTHub pakketten](media/iot-suite-connecting-devices/visualstudio03.png)

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en kies vervolgens **eigenschappen** openen van het project **eigenschappenvensters** in het dialoogvenster. Zie voor meer informatie [instelling Visual C++-Projecteigenschappen](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Kies de **C/C++** map, en kies vervolgens de **vooraf gecompileerd Headers** eigenschappenpagina.

1. Stel **vooraf gecompileerd Header** naar **niet met behulp van vooraf gecompileerd Headers**. Kies vervolgens **toepassen**.

    ![Projecteigenschappen weergeven project niet met behulp van vooraf gecompileerde headers](media/iot-suite-connecting-devices/visualstudio04.png)

1. Kies de **Linker** map, en kies vervolgens de **invoer** eigenschappenpagina.

1. Voeg `crypt32.lib` naar de **extra afhankelijkheden** eigenschap. Sla het project eigenschapswaarden, kies **OK** en vervolgens **OK** opnieuw.

    ![Projecteigenschappen weergeven Linker crypt32.lib inclusief](media/iot-suite-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>De Parson JSON-bibliotheek toevoegen

Toevoegen van de bibliotheek Parson JSON naar de **RMDevice** project en voeg de vereiste `#include` instructies:

1. Kloon de Parson GitHub-opslagplaats met de volgende opdracht in een geschikte map op uw computer:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopieer de `parson.h` en `parson.c` bestanden uit de lokale kopie van de opslagplaats Parson naar uw **RMDevice** projectmap.

1. In Visual Studio met de rechtermuisknop op de **RMDevice** project, kies **toevoegen**, en kies vervolgens **bestaand Item**.

1. In de **Add Existing Item** dialoogvenster, selecteer de `parson.h` en `parson.c` bestanden in de **RMDevice** projectmap. Deze twee bestanden toevoegen aan uw project, kies **toevoegen**.

    ![Solution Explorer toont parson.h en parson.c-bestanden](media/iot-suite-connecting-devices/visualstudio06.png)

1. Open in Visual Studio de `RMDevice.c` bestand. Vervang de bestaande `#include` instructies met de volgende code:

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

    > [!NOTE]
    > Nu kunt u controleren of uw project de juiste afhankelijkheden die zijn ingesteld heeft door de oplossing bouwen.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Bouwen en uitvoeren van de steekproef

Voeg code aan te roepen de **externe\_bewaking\_uitvoeren** functioneren, en vervolgens bouwen en uitvoeren van de apparaattoepassing:

1. Om aan te roepen de **externe\_bewaking\_uitvoeren** werken, worden vervangen door de **belangrijkste** functie met de volgende code:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Kies **bouwen** en vervolgens **Build Solution** de apparaattoepassing te bouwen. Negeer de waarschuwing over de **gmtime** functie.

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project, kies **Debug**, en kies vervolgens **nieuw exemplaar gestart** om uit te voeren van het voorbeeld . De console worden berichten weergegeven:

    * De toepassing verzendt voorbeeld telemetrie naar de vooraf geconfigureerde oplossing.
    * Gewenste eigenschapswaarden ingesteld in het dashboard van de oplossing ontvangt.
    * Methoden die worden aangeroepen vanuit het dashboard van oplossing reageert.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
