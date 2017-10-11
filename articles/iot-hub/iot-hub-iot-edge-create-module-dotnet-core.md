---
title: Een Azure IoT-Edge-Module maken met C# | Microsoft Docs
description: Deze zelfstudie gepresenteerd het schrijven van een Aanmeldingsprompt gegevens converter module met behulp van de meest recente Azure IoT rand NuGet-pakketten, Visual Studio Code en C#.
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: Azure iot, zelfstudie, module, nuget, vscode, csharp, rand
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.openlocfilehash: 7175ffc8de2c043593d61143b402484d33e4a8cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a>Een Azure IoT-Edge-Module maken met C & #x23;

Deze zelfstudie voor het maken van een module voor gepresenteerd `Azure IoT Edge` met `Visual Studio Code` en `C#`.

In deze zelfstudie we doorlopen omgeving instellen en het schrijven van een [uitschakelen](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) gegevens converter module met behulp van de meest recente `Azure IoT Edge NuGet` pakketten. 

>[!NOTE]
Deze zelfstudie maakt gebruik van de `.NET Core SDK`, die ondersteuning biedt voor meerdere platforms compatibiliteit. De volgende zelfstudie is geschreven met behulp van de `Windows 10` besturingssysteem. Sommige van de opdrachten in deze zelfstudie kan afwijken afhankelijk van uw `development environment`. 

## <a name="prerequisites"></a>Vereisten

In deze sectie we set-up uw omgeving voor `Azure IoT Edge` module ontwikkeling. Van toepassing op beide **64-bits Windows** en **64-bits Linux (8 Ubuntu/Debian)** besturingssystemen.

De volgende software is vereist:

- [GIT-Client](https://git-scm.com/downloads)
- [.NET Core-SDK](https://www.microsoft.com/net/core#windowscmd)
- [Visual Studio Code](https://code.visualstudio.com/)

U hoeft niet voor het klonen van de opslagplaats voor dit voorbeeld, maar alle van de voorbeeldcode besproken in deze zelfstudie bevindt zich in de volgende opslagplaats:

- `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a>Aan de slag

1. Installeer `.NET Core SDK`.
2. Installeer `Visual Studio Code` en de `C# extension` vanuit Visual Studio Code Marketplace.

Bekijk waarvoor [snelle zelfstudie](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) over het aan de slag met `Visual Studio Code` en de `.NET Core SDK`.

## <a name="creating-the-azure-iot-edge-converter-module"></a>Maken van de rand van Azure IoT converter-module

1. Een nieuwe initialisatie `.NET Core` class library C#-project:
    - Open een opdrachtprompt (`Windows + R` -> `cmd` -> `enter`).
    - Navigeer naar de map waarin u wilt maken van de `C#` project.
    - Type **dotnet nieuwe classlib -o IoTEdgeConverterModule -f netstandard1.3**. 
    - Deze opdracht maakt u een lege klasse aangeroepen `Class1.cs` in uw projectdirectory.
2. Navigeer naar de map waar we zojuist hebben gemaakt de class library-project door te typen **cd IoTEdgeConverterModule**.
3. Open het project in `Visual Studio Code` door **code.**.
4. Nadat het project is geopend `Visual Studio Code`, klikt u op de **IoTEdgeConverterModule.csproj** het bestand te openen zoals weergegeven in de volgende afbeelding:

    ![Venster van Visual Studio Code bewerken](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. Plaats de `XML` blob wordt weergegeven in het volgende codefragment tussen de afsluitende `PropertyGroup` code en de afsluitcode `Project` tag; regel zes in de voorgaande afbeelding en sla het bestand door te drukken `Ctrl`  +  `S`.

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. Nadat u hebt opgeslagen de `.csproj` bestand `Visual Studio Code` moet worden gevraagd met een `unresolved dependencies` dialoogvenster zoals te zien is in de volgende afbeelding: 

    ![Visual Studio Code terugzetten afhankelijkheden dialoogvenster](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    een) Klik op `Restore` alle verwijzingen naar de in de projecten te herstellen `.csproj` bestand inclusief de `PackageReferences` toegevoegd. 

    b) `Visual Studio Code` maakt automatisch het `project.assets.json` bestand in uw projecten `obj` map. Dit bestand bevat informatie over de afhankelijkheden van uw project om toekomstige herstelbewerkingen sneller.
 
    >[!NOTE]
    `.NET Core Tools`zijn nu MSBuild-systemen. Wat betekent dat een `.csproj` projectbestand is gemaakt in plaats van een `project.json`.

    - Als `Visual Studio Code` vraagt u die in orde is niet kunt we dit handmatig doen. Open de `Visual Studio Code` geïntegreerde terminalvenster door op de `Ctrl`  +  `backtick` sleutels of via de menu's `View`  ->  `Integrated Terminal`.
    - In de `Integrated Terminal` venstertype **dotnet terugzetten**.
    
7. Wijzig de naam van de `Class1.cs` van het bestand in `BleConverterModule.cs`. 

    a) te Wijzig de naam van het bestand eerst Klik op het bestand vervolgens drukt u op de `F2` sleutel.
    
    b) type in de nieuwe naam **BleConverterModule**, zoals in de volgende afbeelding:

    ![Visual Studio Code naam van een klasse](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. Vervang de bestaande code in de `BleConverterModule.cs` bestand kopiëren en plakken van het volgende codefragment in uw `BleConverterModule.cs` bestand.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. Sla het bestand door te drukken `Ctrl`  +  `S`.

10. Maak een nieuw bestand genaamd `Untitled-1` door op de `Ctrl`  +  `N` sleutels zoals te zien is in de volgende afbeelding:

    ![Visual Studio Code nieuw bestand](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. Deserialiseren van de `JSON` -object dat we van de gesimuleerde ontvangen `BLE` apparaat, Kopieer de volgende code in de `Untitled-1` venster bestand code-editor. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. Sla het bestand als `BleData.cs` door te drukken `Ctrl`  +  `Shift`  +  `S` sleutels.
    - In het dialoogvenster Opslaan als, in de `Save as Type` Selecteer vervolgkeuzemenu `C# (*.cs;*.csx)` zoals te zien is in de volgende afbeelding:

    ![Visual Studio Code opslaan als een dialoogvenster](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. Maak een nieuw bestand genaamd `Untitled-1` door op de `Ctrl`  +  `N` sleutels.

14. Kopieer en plak het volgende codefragment in de `Untitled-1` bestand. Deze klasse is een `Azure IoT Edge` module waarmee we gebruiken de gegevens ontvangen van onze `BleConverterModule`.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. Sla het bestand als `DotNetPrinterModule.cs` door te drukken `Ctrl`  +  `Shift`  +  `S`.
    - In het dialoogvenster Opslaan als, in de `Save as Type` vervolgkeuzemenu Selecteer `C# (*.cs;*.csx)`.

16. Maak een nieuw bestand genaamd `Untitled-1` door op de `Ctrl`  +  `N` sleutels.

17. Deserialiseren van de `JSON` -object dat we van ontvangen de `BleConverterModule`, kopiëren en plak de volgende code codefragment in de `Untitled-1` bestand. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. Sla het bestand als `BleConverterData.cs` door te drukken `Ctrl`  +  `Shift`  +  `S`.
    - In het dialoogvenster Opslaan als, in de `Save as Type` vervolgkeuzemenu Selecteer `C# (*.cs;*.csx)`.

19. Maak een nieuw bestand genaamd `Untitled-1` door op de `Ctrl`  +  `N` sleutels.

20. Kopieer en plak het volgende codefragment in de `Untitled-1` bestand.

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. Sla het bestand als `gw-config.json` door te drukken `Ctrl`  +  `Shift`  +  `S`.
    - In het dialoogvenster Opslaan als, in de `Save as Type` vervolgkeuzemenu Selecteer `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.

22. Om in te schakelen van het configuratiebestand kopiëren naar de uitvoermap, werken de `IoTEdgeConverterModule.csproj` met de volgende XML-blob:

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - De bijgewerkte `IoTEdgeConverterModule.csproj` moet eruitzien als in de volgende afbeelding:

    ![Visual Studio Code bijgewerkt .csproj-bestand](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. Maak een nieuw bestand genaamd `Untitled-1` door op de `Ctrl`  +  `N` sleutels.

24. Kopieer en plak het volgende codefragment in de `Untitled-1` bestand.

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. Sla het bestand als `binplace.ps1` door te drukken `Ctrl`  +  `Shift`  +  `S`.
    - In het dialoogvenster Opslaan als, in de `Save as Type` vervolgkeuzemenu Selecteer `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.

26. Bouw het project door op de `Ctrl`  +  `Shift`  +  `B` sleutels. Als u het project voor het eerst bouwen `Visual Studio Code` vraagt u met de `No build task defined.` dialoogvenster zoals te zien is in de volgende afbeelding:

    ![Dialoogvenster voor Visual Studio Code build-taak](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    een) Klik op de `Configure Build Task` knop.

    b) in de `Select a Task Runner` vervolgkeuzemenu in het dialoogvenster. Selecteer `.NET Core` zoals te zien is in de volgende afbeelding: 

    ![Visual Studio Code selecteren een dialoogvenster taak](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    c) op de `.NET Core` artikel maakt de `tasks.json` bestand uw `.vscode` directory en opent u het bestand in de `code editor` venster. Er is niet nodig om te wijzigen van dit bestand, het tabblad sluit.

27.  Open de `Visual Studio Code` geïntegreerde terminalvenster door op de `Ctrl`  +  `backtick` sleutels of via de menu's `View`  ->  `Integrated Terminal` en het type **.\binplace.ps1** in de `PowerShell` opdrachtprompt. Met deze opdracht wordt alle afhankelijkheden van onze gekopieerd naar de uitvoermap.

28. Navigeer naar de uitvoermap projecten in de `Integrated Terminal` venster door op te geven **cd.\bin\Debug\netstandard1.3**.

29. Het voorbeeldproject uitvoeren door te typen **. \gw.exe gw-config.json** in de `Integrated Terminal` venster prompt. 
    - Als u nauw de stappen in deze zelfstudie hebt gevolgd, u moet nu worden uitgevoerd de `Azure IoT Edge BLE Data Converter Module` voorbeeldproject zoals te zien is in de volgende afbeelding:
    
        ![Voorbeeld van het gesimuleerde apparaat uitgevoerd in Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - Als u wilt dat de toepassing beëindigt, drukt u op de `<Enter>` sleutel.

>[!IMPORTANT]
Het wordt niet aangeraden om te gebruiken `Ctrl`  +  `C` worden beëindigd of dat de `IoT Edge` application gateway (dat wil zeggen, **gw.exe**). Als u deze actie kan ertoe leiden dat het proces abnormaal beëindigd.

