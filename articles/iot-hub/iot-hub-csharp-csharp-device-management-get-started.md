---
title: Aan de slag met Azure IoT Hub device management (.NET/.NET) | Microsoft Docs
description: Het gebruik van Apparaatbeheer via Azure IoT Hub om te beginnen met een extern apparaat opnieuw opstarten. De Azure IoT-device-SDK voor .NET kunt u implementeert een gesimuleerde apparaat-app met een rechtstreekse methode en de Azure IoT service SDK voor .NET voor het implementeren van een service-app die de directe methode aanroept.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: dobett
ms.openlocfilehash: fd26fb9e45f8790342472fa0bae92f74bea2fc70
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242452"
---
# <a name="get-started-with-device-management-netnet"></a>Aan de slag met Apparaatbeheer (.NET/.NET)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze zelfstudie ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maken in uw IoT-hub.

* Maak een gesimuleerde apparaat-app met een rechtstreekse methode die het apparaat opnieuw wordt opgestart. Directe methoden zijn aangeroepen vanuit de cloud.

* Maak een .NET-consoletoepassing die de directe methode voor opnieuw opstarten in de gesimuleerde apparaattoepassing via uw IoT-hub roept.

Aan het eind van deze zelfstudie beschikt u over twee .NET-consoletoepassingen:

* **SimulateManagedDevice**, deze toepassing koppelt uw IoT-hub aan de apparaat-id die eerder hebt gemaakt, ontvangt u een rechtstreekse methode voor opnieuw opstarten, simuleert fysieke opnieuw worden opgestart en rapporten van de tijd voor de laatste keer opnieuw opstarten.

* **TriggerReboot**, die een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app, wordt het antwoord weergegeven en wordt de bijgewerkte gerapporteerde eigenschappen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2017.

* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een extern opnieuw opstarten op het apparaat met een rechtstreekse methode activeren

In deze sectie maakt u een .NET-console-app (met behulp van C#) die een extern opnieuw opstarten op een apparaat met een rechtstreekse methode initieert. De app maakt gebruik van apparaatdubbel-query's voor het detecteren van de laatste keer opnieuw opstarten voor dat apparaat.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de nieuwe oplossing met behulp van de projectsjabloon **Console App (.NET Framework)**. Zorg ervoor dat de versie van .NET Framework minimaal 4.5.1 is. Noem het project **TriggerReboot**.

    ![Nieuw Windows Classic Desktop-project in Visual C#](./media/iot-hub-csharp-csharp-device-management-get-started/createserviceapp.png)

2. Klik in Solution Explorer met de rechtermuisknop op de **TriggerReboot** project en klik vervolgens op **NuGet-pakketten beheren**.

3. In de **NuGet Package Manager** venster **Bladeren**, zoeken naar **Microsoft.Azure.Devices**, selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices** verpakt en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT service SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet-pakket en de bijbehorende afhankelijkheden.

    ![Sluit het venster Nuget Package Manager.](./media/iot-hub-csharp-csharp-device-management-get-started/servicesdknuget.png)

4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Shared;
   ```
        
5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de sectie 'Een IoT-hub maken'. 
   
   ```csharp
   static RegistryManager registryManager;
   static string connString = "{iot hub connection string}";
   static ServiceClient client;
   static string targetDevice = "myDeviceId";
   ```

6. Voeg de volgende methode toe aan de klasse **Program**.  Deze code ophaalt van het dubbele apparaat voor het apparaat opnieuw wordt opgestart en de gerapporteerde eigenschappen weergeeft.
   
   ```csharp
   public static async Task QueryTwinRebootReported()
   {
       Twin twin = await registryManager.GetTwinAsync(targetDevice);
       Console.WriteLine(twin.Properties.Reported.ToJson());
   }
   ```
        
7. Voeg de volgende methode toe aan de klasse **Program**.  Deze code wordt het opnieuw opstarten op het apparaat met een rechtstreekse methode gestart.

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
> In deze zelfstudie voert een eenvoudige query uitvoeren voor de gerapporteerde eigenschappen van het apparaat. Bij de productiecode aangeraden polling voor het detecteren van wijzigingen in de gerapporteerde eigenschappen.

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie gaat u het volgende doen:

* Maak een .NET-consoletoepassing die op een rechtstreekse methode aangeroepen door de cloud reageert.

* Activeert een gesimuleerde apparaat opnieuw opstarten.

* Gebruik de gerapporteerde eigenschappen om in te schakelen apparaatdubbel-query's voor het identificeren van apparaten en wanneer ze het laatst zijn opgestart.

1. Voeg in Visual Studio een Visual C# Classic Windows Desktop-project toe aan de huidige oplossing met behulp van de projectsjabloon **Console Application**. Noem het project **SimulateManagedDevice**.
   
    ![Nieuwe Visual C# Windows klassieke apparaat-app](./media/iot-hub-csharp-csharp-device-management-get-started/createdeviceapp.png)
    
2. Klik in Solution Explorer met de rechtermuisknop op de **SimulateManagedDevice** project en klik vervolgens op **NuGet-pakketten beheren...** .

3. In de **NuGet Package Manager** venster **Bladeren** en zoek naar de **Microsoft.Azure.Devices.Client**. Selecteer **installeren** voor het installeren van de **Microsoft.Azure.Devices.Client** verpakt en accepteer de gebruiksvoorwaarden. Deze procedure downloadt, installeert en voegt u een verwijzing naar de [Azure IoT device-SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet-pakket en de bijbehorende afhankelijkheden.
   
    ![NuGet-Pakketbeheer venster Client-app](./media/iot-hub-csharp-csharp-device-management-get-started/clientsdknuget.png)
    
4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Voeg de volgende velden toe aan de klasse **Program**: Vervang de tijdelijke aanduidingswaarde met de apparaatverbindingsreeks die u in de vorige sectie hebt genoteerd.

    ```csharp
    static string DeviceConnectionString = 
      "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```
6. Voeg het volgende voor het implementeren van de directe methode die op het apparaat:

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

7. Voeg de volgende code om de **Main** methode voor het openen van de verbinding met uw IoT-hub en de methode-listener initialiseren:

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
        
8. Klik in Solution Explorer van Visual Studio met de rechtermuisknop op uw oplossing en klik op **Set StartUp Projects...**. Selecteer **Single startup project**, en selecteer vervolgens de **SimulateManagedDevice** -project in de vervolgkeuzelijst. Bouw de oplossing.       

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Om uit te voeren van de .NET-app voor Apparaatbeheer **SimulateManagedDevice**. Met de rechtermuisknop op de **SimulateManagedDevice** project, selecteer **Debug**, en selecteer vervolgens **nieuwe instantie starten**. Deze moet beginnen met het luisteren naar methodeaanroepen vanuit uw IoT-hub. 

2. Nu dat het apparaat is verbonden en methodeaanroepen, wachten van de .NET uitvoeren **TriggerReboot** app de methode voor opnieuw opstarten in het gesimuleerde apparaat-app aan te roepen. U doet dit door met de rechtermuisknop op de **TriggerReboot** project, selecteer **Debug**, en selecteer vervolgens **nieuwe instantie starten**. U ziet "Opnieuw opstarten!" die zijn geschreven in de **SimulatedManagedDevice** -console en de gerapporteerde eigenschappen van het apparaat, waaronder de laatste opnieuw opstarten tijd, die zijn geschreven in de **TriggerReboot** console.
   
    ![Service- en apparaat-app uitvoeren](./media/iot-hub-csharp-csharp-device-management-get-started/combinedrun.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
