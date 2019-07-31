---
title: Azure Diagnostics 1,0-configuratie schema
description: ALLEEN relevant als u Azure SDK 2,4 en lager gebruikt met Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric of Cloud Services.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: ac2b79d670b803573a359dfc9f8738f972f2d9b5
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "60237845"
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Azure Diagnostics 1,0-configuratie schema
> [!NOTE]
> Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatie meter items en andere statistieken van Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric en Cloud Services.  Deze pagina is alleen relevant als u een van deze services gebruikt.
>

Azure Diagnostics wordt gebruikt in combi natie met andere micro soft Diagnostics-producten, zoals Azure Monitor, waaronder Application Insights en Log Analytics.

Het Azure Diagnostics configuratie bestand definieert waarden die worden gebruikt voor het initialiseren van de diagnostische monitor. Dit bestand wordt gebruikt om diagnostische configuratie-instellingen te initialiseren wanneer de diagnostische monitor wordt gestart.  

 Het Azure Diagnostics-configuratie schema bestand wordt standaard geïnstalleerd in de `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` map. Vervang `<version>` door de geïnstalleerde versie van de [Azure SDK](https://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  Het diagnostische configuratie bestand wordt doorgaans gebruikt met opstart taken waarvoor diagnostische gegevens moeten worden verzameld in het opstart proces. Zie [registratie gegevens verzamelen](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7)met behulp van Azure Diagnostics voor meer informatie over het gebruik van Azure Diagnostics.  

## <a name="example-of-the-diagnostics-configuration-file"></a>Voor beeld van het configuratie bestand voor diagnostische gegevens  
 In het volgende voor beeld ziet u een typisch configuratie bestand voor diagnostische gegevens:  

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

## <a name="diagnosticsconfiguration-namespace"></a>DiagnosticsConfiguration-naam ruimte  
 De XML-naam ruimte voor het diagnostische configuratie bestand is:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Schema-elementen  
 Het configuratie bestand voor diagnostische gegevens bevat de volgende elementen.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration-element  
Het element op het hoogste niveau van het diagnostische configuratie bestand.  

Kenmerken:

|Kenmerk  |type   |Vereist| Standaard | Description|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|duration|Optioneel | PT1M| Hiermee geeft u het interval op waarmee de diagnostische monitor de diagnostische configuratie wijzigingen navraagt.|  
|**overallQuotaInMB**|unsignedInt|Optioneel| 4000 MB. Als u een waarde opgeeft, mag deze de hoeveelheid niet overschrijden |De totale hoeveelheid bestandssysteem opslag die is toegewezen voor alle logboek buffers.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs-element  
Hiermee definieert u de buffer configuratie voor de logboeken die worden gegenereerd door de onderliggende diagnose-infra structuur.

Bovenliggend element: DiagnosticMonitorConfiguration-element.  

Kenmerken:

|Kenmerk|type|Description|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid bestandssysteem opslag op die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaard waarde is 0.|  
|**scheduledTransferLogLevelFilter**|string|Optioneel. Hiermee geeft u het minimale Ernst niveau voor logboek vermeldingen die worden overgebracht. De standaard waarde is niet **gedefinieerd**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**en **kritiek**.|  
|**scheduledTransferPeriod**|duration|Optioneel. Hiermee geeft u het interval op tussen de geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaard waarde is PT0S.|  

## <a name="logs-element"></a>Logboek element  
 Hiermee definieert u de buffer configuratie voor Basic Azure-Logboeken.

 Bovenliggend element: DiagnosticMonitorConfiguration-element.  

Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid bestandssysteem opslag op die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaard waarde is 0.|  
|**scheduledTransferLogLevelFilter**|string|Optioneel. Hiermee geeft u het minimale Ernst niveau voor logboek vermeldingen die worden overgebracht. De standaard waarde is niet **gedefinieerd**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**en **kritiek**.|  
|**scheduledTransferPeriod**|duration|Optioneel. Hiermee geeft u het interval op tussen de geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaard waarde is PT0S.|  

## <a name="directories-element"></a>Element directory's  
Hiermee definieert u de buffer configuratie voor op bestanden gebaseerde logboeken die u kunt definiëren.

Bovenliggend element: DiagnosticMonitorConfiguration-element.  


Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid bestandssysteem opslag op die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaard waarde is 0.|  
|**scheduledTransferPeriod**|duration|Optioneel. Hiermee geeft u het interval op tussen de geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaard waarde is PT0S.|  

## <a name="crashdumps-element"></a>CrashDumps-element  
 Hiermee definieert u de map crash dumps.

 Bovenliggend element: Het element directory's.  

Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waarin de inhoud van de map moet worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in mega bytes.<br /><br /> De standaard waarde is 0.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs Element  
 Hiermee definieert u de map voor het mislukte aanvragen logboek.

 Element mappen van bovenliggend element.  

Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waarin de inhoud van de map moet worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in mega bytes.<br /><br /> De standaard waarde is 0.|  

##  <a name="iislogs-element"></a>IISLogs-element  
 Hiermee definieert u de IIS-logboek Directory.

 Element mappen van bovenliggend element.  

Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waarin de inhoud van de map moet worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in mega bytes.<br /><br /> De standaard waarde is 0.|  

## <a name="datasources-element"></a>Element gegevens bronnen  
 Hiermee worden nul of meer extra logboek mappen gedefinieerd.

 Bovenliggend element: Het element directory's.

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration-element  
 Hiermee definieert u de map met te bewaken logboek bestanden.

 Bovenliggend element: Het element data sources.

Kenmerken:

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**container**|string|De naam van de container waarin de inhoud van de map moet worden overgedragen.|  
|**directoryQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale grootte van de map in mega bytes.<br /><br /> De standaard waarde is 0.|  

## <a name="absolute-element"></a>Absoluut element  
 Hiermee definieert u een absoluut pad van de directory die moet worden bewaakt met optionele omgevings uitbreiding.

 Bovenliggend element: DirectoryConfiguration Element.  

Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**Pad**|string|Vereist. Het absolute pad naar de map die u wilt bewaken.|  
|**expandEnvironment**|boolean|Vereist. Als deze eigenschap is ingesteld op **True**, worden de omgevings variabelen in het pad uitgevouwen.|  

## <a name="localresource-element"></a>LocalResource-element  
 Hiermee wordt een pad gedefinieerd dat relatief is ten opzichte van een lokale bron die in de service definitie is gedefinieerd.

 Bovenliggend element: DirectoryConfiguration Element.  

Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**name**|string|Vereist. De naam van de lokale resource die de directory bevat die moet worden bewaakt.|  
|**relativePath**|string|Vereist. Het pad ten opzichte van de lokale bron die moet worden bewaakt.|  

## <a name="performancecounters-element"></a>Performance Counters-element  
 Hiermee definieert u het pad naar het prestatie meter item dat moet worden verzameld.

 Bovenliggend element: DiagnosticMonitorConfiguration-element.


 Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid bestandssysteem opslag op die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaard waarde is 0.|  
|**scheduledTransferPeriod**|duration|Optioneel. Hiermee geeft u het interval op tussen de geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaard waarde is PT0S.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration-element  
 Hiermee wordt het prestatie meter item gedefinieerd dat moet worden verzameld.

 Bovenliggend element: Performance Counters-element.  

 Kenmerken:  

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**counterSpecifier**|string|Vereist. Het pad naar het prestatie meter item dat moet worden verzameld.|  
|**sampleRate**|duration|Vereist. De snelheid waarmee het prestatie meter item moet worden verzameld.|  

## <a name="windowseventlog-element"></a>WindowsEventLog-element  
 Hiermee worden de gebeurtenis logboeken gedefinieerd die moeten worden gecontroleerd.

 Bovenliggend element: DiagnosticMonitorConfiguration-element.

  Kenmerken:

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Optioneel. Hiermee geeft u de maximale hoeveelheid bestandssysteem opslag op die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaard waarde is 0.|  
|**scheduledTransferLogLevelFilter**|string|Optioneel. Hiermee geeft u het minimale Ernst niveau voor logboek vermeldingen die worden overgebracht. De standaard waarde is niet **gedefinieerd**. Andere mogelijke waarden zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**en **kritiek**.|  
|**scheduledTransferPeriod**|duration|Optioneel. Hiermee geeft u het interval op tussen de geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaard waarde is PT0S.|  

## <a name="datasource-element"></a>Data Source-element  
 Hiermee wordt het gebeurtenis logboek gedefinieerd dat moet worden bewaakt.

 Bovenliggend element: WindowsEventLog-element.  

 Kenmerken:

|Kenmerk|type|Description|  
|---------------|----------|-----------------|  
|**name**|string|Vereist. Een XPath-expressie waarmee het te verzamelen logboek wordt opgegeven.|  

