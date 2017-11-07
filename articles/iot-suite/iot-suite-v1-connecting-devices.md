---
title: Verbinding maken met een apparaat met C op Windows | Microsoft Docs
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
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 56c58a31c6b20bd8da7d1442ae75425cb3e20f3d
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Verbind het apparaat met de vooraf geconfigureerde oplossing voor externe controle (Windows)
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Een voorbeeld C oplossing maken in Windows
De volgende stappen ziet u het maken van een clienttoepassing die communiceert met de vooraf geconfigureerde oplossing voor externe controle. Deze toepassing is geschreven in C en gebouwd en op Windows worden uitgevoerd.

Maak een starter-project in Visual Studio 2015 of Visual Studio 2017 en de IoT Hub apparaat-client NuGet-pakketten toevoegen:

1. Maak in Visual Studio een C-consoletoepassing met Visual C++ **Win32-consoletoepassing** sjabloon. Noem het project **RMDevice**.
2. Op de **toepassingsinstellingen** pagina in de **Wizard Win32-toepassing**, zorg ervoor dat **consoletoepassing** is geselecteerd en schakel het selectievakje **Precompiled koptekst** en **Security Development Lifecycle (SDL) controleert**.
3. In **Solution Explorer**, de bestanden stdafx.h, targetver.h en stdafx.cpp worden verwijderd.
4. In **Solution Explorer**, de naam van het bestand RMDevice.cpp RMDevice.c.
5. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en klik vervolgens op **beheren NuGet-pakketten**. Klik op **Bladeren**, zoek vervolgens naar en installeren van de volgende NuGet-pakketten:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project en klik vervolgens op **eigenschappen** openen van het project **eigenschappenvensters** in het dialoogvenster. Zie voor meer informatie [instelling Visual C++-Projecteigenschappen][lnk-c-project-properties]. 
7. Klik op de **Linker** map, klikt u vervolgens op de **invoer** eigenschappenpagina.
8. Voeg **crypt32.lib** naar de **extra afhankelijkheden** eigenschap. Klik op **OK** en vervolgens **OK** om opnieuw op te slaan van het project eigenschapswaarden.

Toevoegen van de bibliotheek Parson JSON naar de **RMDevice** project en voeg de vereiste `#include` instructies:

1. Kloon de Parson GitHub-opslagplaats met de volgende opdracht in een geschikte map op uw computer:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopieer de bestanden parson.h en parson.c uit de lokale kopie van de opslagplaats Parson naar uw **RMDevice** projectmap.

1. In Visual Studio met de rechtermuisknop op de **RMDevice** project, klikt u op **toevoegen**, en klik vervolgens op **bestaand Item**.

1. In de **Add Existing Item** dialoogvenster, selecteer de parson.h en parson.c bestanden in de **RMDevice** projectmap. Klik vervolgens op **toevoegen** deze twee bestanden toevoegen aan uw project.

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
    > Nu kunt u controleren of uw project de juiste afhankelijkheden die zijn ingesteld heeft door het opstellen.

[!INCLUDE [iot-suite-v1-connecting-code](../../includes/iot-suite-v1-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Bouwen en uitvoeren van de steekproef

Voeg code aan te roepen de **externe\_bewaking\_uitvoeren** werken en bouwen en uitvoeren van de apparaattoepassing.

1. Vervang de **belangrijkste** functie met de volgende code aan te roepen de **externe\_bewaking\_uitvoeren** functie:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klik op **bouwen** en vervolgens **Build Solution** de apparaattoepassing te bouwen.

1. In **Solution Explorer**, met de rechtermuisknop op de **RMDevice** project, klikt u op **Debug**, en klik vervolgens op **nieuw exemplaar gestart** om uit te voeren van het voorbeeld. De console worden berichten weergegeven als de toepassing voorbeeld telemetrie naar de vooraf geconfigureerde oplossing verzendt, ontvangt de gewenste eigenschapswaarden ingesteld in het dashboard van de oplossing en op de methoden die worden aangeroepen vanuit het dashboard van oplossing reageert.

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
