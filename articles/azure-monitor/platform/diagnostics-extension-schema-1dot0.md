---
title: Het Schema van Azure Diagnostics 1.0
description: Dit is alleen relevant als u van Azure SDK 2.4 gebruikmaakt en onder met Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric of Cloud Services.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: 0e1a0919975253e24318bb8029249958cbc50d62
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54473160"
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Het Schema van Azure Diagnostics 1.0
> [!NOTE]
> Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken van Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric en Cloud Services.  Deze pagina is alleen van toepassing als u een van deze services.
>

Azure Diagnostics wordt gebruikt met andere Microsoft-producten voor diagnostische gegevens, zoals Azure Monitor, Application Insights en Log Analytics.

Het configuratiebestand van de Azure Diagnostics definieert u waarden die worden gebruikt voor het initialiseren van de Monitor diagnostische gegevens. Dit bestand wordt gebruikt om te initialiseren diagnostische configuratie-instellingen wanneer de monitor diagnostische gegevens wordt gestart.  

 Het hulpprogramma voor het schema van de Azure Diagnostics-configuratiebestand is standaard geïnstalleerd op de `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory. Vervang `<version>` met de geïnstalleerde versie van de [Azure SDK](https://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  Het configuratiebestand van de diagnostische gegevens wordt meestal gebruikt met opstarttaken waarbij diagnostische gegevens moeten worden verzameld eerder in het opstartproces. Zie voor meer informatie over het gebruik van Azure Diagnostics [Logboekregistratiegegevens verzamelen met behulp van Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Voorbeeld van het configuratiebestand van de diagnostische gegevens  
 Het volgende voorbeeld ziet u een configuratiebestand typische diagnostische gegevens:  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify the special directories   
           that are set up for the log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories the DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative to a local   
                 resource defined in the service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- The counter specifier is in the same format as the imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- The event log name is in the same format as the imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a>DiagnosticsConfiguration Namespace  
 De XML-naamruimte voor het configuratiebestand van de diagnostische gegevens is:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Schema-elementen  
 De diagnostische gegevens over configuratie-bestand bevat de volgende elementen.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration-Element  
Het element op het hoogste niveau van het configuratiebestand van de diagnostische gegevens.  

Kenmerken:

|Kenmerk  |Type   |Vereist| Standaard | Description|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|duur|Optioneel | PT1M| Hiermee geeft u het interval waarmee de diagnostische monitor pollt voor diagnostische configuratiewijzigingen.|  
|**overallQuotaInMB**|unsignedInt|Optioneel| 4000 MB. Als u een waarde opgeeft, het mag niet groter zijn dan deze hoeveelheid |De totale hoeveelheid opslag bestandssysteem toegewezen voor alle buffers van logboekregistratie.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs-Element  
Definieert de configuratie van de buffer voor de logboeken die worden gegenereerd door de onderliggende infrastructuur van diagnostische gegevens.

Bovenliggend Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  

Kenmerken:

|Kenmerk|Type|Description|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslag bestandssysteem dat beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilter**|string|Optioneel. Hiermee geeft u het minimale ernstniveau voor logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **kritieke**.|  
|**scheduledTransferPeriod**|duur|Optioneel. Hiermee geeft u het interval tussen geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="logs-element"></a>Logboeken-Element  
 Definieert de configuratie van de buffer voor basic Logboeken in Azure.

 Bovenliggend element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  

Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslag bestandssysteem dat beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilter**|string|Optioneel. Hiermee geeft u het minimale ernstniveau voor logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **kritieke**.|  
|**scheduledTransferPeriod**|duur|Optioneel. Hiermee geeft u het interval tussen geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="directories-element"></a>Mappen-Element  
Definieert de configuratie van de buffer voor logboeken op basis van bestanden die u kunt definiëren.

Bovenliggend element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  


Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslag bestandssysteem dat beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferPeriod**|duur|Optioneel. Hiermee geeft u het interval tussen geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="crashdumps-element"></a>CrashDumps-Element  
 Hiermee definieert u de map van crashes dumpbestanden voor foutopsporing.

 Bovenliggend Element: [Mappen Element](#Directories).  

Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waar de inhoud van de map is om te worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs Element  
 Hiermee definieert u de map voor mislukte aanvragen.

 Bovenliggend Element [mappen Element](#Directories).  

Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waar de inhoud van de map is om te worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

##  <a name="iislogs-element"></a>IISLogs-Element  
 Hiermee definieert u de logboekmap voor IIS.

 Bovenliggend Element [mappen Element](#Directories).  

Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waar de inhoud van de map is om te worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

## <a name="datasources-element"></a>Gegevensbronnen-Element  
 Nul of meer extra logboekmappen definieert.

 Bovenliggend Element: [Mappen Element](#Directories).

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 Hiermee definieert u de map van logboekbestanden om te controleren.

 Bovenliggend Element: [Gegevensbronnen Element](#DataSources).

Kenmerken:

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waar de inhoud van de map is om te worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

## <a name="absolute-element"></a>Absolute-Element  
 Hiermee definieert u een absoluut pad van de map om te controleren met de optionele uitbreiding.

 Bovenliggend Element: [DirectoryConfiguration Element](#DirectoryConfiguration).  

Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**Pad**|string|Vereist. Het absolute pad naar de map om te controleren.|  
|**expandEnvironment**|booleaans|Vereist. Indien ingesteld op **waar**, omgevingsvariabelen in het pad worden uitgevouwen.|  

## <a name="localresource-element"></a>LocalResource Element  
 Hiermee definieert u een pad relatief ten opzichte van een lokale bron die is gedefinieerd in het servicedefinitie van de.

 Bovenliggend Element: [DirectoryConfiguration Element](#DirectoryConfiguration).  

Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**De naam**|string|Vereist. De naam van de lokale resource die de map voor het bewaken van bevat.|  
|**relativePath**|string|Vereist. Het pad relatief ten opzichte van de lokale resource om te controleren.|  

## <a name="performancecounters-element"></a>PerformanceCounters-Element  
 Definieert het pad voor het prestatiemeteritem voor het verzamelen van.

 Bovenliggend Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).


 Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslag bestandssysteem dat beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferPeriod**|duur|Optioneel. Hiermee geeft u het interval tussen geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration-Element  
 Het prestatiemeteritem voor het verzamelen van definieert.

 Bovenliggend Element: [PerformanceCounters Element](#PerformanceCounters).  

 Kenmerken:  

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**counterSpecifier**|string|Vereist. Het pad naar het prestatiemeteritem voor het verzamelen van.|  
|**sampleRate**|duur|Vereist. De snelheid waarmee het prestatiemeteritem moet worden verzameld.|  

## <a name="windowseventlog-element"></a>WindowsEventLog Element  
 Hiermee definieert u de gebeurtenislogboeken om te controleren.

 Bovenliggend Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).

  Kenmerken:

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslag bestandssysteem dat beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilter**|string|Optioneel. Hiermee geeft u het minimale ernstniveau voor logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **kritieke**.|  
|**scheduledTransferPeriod**|duur|Optioneel. Hiermee geeft u het interval tussen geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="datasource-element"></a>DataSource-Element  
 Hiermee definieert u het gebeurtenislogboek om te controleren.

 Bovenliggend Element: [WindowsEventLog Element](#windowsEventLog).  

 Kenmerken:

|Kenmerk|Type|Description|  
|---------------|----------|-----------------|  
|**De naam**|string|Vereist. Een XPath-expressie op te geven van het logboek te verzamelen.|  

