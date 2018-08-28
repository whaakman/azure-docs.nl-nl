---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: chrissie926
ms.service: iot-hub
ms.topic: include
ms.date: 04/26/2018
ms.author: menchi
ms.custom: include file
ms.openlocfilehash: 603a4892cc19991532c2858edd8d4ca90f850d1f
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43087089"
---
## <a name="create-a-module-identity"></a>Een module-id maken

In dit gedeelte gaat u een .NET-consoletoepassing maken die een apparaat-id en een module-id kan maken in het identiteitsregister van uw IoT Hub. Een apparaat of module kan alleen verbinding maken met de IoT-hub als het apparaat of de module is vermeld in het identiteitsregister. Zie voor meer informatie de [sectie id-register van de IoT Hub developer guide](../articles/iot-hub/iot-hub-devguide-identity-registry.md). Als u deze consoletoepassing uitvoert, worden er een unieke id en een unieke sleutel gemaakt voor zowel het apparaat als de module. Deze waarden worden door het apparaat en de module gebruikt om zichzelf te identificeren bij het verzenden van apparaat-naar-cloud-berichten naar IoT Hub. De id's zijn hoofdlettergevoelig.


1. **Een Visual Studio-project maken**: voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan een nieuwe oplossing met behulp van de projectsjabloon **Console App (.NET Framework)**. Zorg ervoor dat de versie van .NET Framework minimaal 4.6.1 is. Geef het project de naam **CreateIdentities** en geef de oplossing de naam **IoTHubGetStarted**.

    ![Een Visual Studio-project maken](./media/iot-hub-get-started-create-module-identity-csharp/create-identities-csharp1.JPG)

2. **Azure IoT Hub .NET service SDK V1.16.0-preview-001 installeren**: Module identity and module twin is beschikbaar als openbare preview-versie. Het is alleen beschikbaar in de IoT Hub prerelease-service-SDK's. Open in Visual Studio tools > Nuget package manager > manage Nuget packages for solution. Zoek naar Microsoft.Azure.Devices. Zorg ervoor dat u het selectievakje Prerelease opnemen hebt ingeschakeld. Selecteer de versie V1.16.0-preview-001 en installeer deze. U hebt nu toegang tot alle modulefuncties. 

    ![Azure IoT Hub .NET service SDK V1.16.0-preview-001 installeren](./media/iot-hub-get-started-create-module-identity-csharp/install-sdk.png)

3. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Common.Exceptions;
   ```

4. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de vorige sectie.

   ```csharp
   const string connectionString = "<replace_with_iothub_connection_string>";
   const string deviceID = "myFirstDevice";
   const string moduleID = "myFirstModule";
   ```

5. Voeg de volgende code aan de **Main** klasse.
   
   ```csharp
   static void Main(string[] args)
   {
       AddDeviceAsync().Wait();
       AddModuleAsync().Wait();
   }
   ```

6. Voeg de volgende methoden toe aan de klasse **Program**:

    ```csharp
    private static async Task AddDeviceAsync()
    {
       RegistryManager registryManager = 
         RegistryManager.CreateFromConnectionString(connectionString);
       Device device;

       try
       {
           device = await registryManager.AddDeviceAsync(new Device(deviceID));
       }
       catch (DeviceAlreadyExistsException)
        {
            device = await registryManager.GetDeviceAsync(deviceID);
        }

        Console.WriteLine("Generated device key: {0}", 
          device.Authentication.SymmetricKey.PrimaryKey);
    }

    private static async Task AddModuleAsync()
    {
        RegistryManager registryManager = 
          RegistryManager.CreateFromConnectionString(connectionString);
        Module module;

        try
        {
            module = 
              await registryManager.AddModuleAsync(new Module(deviceID, moduleID));
        }
        catch (ModuleAlreadyExistsException)
        {
            module = await registryManager.GetModuleAsync(deviceID, moduleID);
        }

        Console.WriteLine("Generated module key: {0}", module.Authentication.SymmetricKey.PrimaryKey);
    }
    ```

    Met de methode AddDeviceAsync() maakt u een apparaat met de id **myFirstDevice**. (Als deze apparaat-id al in het id-register staat, haalt de code gewoon de bestaande apparaatgegevens op.) De app geeft vervolgens de primaire sleutel voor die identiteit weer. U gebruikt deze sleutel in de gesimuleerde apparaat-app om verbinding te maken met uw IoT-hub.

    Met de methode AddModuleAsync() maakt u een module met de id **myFirstModule** onder het apparaat **myFirstDevice**. (Als deze module-id al in het id-register staat, haalt de code gewoon de bestaande modulegegevens op.) De app geeft vervolgens de primaire sleutel voor die identiteit weer. U gebruikt deze sleutel in de gesimuleerde module-app om verbinding te maken met uw IoT-hub.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. Voer deze toepassing uit en noteer de apparaatsleutel en de modulesleutel.

> [!NOTE]
> In het identiteitsregister van IoT Hub worden alleen apparaat- en module-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. In het identiteitsregister worden apparaat-id's en -sleutels opgeslagen die als beveiligingsreferenties worden gebruikt. In het identiteitsregister wordt ook een vlag ingeschakeld/uitgeschakeld voor elk apparaat opgeslagen die u kunt gebruiken om de toegang tot dat apparaat uit te schakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Er is geen vlag voor ingeschakeld/uitgeschakeld voor module-id's. Zie voor meer informatie, [Ontwikkelaarshandleiding voor IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
