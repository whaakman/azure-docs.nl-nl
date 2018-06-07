---
title: Processtappen verbinden een Pi frambozen aan uw Azure IoT centrale toepassing (C#) | Microsoft Docs
description: Als een ontwikkelaar van het apparaat een Pi frambozen verbinding met uw Azure IoT centrale toepassing met C#.
author: dominicbetts
ms.author: dobett
ms.date: 01/22/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 58f363c522f3e5abe6bf49a2aebafe4e953e00df
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628586"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>Verbinding maken met een Pi frambozen aan uw Azure IoT centrale toepassing (C#)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Dit artikel wordt beschreven hoe u, als een ontwikkelaar apparaat een Pi frambozen verbinding met uw Microsoft Azure IoT centrale toepassing met de programmeertaal C#.

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om de stappen in dit artikel uit te voeren:

* [.NET core 2](https://www.microsoft.com/net) geïnstalleerd op uw ontwikkelcomputer. U moet ook een geschikte code-editor, zoals hebben [Visual Studio Code](https://code.visualstudio.com/).
* Een Azure IoT centrale toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon. Zie voor meer informatie [maken van uw Azure IoT centrale toepassing](howto-create-application.md).
* Een frambozen Pi-apparaat waarop het besturingssysteem Raspbian.

Een toepassing gemaakt op basis van de **voorbeeld Devkits** toepassingssjabloon bevat een **frambozen Pi** apparaat sjabloon met de volgende kenmerken:

### <a name="telemetry-measurements"></a>Telemetrie metingen

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| Vochtigheid       | %      | 0       | 100     | 0              |
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
| Spanning      | setVoltage | V | 0              | 0       | 240     | 0       |
| Huidige      | setCurrent | Stroom  | 0              | 0       | 100     | 0       |
| Snelheid van ventilator    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Instellingen voor in-of uitschakelen

| Weergavenaam | Veldnaam | Op tekst | Uit tekst | Oorspronkelijk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AAN      | UIT      | Uit     |

### <a name="properties"></a>Eigenschappen

| Type            | Weergavenaam | Veldnaam | Gegevenstype |
| --------------- | ------------ | ---------- | --------- |
| Apparaateigenschap | Aantal die   | dieNumber  | getal    |
| Tekst            | Locatie     | location   | N/A       |

### <a name="add-a-real-device"></a>Echt apparaat toevoegen

In uw Azure IoT centrale toepassing toevoegen een echte apparaat van de **frambozen Pi** apparaat sjabloon en maak een notitie van de verbindingsreeks van het apparaat. Zie voor meer informatie [een echte apparaat toevoegt aan uw Azure IoT centrale toepassing](tutorial-add-device.md).

## <a name="create-your-net-application"></a>Uw .NET-toepassing maken

U maakt en test de apparaattoepassing op uw computer.

De volgende stappen uit te voeren, kunt u Visual Studio Code. Zie voor meer informatie [werken met C#](https://code.visualstudio.com/docs/languages/csharp).

> [!NOTE]
> Als u liever, kunt u de volgende stappen uit met een andere code-editor uitvoeren.

1. Voor het initialiseren van uw .NET-project en voeg de vereiste NuGet-pakketten, voer de volgende opdrachten:

  ```cmd/sh
  mkdir pisample
  cd pisample
  dotnet new console
  dotnet add package Microsoft.Azure.Devices.Client
  dotnet restore
  ```

1. Open de `pisample` map in Visual Studio Code. Open vervolgens de **pisample.csproj** projectbestand. Voeg de `<RuntimeIdentifiers>` label wordt weergegeven in het volgende fragment:

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
    > Uw **Microsoft.Azure.Devices.Client** versienummer pakket mag niet hoger zijn dan wordt weergegeven.

1. Sla **pisample.csproj**. Als Visual Studio Code u de opdracht restore uitvoeren vraagt, kiest u **herstellen**.

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
    > Bijwerken van de tijdelijke aanduiding voor `{your device connection string}` in de volgende stap.

## <a name="run-your-net-application"></a>Voer uw .NET-toepassing

De apparaat-specifieke verbindingsreeks toevoegen aan de code voor het apparaat te verifiëren met Azure IoT centrale. U hebt deze verbindingsreeks wanneer u uw echte apparaat toegevoegd aan uw Azure IoT centrale toepassing.

1. Vervang `{your device connection string}` in de **Program.cs** bestand met de verbindingsreeks die u eerder hebt genoteerd.

1. Voer de volgende opdracht in uw omgeving opdrachtregel:

  ```cmd/sh
  dotnet restore
  dotnet publish -r linux-arm
  ```

1. Kopieer de `pisample\bin\Debug\netcoreapp2.0\linux-arm\publish` map op uw apparaat frambozen Pi. U kunt de **scp** opdracht Kopieer de bestanden, bijvoorbeeld:

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    Zie voor meer informatie [frambozen Pi RAS](https://www.raspberrypi.org/documentation/remote-access/).

1. Aanmelden bij uw apparaat frambozen Pi en voer de volgende opdrachten in een shell:

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. Voer de volgende opdrachten op uw Pi frambozen:

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![Wordt gestart](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. In de Azure IoT centrale toepassing, kunt u zien hoe de code die wordt uitgevoerd op de Pi frambozen samenwerkt met de toepassing:

    * Op de **metingen** pagina voor het echte apparaat ziet u de telemetrie.
    * Op de **eigenschappen** pagina ziet u de waarde van de gerapporteerde **Die nummer** eigenschap.
    * Op de **instellingen** pagina kunt u verschillende instellingen op de Pi frambozen zoals spanning en ventilator snelheid.

    De volgende schermafbeelding ziet de frambozen Pi ontvangen van de gewijzigde instelling:

    ![Raspberry Pi ontvangt instelling wijziging](./media/howto-connect-raspberry-pi-csharp/device_switch.png)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een Pi frambozen verbinding te maken met uw Azure IoT centrale toepassing, vindt hier u de voorgestelde volgende stappen uit:

* [Verbinding maken met een algemene Node.js-clienttoepassing naar Azure IoT Central](howto-connect-nodejs.md)