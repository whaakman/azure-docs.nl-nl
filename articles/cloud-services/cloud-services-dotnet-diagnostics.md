---
title: Het gebruik van Azure diagnostics (.NET) met Cloudservices | Microsoft Docs
description: Met behulp van Azure diagnostics voor het verzamelen van gegevens van Azure-cloud-Services voor foutopsporing, prestaties, bewaking, analyse van het netwerkverkeer en meer meten.
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 89623a0e-4e78-4b67-a446-7d19a35a44be
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: robb
ms.openlocfilehash: 333d2f26ce043a167fb84858c8327cb39e868ffa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Inschakelen van Azure Diagnostics in Azure-Cloudservices
Zie [overzicht van Azure Diagnostics](../azure-diagnostics.md) voor de achtergrond op Azure Diagnostics.

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Het inschakelen van diagnostische gegevens in een Werkrol
Dit scenario wordt beschreven hoe u een Azure-werkrol die verzendt met behulp van de klasse .NET EventSource telemetriegegevens implementeert. Azure Diagnostics wordt gebruikt om de telemetriegegevens verzamelen en opslaan in een Azure storage-account. Bij het maken van een werkrol kunt Visual Studio automatisch de Diagnostics 1.0 als onderdeel van de oplossing in Azure SDK voor .NET 2.4 en eerdere versies. De volgende instructies beschrijven het proces voor het maken van de werkrol Diagnostics 1.0 uitschakelen van de oplossing en het implementeren van diagnostische gegevens 1.2 of 1.3 aan uw werkrol.

### <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt een Azure-abonnement en gebruik van Visual Studio met de Azure SDK. Als u niet een Azure-abonnement hebt, kunt u zich aanmelden voor de [gratis proefversie][Free Trial]. Zorg ervoor dat u [installeren en configureren van Azure PowerShell versie 0.8.7 of hoger][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-worker-role"></a>Stap 1: Een Werkrol maken
1. Start **Visual Studio**.
2. Maak een **Azure Cloud Service** project uit de **Cloud** sjabloon die gericht is op .NET Framework 4.5.  Noem het project 'WadExample' en klik op Ok.
3. Selecteer **Werkrol** en klik op Ok. Het project wordt gemaakt.
4. In **Solution Explorer**, dubbelklikt u op de **WorkerRole1** eigenschappenbestand.
5. In de **configuratie** tabblad un selectievakje **diagnostische gegevens inschakelen** uitschakelen Diagnostics 1.0 (Azure SDK 2.4 en eerder).
6. Het bouwen van uw oplossing om te controleren of er geen fouten.

### <a name="step-2-instrument-your-code"></a>Stap 2: Uw code softwareontwikkelaars
De inhoud van WorkerRole.cs vervangen door de volgende code. De klasse SampleEventSourceWriter, overgenomen van de [EventSource klasse][EventSource Class], implementeert vier logboekregistratiemethoden: **SendEnums**, **MessageMethod**, **SetOther** en **HighFreq**. De eerste parameter voor de **WriteEvent** methode definieert u de ID voor de betreffende gebeurtenis. De methode Run implementeert een oneindige lus die voor elk van de logboekregistratiemethoden geïmplementeerd aanroept in de **SampleEventSourceWriter** klasse elke 10 seconden.

```csharp
using Microsoft.WindowsAzure.ServiceRuntime;
using System;
using System.Diagnostics;
using System.Diagnostics.Tracing;
using System.Net;
using System.Threading;

namespace WorkerRole1
{
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a>Stap 3: Uw Werkrol implementeren

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. Uw werkrol door te selecteren in Azure uit vanuit Visual Studio implementeert het **WadExample** project in Solution Explorer vervolgens **publiceren** van de **bouwen** menu.
2. Kies uw abonnement.
3. In de **Microsoft Azure Publish Settings** dialoogvenster Selecteer **nieuwe maken...** .
4. In de **Cloudservice maken en Storage-Account** dialoogvenster, voer een **naam** (bijvoorbeeld ' WadExample') en selecteer een regio of affiniteitsgroep.
5. Stel de **omgeving** naar **fasering**.
6. Wijzigen van een andere **instellingen** als in de betreffende en klikt u op **publiceren**.
7. Wanneer implementatie is voltooid, controleert u of in de Azure portal dat uw service in de cloud zich in een **met** status.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Stap 4: Uw configuratiebestand diagnostische gegevens maken en de uitbreiding installeren
1. De schemadefinitie van de openbare configuratie bestand downloaden door het uitvoeren van de volgende PowerShell-opdracht:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. Toevoegen van een XML-bestand naar uw **WorkerRole1** project door met de rechtermuisknop op de **WorkerRole1** project en selecteer **toevoegen** -> **Nieuw Item...** -> **Visual C# items** -> **gegevens** -> **XML-bestand**. Noem het bestand 'WadExample.xml'.

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. De WadConfig.xsd koppelen aan het configuratiebestand. Zorg ervoor dat de editor voor WadExample.xml het actieve venster. Druk op **F4** openen de **eigenschappen** venster. Klik op de **schema's** eigenschap in de **eigenschappen** venster. Klik op de **...** in de **schema's** eigenschap. Klik op de knop **Toevoegen...** knop en navigeer naar de locatie waar u het XSD-bestand hebt opgeslagen en selecteert u het bestand WadConfig.xsd. Klik op **OK**.

4. De inhoud van het configuratiebestand WadExample.xml vervangen door de volgende XML en sla het bestand. Dit configuratiebestand definieert een aantal prestatiemeteritems voor het verzamelen van: één voor CPU-gebruik en één voor geheugengebruik. De configuratie definieert de vier gebeurtenissen die overeenkomt met de methoden in de klasse SampleEventSourceWriter.

```xml
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <Event id="2" eventDestination="MessageTable"/>
          <Event id="3" eventDestination="SetOtherTable"/>
          <Event id="4" eventDestination="HighFreqTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Stap 5: Diagnostische gegevens op uw Werkrol installeren
De PowerShell-cmdlets voor het beheren van diagnostische gegevens op een web- of worker-rol zijn: Set-AzureServiceDiagnosticsExtension en Get-AzureServiceDiagnosticsExtension verwijderen AzureServiceDiagnosticsExtension.

1. Open Azure PowerShell.
2. Voer het script voor het installeren van diagnostische gegevens op uw werkrol (Vervang *StorageAccountKey* door de sleutel van het opslagaccount voor uw opslagaccount wadexample en *config_path* met het pad naar de *WadExample.xml* bestand):

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Stap 6: Bekijk de telemetrische gegevens
In de Visual Studio **Server Explorer**, gaat u naar het opslagaccount wadexample. Nadat de cloudservice actief is geweest ongeveer vijf (5) minuten, ziet u de tabellen **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** en **WADSetOtherTable**. Dubbelklik op een van de tabellen om weer te geven van de telemetrie die is verzameld.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Schema van het configuratiebestand
Het configuratiebestand van de diagnostische gegevens definieert waarden die worden gebruikt voor het initialiseren van diagnostische configuratie-instellingen wanneer de diagnostics-agent wordt gestart. Zie de [nieuwste schemaverwijzing](https://msdn.microsoft.com/library/azure/mt634524.aspx) voor geldige waarden en voorbeelden.

## <a name="troubleshooting"></a>Problemen oplossen
Als u problemen ondervindt, raadpleegt u [probleemoplossing Azure Diagnostics](../azure-diagnostics-troubleshooting.md) voor meer informatie over bekende problemen.

## <a name="next-steps"></a>Volgende stappen
[Een overzicht van verwante Azure virtuele machines diagnostische artikelen](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) wijzigen van de gegevens die u verzamelt, oplossen van problemen of meer informatie over diagnostische gegevens over het algemeen.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
