---
title: Verzamelen van prestatiemeteritems in Azure-Cloudservices | Microsoft Docs
description: Informatie over het detecteren, gebruiken en prestatie-items maken in Cloud Services met Azure Diagnostics en Application Insights.
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/18
ms.author: adegeo
ms.openlocfilehash: 3e0af48c172fa912f0ac9e05b7b761dd7eaad795
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="collect-performance-counters-for-your-azure-cloud-service"></a>Prestatiemeteritems voor uw Azure Cloud Service verzamelen

Prestatiemeteritems kunnen u bijhouden hoe goed uw toepassing en de host worden uitgevoerd. Windows Server biedt veel verschillende prestatiemeteritems die betrekking hebben op hardware, toepassingen en het besturingssysteem. Verzamelen en verzenden van prestatiemeteritems naar Azure, kunt u deze informatie om u te helpen betere beslissingen analyseren. 

## <a name="discover-available-counters"></a>Detecteren van beschikbare items

Een prestatiemeteritem bestaat uit twee onderdelen: de setnaam van een (categorie) en een of meer items. U kunt PowerShell gebruiken om een lijst van beschikbare prestatiemeteritems:

```PowerShell
Get-Counter -ListSet * | Select-Object CounterSetName, Paths | Sort-Object CounterSetName

CounterSetName                                  Paths
--------------                                  -----
.NET CLR Data                                   {\.NET CLR Data(*)\SqlClient...
.NET CLR Exceptions                             {\.NET CLR Exceptions(*)\# o...
.NET CLR Interop                                {\.NET CLR Interop(*)\# of C...
.NET CLR Jit                                    {\.NET CLR Jit(*)\# of Metho...
.NET Data Provider for Oracle                   {\.NET Data Provider for Ora...
.NET Data Provider for SqlServer                {\.NET Data Provider for Sql...
.NET Memory Cache 4.0                           {\.NET Memory Cache 4.0(*)\C...
AppV Client Streamed Data Percentage            {\AppV Client Streamed Data ...
ASP.NET                                         {\ASP.NET\Application Restar...
ASP.NET Apps v4.0.30319                         {\ASP.NET Apps v4.0.30319(*)...
ASP.NET State Service                           {\ASP.NET State Service\Stat...
ASP.NET v2.0.50727                              {\ASP.NET v2.0.50727\Applica...
ASP.NET v4.0.30319                              {\ASP.NET v4.0.30319\Applica...
Authorization Manager Applications              {\Authorization Manager Appl...

#... results cut to save space ...
```

De `CounterSetName` eigenschap vertegenwoordigt een set (of een categorie) en is een goede indicatie van wat de prestatiemeteritems zijn gerelateerd aan. De `Paths` eigenschap vertegenwoordigt een verzameling items voor een set. U kunt ook krijgen de `Description` eigenschap voor meer informatie over de itemset.

Als u alle items voor een set, gebruikt de `CounterSetName` waarde en vouw de `Paths` verzameling. Elk item van het pad is een item dat u kunt een query. Bijvoorbeeld, om op te halen van de beschikbare items die betrekking hebben op de `Processor` instellen uit, vouw de `Paths` verzameling:

```PowerShell
Get-Counter -ListSet * | Where-Object CounterSetName -eq "Processor" | Select -ExpandProperty Paths

\Processor(*)\% Processor Time
\Processor(*)\% User Time
\Processor(*)\% Privileged Time
\Processor(*)\Interrupts/sec
\Processor(*)\% DPC Time
\Processor(*)\% Interrupt Time
\Processor(*)\DPCs Queued/sec
\Processor(*)\DPC Rate
\Processor(*)\% Idle Time
\Processor(*)\% C1 Time
\Processor(*)\% C2 Time
\Processor(*)\% C3 Time
\Processor(*)\C1 Transitions/sec
\Processor(*)\C2 Transitions/sec
\Processor(*)\C3 Transitions/sec
```

Deze afzonderlijke teller paden kunnen worden toegevoegd aan het framework diagnostische gegevens van uw cloudservice wordt gebruikt. Zie voor meer informatie over hoe een pad voor prestatiemeteritems is samengesteld, [geven een itempad](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85)).

## <a name="collect-a-performance-counter"></a>Een prestatiemeteritem verzamelen

Een prestatiemeteritem kan worden toegevoegd aan uw cloudservice voor Azure Diagnostics of Application Insights.

### <a name="application-insights"></a>Application Insights

Azure Application Insights voor Cloud-Services kunt dat u opgeven welke prestatiemeteritems die u wilt verzamelen. Nadat u [Application Insights toevoegen aan uw project](../application-insights/app-insights-cloudservices.md#sdk), een configuratiebestand met de naam **ApplicationInsights.config** wordt toegevoegd aan uw Visual Studio-project. Dit configuratiebestand definieert wat voor soort informatie Application Insights verzamelt en verstuurt naar Azure.

Open de **ApplicationInsights.config** bestand en zoek de **ApplicationInsights** > **TelemetryModules** element. Elke `<Add>` onderliggend element definieert een type telemetriegegevens te verzamelen, samen met de configuratie ervan. Het moduletype prestaties teller telemetrie is `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector`. Als dit element is al gedefinieerd, voeg deze niet een tweede keer. Elk prestatiemeteritem voor het verzamelen van wordt gedefinieerd in een knooppunt met de naam `<Counters>`. Hier volgt een voorbeeld waarin station prestatiemeteritems worden verzameld:

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Write Bytes/sec" ReportAs="Disk write (C:)" />
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Read Bytes/sec" ReportAs="Disk read (C:)" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

Elk prestatiemeteritem wordt weergegeven als een `<Add>` element onder `<Counters>`. De `PerformanceCounter` kenmerk bepaalt welke prestatiemeteritem verzamelen. De `ReportAs` kenmerk is de titel moet worden weergegeven in de Azure-portal voor het prestatiemeteritem. Elk prestatiemeteritem dat u hebt verzameld wordt geplaatst in een categorie met de naam **aangepaste** in de portal. In tegenstelling tot Azure Diagnostics, kunt u het interval dat met deze prestatiemeteritems worden verzameld en verzonden naar Azure niet instellen. Met Application Insights-prestatiemeteritems verzameld en verzonden elke minuut. 

Application Insights verzamelt automatisch de volgende prestatiemeteritems:

* \Process(??APP_WIN32_PROC??)\% Processor Time
* \Memory\Available Bytes
* \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
* \Process(??APP_WIN32_PROC??)\Private Bytes
* \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
* \Processor(_Total)\% Processor Time

Zie voor meer informatie [systeemprestatiemeteritems in Application Insights](../application-insights/app-insights-performance-counters.md) en [Application Insights voor Azure Cloud Services](../application-insights/app-insights-cloudservices.md#performance-counters).

### <a name="azure-diagnostics"></a>Azure Diagnostics

> [!IMPORTANT]
> Hoewel al deze gegevens worden samengevoegd in de storage-account, de portal biedt **niet** bieden een systeemeigen manier om de grafiekgegevens. Het is raadzaam dat u een andere service van de diagnostische gegevens, zoals Application Insights in uw toepassing integreren.

De extensie Azure Diagnostics voor Cloud-Services kunt dat u opgeven welke prestatiemeteritems die u wilt verzamelen. Als u Azure Diagnostics instelt, Zie [Cloud Service Bewakingsoverzicht](cloud-services-how-to-monitor.md#setup-diagnostics-extension).

De prestatiemeteritems die u wilt verzamelen zijn gedefinieerd in de **diagnostics.wadcfgx** bestand. Open dit bestand (dit is gedefinieerd per rol) in Visual Studio en zoek de **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **PerformanceCounters** element. Voeg een nieuwe **PerformanceCounterConfiguration** element als een onderliggend element. Dit element heeft twee kenmerken: `counterSpecifier` en `sampleRate`. De `counterSpecifier` kenmerk bepaalt welke meteritem ingesteld (beschreven in de vorige sectie) voor het verzamelen van prestaties van het systeem. De `sampleRate` waarde geeft aan hoe vaak dat de waarde moet worden doorzocht. Als een geheel alle prestatiemeteritems worden overgedragen naar Azure volgens de bovenliggende `PerformanceCounters` van element `scheduledTransferPeriod` kenmerkwaarde.

Voor meer informatie over de `PerformanceCounters` schema-element, Zie de [Azure Diagnostics Schema](../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md#performancecounters-element).

De periode die is gedefinieerd door de `sampleRate` kenmerkgebruiken typt u de duur van de XML-gegevens om aan te geven hoe vaak het prestatiemeteritem moet worden doorzocht. In het onderstaande voorbeeld is de snelheid wordt ingesteld op `PT3M`, wat betekent dat `[P]eriod[T]ime[3][M]inutes`: elke drie minuten.

Voor meer informatie over hoe de `sampleRate` en `scheduledTransferPeriod` zijn gedefinieerd, Zie de **gegevenstype duur** sectie het [W3 XML-datum en tijd datum typen](https://www.w3schools.com/XML/schema_dtypes_date.asp) zelfstudie.

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />

          <!-- This is a new perf counter which will track the C: disk read activity in bytes per second, every minute -->
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(C:)\Disk Read Bytes/sec" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="create-a-new-perf-counter"></a>Maak een nieuwe-prestatiemeteritem

Een nieuwe prestatiemeteritem kan worden gemaakt en gebruikt door uw code. Uw code die u een nieuwe prestatiemeteritem maakt moet worden uitgevoerd met verhoogde bevoegdheden, dat anders zal mislukken. Uw cloudservice `OnStart` starten van de code kunt maken van het prestatiemeteritem dat u de rol uitvoeren met verhoogde bevoegdheden vereist. Of u kunt een starten van de taak die wordt uitgevoerd met verhoogde bevoegdheden en maakt het prestatiemeteritem maken. Zie voor meer informatie over het starten van de taken [configureren en starten van de taken voor een cloudservice uitvoeren](cloud-services-startup-tasks.md).

Voor het configureren van uw rol om uit te voeren met verhoogde bevoegdheden, Voeg een `<Runtime>` element op de [csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) bestand.

```xml
<ServiceDefinition name="CloudServiceLoadTesting" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Large">

    <!-- ... cut to save space ... -->

    <Runtime executionContext="elevated">
      
    </Runtime>

    <!-- ... cut to save space ... -->

  </WorkerRole>
</ServiceDefinition>
```

U kunt maken en registreren van een nieuwe prestatiemeteritem met een paar regels code. Gebruik de `System.Diagnostics.PerformanceCounterCategory.Create` methode-overload die de categorie en de teller maakt. De volgende code wordt eerst gecontroleerd of de categorie bestaat, en indien deze ontbreken, de categorie en de teller maakt.

```csharp
using System.Diagnostics;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRoleWithSBQueue1
{
    public class WorkerRole : RoleEntryPoint
    {
        // Perf counter variable representing times service was used.
        private PerformanceCounter counterServiceUsed;

        public override bool OnStart()
        {
            // ... Other startup code here ...

            // Define the cateogry and counter names.
            string perfCounterCatName = "MyService";
            string perfCounterName = "Times Used";

            // Create the counter if needed. Our counter category only has a single counter.
            // Both the category and counter are created in the same method call.
            if (!PerformanceCounterCategory.Exists(perfCounterCatName))
            {
                PerformanceCounterCategory.Create(perfCounterCatName, "Collects information about the cloud service.", 
                                                  PerformanceCounterCategoryType.SingleInstance, 
                                                  perfCounterName, "How many times the cloud service was used.");
            }

            // Get reference to our counter
            counterServiceUsed = new PerformanceCounter(perfCounterCatName, perfCounterName);
            counterServiceUsed.ReadOnly = false;
            
            return base.OnStart();
        }

        // ... cut class code to save space
    }
}
```

Wanneer u gebruiken van de teller wilt, roept de `Increment` of `IncrementBy` methode.

```csharp
// Increase the counter by 1
counterServiceUsed.Increment();
```

Nu uw toepassing gebruikmaakt van uw aangepaste teller, moet u voor het configureren van Azure Diagnostics of Application Insights om bij te houden van de teller.


### <a name="application-insights"></a>Application Insights

Zoals eerder vermeld, de prestatiemeteritems voor Application Insights zijn gedefinieerd in de **ApplicationInsights.config** bestand. Open **ApplicationInsights.config** en zoek de **ApplicationInsights** > **TelemetryModules** > **toevoegen**  >  **Tellers** element. Maak een `<Add>` onderliggend element en stel de `PerformanceCounter` kenmerk voor de categorie en de naam van het prestatiemeteritem dat u hebt gemaakt in uw code. Stel de `ReportAs` kenmerk een beschrijvende naam die u wilt zien in de portal.

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <!-- ... cut other perf counters to save space ... -->

        <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
        <Add PerformanceCounter="\MyService\Times Used" ReportAs="Service used counter" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

### <a name="azure-diagnostics"></a>Azure Diagnostics

Zoals eerder is vermeld, worden de prestatiemeteritems die u wilt verzamelen gedefinieerd in de **diagnostics.wadcfgx** bestand. Open dit bestand (dit is gedefinieerd per rol) in Visual Studio en zoek de **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **PerformanceCounters** element. Voeg een nieuwe **PerformanceCounterConfiguration** element als een onderliggend element. Stel de `counterSpecifier` kenmerk voor de categorie en de naam van het prestatiemeteritem dat u hebt gemaakt in uw code. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <!-- ... cut other perf counters to save space ... -->
          
          <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
          <PerformanceCounterConfiguration counterSpecifier="\MyService\Times Used" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="more-information"></a>Meer informatie

- [Application Insights voor Azure-Cloudservices](../application-insights/app-insights-cloudservices.md#performance-counters)
- [Systeemprestatiemeteritems in Application Insights](../application-insights/app-insights-performance-counters.md)
- [Een itempad opgeven](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85))
- [Azure Diagnostics Schema - prestatiemeteritems](../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md#performancecounters-element)