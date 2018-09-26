---
title: Verbinding maken met een apparaat met c# op Windows | Microsoft Docs
description: Beschrijft hoe u een apparaat verbinden met de Azure IoT Suite vooraf geconfigureerde oplossing voor externe controle met behulp van een toepassing die is geschreven in C die worden uitgevoerd op Windows.
services: ''
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 56c58a31c6b20bd8da7d1442ae75425cb3e20f3d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093092"
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Uw apparaat aansluiten op de vooraf geconfigureerde oplossing voor externe controle (Windows)
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Maken van een Voorbeeldoplossing C op Windows
De volgende stappen laten zien hoe u een clienttoepassing die communiceert met de vooraf geconfigureerde oplossing voor externe controle maken. Deze toepassing is geschreven in C en gebouwd en uitgevoerd op Windows.

Maak een startersproject in Visual Studio 2015 of Visual Studio 2017 en de IoT Hub device client NuGet-pakketten toevoegen:

1. Maak in Visual Studio een C-consoletoepassing met behulp van de Visual C++ **Win32-consoletoepassing** sjabloon. Noem het project **RMDevice**.
2. Op de **toepassingsinstellingen** pagina in de **Win32-toepassingswizard**, zorg ervoor dat **consoletoepassing** is geselecteerd en schakel het selectievakje **Precompiled koptekst** en **Security Development Lifecycle (SDL) controleert**.
3. In **Solution Explorer**, verwijdert u de bestanden stdafx.h, targetver.h en stdafx.cpp.
4. In **Solution Explorer**, naam van het bestand RMDevice.cpp RMDevice.c.
5. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en klik vervolgens op **NuGet-pakketten beheren**. Klik op **Bladeren**en zoek vervolgens naar de volgende NuGet-pakketten installeren:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en klik vervolgens op **eigenschappen** openen van het project **eigenschappenvensters** in het dialoogvenster. Zie voor meer informatie, [instelling Visual C++-Projecteigenschappen][lnk-c-project-properties]. 
7. Klik op de **Linker** map, klikt u vervolgens op de **invoer** eigenschappenpagina.
8. Voeg **crypt32.lib** naar de **extra afhankelijkheden** eigenschap. Klik op **OK** en vervolgens **OK** opnieuw op te slaan van het project eigenschapswaarden.

Toevoegen van de bibliotheek Parson JSON naar de **RMDevice** project en voeg de vereiste `#include` instructies:

1. Kloon de Parson GitHub-opslagplaats met de volgende opdracht in een geschikte map op uw computer:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopieer de bestanden parson.h en parson.c van de lokale kopie van de opslagplaats van de Parson naar uw **RMDevice** projectmap.

1. In Visual Studio met de rechtermuisknop op de **RMDevice** project, klikt u op **toevoegen**, en klik vervolgens op **bestaand Item**.

1. In de **bestaand Item toevoegen** dialoogvenster, selecteer de parson.h en parson.c bestanden in de **RMDevice** projectmap. Klik vervolgens op **toevoegen** deze twee bestanden toevoegen aan uw project.

1. Open het bestand RMDevice.c in Visual Studio. Vervang de bestaande `#include` instructies met de volgende code:
   
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
    > Nu kunt u controleren of uw project de juiste afhankelijkheden ingesteld heeft bij het bouwen van deze.

[!INCLUDE [iot-suite-v1-connecting-code](../../includes/iot-suite-v1-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

Voeg code toe aan het aanroepen van de **externe\_bewaking\_uitvoeren** functioneren en bouwen en uitvoeren van de apparaattoepassing.

1. Vervang de **belangrijkste** functie met de volgende code om aan te roepen de **externe\_bewaking\_uitvoeren** functie:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klik op **bouwen** en vervolgens **Build Solution** om de apparaattoepassing te bouwen.

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project, klikt u op **Debug**, en klik vervolgens op **nieuwe instantie starten** om uit te voeren van het voorbeeld. De console worden berichten weergegeven als de toepassing voorbeeld telemetrie naar de vooraf geconfigureerde oplossing verzendt, ontvangt de gewenste eigenschapswaarden instellen op het dashboard van de oplossing en op methoden aangeroepen vanuit het dashboard van de oplossing reageert.

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
