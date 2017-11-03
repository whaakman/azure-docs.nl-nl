---
title: Azure Diagnostics 1.0 configuratieschema | Microsoft Docs
description: Dit is alleen relevant als u van Azure SDK 2.4 gebruikmaakt en onder met Azure Virtual Machines, virtuele-Machineschaalsets, Service Fabric of Cloud Services.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: robb
ms.openlocfilehash: a8fdfb52d5091d3fc9779657737c7430fcfada51
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Het Schema van Azure Diagnostics 1.0
> [!NOTE]
> Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken van Azure Virtual Machines, virtuele-Machineschaalsets, Service Fabric en Cloud-Services.  Deze pagina is alleen relevant als u een van deze services.
>

Azure Diagnostics wordt gebruikt met andere Microsoft-producten voor diagnostische gegevens zoals Azure Monitor, Application Insights en Log Analytics.

Het configuratiebestand van Azure Diagnostics definieert waarden die worden gebruikt voor het initialiseren van de Monitor diagnostische gegevens. Dit bestand wordt gebruikt voor het initialiseren van diagnostische configuratie-instellingen als de monitor diagnostics wordt gestart.  

 Het schema van Azure Diagnostics configuratiebestand worden standaard geïnstalleerd op de `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory. Vervang `<version>` met de geïnstalleerde versie van de [Azure SDK](http://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  Het configuratiebestand van de diagnostische gegevens wordt doorgaans gebruikt bij starten van de taken waarvoor de diagnostische gegevens worden verzameld eerder in het opstartproces. Zie voor meer informatie over het gebruik van Azure Diagnostics [Logboekregistratiegegevens verzamelen met behulp van Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

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
 De XML-naamruimte voor de diagnostics-configuratiebestand is:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Schema-elementen  
 Het configuratiebestand van de diagnostische gegevens van bevat de volgende elementen.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration Element  
Het element op het hoogste niveau van het configuratiebestand van de diagnostische gegevens.  

Kenmerken:

|Kenmerk  |Type   |Vereist| Standaard | Beschrijving|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|Duur|Optioneel | PT1M| Geeft het interval waarmee de diagnostische monitor worden opgevraagd voor diagnostische configuratiewijzigingen.|  
|**overallQuotaInMB**|unsignedInt|Optioneel| 4000 MB. Als u een waarde opgeeft, deze mag niet groter zijn dan deze hoeveelheid |De totale hoeveelheid system bestandsopslag voor alle logboekregistratie buffers toegewezen.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs Element  
Definieert de configuratie van de buffer voor de logboeken die worden gegenereerd door de onderliggende infrastructuur van diagnostische gegevens.

Bovenliggend Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  

Kenmerken:

|Kenmerk|Type|Beschrijving|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslagruimte op het bestand systeem die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilter**|Tekenreeks|Optioneel. Hiermee geeft u het minimale ernstniveau van logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **Kritiek**.|  
|**scheduledTransferPeriod**|Duur|Optioneel. Hiermee geeft u het interval tussen geplande de overdracht van gegevens, afgerond naar het dichtstbijzijnde aantal minuten.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="logs-element"></a>Logboeken Element  
 Definieert de configuratie van de buffer voor basic-Azure Logboeken.

 Bovenliggend element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  

Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslagruimte op het bestand systeem die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilter**|Tekenreeks|Optioneel. Hiermee geeft u het minimale ernstniveau van logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **Kritiek**.|  
|**scheduledTransferPeriod**|Duur|Optioneel. Hiermee geeft u het interval tussen geplande de overdracht van gegevens, afgerond naar het dichtstbijzijnde aantal minuten.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="directories-element"></a>Mappen Element  
Definieert de configuratie van de buffer voor logboeken op basis van bestanden die u kunt definiëren.

Bovenliggend element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  


Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslagruimte op het bestand systeem die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferPeriod**|Duur|Optioneel. Hiermee geeft u het interval tussen geplande de overdracht van gegevens, afgerond naar het dichtstbijzijnde aantal minuten.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="crashdumps-element"></a>CrashDumps Element  
 Definieert de crash dumpbestanden map.

 Bovenliggend Element: [mappen Element](#Directories).  

Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**container**|Tekenreeks|De naam van de container waar de inhoud van de map kan worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs Element  
 Hiermee definieert u de map voor mislukte aanvragen.

 Bovenliggende Element [mappen Element](#Directories).  

Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**container**|Tekenreeks|De naam van de container waar de inhoud van de map kan worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

##  <a name="iislogs-element"></a>IISLogs Element  
 Hiermee definieert u de logboekmap voor IIS.

 Bovenliggende Element [mappen Element](#Directories).  

Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**container**|Tekenreeks|De naam van de container waar de inhoud van de map kan worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

## <a name="datasources-element"></a>Gegevensbronnen Element  
 Hiermee definieert u nul of meer extra logboekback-mappen.

 Bovenliggend Element: [mappen Element](#Directories).

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 Hiermee definieert u de map van de logboekbestanden te controleren.

 Bovenliggend Element: [gegevensbronnen Element](#DataSources).

Kenmerken:

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**container**|Tekenreeks|De naam van de container waar de inhoud van de map kan worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in megabytes.<br /><br /> De standaardwaarde is 0.|  

## <a name="absolute-element"></a>Absolute Element  
 Hiermee definieert u een absoluut pad van de map bewaken met optionele omgeving uitbreiding.

 Bovenliggend Element: [DirectoryConfiguration Element](#DirectoryConfiguration).  

Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**pad**|Tekenreeks|Vereist. Het absolute pad naar de map bewaken.|  
|**expandEnvironment**|Booleaanse waarde|Vereist. Indien ingesteld op **true**, omgevingsvariabelen in het pad worden uitgevouwen.|  

## <a name="localresource-element"></a>LocalResource Element  
 Hiermee definieert u een pad relatief ten opzichte van een lokale bron gedefinieerd in de definitie van de service.

 Bovenliggend Element: [DirectoryConfiguration Element](#DirectoryConfiguration).  

Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**naam**|Tekenreeks|Vereist. De naam van de lokale resource die de map voor het bewaken van bevat.|  
|**relativePath**|Tekenreeks|Vereist. Het pad ten opzichte van de lokale resource om te controleren.|  

## <a name="performancecounters-element"></a>PerformanceCounters Element  
 Hiermee wordt het pad naar het prestatiemeteritem verzamelen.

 Bovenliggend Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).


 Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslagruimte op het bestand systeem die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferPeriod**|Duur|Optioneel. Hiermee geeft u het interval tussen geplande de overdracht van gegevens, afgerond naar het dichtstbijzijnde aantal minuten.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration Element  
 Hiermee definieert u de Prestatiemeter te verzamelen.

 Bovenliggend Element: [PerformanceCounters Element](#PerformanceCounters).  

 Kenmerken:  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**counterSpecifier**|Tekenreeks|Vereist. Het pad naar het prestatiemeteritem verzamelen.|  
|**sampleRate**|Duur|Vereist. De snelheid waarmee het prestatiemeteritem moet worden verzameld.|  

## <a name="windowseventlog-element"></a>WindowsEventLog Element  
 Hiermee definieert u de gebeurtenislogboeken bewaken.

 Bovenliggend Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).

  Kenmerken:

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid opslagruimte op het bestand systeem die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilter**|Tekenreeks|Optioneel. Hiermee geeft u het minimale ernstniveau van logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **Kritiek**.|  
|**scheduledTransferPeriod**|Duur|Optioneel. Hiermee geeft u het interval tussen geplande de overdracht van gegevens, afgerond naar het dichtstbijzijnde aantal minuten.<br /><br /> De standaardwaarde is PT0S.|  

## <a name="datasource-element"></a>DataSource-Element  
 Hiermee definieert u het gebeurtenislogboek om te controleren.

 Bovenliggend Element: [WindowsEventLog Element](#windowsEventLog).  

 Kenmerken:

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**naam**|Tekenreeks|Vereist. Een XPath-expressie voor het opgeven van het logboek te verzamelen.|  
