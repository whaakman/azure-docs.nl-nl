---
title: Firmware-update van het apparaat met Azure IoT Hub (.NET/knooppunt) | Microsoft Docs
description: Klik hier voor meer informatie over het beheer van apparaten op Azure IoT Hub gebruiken om te zetten van een apparaat firmware-update. U kunt het apparaat met Azure IoT SDK voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en de Azure IoT service SDK voor .NET voor het implementeren van een service-app waarmee de firmware-update wordt geactiveerd.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2017
ms.author: juanpere
ms.openlocfilehash: 157f112869f0042e330e6b281367632ca015e890
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnode"></a>Apparaatbeheer gebruiken om te zetten van een apparaat firmware-update (.NET/knooppunt)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaat twin] [ lnk-devtwin] en [methoden directe] [ lnk-c2dmethod] primitieven op afstand opnieuw opstarten van een apparaat. Deze zelfstudie gebruikt de dezelfde IoT Hub primitieven en ziet u hoe u een end-to-end gesimuleerde firmware-update.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de [frambozen Pi apparaat implementatie voorbeeld][lnk-rpi-implementation].

In deze handleiding ontdekt u hoe u:

* Een .NET-consoletoepassing die de directe firmwareUpdate-methode in de gesimuleerde apparaattoepassing via uw IoT-hub aanroept maken.
* Maakt een gesimuleerd apparaat-app die u implementeert een **firmwareUpdate** directe methode. Deze methode initieert een meerdere fasen die wacht de firmware-installatiekopie te downloaden, de installatiekopie van het firmware downloaden en ten slotte is de installatiekopie van de firmware van toepassing. In elke fase van de update wordt het apparaat de gerapporteerde eigenschappen gebruikt om te rapporteren over de voortgang.

Aan het einde van deze zelfstudie hebt u een Node.js-consoletoepassing apparaat en een app in de back-end voor .NET (C#)-console:

**dmpatterns_fwupdate_service.js**, wordt de reactie die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing weergegeven, en regelmatig (elke 500ms) geeft de bijgewerkte gerapporteerde eigenschappen.

**TriggerFWUpdate**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt, ontvangen een directe methode firmwareUpdate, wordt uitgevoerd via een proces met meerdere status om te simuleren een firmware-update inclusief: wachten op voor het downloaden van de installatiekopie, downloaden van de nieuwe installatiekopie en ten slotte de installatiekopie toe te passen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Node.js-versie 4.0.x of hoger <br/>  [Uw ontwikkelingsomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Ga als volgt de [aan de slag met Apparaatbeheer](iot-hub-csharp-node-device-management-get-started.md) artikel voor het maken van uw IoT-hub en uw IoT Hub-verbindingsreeks ophalen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een directe methode
In deze sectie maakt u een .NET-consoletoepassing (met C#) die een externe firmware-update op een apparaat start. De app gebruikmaakt van een directe methode om de update te initiëren en apparaat twin query's gebruikt om het periodiek de status van de actieve firmware-update niet ophalen.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **TriggerFWUpdate**.

    ![Nieuw Windows Classic Desktop-project in Visual C#][img-createapp]

1. Klik in Solution Explorer met de rechtermuisknop op de **TriggerFWUpdate** project en klik vervolgens op **NuGet-pakketten beheren...** .
1. Klik in de **NuGet Package Manager** op **Browse** en zoek naar **microsoft.azure.devices**. Accepteer de gebruiksvoorwaarden en klik op **Install** om het **Microsoft.Azure.Devices**-pakket te installeren. Met deze procedure worden de [Azure IoT-service-SDK][lnk-nuget-service-sdk], het NuGet-pakket en de bijbehorende afhankelijkheden gedownload en geïnstalleerd. Ook worden verwijzingen hiernaar toegevoegd.

    ![Sluit het venster Nuget Package Manager.][img-servicenuget]
1. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. Voeg de volgende velden toe aan de klasse **Program**: De meerdere tijdelijke aanduiding voor waarden vervangt door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie en de Id van uw apparaat.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. Voeg de volgende methode toe aan de klasse **Program**:
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. Voeg de volgende methode toe aan de klasse **Program**:

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
1. Open in Solution Explorer de **opstartprojecten instellen...**  en zorg ervoor dat de **actie** voor **TriggerFWUpdate** project **Start**.

1. De oplossing bouwen.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt in de **manageddevice** map, voer de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. In Visual Studio met de rechtermuisknop op de **TriggerFWUpdate** project, selecteer **Debug** en **nieuw exemplaar gestart**.

3. U ziet de reactie van het apparaat aan de directe methode in de console.

    ![Firmware is bijgewerkt][img-fwupdate]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een directe methode gebruikt voor het activeren van een externe firmware-update op een apparaat en de gerapporteerde eigenschappen gebruikt voor het bijhouden van de voortgang van de firmware-update.

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/