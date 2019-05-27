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
ms.openlocfilehash: 3b1872699b8b3ac72424f00cd74bb90b8b7be87f
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873164"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Aan de slag met IoT Hub-module-id's en moduledubbels met behulp van de portal en .NET

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Met een Azure IoT Hub-apparaat-id en apparaatdubbel kan een apparaat worden geconfigureerd via de back-endtoepassing en kunt u inzicht krijgen in de toestand van een apparaat. Een module-id en moduledubbel bieden deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten met meerdere onderdelen, zoals apparaten met een besturingssysteem of firmware, kunt u afzonderlijke configuraties en voorwaarden voor elk onderdeel instellen.
>

In deze zelfstudie leert u het volgende:

1. het maken van de identiteit van een module in de portal.

2. Het gebruik van een update voor .NET-apparaat-SDK de moduledubbel van uw apparaat.

> [!NOTE]
> Zie voor informatie over de Azure IoT SDK's die u gebruiken kunt om beide toepassingen uit te voeren op apparaten en de back-end van uw oplossing te bouwen, [Azure IoT SDK's](iot-hub-devguide-sdks.md).
>

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio.
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Verbindingsreeks voor IoT-hub ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

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

Voeg in Visual Studio een Visual C# Windows Classic Desktop-project aan de bestaande oplossing met behulp van de **Console-App (.NET Framework)** projectsjabloon. Zorg ervoor dat de versie van .NET Framework minimaal 4.6.1 is. Noem het project **UpdateModuleTwinReportedProperties**.

  ![Een Visual Studio-project maken](./media/iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.png)

## <a name="install-the-latest-azure-iot-hub-net-device-sdk"></a>De meest recente Azure IoT Hub .NET-apparaat-SDK installeren

Moduledubbel voor identiteits- en -module is in openbare preview. Het is alleen beschikbaar in de apparaat-SDK's van de voorlopige versie in IoT Hub. Open in Visual Studio tools > Nuget package manager > manage Nuget packages for solution. Zoek naar Microsoft.Azure.Devices.Client. Zorg ervoor dat u hebt gecontroleerd selectievakje van de voorlopige versie opnemen. Selecteer de meest recente versie en installeren. U hebt nu toegang tot alle modulefuncties.

  ![Azure IoT Hub .NET service SDK V1.16.0-preview-005 installeren](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png)

## <a name="get-your-module-connection-string"></a>De verbindingsreeks van uw module ophalen

Meld u aan bij [Azure-portal](https://portal.azure.com/). Navigeer naar uw IoT Hub en klik op IoT-apparaten. Zoeken naar myFirstDevice, open deze en u ziet myFirstModule is aangemaakt. Kopieer de moduleverbindingsreeks. Deze is vereist voor de volgende stap.

  ![Details van de Azure Portal-module](./media/iot-hub-csharp-csharp-module-twin-getstarted/module-detail.png)

## <a name="create-updatemoduletwinreportedproperties-console-app"></a>UpdateModuleTwinReportedProperties console-app maken

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

U kunt nu de apps uitvoeren. Klik in Solution Explorer, in Visual Studio, met de rechtermuisknop op uw oplossing en klik vervolgens op **Set StartUp projects**. Selecteer **Meerdere opstartprojecten** en selecteer vervolgens **Starten** als de actie voor de consoletoepassing. En druk op F5 om te starten beide apps die worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met IoT Hub identiteits- en module moduledubbel met behulp van .NET-back-up- en .NET-apparaat](iot-hub-csharp-csharp-module-twin-getstarted.md)

* [Aan de slag met IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)