---
title: Aan de slag met Azure IoT Hub-apparaatdubbels (.NET/.NET) | Microsoft Docs
description: Het gebruik van Azure IoT Hub-apparaatdubbels tags toevoegen en vervolgens een IoT Hub-query. U de Azure IoT-device-SDK voor .NET gebruiken voor het implementeren van het gesimuleerde apparaat-app en de Azure IoT service SDK voor .NET voor het implementeren van een service-app die wordt toegevoegd de labels en de IoT Hub-query wordt uitgevoerd.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: dobett
ms.openlocfilehash: defb9f72148e319b36954a8d448d5f441621fc43
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321577"
---
# <a name="get-started-with-device-twins-netnet"></a>Aan de slag met apparaatdubbels (.NET/.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelfstudie hebt u deze .NET-consoletoepassingen:

* **CreateDeviceIdentity**, een .NET-app die u maakt een apparaat-id en de bijbehorende beveiligingssleutel waarmee uw gesimuleerde apparaat-app.

* **AddTagsAndQuery**, een .NET-back-end-app die wordt toegevoegd tags en apparaatdubbels-query's.

* **ReportConnectivity**, een .NET-apparaat-app die een apparaat simuleert dat verbinding met uw IoT-hub aan de apparaat-id die eerder hebt gemaakt maakt, en rapporten van de voorwaarde van de verbinding.

> [!NOTE]
> Het artikel [Azure IoT SDK's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IoT SDK's die u gebruiken kunt om apparaat- en back-end-apps te bouwen.
> 

Voor deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2017.
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](http://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>De service-app maken

In deze sectie maakt u een .NET-consoletoepassing maken (met behulp van C#) waarmee metagegevens van de locatie worden toegevoegd aan het dubbele apparaat dat is gekoppeld aan **myDeviceId**. Deze vervolgens de dubbele apparaten die zijn opgeslagen in de IoT-hub selecteren van de apparaten die zich in de Verenigde Staten, en vervolgens de virtuele machines die gerapporteerd van een mobiele verbinding een query.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **AddTagsAndQuery**.
   
    ![Nieuw Windows Classic Desktop-project in Visual C#](./media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png)

2. Klik in Solution Explorer met de rechtermuisknop op de **AddTagsAndQuery** project en klik vervolgens op **NuGet-pakketten beheren...** .

3. In de **NuGet Package Manager** venster **Bladeren** en zoek naar de **Microsoft.Azure.Devices**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices** verpakt en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT service SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![Sluit het venster Nuget Package Manager.](./media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png)

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie.

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

6. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp  
    public static async Task AddTagsAndQuery()
    {
        var twin = await registryManager.GetTwinAsync("myDeviceId");
        var patch =
            @"{
                tags: {
                    location: {
                        region: 'US',
                        plant: 'Redmond43'
                    }
                }
            }";
        await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
        var query = registryManager.CreateQuery(
          "SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
        var twinsInRedmond43 = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43: {0}", 
          string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
        query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
        var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43 using cellular network: {0}", 
          string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
    }
    ```
   
    De **RegistryManager** klasse beschrijft de methoden die zijn vereist om te communiceren met de dubbele apparaten uit de service. De vorige code eerst initialiseert de **registryManager** object en vervolgens haalt de apparaatdubbel voor **myDeviceId**, en ten slotte de tags bijgewerkt met informatie over de gewenste locatie.
   
    Na het bijwerken van, het uitvoeren van twee query's: de eerste selecteert alleen de apparaatdubbels van apparaten die zich in de **Redmond43** plant en het tweede verfijning van de query voor het selecteren van alleen de apparaten die ook zijn verbonden via mobiel netwerk.
   
    Houd er rekening mee dat de vorige code bij het maken van de **query** object, geeft u een maximum aantal geretourneerde documenten. De **query** -object bevat een **HasMoreResults** Booleaanse eigenschap die u gebruiken kunt om aan te roepen de **GetNextAsTwinAsync** methoden meerdere keren om op te halen van alle resultaten. Een methode aangeroepen **GetNextAsJson** is beschikbaar voor de resultaten die niet de dubbele apparaten, bijvoorbeeld: resultaten van aggregatie-query's.

7. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

8. Open in Solution Explorer de **opstartprojecten instellen...**  en zorg ervoor dat de **actie** voor **AddTagsAndQuery** project **Start**. Bouw de oplossing.

9. Voer deze toepassing uit met de rechtermuisknop op de **AddTagsAndQuery** project en selecteer **Debug**, gevolgd door **nieuwe instantie starten**. U ziet één apparaat in de resultaten voor het stellen van de query voor alle apparaten in **Redmond43** en niets in de query die de resultaten beperkt tot apparaten die gebruikmaken van een mobiel netwerk.
   
    ![De resultaten van de query in het venster](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

In de volgende sectie maakt u een apparaat-app die de gegevens van hostconnectiviteit rapporten en het resultaat van de query in de vorige sectie wordt gewijzigd.

## <a name="create-the-device-app"></a>De apparaat-app maken

In deze sectie maakt u een .NET-consoletoepassing die verbinding met uw hub als maakt **myDeviceId**, en werkt vervolgens de gerapporteerde eigenschappen om de informatie die is verbonden met een mobiel netwerk.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **ReportConnectivity**.
   
    ![Nieuwe Visual C# Windows klassieke apparaat-app](./media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png)
    
2. Klik in Solution Explorer met de rechtermuisknop op de **ReportConnectivity** project en klik vervolgens op **NuGet-pakketten beheren...** .

3. In de **NuGet Package Manager** venster **Bladeren** en zoek naar de **Microsoft.Azure.Devices.Client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** verpakt en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT device-SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app](./media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png)

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de apparaatverbindingsreeks die u in de vorige sectie hebt genoteerd.

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;
      DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp
    public static async void InitClient()
    {
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
              TransportType.Mqtt);
            Console.WriteLine("Retrieving twin");
            await Client.GetTwinAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

    De **Client** object bevat de methoden die u nodig hebt om te communiceren met de dubbele apparaten van het apparaat. De code die hierboven, initialiseert de **Client** object, en vervolgens haalt de apparaatdubbel voor **myDeviceId**.

7. Voeg de volgende methode toe aan de klasse **Program**:

    ```csharp  
    public static async void ReportConnectivity()
    {
        try
        {
            Console.WriteLine("Sending connectivity data as reported property");
            
            TwinCollection reportedProperties, connectivity;
            reportedProperties = new TwinCollection();
            connectivity = new TwinCollection();
            connectivity["type"] = "cellular";
            reportedProperties["connectivity"] = connectivity;
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

   De bovenstaande updates code **myDeviceId**de eigenschap met de gegevens van hostconnectiviteit gerapporteerd.

8. Voeg tot slot de volgende regels toe aan de methode **Main**:

    ```csharp
    try
    {
        InitClient();
        ReportConnectivity();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

9. Open in Solution Explorer de **opstartprojecten instellen...**  en zorg ervoor dat de **actie** voor **ReportConnectivity** project **Start**. Bouw de oplossing.

10. Voer deze toepassing uit met de rechtermuisknop op de **ReportConnectivity** project en selecteer **Debug**, gevolgd door **nieuwe instantie starten**. Ziet u deze het ophalen van de dubbele gegevens, en vervolgens te verzenden connectiviteit heeft als een *gerapporteerde eigenschap*.
   
    ![Apparaat-app verbinding rapport uitvoeren](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)
       
11. Nu dat het apparaat heeft gemeld dat de gegevens over de connectiviteit, wordt deze weergegeven in beide query's. Uitvoeren van de .NET **AddTagsAndQuery** app opnieuw uit te voeren de query's. Deze keer **myDeviceId** moet worden weergegeven in de resultaten van beide query.
   
    ![Connectiviteit van apparaten is gerapporteerd](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U metagegevens van apparaten als labels toegevoegd vanuit een back-end-app en een gesimuleerde apparaat-app geschreven naar apparaatgegevens met connectiviteit van rapport in de apparaatdubbel. U hebt ook geleerd hoe u query's van deze informatie met behulp van de SQL-achtige IoT Hub-querytaal.

Gebruik de volgende bronnen voor meer informatie over het:

* verzenden van telemetrie van apparaten met de [verzenden van telemetrie vanaf een apparaat naar een IoT-hub](quickstart-send-telemetry-dotnet.md) zelfstudie

* apparaten configureren met de gewenste eigenschappen van het dubbele apparaat met de [gebruik gewenste eigenschappen om apparaten te configureren](tutorial-device-twins.md) zelfstudie

* Beheren van apparaten interactief (zoals het inschakelen van een fan, van een gebruiker beheerde app) met de [directe methoden gebruiken](quickstart-control-device-dotnet.md) zelfstudie.
