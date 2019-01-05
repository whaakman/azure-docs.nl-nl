---
title: Verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing (C#) | Microsoft Docs
description: Als ontwikkelaar in een apparaat, hoe u verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing met behulp van C#.
author: viv-liu
ms.author: viviali
ms.date: 10/31/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: f0232c8d2627cd600f4f05b5b501db85fa7d2ec4
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051387"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>Verbinding maken met een Raspberry Pi aan uw Azure IoT Central-toepassing (C#)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Dit artikel wordt beschreven hoe u, als een apparaat-ontwikkelaar, een Raspberry Pi verbinden met uw Microsoft Azure IoT Central-toepassing met de C#-programmeertaal.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in dit artikel hebt voltooid, moet u de volgende onderdelen:

* [.NET core 2](https://www.microsoft.com/net) geïnstalleerd op uw ontwikkelcomputer. U moet ook een geschikte code-editor, zoals hebben [Visual Studio Code](https://code.visualstudio.com/).
* Een Azure IoT Central-toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
* Een Raspberry Pi-apparaat met het besturingssysteem Raspbian.


## <a name="sample-devkits-application"></a>**Voorbeeld van een Devkits** toepassing

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Raspberry Pi** apparaat sjabloon met de volgende kenmerken: 

- Telemetrie, waaronder de volgende metingen die het apparaat wordt verzameld:
    - Vochtigheid
    - Temperatuur
    - Druk
    - Magnetometer (X, Y, Z)
    - Versnellingsmeter (X, Y, Z)
    - Gyroscoop (X, Y, Z)
- Instellingen
    - Voltage
    - Huidige
    - Snelheid van ventilator
    - IR in-/ uitschakelen.
- Properties
    - Aantal apparaateigenschappen die
    - Cloud-locatie-eigenschap

Raadpleeg voor volledige informatie van de configuratie van de sjabloon apparaat [Raspberry PI Apparaatdetails sjabloon](howto-connect-raspberry-pi-csharp.md#raspberry-pi-device-template-details)


## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Voeg in uw Azure IoT Central-toepassing een echt apparaat uit de **Raspberry Pi** apparaat sjabloon en maak een notitie van de verbindingsreeks van het apparaat. Zie voor meer informatie, [een echt apparaat toevoegen aan uw Azure IoT Central toepassing](tutorial-add-device.md).

### <a name="create-your-net-application"></a>Uw .NET-toepassing maken

U maakt en de apparaattoepassing testen op uw computer.

Als u wilt de volgende stappen hebt voltooid, kunt u Visual Studio Code. Zie voor meer informatie, [werken met C#](https://code.visualstudio.com/docs/languages/csharp).

> [!NOTE]
> Als u liever, kunt u de volgende stappen met behulp van een andere code-editor.

1. Voor het initialiseren van uw .NET-project en voeg de vereiste NuGet-pakketten toe, voer de volgende opdrachten:

  ```cmd/sh
  mkdir pisample
  cd pisample
  dotnet new console
  dotnet add package Microsoft.Azure.Devices.Client
  dotnet restore
  ```

1. Open de `pisample` map in Visual Studio Code. Open vervolgens de **pisample.csproj** projectbestand. Voeg de `<RuntimeIdentifiers>` label wordt weergegeven in het volgende codefragment:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifiers>win-arm;linux-arm</RuntimeIdentifiers>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.5.2" />
      </ItemGroup>
    </Project>
    ```

    > [!NOTE]
    > Uw **Microsoft.Azure.Devices.Client** versienummer-pakket mag niet hoger zijn dan de architectuur die wordt weergegeven.

1. Sla **pisample.csproj**. Als u voor het uitvoeren van de opdracht restore Visual Studio Code wordt gevraagd, kiest u **herstellen**.

1. Open **Program.cs** en vervang de inhoud door de volgende code:

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;

    namespace pisample
    {
      class Program
      {
        static string DeviceConnectionString = "{your device connection string}";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();
        static CancellationTokenSource cts;
        static double baseTemperature = 60;
        static double basePressure = 500;
        static double baseHumidity = 50;
        static void Main(string[] args)
        {
          Console.WriteLine("Raspberry Pi Azure IoT Central example");

          try
          {
            InitClient();
            SendDeviceProperties();

            cts = new CancellationTokenSource();
            SendTelemetryAsync(cts.Token);

            Console.WriteLine("Wait for settings update...");
            Client.SetDesiredPropertyUpdateCallbackAsync(HandleSettingChanged, null).Wait();
            Console.ReadKey();
            cts.Cancel();
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static void InitClient()
        {
          try
          {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static async void SendDeviceProperties()
        {
          try
          {
            Console.WriteLine("Sending device properties:");
            Random random = new Random();
            TwinCollection telemetryConfig = new TwinCollection();
            reportedProperties["dieNumber"] = random.Next(1, 6);
            Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static async void SendTelemetryAsync(CancellationToken token)
        {
          try
          {
            Random rand = new Random();

            while (true)
            {
              double currentTemperature = baseTemperature + rand.NextDouble() * 20;
              double currentPressure = basePressure + rand.NextDouble() * 100;
              double currentHumidity = baseHumidity + rand.NextDouble() * 20;

              var telemetryDataPoint = new
              {
                humidity = currentHumidity,
                pressure = currentPressure,
                temp = currentTemperature
              };
              var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
              var message = new Message(Encoding.ASCII.GetBytes(messageString));

              token.ThrowIfCancellationRequested();
              await Client.SendEventAsync(message);

              Console.WriteLine("{0} > Sending telemetry: {1}", DateTime.Now, messageString);

              await Task.Delay(1000);
            }
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Intentional shutdown: {0}", ex.Message);
          }
        }

        private static async Task HandleSettingChanged(TwinCollection desiredProperties, object userContext)
        {
          try
          {
            Console.WriteLine("Received settings change...");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            string setting = "fanSpeed";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setVoltage";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setCurrent";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "activateIR";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }

          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static void AcknowledgeSettingChange(TwinCollection desiredProperties, string setting)
        {
          reportedProperties[setting] = new
          {
            value = desiredProperties[setting]["value"],
            status = "completed",
            desiredVersion = desiredProperties["$version"],
            message = "Processed"
          };
        }
      }
    }
    ```

    > [!NOTE]
    > Bijwerken van de tijdelijke aanduiding `{your device connection string}` in de volgende stap.

## <a name="run-your-net-application"></a>Uw .NET-toepassing uitvoeren

Uw apparaat-specifieke verbindingsreeks toevoegen aan de code voor het apparaat om te verifiëren met Azure IoT Central. U hebt deze verbindingsreeks wanneer u uw echt apparaat toegevoegd aan uw Azure IoT Central-toepassing.

  > [!NOTE]
   > Azure IoT Central is overgeschakeld naar het gebruik van Azure IoT Hub Device Provisioning service (DPS) voor alle apparaatverbindingen, volgt u deze instrustions naar [de apparaat-verbindingsreeks ophalen](concepts-connectivity.md#getting-device-connection-string) en Ga door met de rest van de zelfstudie.

1. Vervang `{your device connection string}` in de **Program.cs** -bestand met de verbindingsreeks die u eerder hebt genoteerd.

1. Voer de volgende opdracht in uw omgeving opdrachtregel:

  ```cmd/sh
  dotnet restore
  dotnet publish -r linux-arm
  ```

1. Kopieer de `pisample\bin\Debug\netcoreapp2.0\linux-arm\publish` map op uw apparaat Raspberry Pi. U kunt de **scp** opdracht de bestanden te kopiëren, bijvoorbeeld:

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    Zie voor meer informatie, [Raspberry Pi RAS](https://www.raspberrypi.org/documentation/remote-access/).

1. Meld u aan met uw Raspberry Pi-apparaat en voer de volgende opdrachten in een shell:

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. Op uw Raspberry Pi, voer de volgende opdrachten:

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![Programma wordt gestart](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. U kunt zien hoe de code die wordt uitgevoerd op de Raspberry Pi communiceert met de toepassing in uw Azure IoT Central-toepassing:

    * Op de **metingen** pagina voor uw echte apparaten, kunt u de telemetrie bekijken.
    * Op de **eigenschappen** pagina, ziet u de waarde van de gerapporteerde **Die nummer** eigenschap.
    * Op de **instellingen** pagina, kunt u verschillende instellingen op de Raspberry Pi zoals spanning- en fan-snelheid.

    De volgende schermafbeelding ziet u de Raspberry Pi ontvangen van de wijziging in de instelling:

    ![Raspberry Pi ontvangt instelling wijzigen](./media/howto-connect-raspberry-pi-csharp/device_switch.png)


## <a name="raspberry-pi-device-template-details"></a>Details van de sjabloon Raspberry PI-apparaat

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **Raspberry Pi** apparaat sjabloon met de volgende kenmerken:

### <a name="telemetry-measurements"></a>Telemetrie-metingen

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| vochtigheid       | %      | 0       | 100     | 0              |
| TEMP           | ° C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | MDP 's   | -2000   | 2000    | 0              |
| gyroscopeY     | MDP 's   | -2000   | 2000    | 0              |
| gyroscopeZ     | MDP 's   | -2000   | 2000    | 0              |

### <a name="settings"></a>Instellingen

Numerieke instellingen

| Weergavenaam | Veldnaam | Eenheden | Aantal decimalen | Minimum | Maximum | Oorspronkelijk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltage      | setVoltage | Voltage | 0              | 0       | 240     | 0       |
| Huidige      | setCurrent | Stroom  | 0              | 0       | 100     | 0       |
| Snelheid van ventilator    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Instellingen voor in-/ uitschakelen

| Weergavenaam | Veldnaam | Van tekst | Uit tekst | Oorspronkelijk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AAN      | UIT      | Uit     |

### <a name="properties"></a>Properties

| Type            | Weergavenaam | Veldnaam | Gegevenstype |
| --------------- | ------------ | ---------- | --------- |
| Apparaateigenschappen | Aantal die   | dieNumber  | getal    |
| Tekst            | Locatie     | location   | N/A       |

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe een Raspberry Pi verbinden met uw Azure IoT Central-toepassing, vindt hier u de voorgestelde volgende stappen:

* [Verbinding maken met een algemene Node.js-clienttoepassing op Azure IoT Central](howto-connect-nodejs.md)
