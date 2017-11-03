---
title: Azure Diagnostics-extensie 1.3 en hoger configuratieschema | Microsoft Docs
description: Schemaversie 1.3 en hoger Azure diagnostics geleverd als onderdeel van de Microsoft Azure SDK 2.4 en hoger.
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
ms.openlocfilehash: 2ee66e0f41868d7d5411605596a22c00b5712896
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a>Azure Diagnostics 1.3 en hoger configuratieschema
> [!NOTE]
> De extensie Azure Diagnostics is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken van:
> - Azure Virtual Machines 
> - Schaalsets voor virtuele machines
> - Service Fabric 
> - Cloud Services 
> - Netwerkbeveiligingsgroepen
> 
> Deze pagina is alleen relevant als u een van deze services.

Deze pagina is ongeldig voor versies 1.3 en nieuwere (Azure SDK 2,4 en hoger). Nieuwere configuratiesecties zijn opgenomen als opmerkingen om weer te geven in welke versie ze zijn toegevoegd.  

Het configuratiebestand dat hier wordt beschreven, wordt gebruikt om diagnostische configuratie-instellingen wanneer de monitor diagnostics wordt gestart.  

De uitbreiding wordt gebruikt in combinatie met andere Microsoft-producten voor diagnostische gegevens zoals Azure Monitor, Application Insights en Log Analytics.



De schemadefinitie van de openbare configuratie bestand downloaden door het uitvoeren van de volgende PowerShell-opdracht:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

Zie voor meer informatie over het gebruik van Azure Diagnostics [Azure-extensie voor diagnostische gegevens](azure-diagnostics.md).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Voorbeeld van het configuratiebestand van de diagnostische gegevens  
 Het volgende voorbeeld ziet u een configuratiebestand typische diagnostische gegevens:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
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
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
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

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

        <DockerSources> <!-- Added in 1.9 --> 
          <Stats enabled="true" sampleRate="PT1M" scheduledTransferPeriod="PT1M" />
        </DockerSources>

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
        <Sink name="EventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryEventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryStorageAccount"> <!-- Added in 1.7 -->
          <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" />
        </Sink>
   </SinksConfig>

  </WadCfg>  

  <StorageAccount>diagstorageaccount</StorageAccount>
  <StorageType>TableAndBlob</StorageType> <!-- Added in 1.8 -->  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" sasToken="{sas token}"  />  <!-- sasToken in Private config added in 1.8.1 -->  
    <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
   
    <SecondaryStorageAccounts>
       <StorageAccount name="secondarydiagstorageaccount" key="{base64 encoded key}" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
   
    <SecondaryEventHubs>
       <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
    </SecondaryEventHubs>

  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  

JSON-equivalent van het vorige XML-configuratiebestand. 

De PublicConfig en PrivateConfig worden gescheiden omdat in de meeste gevallen voor json-gebruik, de als verschillende variabelen worden doorgegeven. Deze gevallen zijn onder andere Resource Manager-sjablonen, virtuele-machineschaalset PowerShell en Visual Studio. 

```json
"PublicConfig" {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 10000,
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT1M",
                        "unit": "percent"
                    }
                ]
            },
            "Directories": {
                "scheduledTransferPeriod": "PT5M",
                "IISLogs": {
                    "containerName": "iislogs"
                },
                "FailedRequestLogs": {
                    "containerName": "iisfailed"
                },
                "DataSources": [
                    {
                        "containerName": "mynewprocess",
                        "Absolute": {
                            "path": "C:\\MyNewProcess",
                            "expandEnvironment": false
                        }
                    },
                    {
                        "containerName": "badapp",
                        "Absolute": {
                            "path": "%SYSTEMDRIVE%\\BadApp",
                            "expandEnvironment": true
                        }
                    },
                    {
                        "containerName": "goodapp",
                        "LocalResource": {
                            "relativePath": "..\\PeanutButter",
                            "name": "Skippy"
                        }
                    }
                ]
            },
            "EtwProviders": {
                "sinks": "",
                "EtwEventSourceProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT5M",
                        "provider": "MyProviderClass",
                        "Event": [
                            {
                                "id": 0
                            },
                            {
                                "id": 1,
                                "eventDestination": "errorTable"
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ],
                "EtwManifestProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT2M",
                        "scheduledTransferLogLevelFilter": "Information",
                        "provider": "5974b00b-84c2-44bc-9e58-3a2451b4e3ad",
                        "Event": [
                            {
                                "id": 0
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT5M",
                "DataSource": [
                    {
                        "name": "System!*[System[Provider[@Name='Microsoft Antimalware']]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Verbose",
                "sinks": "ApplicationInsights.AppLogs"
            },
            "CrashDumps": {
                "directoryQuotaPercentage": 30,
                "dumpType": "Mini",
                "containerName": "wad-crashdumps",
                "CrashDumpConfiguration": [
                    {
                        "processName": "mynewprocess.exe"
                    },
                    {
                        "processName": "badapp.exe"
                    }
                ]
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "ApplicationInsights",
                    "ApplicationInsights": "{Insert InstrumentationKey}",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "Errors"
                            },
                            {
                                "logLevel": "Verbose",
                                "name": "AppLogs"
                            }
                        ]
                    }
                },
                {
                    "name": "EventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryEventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryStorageAccount",
                    "StorageAccount": {
                        "name": "secondarydiagstorageaccount",
                        "endpoint": "https://core.windows.net"
                    }
                }
            ]
        }
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```json
"PrivateConfig" {
    "storageAccountName": "diagstorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "EventHub": {
        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    },
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "key": "{base64 encoded key}",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    },
    "SecondaryEventHubs": {
        "EventHub": [
            {
                "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                "SharedAccessKeyName": "SendRule",
                "SharedAccessKey": "{base64 encoded key}"
            }
        ]
    }
}

```

## <a name="reading-this-page"></a>Deze pagina lezen  
 De volgende codes zijn ongeveer in volgorde weergegeven in het voorgaande voorbeeld.  Als u niet een volledige beschrijving waar u verwacht ziet, zoekt u de pagina voor het element of kenmerk.  

## <a name="common-attribute-types"></a>Algemene kenmerktypen  
 **scheduledTransferPeriod** kenmerk wordt weergegeven in verschillende elementen. Het interval tussen de geplande overdrachten naar opslag afgerond naar het dichtstbijzijnde aantal minuten is. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/schema/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration Element  
 *Structuur: Root - DiagnosticsConfiguration*

In versie 1.3 toegevoegd.  

Het element op het hoogste niveau van het configuratiebestand van de diagnostische gegevens.  

**Kenmerk** xmlns - de XML-naamruimte voor de diagnostics-configuratiebestand is:  
http://schemas.Microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  


|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**PublicConfig**|Vereist. Zie de beschrijving ergens anders op deze pagina.|  
|**PrivateConfig**|Optioneel. Zie de beschrijving ergens anders op deze pagina.|  
|**IsEnabled**|Booleaanse waarde. Zie de beschrijving ergens anders op deze pagina.|  

## <a name="publicconfig-element"></a>PublicConfig Element  
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig*

 Beschrijft de configuratie van de openbare diagnostische gegevens.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**WadCfg**|Vereist. Zie de beschrijving ergens anders op deze pagina.|  
|**StorageAccount**|De naam van de Azure Storage-account voor het opslaan van de gegevens in. Kan ook worden opgegeven als parameter bij het uitvoeren van de cmdlet Set-AzureServiceDiagnosticsExtension.|  
|**StorageType**|Kan *tabel*, *Blob*, of *TableAndBlob*. De tabel is standaard. Wanneer TableAndBlob is gekozen, diagnostische gegevens worden geschreven tweemaal--eenmaal voor elk type.|  
|**LocalResourceDirectory**|De map op de virtuele machine waar de gebeurtenisgegevens worden opgeslagen in de Monitoring Agent. Als dat niet wordt is ingesteld, de standaardmap gebruikt:<br /><br /> Voor een functie Worker/webservice:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Voor een virtuele Machine:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Vereiste kenmerken zijn:<br /><br /> - **pad** -de map op het systeem moet worden gebruikt door Azure Diagnostics.<br /><br /> - **expandEnvironment** -Hiermee wordt bepaald of omgevingsvariabelen in de padnaam worden uitgevouwen.|  

## <a name="wadcfg-element"></a>WadCFG Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG-*
 
 Identificeert en configureert u de telemetriegegevens te verzamelen.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration Element 
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*

 Vereist 

|Kenmerken|Beschrijving|  
|----------------|-----------------|  
| **overallQuotaInMB** | De maximale hoeveelheid ruimte op lokale schijf die kan worden gebruikt door de verschillende typen diagnostische gegevens verzameld door Azure Diagnostics. De standaardinstelling is 5120 MB.<br />
|**useProxyServer** | Azure Diagnostics voor het gebruik van de proxy-instellingen zoals in de instellingen van Internet Explorer configureren.|  

<br /> <br />

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**CrashDumps**|Zie de beschrijving ergens anders op deze pagina.|  
|**DiagnosticInfrastructureLogs**|Inschakelen van verzamelen van logboeken die worden gegenereerd door Azure Diagnostics. De infrastructuur voor diagnostische logboeken zijn nuttig voor het oplossen van het systeem diagnostische gegevens zelf. Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -configureert u de minimale ernst van de logboeken die worden verzameld.<br /><br /> - **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**Mappen**|Zie de beschrijving ergens anders op deze pagina.|  
|**EtwProviders**|Zie de beschrijving ergens anders op deze pagina.|  
|**Metrische gegevens**|Zie de beschrijving ergens anders op deze pagina.|  
|**PerformanceCounters**|Zie de beschrijving ergens anders op deze pagina.|  
|**WindowsEventLog**|Zie de beschrijving ergens anders op deze pagina.| 
|**DockerSources**|Zie de beschrijving ergens anders op deze pagina. | 



## <a name="crashdumps-element"></a>CrashDumps Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps-*
 
 Schakel het verzamelen van crashdumps.  

|Kenmerken|Beschrijving|  
|----------------|-----------------|  
|**containerName**|Optioneel. De naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van crashdumps.|  
|**crashDumpType**|Optioneel.  Hiermee configureert u Azure Diagnostics voor het verzamelen van dumpbestanden voor mini of volledige loopt vast.|  
|**directoryQuotaPercentage**|Optioneel.  Hiermee configureert u het percentage **overallQuotaInMB** moet worden gereserveerd voor crashdumps op de virtuele machine.|  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Vereist. Hiermee definieert u configuratiewaarden voor elk proces.<br /><br /> Het volgende kenmerk is ook vereist:<br /><br /> **Procesnaam** -de naam van het proces wilt u diagnostische Azure-gegevens voor het verzamelen van een crashdump voor.|  

## <a name="directories-element"></a>Mappen Element 
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mappen*

 Kan de verzameling van de inhoud van een map, logboeken van IIS is mislukt toegang aanvragen en/of IIS-logboeken.  

 Optionele **scheduledTransferPeriod** kenmerk. Zie eerder uitleg.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**IISLogs**|Met inbegrip van dit element in de configuratie, kunt de verzameling van IIS-logboeken:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van de IIS-logboeken.|   
|**FailedRequestLogs**|Met inbegrip van dit element in de configuratie kunt verzamelen van logboeken over mislukte aanvragen voor een IIS-site of toepassing. U moet ook de traceringsopties onder inschakelen **system. WebServer** in **Web.config**.|  
|**Gegevensbronnen**|Een lijst met mappen om te controleren.| 




## <a name="datasources-element"></a>Gegevensbronnen Element  
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mappen - gegevensbronnen*

 Een lijst met mappen om te controleren.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Vereist. Vereist kenmerk:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account dat moet worden gebruikt voor het opslaan van de logboekbestanden.|  





## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mappen - gegevensbronnen - DirectoryConfiguration-*

 Mogelijk bevat de **Absolute** of **LocalResource** element, maar niet beide.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**Absolute**|Het absolute pad naar de map bewaken. De volgende kenmerken zijn vereist:<br /><br /> - **Pad** -het absolute pad naar de map bewaken.<br /><br /> - **expandEnvironment** -configureert u of omgevingsvariabelen in pad worden uitgevouwen.|  
|**LocalResource**|Het pad ten opzichte van een lokale bron om te controleren. Vereiste kenmerken zijn:<br /><br /> - **Naam** -de lokale resource die de map voor het bewaken van bevat<br /><br /> - **relativePath** -het pad ten opzichte van de naam die de map voor het bewaken van bevat|  



## <a name="etwproviders-element"></a>EtwProviders Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders-*

 Hiermee configureert u de verzameling van ETW-gebeurtenissen van EventSource en/of ETW Manifest op basis van providers.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Hiermee configureert u de verzameling van gebeurtenissen die worden gegenereerd op basis van [EventSource klasse](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Vereist kenmerk:<br /><br /> **provider** -de naam van de klasse van de gebeurtenis EventSource.<br /><br /> Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -de minimum ernst om over te dragen naar uw opslagaccount.<br /><br /> - **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**EtwManifestProviderConfiguration**|Vereist kenmerk:<br /><br /> **provider** -de GUID van de gebeurtenisprovider<br /><br /> Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -de minimum ernst om over te dragen naar uw opslagaccount.<br /><br /> - **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*

 Hiermee configureert u de verzameling van gebeurtenissen die worden gegenereerd op basis van [EventSource klasse](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**DefaultEvents**|Het optionele kenmerk:<br/><br/> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Het optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**DefaultEvents**|Het optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Het optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  



## <a name="metrics-element"></a>Element van de metrische gegevens  
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - metrische gegevens*

 Hiermee kunt u voor het genereren van een tabel van prestaties teller die is geoptimaliseerd voor snelle query's. Elk prestatiemeteritem dat is gedefinieerd in de **PerformanceCounters** element is opgeslagen in de tabel metrische gegevens naast het prestatiemeteritem-tabel.  

 De **resourceId** kenmerk is vereist.  De resource-ID van de virtuele Machine of virtuele-Machineschaalset die u om Azure Diagnostics implementeert. Ophalen van de **resourceID** van de [Azure-portal](https://portal.azure.com). Selecteer **Bladeren** -> **resourcegroepen** -> **< naam\>**. Klik op de **eigenschappen** tegel en kopieer de waarde van de **ID** veld.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**MetricAggregation**|Vereist kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters-*

 Hiermee kunt het verzamelen van prestatiemeteritems.  

 Het optionele kenmerk:  

 Optionele **scheduledTransferPeriod** kenmerk. Zie eerder uitleg.

|Onderliggend Element|Beschrijving|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|De volgende kenmerken zijn vereist:<br /><br /> - **counterSpecifier** -de naam van het prestatiemeteritem. Bijvoorbeeld `\Processor(_Total)\% Processor Time`. Als u een lijst met prestatiemeteritems op de host, voert u de opdracht `typeperf`.<br /><br /> - **sampleRate** -hoe vaak de teller moet actieve.<br /><br /> Het optionele kenmerk:<br /><br /> **eenheid** -de eenheid van de teller.|  




## <a name="windowseventlog-element"></a>WindowsEventLog Element
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog-*
 
 Kan de verzameling van het Windows-gebeurtenislogboeken.  

 Optionele **scheduledTransferPeriod** kenmerk. Zie eerder uitleg.  

|Onderliggend Element|Beschrijving|  
|-------------------|-----------------|  
|**Gegevensbron**|De Windows-gebeurtenislogboeken te verzamelen. Vereist kenmerk:<br /><br /> **naam** : de XPath-query met een beschrijving van de windows-gebeurtenissen te verzamelen. Bijvoorbeeld:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> Geef voor het verzamelen van alle gebeurtenissen, ' * '|  




## <a name="logs-element"></a>Logboeken Element  
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Logboeken*

 Aanwezig zijn op versie 1.0 en 1.1. 1.2 ontbreken. Terug in 1.3 toegevoegd.  

 Definieert de configuratie van de buffer voor basic-Azure Logboeken.  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**unsignedInt**|Optioneel. Hiermee geeft u de maximale hoeveelheid opslagruimte op het bestand systeem die beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilterr**|**tekenreeks**|Optioneel. Hiermee geeft u het minimale ernstniveau van logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**, die alle logboeken overdraagt. Andere mogelijke waarden (in volgorde van meest naar minst informatie) zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **Kritiek**.|  
|**scheduledTransferPeriod**|**duur**|Optioneel. Hiermee geeft u het interval tussen geplande de overdracht van gegevens, afgerond naar het dichtstbijzijnde aantal minuten.<br /><br /> De standaardwaarde is PT0S.|  
|**sinks** toegevoegd in 1.5|**tekenreeks**|Optioneel. Verwijst naar een locatie sink ook diagnostische gegevens verzenden. Bijvoorbeeld, Application Insights.|  

## <a name="dockersources"></a>DockerSources
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources-*

 In 1,9 toegevoegd.

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**Statistieken**|Geeft het systeem voor het verzamelen van statistieken voor Docker-containers|  

## <a name="sinksconfig-element"></a>SinksConfig Element  
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*

 Een lijst met locaties voor het verzenden van diagnostische gegevens naar en de configuratie die is gekoppeld aan deze locaties.  

|Elementnaam|Beschrijving|  
|------------------|-----------------|  
|**Sink**|Zie de beschrijving ergens anders op deze pagina.|  

## <a name="sink-element"></a>Sink-Element
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink-*

 In versie 1.5 toegevoegd.  

 Locaties voor het verzenden van diagnostische gegevens naar definieert. De Application Insights-service.  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**naam**|Tekenreeks|Een tekenreeks die de sinkname te identificeren.|  

|Element|Type|Beschrijving|  
|-------------|----------|-----------------|  
|**Application Insights**|Tekenreeks|Alleen gebruikt wanneer het verzenden van gegevens naar Application Insights. De Instrumentatiesleutel voor een actieve Application Insights-account dat u toegang tot hebt bevatten.|  
|**Kanalen**|Tekenreeks|Één voor elke extra filteren die u stream|  

## <a name="channels-element"></a>Kanalen Element  
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanalen*

 In versie 1.5 toegevoegd.  

 Hiermee definieert u filters voor stromen van logboekgegevens een sink te doorlopen.  

|Element|Type|Beschrijving|  
|-------------|----------|-----------------|  
|**Kanaal**|Tekenreeks|Zie de beschrijving ergens anders op deze pagina.|  

## <a name="channel-element"></a>Kanaal-Element
 *Structuur: Basis - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanalen - kanaal*

 In versie 1.5 toegevoegd.  

 Locaties voor het verzenden van diagnostische gegevens naar definieert. De Application Insights-service.  

|Kenmerken|Type|Beschrijving|  
|----------------|----------|-----------------|  
|**logLevel**|**tekenreeks**|Hiermee geeft u het minimale ernstniveau van logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**, die alle logboeken overdraagt. Andere mogelijke waarden (in volgorde van meest naar minst informatie) zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **Kritiek**.|  
|**naam**|**tekenreeks**|Een unieke naam van het kanaal om te verwijzen naar|  


## <a name="privateconfig-element"></a>PrivateConfig Element 
 *Structuur: Basis - DiagnosticsConfiguration - PrivateConfig*

 In versie 1.3 toegevoegd.  

 Optioneel  

 Slaat de persoonlijke gegevens van het opslagaccount (naam, sleutel en -eindpunt). Deze informatie wordt verzonden naar de virtuele machine, maar kan niet worden opgehaald uit.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**StorageAccount**|Het storage-account te gebruiken. De volgende kenmerken zijn vereist<br /><br /> - **naam** -de naam van het opslagaccount.<br /><br /> - **sleutel** -de sleutel tot het opslagaccount.<br /><br /> - **eindpunt** -het eindpunt voor toegang tot het opslagaccount. <br /><br /> -**sasToken** (toegevoegd 1.8.1)-kunt u een SAS-token in plaats van de sleutel van een opslagaccount in de persoonlijke configuratie. Indien opgegeven, wordt de sleutel van het opslagaccount wordt genegeerd. <br />Vereisten voor de SAS-Token: <br />-Ondersteunt alleen account SAS-token <br />- *b*, *t* servicetypen zijn vereist. <br /> - *een*, *c*, *u*, *w* machtigingen zijn vereist. <br /> - *c*, *o* brontypen die zijn vereist. <br /> -Ondersteunt alleen het HTTPS-protocol <br /> -Starten en het verlooptijdstip moet geldig zijn.|  


## <a name="isenabled-element"></a>IsEnabled Element  
 *Structuur: Basis - DiagnosticsConfiguration - IsEnabled*

 Booleaanse waarde. Gebruik `true` de diagnostische gegevens inschakelen of `false` uitschakelen van de diagnostische gegevens.
