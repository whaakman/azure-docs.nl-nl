---
title: Configuratie schema Azure Diagnostics-extensie 1,2
description: ALLEEN relevant als u Azure SDK 2,5 met Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric of Cloud Services gebruikt.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: dae74e730d6e175fa3e447150adce4caecd3d7a3
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "60237837"
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Azure Diagnostics 1,2-Configuratie schema
> [!NOTE]
> Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatie meter items en andere statistieken van Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric en Cloud Services.  Deze pagina is alleen relevant als u een van deze services gebruikt.
>

Azure Diagnostics wordt gebruikt in combi natie met andere micro soft Diagnostics-producten, zoals Azure Monitor, waaronder Application Insights en Log Analytics.

Dit schema definieert de mogelijke waarden die u kunt gebruiken voor het initialiseren van de diagnostische configuratie-instellingen wanneer de diagnostische monitor wordt gestart.  


 Down load de schema definitie van het open bare configuratie bestand door de volgende Power shell-opdracht uit te voeren:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Zie voor meer informatie over het gebruik van Azure Diagnostics [Diagnostische gegevens inschakelen in Azure Cloud Services](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Voor beeld van het configuratie bestand voor diagnostische gegevens  
 In het volgende voor beeld ziet u een typisch configuratie bestand voor diagnostische gegevens:  

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

## <a name="diagnostics-configuration-namespace"></a>Configuratie naam ruimte voor diagnostische gegevens  
 De XML-naam ruimte voor het diagnostische configuratie bestand is:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig-element  
 Element op het hoogste niveau van het diagnostische configuratie bestand. De volgende tabel beschrijft de elementen van het configuratie bestand.  

|Element naam|Description|  
|------------------|-----------------|  
|**WadCfg**|Vereist. De configuratie-instellingen voor de telemetriegegevens die moeten worden verzameld.|  
|**StorageAccount**|De naam van het Azure Storage-account waarin de gegevens moeten worden opgeslagen. Dit kan ook worden opgegeven als een para meter bij het uitvoeren van de cmdlet Set-AzureServiceDiagnosticsExtension.|  
|**LocalResourceDirectory**|De map op de virtuele machine die door de bewakings agent moet worden gebruikt om gebeurtenis gegevens op te slaan. Als deze niet is ingesteld, wordt de standaard Directory gebruikt:<br /><br /> Voor een werk nemer/Web-rol:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Voor een virtuele machine:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> De vereiste kenmerken zijn:<br /><br /> -                      **pad** : de map op het systeem die moet worden gebruikt door Azure Diagnostics.<br /><br /> -                      **expandEnvironment** : Hiermee bepaalt u of omgevings variabelen in de padnaam worden uitgevouwen.|  

## <a name="wadcfg-element"></a>WadCFG-element  
Definieert configuratie-instellingen voor de telemetriegegevens die moeten worden verzameld. In de volgende tabel worden onderliggende elementen beschreven:  

|De naam van element|Description|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Vereist. Optionele kenmerken zijn:<br /><br /> -                     **overallQuotaInMB** : de maximale hoeveelheid lokale schijf ruimte die kan worden gebruikt door de verschillende typen diagnostische gegevens die worden verzameld door Azure Diagnostics. De standaard instelling is 5120MB.<br /><br /> -                     **useProxyServer** : Configureer Azure Diagnostics om de instellingen van de proxy server te gebruiken zoals ingesteld in de instellingen van Internet Explorer.|  
|**CrashDumps**|Het verzamelen van crash dumps inschakelen. Optionele kenmerken zijn:<br /><br /> -                     **containerName** : de naam van de BLOB-container in uw Azure Storage-account die moet worden gebruikt voor het opslaan van crash dumps.<br /><br /> -                     **crashDumpType** : Hiermee configureert u Azure Diagnostics voor het verzamelen van Mini maal of volledige crash dumps.<br /><br /> -                     **directoryQuotaPercentage**: Hiermee configureert u het percentage **overallQuotaInMB** dat moet worden gereserveerd voor crash DUMPS op de virtuele machine.|  
|**DiagnosticInfrastructureLogs**|Verzamelen van logboeken die zijn gegenereerd door Azure Diagnostics inschakelen. De logboeken voor de diagnose infrastructuur zijn handig voor het oplossen van problemen met het diagnose systeem zelf. Optionele kenmerken zijn:<br /><br /> -                     **scheduledTransferLogLevelFilter** : Hiermee configureert u het minimale Ernst niveau van de verzamelde Logboeken.<br /><br /> -                     **scheduledTransferPeriod** : het interval tussen geplande transfers naar opslag die is afgerond op de dichtstbijzijnde minuut. De waarde is een [XML-duur-gegevens type.](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**Adreslijsten**|Hiermee schakelt u de verzameling van de inhoud van een map in, IIS heeft geen toegang tot logboeken en/of IIS-logboeken. Optioneel kenmerk:<br /><br /> **scheduledTransferPeriod** : het interval tussen geplande transfers naar opslag die is afgerond op de dichtstbijzijnde minuut. De waarde is een [XML-duur-gegevens type.](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**EtwProviders**|Hiermee configureert u de verzameling van ETW-gebeurtenissen van Event source-en/of ETW-manifest providers.|  
|**Metrische gegevens**|Met dit element kunt u een tabel voor prestatie meter items genereren die is geoptimaliseerd voor snelle query's. Elk prestatie meter item dat in het **Performance Counters** -element is gedefinieerd, wordt opgeslagen in de tabel metrische gegevens, naast de tabel prestatie meter items. Vereist kenmerk:<br /><br /> **resourceId** -dit is de resource-id van de virtuele machine waarop u Azure Diagnostics implementeert. Haal de **resourceID** op uit de [Azure Portal](https://portal.azure.com). Selecteer **Bladeren** -> **resource groepen** ->  **< naam.\>** Klik op de tegel **Eigenschappen** en kopieer de waarde uit het veld **id** .|  
|**Performance Counters**|Hiermee schakelt u het verzamelen van prestatie meter items in. Optioneel kenmerk:<br /><br /> **scheduledTransferPeriod** : het interval tussen geplande transfers naar opslag die is afgerond op de dichtstbijzijnde minuut. De waarde is een [XML-gegevens type duur.](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Hiermee schakelt u de verzameling van Windows-gebeurtenis Logboeken. Optioneel kenmerk:<br /><br /> **scheduledTransferPeriod** : het interval tussen geplande transfers naar opslag die is afgerond op de dichtstbijzijnde minuut. De waarde is een [XML-gegevens type duur.](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps-element  
 Hiermee kunt u crash dumps verzamelen. In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Vereist. Vereist kenmerk:<br /><br /> naam verwerkings proces: de namen van de processen die u Azure Diagnostics wilt voor het verzamelen van een crash dump voor.|  
|**crashDumpType**|Hiermee configureert u Azure Diagnostics voor het verzamelen van Mini maal of volledige crash dumps.|  
|**directoryQuotaPercentage**|Hiermee configureert u het percentage **overallQuotaInMB** dat moet worden gereserveerd voor crash dumps op de virtuele machine.|  

## <a name="directories-element"></a>Element directory's  
 Hiermee schakelt u de verzameling van de inhoud van een map in, IIS heeft geen toegang tot logboeken en/of IIS-logboeken. In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**Gegevens bronnen**|Een lijst met mappen die moeten worden bewaakt.|  
|**FailedRequestLogs**|Met inbegrip van dit element in de configuratie kan het verzamelen van logboeken over mislukte aanvragen naar een IIS-site of-toepassing. U moet ook tracerings opties inschakelen onder **systeem. Webserver** in **Web. config**.|  
|**IISLogs**|Met dit element in de configuratie kan IIS-logboeken worden verzameld:<br /><br /> **containerName** : de naam van de BLOB-container in uw Azure Storage-account die moet worden gebruikt om de IIS-logboeken op te slaan.|  

## <a name="datasources-element"></a>Element gegevens bronnen  
 Een lijst met mappen die moeten worden bewaakt. In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Vereist. Vereist kenmerk:<br /><br /> **containerName** : de naam van de BLOB-container in uw Azure Storage-account die moet worden gebruikt om de logboek bestanden op te slaan.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration-element  
 **DirectoryConfiguration** kunnen het **absolute** of het **LocalResource** -element bevatten, maar niet beide. In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**Absolute**|Het absolute pad naar de map die u wilt bewaken. De volgende kenmerken zijn vereist:<br /><br /> -                     **Pad** : het absolute pad naar de map die u wilt bewaken.<br /><br /> -                      **expandEnvironment** : Hiermee configureert u of omgevings variabelen in het pad worden uitgevouwen.|  
|**LocalResource**|Het pad ten opzichte van een lokale bron die moet worden bewaakt. De vereiste kenmerken zijn:<br /><br /> -                     **Naam** : de lokale resource die de directory bevat die moet worden bewaakt<br /><br /> -                     **relativePath** : het pad ten opzichte van de naam die de te bewaken Directory bevat|  

## <a name="etwproviders-element"></a>EtwProviders-element  
 Hiermee configureert u de verzameling van ETW-gebeurtenissen van Event source-en/of ETW-manifest providers. In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Hiermee configureert u de verzameling van gebeurtenissen die zijn gegenereerd met de [klasse Event source](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Vereist kenmerk:<br /><br /> **provider** : de klassenaam van de gebeurtenis Event source.<br /><br /> Optionele kenmerken zijn:<br /><br /> -                     **scheduledTransferLogLevelFilter** : het minimale Ernst niveau dat u wilt overdragen naar uw opslag account.<br /><br /> -                     **scheduledTransferPeriod** : het interval tussen geplande transfers naar opslag die is afgerond op de dichtstbijzijnde minuut. Waarde is het [gegevens type XML-duur](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Vereist kenmerk:<br /><br /> **provider** -de GUID van de gebeurtenis provider<br /><br /> Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** : het minimale Ernst niveau dat u wilt overdragen naar uw opslag account.<br /><br /> -                     **scheduledTransferPeriod** : het interval tussen geplande transfers naar opslag die is afgerond op de dichtstbijzijnde minuut. Waarde is het [gegevens type XML-duur](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 Hiermee configureert u de verzameling van gebeurtenissen die zijn gegenereerd met de [klasse Event source](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**DefaultEvents**|Optioneel kenmerk:<br /><br /> **eventDestination** : de naam van de tabel waarin de gebeurtenissen moeten worden opgeslagen|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** : de id van de gebeurtenis.<br /><br /> Optioneel kenmerk:<br /><br /> **eventDestination** : de naam van de tabel waarin de gebeurtenissen moeten worden opgeslagen|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**DefaultEvents**|Optioneel kenmerk:<br /><br /> **eventDestination** : de naam van de tabel waarin de gebeurtenissen moeten worden opgeslagen|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** : de id van de gebeurtenis.<br /><br /> Optioneel kenmerk:<br /><br /> **eventDestination** : de naam van de tabel waarin de gebeurtenissen moeten worden opgeslagen|  

## <a name="metrics-element"></a>Element metrische gegevens  
 Hiermee kunt u een tabel voor prestatie meter items genereren die is geoptimaliseerd voor snelle query's. In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**MetricAggregation**|Vereist kenmerk:<br /><br /> **scheduledTransferPeriod** : het interval tussen geplande transfers naar opslag die is afgerond op de dichtstbijzijnde minuut. Waarde is het [gegevens type XML-duur](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>Performance Counters-element  
 Hiermee schakelt u het verzamelen van prestatie meter items in. In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|De volgende kenmerken zijn vereist:<br /><br /> -                     **counterSpecifier** : de naam van het prestatie meter item. Bijvoorbeeld `\Processor(_Total)\% Processor Time`. Voer de opdracht `typeperf`uit om een lijst met prestatie meter items op uw host op te halen.<br /><br /> -                     **sampleRate** : hoe vaak de teller moet worden gesampled.<br /><br /> Optioneel kenmerk:<br /><br /> **eenheid** : de maat eenheid van het prestatie meter item.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration-element  
 In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**aantekening**|Vereist kenmerk:<br /><br /> **DisplayName** : de weergave naam voor het prestatie meter item<br /><br /> Optioneel kenmerk:<br /><br /> **land instelling** : de land instelling die moet worden gebruikt bij het weer geven van de naam van het item|  

## <a name="windowseventlog-element"></a>WindowsEventLog-element  
 In de volgende tabel worden onderliggende elementen beschreven:  

|Element naam|Description|  
|------------------|-----------------|  
|**Bron**|De Windows-gebeurtenis logboeken die moeten worden verzameld. Vereist kenmerk:<br /><br /> **naam** : de XPath-query die de Windows-gebeurtenissen beschrijft die moeten worden verzameld. Bijvoorbeeld:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Geef ' * ' op om alle gebeurtenissen te verzamelen.|

