---
title: Azure Diagnostics 1.2 configuratieschema | Microsoft Docs
description: ALLEEN relevant als u van Azure SDK 2.5 met Azure Virtual Machines, virtuele-Machineschaalsets, Service Fabric of Cloud Services gebruikmaakt.
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
ms.openlocfilehash: 1e9cc6d0950945df8c4fba74d8e1f6196be224f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Azure Diagnostics 1.2 configuratieschema
> [!NOTE]
> Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken van Azure Virtual Machines, virtuele-Machineschaalsets, Service Fabric en Cloud-Services.  Deze pagina is alleen relevant als u een van deze services.
>

Azure Diagnostics wordt gebruikt met andere Microsoft-producten voor diagnostische gegevens zoals Azure Monitor, Application Insights en Log Analytics.

Dit schema definieert de mogelijke waarden die u gebruiken kunt voor het initialiseren van diagnostische configuratie-instellingen als de monitor diagnostics wordt gestart.  


 De schemadefinitie van de openbare configuratie bestand downloaden door het uitvoeren van de volgende PowerShell-opdracht:  

```PowerShell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Zie voor meer informatie over het gebruik van Azure Diagnostics [diagnostische gegevens inschakelen in Azure Cloud Services](http://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

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

## <a name="diagnostics-configuration-namespace"></a>Diagnostische configuratie Namespace  
 De XML-naamruimte voor de diagnostics-configuratiebestand is:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig Element  
 Element op het hoogste niveau van het configuratiebestand van de diagnostische gegevens. De volgende tabel beschrijft de elementen van het configuratiebestand.  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**WadCfg**|Vereist. Configuratie-instellingen voor de telemetriegegevens te verzamelen.|  
|**StorageAccount**|De naam van de Azure Storage-account voor het opslaan van de gegevens in. Dit kan ook worden opgegeven als parameter bij het uitvoeren van de cmdlet Set-AzureServiceDiagnosticsExtension.|  
|**LocalResourceDirectory**|De map op de virtuele machine moet worden gebruikt door de Monitoring Agent voor het opslaan van gegevens van gebeurtenissen. Als dat niet is ingesteld, de standaardmap wordt gebruikt:<br /><br /> Voor een functie Worker/webservice:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Voor een virtuele Machine:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Vereiste kenmerken zijn:<br /><br /> -                      **pad** -de map op het systeem moet worden gebruikt door Azure Diagnostics.<br /><br /> -                      **expandEnvironment** -Hiermee wordt bepaald of omgevingsvariabelen in de padnaam worden uitgevouwen.|  

## <a name="wadcfg-element"></a>WadCFG Element  
Hiermee definieert u configuratie-instellingen voor de telemetriegegevens te verzamelen. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Vereist. Optionele kenmerken zijn:<br /><br /> -                     **overallQuotaInMB** -de maximale hoeveelheid ruimte op lokale schijf die kan worden gebruikt door de verschillende typen diagnostische gegevens verzameld door Azure Diagnostics. De standaardinstelling is 5120MB.<br /><br /> -                     **useProxyServer** -om de proxyserverinstellingen gebruikt zoals in de instellingen van Internet Explorer configureren Azure Diagnostics.|  
|**CrashDumps**|Verzameling van crashdumps inschakelen. Optionele kenmerken zijn:<br /><br /> -                     **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van crashdumps.<br /><br /> -                     **crashDumpType** -dumpbestanden voor Azure Diagnostics configureert voor het verzamelen van Mini of volledige vastlopen.<br /><br /> -                     **directoryQuotaPercentage**-Hiermee configureert u het percentage **overallQuotaInMB** moet worden gereserveerd voor crashdumps op de virtuele machine.|  
|**DiagnosticInfrastructureLogs**|Inschakelen van verzamelen van logboeken die worden gegenereerd door Azure Diagnostics. De infrastructuur voor diagnostische logboeken zijn nuttig voor het oplossen van het systeem diagnostische gegevens zelf. Optionele kenmerken zijn:<br /><br /> -                     **scheduledTransferLogLevelFilter** -configureert u de minimale ernst van de logboeken die worden verzameld.<br /><br /> -                     **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**Mappen**|Kan de verzameling van de inhoud van een map, logboeken van IIS is mislukt toegang aanvragen en/of IIS-logboeken. Het optionele kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**EtwProviders**|Hiermee configureert u de verzameling van ETW-gebeurtenissen van EventSource en/of ETW Manifest op basis van providers.|  
|**Metrische gegevens**|Dit element kunt u voor het genereren van een tabel van prestaties teller die is geoptimaliseerd voor snelle query's. Elk prestatiemeteritem dat is gedefinieerd in de **PerformanceCounters** element is opgeslagen in de tabel metrische gegevens naast het prestatiemeteritem-tabel. Vereist kenmerk:<br /><br /> **resourceId** -dit is de resource-ID van de virtuele Machine die u om Azure Diagnostics implementeert. Ophalen van de **resourceID** van de [Azure-portal](https://portal.azure.com). Selecteer **Bladeren** -> **resourcegroepen** -> **< naam\>**. Klik op de **eigenschappen** tegel en kopieer de waarde van de **ID** veld.|  
|**PerformanceCounters**|Hiermee kunt het verzamelen van prestatiemeteritems. Het optionele kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML 'Duur gegevenstype'.](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Kan de verzameling van het Windows-gebeurtenislogboeken. Het optionele kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML 'Duur gegevenstype'.](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps Element  
 Verzameling van crashdumps kunt. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Vereist. Vereist kenmerk:<br /><br /> **Procesnaam** -de naam van het proces wilt u diagnostische Azure-gegevens voor het verzamelen van een crashdump voor.|  
|**crashDumpType**|Hiermee configureert u Azure Diagnostics voor het verzamelen van dumpbestanden voor mini of volledige loopt vast.|  
|**directoryQuotaPercentage**|Hiermee configureert u het percentage **overallQuotaInMB** moet worden gereserveerd voor crashdumps op de virtuele machine.|  

## <a name="directories-element"></a>Mappen Element  
 Kan de verzameling van de inhoud van een map, logboeken van IIS is mislukt toegang aanvragen en/of IIS-logboeken. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**Gegevensbronnen**|Een lijst met mappen om te controleren.|  
|**FailedRequestLogs**|Met inbegrip van dit element in de configuratie kunt verzamelen van logboeken over mislukte aanvragen voor een IIS-site of toepassing. U moet ook de traceringsopties onder inschakelen **system. WebServer** in **Web.config**.|  
|**IISLogs**|Met inbegrip van dit element in de configuratie, kunt de verzameling van IIS-logboeken:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van de IIS-logboeken.|  

## <a name="datasources-element"></a>Gegevensbronnen Element  
 Een lijst met mappen om te controleren. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Vereist. Vereist kenmerk:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van de logboekbestanden.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 **DirectoryConfiguration** mogelijk bevat de **Absolute** of **LocalResource** element, maar niet beide. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**Absolute**|Het absolute pad naar de map bewaken. De volgende kenmerken zijn vereist:<br /><br /> -                     **Pad** -het absolute pad naar de map bewaken.<br /><br /> -                      **expandEnvironment** -configureert u of omgevingsvariabelen in pad worden uitgevouwen.|  
|**LocalResource**|Het pad ten opzichte van een lokale bron om te controleren. Vereiste kenmerken zijn:<br /><br /> -                     **Naam** -de lokale resource die de map voor het bewaken van bevat<br /><br /> -                     **relativePath** -het pad ten opzichte van de naam die de map voor het bewaken van bevat|  

## <a name="etwproviders-element"></a>EtwProviders Element  
 Hiermee configureert u de verzameling van ETW-gebeurtenissen van EventSource en/of ETW Manifest op basis van providers. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Hiermee configureert u de verzameling van gebeurtenissen die worden gegenereerd op basis van [EventSource klasse](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Vereist kenmerk:<br /><br /> **provider** -de naam van de klasse van de gebeurtenis EventSource.<br /><br /> Optionele kenmerken zijn:<br /><br /> -                     **scheduledTransferLogLevelFilter** -de minimum ernst om over te dragen naar uw opslagaccount.<br /><br /> -                     **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [duur van het gegevenstype XML](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Vereist kenmerk:<br /><br /> **provider** -de GUID van de gebeurtenisprovider<br /><br /> Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -de minimum ernst om over te dragen naar uw opslagaccount.<br /><br /> -                     **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [duur van het gegevenstype XML](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 Hiermee configureert u de verzameling van gebeurtenissen die worden gegenereerd op basis van [EventSource klasse](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**DefaultEvents**|Het optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Het optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**DefaultEvents**|Het optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Het optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  

## <a name="metrics-element"></a>Element van de metrische gegevens  
 Hiermee kunt u voor het genereren van een tabel van prestaties teller die is geoptimaliseerd voor snelle query's. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**MetricAggregation**|Vereist kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [duur van het gegevenstype XML](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters Element  
 Hiermee kunt het verzamelen van prestatiemeteritems. De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|De volgende kenmerken zijn vereist:<br /><br /> -                     **counterSpecifier** -de naam van het prestatiemeteritem. Bijvoorbeeld `\Processor(_Total)\% Processor Time`. Voor een lijst van de prestaties van de prestatiemeteritems op uw host de opdracht uitvoert `typeperf`.<br /><br /> -                     **sampleRate** -hoe vaak de teller moet actieve.<br /><br /> Het optionele kenmerk:<br /><br /> **eenheid** -de eenheid van de teller.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration Element  
 De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**aantekening**|Vereist kenmerk:<br /><br /> **displayName** -de weergavenaam voor de teller<br /><br /> Het optionele kenmerk:<br /><br /> **landinstelling** -de landinstellingen te gebruiken bij het weergeven van de naam van het meteritem|  

## <a name="windowseventlog-element"></a>WindowsEventLog Element  
 De volgende tabel beschrijft de onderliggende elementen:  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**Gegevensbron**|De Windows-gebeurtenislogboeken te verzamelen. Vereist kenmerk:<br /><br /> **naam** : de XPath-query met een beschrijving van de windows-gebeurtenissen te verzamelen. Bijvoorbeeld:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Geef voor het verzamelen van alle gebeurtenissen, ' * '.|
