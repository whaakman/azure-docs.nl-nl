---
title: Aan de slag met Azure IoT Hub-module-id's en moduledubbels (portal en .NET) | Microsoft Docs
description: Informatie over het maken van module-id's en het bijwerken van moduledubbels met de portal en .NET.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/26/2018
ms.openlocfilehash: cb7b60de3bfcd31b523b9be0fc49a3c621c8446f
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668116"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Aan de slag met IoT Hub-module-id's en moduledubbels met behulp van de portal en .NET

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Met een Azure IoT Hub-apparaat-id en apparaatdubbel kan een apparaat worden geconfigureerd via de back-endtoepassing en kunt u inzicht krijgen in de toestand van een apparaat. Een module-id en moduledubbel bieden deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten met meerdere onderdelen, zoals apparaten met een besturingssysteem of firmware, kunt u afzonderlijke configuraties en voorwaarden voor elk onderdeel instellen.
>

In deze zelfstudie leert u het volgende:

1. Een module-identiteit maken in de portal.

2. Een .NET-apparaat-SDK gebruiken de module dubbele van uw apparaat bijwerken.

> [!NOTE]
> Voor informatie over de Azure IoT-Sdk's die u kunt gebruiken om beide toepassingen te bouwen die op apparaten kunnen worden uitgevoerd en de back-end van uw oplossing, raadpleegt u [Azure IOT sdk's](iot-hub-devguide-sdks.md).
>

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio.
* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-module-identity-in-the-portal"></a>Een module-id in de portal maken

Binnen één apparaat-id kunt u 20 module-entiteiten maken. Klik op de knop **Module-id toevoegen** bovenaan om uw eerste module-id met de naam **myFirstModule** te maken.

  ![Apparaatdetails](./media/iot-hub-portal-csharp-module-twin-getstarted/create-module-id.png)

Sla de zojuist gemaakte module-id op en klik erop. U krijgt de gegevens van de module-id te zien. Sla de verbindingsreeks op - primaire sleutel. Deze wordt gebruikt in de volgende sectie waar u de module op het apparaat gaat installeren.

  ![Apparaatdetails](./media/iot-hub-portal-csharp-module-twin-getstarted/module-details.png)

## <a name="update-the-module-twin-using-net-device-sdk"></a>De moduledubbel bijwerken met de SDK voor .NET-apparaten

U hebt nu de module-id in uw IoT Hub gemaakt. Laten we met de cloud proberen te communiceren vanaf het gesimuleerde apparaat. Door de module-id te maken, wordt impliciet ook een moduledubbel in de IoT Hub gemaakt. In deze sectie maakt u een .NET-consoletoepassing op het gesimuleerde apparaat waarmee de gerapporteerde eigenschappen van de moduledubbel worden bijgewerkt.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

Voeg in Visual Studio een klassiek C# Windows-bureau blad-project toe aan de bestaande oplossing met behulp van de project sjabloon console- **app (.NET Framework)** . Zorg ervoor dat de versie van .NET Framework minimaal 4.6.1 is. Noem het project **UpdateModuleTwinReportedProperties**.

  ![Een Visual Studio-project maken](./media/iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.png)

## <a name="install-the-latest-azure-iot-hub-net-device-sdk"></a>De nieuwste Azure IoT Hub .NET-apparaat-SDK installeren

Module-identiteit en module dubbele is beschikbaar als open bare preview. Het is alleen beschikbaar in de IoT Hub voorlopige apparaat-Sdk's. Open in Visual Studio tools > Nuget package manager > manage Nuget packages for solution. Zoek naar Microsoft.Azure.Devices.Client. Zorg ervoor dat het selectie vakje include Prerelease is ingeschakeld. Selecteer de nieuwste versie en installeer deze. U hebt nu toegang tot alle modulefuncties.

  ![Azure IoT Hub .NET service SDK V1.16.0-preview-005 installeren](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png)

## <a name="get-your-module-connection-string"></a>Uw module connection string ophalen

Meld u aan bij de [Azure-portal](https://portal.azure.com/). Navigeer naar uw IoT Hub en klik op IoT-apparaten. Zoek myFirstDevice, open het en controleer of myFirstModule is gemaakt. Kopieer de moduleverbindingsreeks. Deze is vereist voor de volgende stap.

  ![Details van de Azure Portal-module](./media/iot-hub-csharp-csharp-module-twin-getstarted/module-detail.png)

## <a name="create-updatemoduletwinreportedproperties-console-app"></a>UpdateModuleTwinReportedProperties-console-app maken

Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

```csharp
using Microsoft.Azure.Devices.Client;
using Microsoft.Azure.Devices.Shared;
```

Voeg de volgende velden toe aan de klasse **Program**: Vervang de waarde van de tijdelijke aanduiding door de moduleverbindingsreeks.

```csharp
private const string ModuleConnectionString = "<Your module connection string>";
private static ModuleClient Client = null;
```

Voeg de methode **OnDesiredPropertyChanged** toe aan de klasse **Program**:

```csharp
private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
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
    Microsoft.Azure.Devices.Client.TransportType transport = Microsoft.Azure.Devices.Client.TransportType.Amqp;

    try
    {
        Client = ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
        Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

        Console.WriteLine("Retrieving twin");
        var twinTask = Client.GetTwinAsync();
        twinTask.Wait();
        var twin = twinTask.Result;
        Console.WriteLine(JsonConvert.SerializeObject(twin));

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
    Console.ReadKey();
    Client.CloseAsync().Wait();
}

private static void ConnectionStatusChangeHandler(ConnectionStatus status, ConnectionStatusChangeReason reason)
{
    Console.WriteLine($"Status {status} changed: {reason}");
}
```

In dit codevoorbeeld ziet u hoe u de gerapporteerde eigenschappen van de moduledubbel kunt ophalen en bijwerken met het AMQP-protocol. In de openbare preview-versie wordt alleen AMQP ondersteund voor moduledubbelbewerkingen.

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren. Klik in Solution Explorer, in Visual Studio, met de rechtermuisknop op uw oplossing en klik vervolgens op **Set StartUp projects**. Selecteer **Meerdere opstartprojecten** en selecteer vervolgens **Starten** als de actie voor de consoletoepassing. En druk vervolgens op F5 om de uitvoering van beide apps te starten.

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met de identiteit en module van IoT Hub module, met behulp van .NET backup en .NET Device](iot-hub-csharp-csharp-module-twin-getstarted.md)

* [Aan de slag met IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
