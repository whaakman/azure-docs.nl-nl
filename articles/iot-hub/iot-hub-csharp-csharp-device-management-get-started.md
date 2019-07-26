---
title: Aan de slag met Azure IoT Hub Device Management (.NET/.NET) | Microsoft Docs
description: Azure IoT Hub Apparaatbeheer gebruiken om het opnieuw opstarten van een extern apparaat te initiëren. U gebruikt de Azure IoT Device SDK voor .NET voor het implementeren van een gesimuleerde apparaat-app die een directe methode bevat en de Azure IoT Service SDK voor .NET voor het implementeren van een service-app die de directe methode aanroept.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: robinsh
ms.openlocfilehash: 40db247dba1d55b5121f95a4d69ca853f3d7ee56
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68404577"
---
# <a name="get-started-with-device-management-netnet"></a>Aan de slag met Apparaatbeheer (.NET/.NET)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze zelfstudie ontdekt u hoe u:

* Gebruik de Azure Portal voor het maken van een IoT Hub en het maken van een apparaat-id in uw IoT-hub.

* Maak een gesimuleerde apparaat-app die een directe methode bevat waarmee dat apparaat opnieuw wordt opgestart. Directe methoden worden vanuit de Cloud aangeroepen.

* Maak een .NET-console-app die de directe methode voor opnieuw opstarten aanroept in de gesimuleerde apparaat-app via uw IoT-hub.

Aan het eind van deze zelfstudie beschikt u over twee .NET-consoletoepassingen:

* **SimulateManagedDevice**, die verbinding maakt met uw IOT-hub met de apparaat-id die u eerder hebt gemaakt, ontvangt een directe methode voor opnieuw opstarten, simuleert fysieke opnieuw opstarten en rapporteert de tijd voor de laatste keer opnieuw opstarten.

* **TriggerReboot**, waarmee een directe methode wordt aangeroepen in de gesimuleerde apparaat-app, het antwoord wordt weer gegeven en de bijgewerkte gerapporteerde eigenschappen worden weer gegeven.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio.

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een externe keer opnieuw opstarten op het apparaat activeren met behulp van een directe methode

In deze sectie maakt u een .NET-console-app ( C#met) die op afstand opnieuw wordt opgestart op een apparaat met behulp van een directe methode. De app maakt gebruik van Device-dubbele query's om de tijd van de laatste keer opnieuw opstarten voor dat apparaat te detecteren.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de nieuwe oplossing met behulp van de projectsjabloon **Console App (.NET Framework)** . Zorg ervoor dat de versie van .NET Framework minimaal 4.5.1 is. Geef het project de naam **TriggerReboot**.

    ![Nieuw Windows Classic Desktop-project in Visual C#](./media/iot-hub-csharp-csharp-device-management-get-started/createserviceapp.png)

2. Klik in Solution Explorer met de rechter muisknop op het project **TriggerReboot** en klik vervolgens op **NuGet-pakketten beheren**.

3. Selecteer in het venster **NuGet package manager** **Bladeren**, zoek naar **micro soft. Azure. devices**, selecteer **installeren** om het **micro soft. Azure. devices** -pakket te installeren en accepteer de gebruiks voorwaarden. Met deze procedure wordt een verwijzing naar het [Azure IOT Service SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet-pakket en de bijbehorende afhankelijkheden gedownload, geïnstalleerd en toegevoegd.

    ![Sluit het venster Nuget Package Manager.](./media/iot-hub-csharp-csharp-device-management-get-started/servicesdknuget.png)

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Shared;
   ```
        
5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de waarde van de tijdelijke aanduiding door de IoT Hub connection string u eerder hebt gekopieerd in [de IOT hub-Connection String ophalen](#get-the-iot-hub-connection-string).
   
   ```csharp
   static RegistryManager registryManager;
   static string connString = "{iot hub connection string}";
   static ServiceClient client;
   static string targetDevice = "myDeviceId";
   ```

6. Voeg de volgende methode toe aan de klasse **Program**.  Met deze code wordt het apparaat voor het opnieuw opstarten van het apparaat opgehaald en worden de gerapporteerde eigenschappen uitgevoerd.
   
   ```csharp
   public static async Task QueryTwinRebootReported()
   {
       Twin twin = await registryManager.GetTwinAsync(targetDevice);
       Console.WriteLine(twin.Properties.Reported.ToJson());
   }
   ```
        
7. Voeg de volgende methode toe aan de klasse **Program**.  Met deze code wordt de computer opnieuw opgestart op het apparaat met behulp van een directe methode.

   ```csharp
   public static async Task StartReboot()
   {
       client = ServiceClient.CreateFromConnectionString(connString);
       CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
       method.ResponseTimeout = TimeSpan.FromSeconds(30);

       CloudToDeviceMethodResult result = await 
         client.InvokeDeviceMethodAsync(targetDevice, method);

       Console.WriteLine("Invoked firmware update on device.");
   }
   ```

7. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
   ```csharp
   registryManager = RegistryManager.CreateFromConnectionString(connString);
   StartReboot().Wait();
   QueryTwinRebootReported().Wait();
   Console.WriteLine("Press ENTER to exit.");
   Console.ReadLine();
   ```

8. Bouw de oplossing.

> [!NOTE]
> In deze zelf studie wordt slechts één query uitgevoerd voor de gerapporteerde eigenschappen van het apparaat. In productie code kunt u het beste navragen om wijzigingen in de gerapporteerde eigenschappen te detecteren.

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie gaat u het volgende doen:

* Maak een .NET-console-app die reageert op een directe methode die wordt aangeroepen door de Cloud.

* Activeer een gesimuleerd apparaat opnieuw opstarten.

* Gebruik de gerapporteerde eigenschappen om Device-dubbele query's in te scha kelen om apparaten te identificeren en wanneer ze voor het laatst opnieuw zijn opgestart.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Geef het project de naam **SimulateManagedDevice**.
   
    ![Nieuwe app C# voor klassieke Windows-apparaten](./media/iot-hub-csharp-csharp-device-management-get-started/createdeviceapp.png)
    
2. Klik in Solution Explorer met de rechter muisknop op het project **SimulateManagedDevice** en klik vervolgens op **NuGet-pakketten beheren...** .

3. Klik in het venster **NuGet package manager** op **Browse** en zoek naar **micro soft. Azure. devices. client**. Selecteer **installeren** om het **micro soft. Azure. devices. client** -pakket te installeren en accepteer de gebruiks voorwaarden. Met deze procedure wordt een verwijzing naar het [Azure IOT Device SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet-pakket en de bijbehorende afhankelijkheden gedownload, geïnstalleerd en toegevoegd.
   
    ![NuGet Package Manager-venster Client-App](./media/iot-hub-csharp-csharp-device-management-get-started/clientsdknuget.png)
    
4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de waarde van de tijdelijke aanduiding door het apparaat connection string dat u in de vorige sectie hebt genoteerd.

    ```csharp
    static string DeviceConnectionString = 
      "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```
6. Voeg het volgende toe om de directe methode op het apparaat te implementeren:

   ```csharp
   static Task<MethodResponse> onReboot(MethodRequest methodRequest, object userContext)
   {
       // In a production device, you would trigger a reboot 
       //   scheduled to start after this method returns.
       // For this sample, we simulate the reboot by writing to the console
       //   and updating the reported properties.
       try
       {
           Console.WriteLine("Rebooting!");

           // Update device twin with reboot time. 
           TwinCollection reportedProperties, reboot, lastReboot;
           lastReboot = new TwinCollection();
           reboot = new TwinCollection();
           reportedProperties = new TwinCollection();
           lastReboot["lastReboot"] = DateTime.Now;
           reboot["reboot"] = lastReboot;
           reportedProperties["iothubDM"] = reboot;
           Client.UpdateReportedPropertiesAsync(reportedProperties).Wait();
       }
       catch (Exception ex)
       {
           Console.WriteLine();
           Console.WriteLine("Error in sample: {0}", ex.Message);
       }

       string result = "'Reboot started.'";
       return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
   }
   ```

7. Voeg tot slot de volgende code toe aan de methode **Main** om de verbinding met uw IOT-hub te openen en de methode-listener te initialiseren:

   ```csharp
   try
   {
       Console.WriteLine("Connecting to hub");
       Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
         TransportType.Mqtt);

       // setup callback for "reboot" method
       Client.SetMethodHandlerAsync("reboot", onReboot, null).Wait();
       Console.WriteLine("Waiting for reboot method\n Press enter to exit.");
       Console.ReadLine();

       Console.WriteLine("Exiting...");

       // as a good practice, remove the "reboot" handler
       Client.SetMethodHandlerAsync("reboot", null, null).Wait();
       Client.CloseAsync().Wait();
   }
   catch (Exception ex)
   {
       Console.WriteLine();
       Console.WriteLine("Error in sample: {0}", ex.Message);
   }
   ```
        
8. Klik in Solution Explorer van Visual Studio met de rechtermuisknop op uw oplossing en klik op **Set StartUp Projects...** . Selecteer **één opstart project**en selecteer vervolgens het project **SimulateManagedDevice** in de vervolg keuzelijst. Bouw de oplossing.       

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productie code moet u beleid voor opnieuw proberen implementeren (zoals een exponentiële uitstel), zoals wordt voorgesteld in het artikel, [tijdelijke fout afhandeling](/azure/architecture/best-practices/transient-faults).

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. De .NET-app voor **SimulateManagedDevice**uitvoeren. Klik met de rechter muisknop op het project **SimulateManagedDevice** , selecteer **fout opsporing**en selecteer vervolgens **nieuw exemplaar starten**. Het moet beginnen met Luis teren naar methode aanroepen vanuit uw IoT-hub. 

2. Nu het apparaat is verbonden en er wordt gewacht op methode aanroepen, voert u de .NET **TriggerReboot** -app uit om de methode reboot aan te roepen in de app gesimuleerde apparaten. U doet dit door met de rechter muisknop op het project **TriggerReboot** te klikken, **debug**te selecteren en vervolgens **nieuwe instantie starten**te selecteren. Als het goed is, wordt ' opnieuw opstarten ' weer geven. geschreven in de **SimulatedManagedDevice** -console en de gerapporteerde eigenschappen van het apparaat, waaronder de laatste keer opnieuw opstarten, geschreven in de **TriggerReboot** -console.
   
    ![App voor service en apparaat uitvoeren](./media/iot-hub-csharp-csharp-device-management-get-started/combinedrun.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
