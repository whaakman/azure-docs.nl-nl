---
title: Aan de slag met Azure IoT Hub module identity and module twin (.NET) | Microsoft Docs
description: Informatie over het maken van module-id's en het bijwerken van moduledubbels met IoT SDK's voor .NET.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: 60ed1f6d6b3904055c01aa32b69299a5872c38ac
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68404568"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-net-back-end-and-net-device"></a>Aan de slag met IoT Hub module-identiteit en-module, met behulp van .NET-back-end en .NET-apparaat

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Met een Azure IoT Hub-apparaat-id en apparaatdubbel kan een apparaat worden geconfigureerd via de back-endtoepassing en kunt u inzicht krijgen in de toestand van een apparaat. Een module-id en moduledubbel bieden deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten met meerdere onderdelen, zoals apparaten met een besturingssysteem of firmware, kunt u afzonderlijke configuraties en voorwaarden voor elk onderdeel instellen.

Aan het eind van deze zelfstudie beschikt u over twee .NET-consoletoepassingen:

* **CreateIdentities**: deze toepassing maakt een apparaat-id, een module-id en de bijbehorende beveiligingssleutel waarmee uw apparaat- en moduleclients verbonden kunnen worden.

* **UpdateModuleTwinReportedProperties**: deze toepassing verzendt bijgewerkte gerapporteerde eigenschappen van de moduledubbel naar uw IoT Hub.

> [!NOTE]
> Voor informatie over de Azure IoT-Sdk's die u kunt gebruiken om beide toepassingen te bouwen die op apparaten kunnen worden uitgevoerd en de back-end van uw oplossing, raadpleegt u [Azure IOT sdk's](iot-hub-devguide-sdks.md).

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio.

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-module-identity-csharp](../../includes/iot-hub-get-started-create-module-identity-csharp.md)]

## <a name="update-the-module-twin-using-net-device-sdk"></a>De moduledubbel bijwerken met de SDK voor .NET-apparaten

In deze sectie maakt u een .NET-consoletoepassing op het gesimuleerde apparaat waarmee de gerapporteerde eigenschappen van de moduledubbel worden bijgewerkt.

1. **Een Visual Studio-project maken:** Voeg in Visual Studio een klassiek C# Windows-bureau blad-project toe aan de bestaande oplossing met behulp van de project sjabloon console- **app (.NET Framework)** . Zorg ervoor dat de versie van .NET Framework minimaal 4.6.1 is. Noem het project **UpdateModuleTwinReportedProperties**.

    ![Een Visual Studio-project maken](./media/iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.png)

2. **Installeer de nieuwste Azure IoT Hub .NET-apparaat-SDK:** Module-identiteit en module dubbele is beschikbaar als open bare preview. Het is alleen beschikbaar in de IoT Hub Prerelease-apparaat-Sdk's. Open in Visual Studio tools > Nuget package manager > manage Nuget packages for solution. Zoek naar Microsoft.Azure.Devices.Client. Zorg ervoor dat u het selectievakje Prerelease opnemen hebt ingeschakeld. Selecteer de nieuwste versie en installeer deze. U hebt nu toegang tot alle modulefuncties. 

    ![Azure IoT Hub .NET service SDK V1.16.0-preview-005 installeren](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png)

3. **Down load uw module Connection String** --nu als u zich aanmeldt bij [Azure Portal](https://portal.azure.com/). Navigeer naar uw IoT Hub en klik op IoT-apparaten. Zoek myFirstDevice, open het en controleer of myFirstModule is gemaakt. Kopieer de moduleverbindingsreeks. Deze is vereist voor de volgende stap.

    ![Details van de Azure Portal-module](./media/iot-hub-csharp-csharp-module-twin-getstarted/module-detail.png)

4. **UpdateModuleTwinReportedProperties-console-app maken**

    Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    ```

    Voeg de volgende velden toe aan de klasse **Program**: Vervang de waarde van de tijdelijke aanduiding door de moduleverbindingsreeks.

    ```csharp
    private const string ModuleConnectionString = 
      "<Your module connection string>";
    private static ModuleClient Client = null;
    static void ConnectionStatusChangeHandler(ConnectionStatus status, 
      ConnectionStatusChangeReason reason)
    {
        Console.WriteLine("Connection Status Changed to {0}; the reason is {1}", 
          status, reason);
    }
    ```

    Voeg de methode **OnDesiredPropertyChanged** toe aan de klasse **Program**:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
        {
            Console.WriteLine("desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
            Console.WriteLine("Sending current time as reported property");
            TwinCollection reportedProperties = new TwinCollection
            {
                ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
            };

            await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
        }
    ```

    Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    static void Main(string[] args)
    {
        Microsoft.Azure.Devices.Client.TransportType transport = 
          Microsoft.Azure.Devices.Client.TransportType.Amqp;

        try
        {
            Client = 
              ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
            Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

            Console.WriteLine("Retrieving twin");
            var twinTask = Client.GetTwinAsync();
            twinTask.Wait();
            var twin = twinTask.Result;
            Console.WriteLine(JsonConvert.SerializeObject(twin.Properties)); 

            Console.WriteLine("Sending app start time as reported property");
            TwinCollection reportedProperties = new TwinCollection();
            reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;

            Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (AggregateException ex)
        {
            Console.WriteLine("Error in sample: {0}", ex);
        }

        Console.WriteLine("Waiting for Events.  Press enter to exit...");
        Console.ReadLine();
        Client.CloseAsync().Wait();
    }
    ```

    In dit codevoorbeeld ziet u hoe u de gerapporteerde eigenschappen van de moduledubbel kunt ophalen en bijwerken met het AMQP-protocol. In de openbare preview-versie wordt alleen AMQP ondersteund voor moduledubbelbewerkingen.

5. Naast de bovenstaande **hoofd** methode kunt u onder code blok toevoegen om een gebeurtenis te verzenden naar IOT hub vanuit uw module:

    ```csharp
    Byte[] bytes = new Byte[2];
    bytes[0] = 0;
    bytes[1] = 1;
    var sendEventsTask = Client.SendEventAsync(new Message(bytes));
    sendEventsTask.Wait();
    Console.WriteLine("Event sent to IoT Hub.");
    ```

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren. Klik in Solution Explorer, in Visual Studio, met de rechtermuisknop op uw oplossing en klik vervolgens op **Set StartUp projects**. Selecteer **Meerdere opstartprojecten** en selecteer vervolgens **Starten** als de actie voor de consoletoepassing. Druk op F5 om de toepassing te starten.

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met Apparaatbeheer](iot-hub-node-node-device-management-get-started.md)
* [Aan de slag met IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
