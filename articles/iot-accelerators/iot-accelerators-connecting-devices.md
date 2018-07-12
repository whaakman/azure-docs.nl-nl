---
title: Inrichten van Windows-apparaten voor externe controle in C - Azure | Microsoft Docs
description: Beschrijft hoe u een apparaat verbinden met de oplossingsversnellers bewaking op afstand met behulp van een toepassing die is geschreven in C die worden uitgevoerd op Windows.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 139daea3e885636b352d4c9a1ba2651a24195b21
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38309867"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Uw apparaat aansluiten op de Remote Monitoring solution accelerator (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Deze zelfstudie leert u hoe u een fysiek apparaat verbinden met de oplossingsverbetering voor externe controle.

## <a name="create-a-c-client-solution-on-windows"></a>Maken van een oplossing voor C-client op Windows

Net als bij de meeste embedded-toepassingen die worden uitgevoerd op apparaten met beperkte, is de clientcode voor de apparaattoepassing geschreven in C. In deze zelfstudie bouwt u de toepassing op een computer met Windows.

### <a name="create-the-starter-project"></a>Het startersproject maken

Maak een startersproject in Visual Studio 2017 en de IoT Hub device client NuGet-pakketten toevoegen:

1. Maak in Visual Studio een C-consoletoepassing met behulp van de Visual C++ **Windows-consoletoepassing** sjabloon. Noem het project **RMDevice**.

    ![Visual C++-Windows-consoletoepassing maken](./media/iot-accelerators-connecting-devices/visualstudio01.png)

1. In **Solution Explorer**, verwijdert u de bestanden `stdafx.h`, `targetver.h`, en `stdafx.cpp`.

1. In **Solution Explorer**, naam van het bestand `RMDevice.cpp` naar `RMDevice.c`.

    ![Solution Explorer waarin de naam RMDevice.c bestand gewijzigd](./media/iot-accelerators-connecting-devices/visualstudio02.png)

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en klik vervolgens op **NuGet-pakketten beheren**. Kies **Bladeren**en zoek vervolgens naar de volgende NuGet-pakketten installeren:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![NuGet-Pakketbeheer geeft geïnstalleerde Microsoft.Azure.IoTHub pakketten](./media/iot-accelerators-connecting-devices/visualstudio03.png)

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en kies vervolgens **eigenschappen** openen van het project **eigenschappenvensters** in het dialoogvenster. Zie voor meer informatie, [instelling Visual C++-Projecteigenschappen](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Kies de **C/C++** map, en kies vervolgens de **vooraf geschreven Headers** eigenschappenpagina.

1. Stel **vooraf geschreven Header** naar **niet met behulp van vooraf geschreven Headers**. Kies vervolgens **toepassen**.

    ![Projecteigenschappen weergeven project niet met behulp van vooraf gecompileerde headers](./media/iot-accelerators-connecting-devices/visualstudio04.png)

1. Kies de **Linker** map, en kies vervolgens de **invoer** eigenschappenpagina.

1. Voeg `crypt32.lib` naar de **extra afhankelijkheden** eigenschap. Als u wilt opslaan eigenschapswaarden van het project, kies **OK** en vervolgens **OK** opnieuw.

    ![Projecteigenschappen weergeven Linker, met inbegrip van crypt32.lib](./media/iot-accelerators-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>De Parson JSON-bibliotheek toevoegen

Toevoegen van de bibliotheek Parson JSON naar de **RMDevice** project en voeg de vereiste `#include` instructies:

1. Kloon de Parson GitHub-opslagplaats met de volgende opdracht in een geschikte map op uw computer:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopieer de `parson.h` en `parson.c` bestanden van de lokale kopie van de opslagplaats van de Parson naar uw **RMDevice** projectmap.

1. In Visual Studio met de rechtermuisknop op de **RMDevice** project, kies **toevoegen**, en kies vervolgens **bestaand Item**.

1. In de **bestaand Item toevoegen** dialoogvenster, selecteer de `parson.h` en `parson.c` bestanden in de **RMDevice** projectmap. Deze twee bestanden toevoegen aan uw project, kies **toevoegen**.

    ![Solution Explorer toont de parson.h en parson.c-bestanden](./media/iot-accelerators-connecting-devices/visualstudio06.png)

1. Open in Visual Studio, de `RMDevice.c` bestand. Vervang de bestaande `#include` instructies met de volgende code:

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

    > [!NOTE]
    > Nu kunt u controleren of uw project de juiste afhankelijkheden ingesteld heeft bij het bouwen van de oplossing.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

Voeg code toe aan het aanroepen van de **externe\_bewaking\_uitvoeren** functioneren, en vervolgens bouwen en uitvoeren van de apparaattoepassing:

1. Om aan te roepen de **externe\_bewaking\_uitvoeren** Vervang de **belangrijkste** functie met de volgende code:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Kies **bouwen** en vervolgens **Build Solution** om de apparaattoepassing te bouwen.

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project, kies **Debug**, en kies vervolgens **nieuwe instantie starten** het voorbeeld uitvoeren . De console worden berichten weergegeven:

    * De toepassing verzendt voorbeeld telemetrie naar de solution accelerator.
    * Gewenste eigenschapswaarden instellen in het oplossingsdashboard ontvangt.
    * Reageert op methoden aangeroepen vanuit het dashboard van de oplossing.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
