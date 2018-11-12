---
title: Azure Diagnostics-extensie 1.2 configuratieschema
description: ALLEEN relevant als u van Azure SDK 2.5 met Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric of Cloud Services gebruikmaakt.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: 322cd75fe9198bae459e7c22bed794f583d13363
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260292"
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Azure Diagnostics 1.2 configuratieschema
> [!NOTE]
> Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken van Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric en Cloud Services.  Deze pagina is alleen van toepassing als u een van deze services.
>

Azure Diagnostics wordt gebruikt met andere Microsoft-producten voor diagnostische gegevens, zoals Azure Monitor, Application Insights en Log Analytics.

Dit schema definieert de mogelijke waarden die u gebruiken kunt voor het initialiseren van diagnostische configuratie-instellingen wanneer de monitor diagnostische gegevens wordt gestart.  


 De schemadefinitie voor de configuratie van de openbare-bestand downloaden door het uitvoeren van de volgende PowerShell-opdracht:  

```PowerShell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Zie voor meer informatie over het gebruik van Azure Diagnostics [diagnostische gegevens inschakelen in Azure Cloud Services](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Voorbeeld van het configuratiebestand van de diagnostische gegevens  
 Het volgende voorbeeld ziet u een configuratiebestand typische diagnostische gegevens:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">  
  <WadCfg>  
    <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  
      <PerformanceCounters scheduledTransferPeriod="PT1M">  
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
      </PerformanceCounters>  
      <Directories scheduledTransferPeriod="PT5M">  
        <IISLogs containerName="iislogs" />  
        <FailedRequestLogs containerName="iisfailed" />  
        <DataSources>  
          <DirectoryConfiguration containerName="mynewprocess">  
            <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
          </DirectoryConfiguration>  
          <DirectoryConfiguration containerName="badapp">  
            <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
          </DirectoryConfiguration>  
          <DirectoryConfiguration containerName="goodapp">  
            <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
          </DirectoryConfiguration>  
        </DataSources>  
      </Directories>  
      <EtwProviders>  
        <EtwEventSourceProviderConfiguration provider="MyProviderClass" scheduledTransferPeriod="PT5M">  
          <Event id="0"/>  
          <Event id="1" eventDestination="errorTable"/>  
          <DefaultEvents />  
        </EtwEventSourceProviderConfiguration>  
        <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
          <Event id="0"/>  
          <DefaultEvents eventDestination="defaultTable"/>  
        </EtwManifestProviderConfiguration>  
      </EtwProviders>  
      <WindowsEventLog scheduledTransferPeriod="PT5M">  
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
        <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
        <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
      </WindowsEventLog>  
      <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
        <CrashDumpConfiguration processName="mynewprocess.exe" />  
        <CrashDumpConfiguration processName="badapp.exe"/>  
      </CrashDumps>  
    </DiagnosticMonitorConfiguration>  
  </WadCfg>  
</PublicConfig>  

```  

## <a name="diagnostics-configuration-namespace"></a>Diagnostische gegevens over configuratie Namespace  
 De XML-naamruimte voor het configuratiebestand van de diagnostische gegevens is:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig-Element  
 Element op het hoogste niveau van het configuratiebestand van de diagnostische gegevens. De volgende tabel beschrijft de elementen van het configuratiebestand.  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**WadCfg**|Vereist. Configuratie-instellingen voor de telemetriegegevens te verzamelen.|  
|**StorageAccount**|De naam van de Azure Storage-account voor het opslaan van de gegevens in. Dit kan ook worden opgegeven als parameter bij het uitvoeren van de cmdlet Set-AzureServiceDiagnosticsExtension.|  
|**LocalResourceDirectory**|De map op de virtuele machine moet worden gebruikt door de Monitoring Agent voor het opslaan van gebeurtenisgegevens. Als dat niet is ingesteld, de standaard-map wordt gebruikt:<br /><br /> Voor een rol Worker/webservice: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Voor een virtuele Machine: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Vereiste kenmerken zijn:<br /><br /> -                      **pad** -de map op het systeem moet worden gebruikt door Azure Diagnostics.<br /><br /> -                      **expandEnvironment** -bepaalt of omgevingsvariabelen worden uitgebreid in het padnaam.|  

## <a name="wadcfg-element"></a>WadCFG-Element  
Definieert de configuratie-instellingen voor de telemetriegegevens te verzamelen. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Vereist. Optionele kenmerken zijn:<br /><br /> -                     **overallQuotaInMB** -de maximale hoeveelheid lokale schijfruimte die kan worden gebruikt door de verschillende typen diagnostische gegevens die worden verzameld door Azure Diagnostics. De standaardinstelling is 5120MB.<br /><br /> -                     **useProxyServer** -Azure Diagnostics configureren voor het gebruik van de proxy-instellingen zoals in de instellingen van Internet Explorer.|  
|**CrashDumps**|Het verzamelen van crashdumps inschakelen. Optionele kenmerken zijn:<br /><br /> -                     **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van crashdumps.<br /><br /> -                     **crashDumpType** -geheugendumps van Azure Diagnostics configureren voor het verzamelen van Mini of volledige vastlopen.<br /><br /> -                     **directoryQuotaPercentage**-configureert u het percentage van de **overallQuotaInMB** moet worden gereserveerd voor crashdumps op de virtuele machine.|  
|**DiagnosticInfrastructureLogs**|Het verzamelen van logboeken die worden gegenereerd door Azure Diagnostics inschakelen. De logboeken met diagnostische gegevens over infrastructuur zijn handig voor het oplossen van het systeem diagnostics zelf. Optionele kenmerken zijn:<br /><br /> -                     **scheduledTransferLogLevelFilter** -configureert u het minimale ernstniveau van de logboeken die worden verzameld.<br /><br /> -                     **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**Adreslijsten**|Zorgt ervoor dat de inhoud van een map, IIS kan geen toegang tot aanvraag logboeken en/of IIS-logboeken. Optionele kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**EtwProviders**|Hiermee configureert u verzamelen van ETW-gebeurtenissen uit de gebeurtenisbron en/of ETW Manifest op basis van de providers.|  
|**Metrische gegevens**|Dit element kunt u voor het genereren van een tabel met prestaties teller die is geoptimaliseerd voor snelle query's. Elk prestatiemeteritem dat is gedefinieerd in de **PerformanceCounters** element wordt opgeslagen in de metrische gegevenstabel voor naast het prestatiemeteritem-tabel. Vereist kenmerk:<br /><br /> **resourceId** -dit is de resource-ID van de virtuele Machine die u om de Azure Diagnostics implementeert. Krijgen de **resourceID** uit de [Azure-portal](https://portal.azure.com). Selecteer **Bladeren** -> **resourcegroepen** -> **< naam\>**. Klik op de **eigenschappen** tegel en kopieer de waarde van de **ID** veld.|  
|**PerformanceCounters**|Kunt u het verzamelen van prestatiemeteritems. Optionele kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML 'Gegevenstype duur'.](http://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Kunt u het verzamelen van de Windows-gebeurtenislogboeken. Optionele kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML 'Gegevenstype duur'.](http://www.w3schools.com/xml/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps-Element  
 Kunt u verzamelen van crashdumps. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Vereist. Vereist kenmerk:<br /><br /> **Procesnaam** -de naam van het proces dat u wilt dat Azure Diagnostics voor het verzamelen van een crashdump voor.|  
|**crashDumpType**|Hiermee configureert u Azure Diagnostics voor het verzamelen van dumpbestanden voor foutopsporing mini of volledige loopt vast.|  
|**directoryQuotaPercentage**|Hiermee configureert u het percentage van de **overallQuotaInMB** moet worden gereserveerd voor crashdumps op de virtuele machine.|  

## <a name="directories-element"></a>Mappen-Element  
 Zorgt ervoor dat de inhoud van een map, IIS kan geen toegang tot aanvraag logboeken en/of IIS-logboeken. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**Gegevensbronnen**|Een lijst met mappen om te controleren.|  
|**FailedRequestLogs**|Met inbegrip van dit element in de configuratie kunt verzamelen van logboeken over mislukte aanvragen voor een IIS-site of toepassing. U moet ook traceringsopties onder inschakelen **system. WebServer** in **Web.config**.|  
|**IISLogs**|Met inbegrip van dit element in de configuratie kunt u het verzamelen van IIS-logboeken:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van de IIS-logboeken.|  

## <a name="datasources-element"></a>Gegevensbronnen-Element  
 Een lijst met mappen om te controleren. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Vereist. Vereist kenmerk:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van de logboekbestanden.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration-Element  
 **DirectoryConfiguration** omvat mogelijk een de **Absolute** of **LocalResource** element, maar niet beide. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**Absolute**|Het absolute pad naar de map om te controleren. De volgende kenmerken zijn vereist:<br /><br /> -                     **Pad** -het absolute pad naar de map om te controleren.<br /><br /> -                      **expandEnvironment** -configureert u of omgevingsvariabelen in het pad worden uitgevouwen.|  
|**LocalResource**|Het pad relatief ten opzichte van een lokale bron om te controleren. Vereiste kenmerken zijn:<br /><br /> -                     **Naam** -de lokale resource die de map voor het bewaken van bevat<br /><br /> -                     **relativePath** -het pad relatief ten opzichte van de naam die de map voor het bewaken van bevat|  

## <a name="etwproviders-element"></a>EtwProviders-Element  
 Hiermee configureert u verzamelen van ETW-gebeurtenissen uit de gebeurtenisbron en/of ETW Manifest op basis van de providers. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Hiermee configureert u verzamelen van gebeurtenissen die zijn gegenereerd op basis van [EventSource klasse](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Vereist kenmerk:<br /><br /> **provider** -de naam van de klasse van de gebeurtenis gebeurtenisbron.<br /><br /> Optionele kenmerken zijn:<br /><br /> -                     **scheduledTransferLogLevelFilter** -het minimale ernstniveau om over te dragen naar uw opslagaccount.<br /><br /> -                     **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [duur van het gegevenstype XML](http://www.w3schools.com/xml/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Vereist kenmerk:<br /><br /> **provider** -de GUID van de gebeurtenisprovider<br /><br /> Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -het minimale ernstniveau om over te dragen naar uw opslagaccount.<br /><br /> -                     **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [duur van het gegevenstype XML](http://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration-Element  
 Hiermee configureert u verzamelen van gebeurtenissen die zijn gegenereerd op basis van [EventSource klasse](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**DefaultEvents**|Optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration-Element  
 De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**DefaultEvents**|Optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  

## <a name="metrics-element"></a>Element van de metrische gegevens  
 Hiermee kunt u voor het genereren van een tabel met prestaties teller die is geoptimaliseerd voor snelle query's. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**MetricAggregation**|Vereist kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [duur van het gegevenstype XML](http://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters-Element  
 Kunt u het verzamelen van prestatiemeteritems. De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|De volgende kenmerken zijn vereist:<br /><br /> -                     **counterSpecifier** -de naam van het prestatiemeteritem. Bijvoorbeeld `\Processor(_Total)\% Processor Time`. Voor een lijst van de prestaties van prestatiemeteritems op uw host voert u de opdracht `typeperf`.<br /><br /> -                     **sampleRate** -hoe vaak het item dat moet worden verzameld.<br /><br /> Optionele kenmerk:<br /><br /> **eenheid** -de eenheid van de teller.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration-Element  
 De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**Aantekening**|Vereist kenmerk:<br /><br /> **displayName** -de weergavenaam voor de teller<br /><br /> Optionele kenmerk:<br /><br /> **landinstelling** -de landinstelling die moet worden gebruikt bij het weergeven van de naam van het prestatiemeteritem|  

## <a name="windowseventlog-element"></a>WindowsEventLog-Element  
 De volgende tabel beschrijft de onderliggende elementen:  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**Gegevensbron**|De Windows-gebeurtenislogboeken te verzamelen. Vereist kenmerk:<br /><br /> **naam** : de XPath-query met een beschrijving van de windows-gebeurtenissen te verzamelen. Bijvoorbeeld:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Geef voor het verzamelen van alle gebeurtenissen, ' * '.|
