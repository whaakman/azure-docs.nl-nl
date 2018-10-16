---
title: Aan de slag met Azure IoT Hub-module-id's en moduledubbels (portal en .NET) | Microsoft Docs
description: Informatie over het maken van module-id's en het bijwerken van moduledubbels met de portal en .NET.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: dobett
ms.openlocfilehash: 2c7857cd787e9298e94def5341d61238c5bb3a78
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321156"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Aan de slag met IoT Hub-module-id's en moduledubbels met behulp van de portal en .NET

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Met een Azure IoT Hub-apparaat-id en apparaatdubbel kan een apparaat worden geconfigureerd via de back-endtoepassing en kunt u inzicht krijgen in de toestand van een apparaat. Een module-id en moduledubbel bieden deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten met meerdere onderdelen, zoals apparaten met een besturingssysteem of firmware, kunt u afzonderlijke configuraties en voorwaarden voor elk onderdeel instellen.
>

In deze zelfstudie leert u het volgende:

1. Een module-id in de portal maken. 
1. SDK voor .NET-apparaten gebruiken om de moduledubbel vanuit uw apparaat bij te werken.

> [!NOTE]
> Raadpleeg het artikel [Azure IoT-SDKs][lnk-hub-sdks] voor meer informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.
>

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

## <a name="create-a-device-identity-in-the-portal"></a>Een apparaat-id in de portal maken

U hebt nu uw IoT Hub. Open [portal](https://portal.azure.com) en ga naar uw IoT Hub. Klik op IoT-apparaten en klik vervolgens op Toevoegen om een apparaat-id te maken. Geef deze de naam **MyFirstDevice**. 

  ![Apparaat-id maken][8]

Nadat de apparaat-id is opgeslagen, kunt u in de lijst met apparaat-id's zien dat MyFirstDevice is gemaakt.

  ![Apparaat-id gemaakt][11]

Klik nu op de rij. De apparaatgegevens worden weergegeven.

  ![Apparaatdetails][10]

## <a name="create-a-module-identity-in-the-portal"></a>Een module-id in de portal maken

Binnen één apparaat-id kunt u 20 module-entiteiten maken. Klik op de knop **Module-id toevoegen** bovenaan om uw eerste module-id met de naam **myFirstModule** te maken. 

  ![Apparaatdetails][9]

Sla de zojuist gemaakte module-id op en klik erop. U krijgt de gegevens van de module-id te zien. Sla de verbindingsreeks op - primaire sleutel. Deze wordt gebruikt in de volgende sectie waar u de module op het apparaat gaat installeren.

  ![Apparaatdetails][12]

## <a name="update-the-module-twin-using-net-device-sdk"></a>De moduledubbel bijwerken met de SDK voor .NET-apparaten

U hebt nu de module-id in uw IoT Hub gemaakt. Laten we met de cloud proberen te communiceren vanaf het gesimuleerde apparaat. Door de module-id te maken, wordt impliciet ook een moduledubbel in de IoT Hub gemaakt. In deze sectie maakt u een .NET-consoletoepassing op het gesimuleerde apparaat waarmee de gerapporteerde eigenschappen van de moduledubbel worden bijgewerkt.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

Voeg in Visual Studio een Visual C# Windows Classic Desktop-project aan de bestaande oplossing met behulp van de **Console-App (.NET Framework)** projectsjabloon. Zorg ervoor dat de versie van .NET Framework minimaal 4.6.1 is. Noem het project **UpdateModuleTwinReportedProperties**.

  ![Een Visual Studio-project maken][13]

## <a name="install-the-latest-azure-iot-hub-net-device-sdk"></a>De meest recente Azure IoT Hub .NET-apparaat-SDK installeren

Moduledubbel voor identiteits- en -module is in openbare preview. Het is alleen beschikbaar in de prerelease-SDK's voor IoT Hub-apparaten. Open in Visual Studio tools > Nuget package manager > manage Nuget packages for solution. Zoek naar Microsoft.Azure.Devices.Client. Zorg ervoor dat u het selectievakje Prerelease opnemen hebt ingeschakeld. Selecteer de meest recente versie en installeren. U hebt nu toegang tot alle modulefuncties. 

  ![Azure IoT Hub .NET service SDK V1.16.0-preview-005 installeren][14]

## <a name="get-your-module-connection-string"></a>De verbindingsreeks van uw module ophalen

Meld u aan bij [Azure-portal][lnk-portal]. Navigeer naar uw IoT Hub en klik op IoT-apparaten. Zoek naar MyFirstDevice, open het apparaat en u ziet dat myFirstModule is gemaakt. Kopieer de moduleverbindingsreeks. Deze is vereist voor de volgende stap.

  ![Details van de Azure Portal-module][15]

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

* [Aan de slag met IoT Hub identiteits- en module moduledubbel met behulp van .NET-back-up- en .NET-apparaat][lnk-csharp-csharp-getstarted]
* [Aan de slag met IoT Edge][lnk-iot-edge]


<!-- Images. -->
[8]:./media\iot-hub-portal-csharp-module-twin-getstarted/create-device-id.JPG
[9]:./media\iot-hub-portal-csharp-module-twin-getstarted/create-module-id.JPG
[10]:./media\iot-hub-portal-csharp-module-twin-getstarted/device-details.JPG
[11]:./media\iot-hub-portal-csharp-module-twin-getstarted/device-id-created.JPG
[12]:./media\iot-hub-portal-csharp-module-twin-getstarted/module-details.JPG
[13]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.JPG
[14]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png
[15]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/module-detail.JPG
<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-csharp-csharp-getstarted]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
