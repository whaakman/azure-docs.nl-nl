---
title: De extensie van Azure Diagnostics 1.3 en hoger configuratieschema
description: Schemaversie 1.3 en hoger van Azure diagnostics geleverd als onderdeel van de Microsoft Azure SDK 2.4 en hoger.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/20/2018
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: 5e74a845fbf263791e3e07006e79e4332646dfd5
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256303"
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a>Azure Diagnostics 1.3 en hoger configuratieschema
> [!NOTE]
> De Azure Diagnostics-extensie is het onderdeel dat wordt gebruikt voor het verzamelen van prestatiemeteritems en andere statistieken met:
> - Azure Virtual Machines
> - Virtual Machine Scale Sets
> - Service Fabric
> - Cloud Services
> - Netwerkbeveiligingsgroepen
>
> Deze pagina is alleen van toepassing als u een van deze services.

Deze pagina is geldig voor versie 1.3 en nieuwer (Azure SDK 2.4 en hoger). Nieuwere configuratiesecties zijn opgenomen als om weer te geven in welke versie ze zijn toegevoegd.  

Het configuratiebestand dat hier wordt beschreven, wordt gebruikt om in te stellen diagnostische configuratie-instellingen wanneer de monitor diagnostische gegevens wordt gestart.  

De extensie wordt gebruikt in combinatie met andere Microsoft-producten voor diagnostische gegevens, zoals Azure Monitor, Application Insights en Log Analytics.



De schemadefinitie voor de configuratie van de openbare-bestand downloaden door het uitvoeren van de volgende PowerShell-opdracht:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

Zie voor meer informatie over het gebruik van Azure Diagnostics [Azure Diagnostics-extensie](azure-diagnostics.md).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Voorbeeld van het configuratiebestand van de diagnostische gegevens  
 Het volgende voorbeeld ziet u een configuratiebestand typische diagnostische gegevens:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M", sinks="AzureMonitorSink">  
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
        <Sink name="AzureMonitorSink">
            <AzureMonitor> <!-- Added in 1.11 -->
                <resourceId>{insert resourceId}</ResourceId> <!-- Parameter only needed for classic VMs and Classic Cloud Services, exclude VMSS and Resource Manager VMs-->
                <Region>{insert Azure region of resource}</Region> <!-- Parameter only needed for classic VMs and Classic Cloud Services, exclude VMSS and Resource Manager VMs -->
            </AzureMonitor>
        </Sink>
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

    <AzureMonitorAccount>
        <ServicePrincipalMeta> <!-- Added in 1.11; only needed for classic VMs and Classic cloud services -->
            <PrincipalId>{Insert service principal clientId}</PrincipalId>
            <Secret>{Insert service principal client secret}</Secret>
        </ServicePrincipalMeta>
    </AzureMonitorAccount>

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
> [!NOTE]
> De configuratie van de openbare Azure Monitor-sink-definitie heeft twee eigenschappen, resourceId en regio. Deze zijn alleen vereist voor klassieke virtuele machines en klassieke Cloud services. Deze eigenschappen mogen niet worden gebruikt voor Resource Manager Virtual Machines of virtuele-machineschaalsets.
> Er is ook een extra element in de persoonlijke configuratie voor de Azure Monitor-sink, die wordt doorgegeven in een Principal-Id en geheim. Dit is alleen vereist voor klassieke virtuele machines en klassieke Cloud Services. Voor Resource Manager-virtuele machines en VMSS de Azure Monitor kan definitie in het persoonlijke configuratie-element worden uitgesloten.
>

JSON-equivalent van de vorige XML-configuratiebestand.

De PublicConfig en PrivateConfig worden gescheiden omdat in de meeste gevallen voor json-gebruik, ze als verschillende variabelen doorgegeven worden. Deze Resource Manager-sjablonen voor gegevensuitwisseling, virtuele-machineschaalset PowerShell en Visual Studio.

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
                "sinks": "AzureMonitorSink",
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
                    "name": "AzureMonitorSink",
                    "AzureMonitor":
                    {
                        "ResourceId": "{insert resourceId if a classic VM or cloud service, else property not needed}",
                        "Region": "{insert Azure region of resource if a classic VM or cloud service, else property not needed}"
                    }
                },
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

> [!NOTE]
> De configuratie van de openbare Azure Monitor-sink-definitie heeft twee eigenschappen, resourceId en regio. Deze zijn alleen vereist voor klassieke virtuele machines en klassieke Cloud services.
> Deze eigenschappen mogen niet worden gebruikt voor Resource Manager Virtual Machines of virtuele-machineschaalsets.
>

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
    "AzureMonitorAccount": {
        "ServicePrincipalMeta": {
            "PrincipalId": "{Insert service principal client Id}",
            "Secret": "{Insert service principal client secret}"
        }
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

> [!NOTE]
> Er is een extra element in de persoonlijke configuratie voor de Azure Monitor-sink, die wordt doorgegeven in een Principal-Id en geheim. Dit is alleen vereist voor klassieke virtuele machines en klassieke Cloud Services. Voor Resource Manager-virtuele machines en VMSS de Azure Monitor kan definitie in het persoonlijke configuratie-element worden uitgesloten.
>


## <a name="reading-this-page"></a>Deze pagina lezen  
 De volgende codes zijn grofweg in volgorde weergegeven in het voorgaande voorbeeld.  Als u niet een volledige beschrijving waar u verwacht dat het ziet, zoekt u de pagina voor het element of kenmerk.  

## <a name="common-attribute-types"></a>Algemene kenmerktypen  
 **scheduledTransferPeriod** kenmerk wordt weergegeven in de verschillende elementen. Dit is het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/xml/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration-Element  
 *Structuur: Root - DiagnosticsConfiguration*

Toegevoegd in versie 1.3.  

Het element op het hoogste niveau van het configuratiebestand van de diagnostische gegevens.  

**Kenmerk** xmlns - de XML-naamruimte voor het configuratiebestand van de diagnostische gegevens is:  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  


|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**PublicConfig**|Vereist. Zie de beschrijving elders op deze pagina.|  
|**PrivateConfig**|Optioneel. Zie de beschrijving elders op deze pagina.|  
|**isEnabled**|Booleaanse waarde. Zie de beschrijving elders op deze pagina.|  

## <a name="publicconfig-element"></a>PublicConfig-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig*

 Beschrijving van de configuratie van de openbare diagnostische gegevens.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**WadCfg**|Vereist. Zie de beschrijving elders op deze pagina.|  
|**StorageAccount**|De naam van de Azure Storage-account voor het opslaan van de gegevens in. Kan ook worden opgegeven als parameter bij het uitvoeren van de cmdlet Set-AzureServiceDiagnosticsExtension.|  
|**StorageType**|Kan *tabel*, *Blob*, of *TableAndBlob*. De tabel is standaard. Wanneer TableAndBlob is gekozen, diagnostische gegevens worden geschreven tweemaal--eenmaal voor elk type.|  
|**LocalResourceDirectory**|De map op de virtuele machine waar gebeurtenisgegevens worden opgeslagen in de Monitoring Agent. Als dit niet het geval is, wordt stelt de standaard-map gebruikt:<br /><br /> Voor een rol Worker/webservice: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Voor een virtuele Machine: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Vereiste kenmerken zijn:<br /><br /> - **pad** -de map op het systeem moet worden gebruikt door Azure Diagnostics.<br /><br /> - **expandEnvironment** -bepaalt of omgevingsvariabelen worden uitgebreid in het padnaam.|  

## <a name="wadcfg-element"></a>WadCFG-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*

 Identificeert en configureert u de telemetriegegevens te verzamelen.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration-Element
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*

 Vereist

|Kenmerken|Beschrijving|  
|----------------|-----------------|  
| **overallQuotaInMB** | De maximale hoeveelheid lokale schijfruimte die kan worden gebruikt door de verschillende typen diagnostische gegevens die zijn verzameld door Azure Diagnostics. De standaardinstelling is 4096 MB.<br />
|**useProxyServer** | Azure Diagnostics voor het gebruik van de proxy-instellingen zoals in de instellingen van Internet Explorer configureren.|
|**sinks** | In 1.5 toegevoegd. Optioneel. Verwijst naar een sink-locatie voor het verzenden van diagnostische gegevens voor alle onderliggende elementen die ondersteuning bieden voor sinks ook. Voorbeeld van de sink is Application Insights of Event Hubs.|  


<br /> <br />

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**CrashDumps**|Zie de beschrijving elders op deze pagina.|  
|**DiagnosticInfrastructureLogs**|Het verzamelen van logboeken die worden gegenereerd door Azure Diagnostics inschakelen. De logboeken met diagnostische gegevens over infrastructuur zijn handig voor het oplossen van het systeem diagnostics zelf. Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -configureert u het minimale ernstniveau van de logboeken die worden verzameld.<br /><br /> - **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/xml/schema_dtypes_date.asp) |  
|**Adreslijsten**|Zie de beschrijving elders op deze pagina.|  
|**EtwProviders**|Zie de beschrijving elders op deze pagina.|  
|**Metrische gegevens**|Zie de beschrijving elders op deze pagina.|  
|**PerformanceCounters**|Zie de beschrijving elders op deze pagina.|  
|**WindowsEventLog**|Zie de beschrijving elders op deze pagina.|
|**DockerSources**|Zie de beschrijving elders op deze pagina. |



## <a name="crashdumps-element"></a>CrashDumps-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*

 Schakel het verzamelen van crashdumps.  

|Kenmerken|Beschrijving|  
|----------------|-----------------|  
|**containerName**|Optioneel. De naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van crashdumps.|  
|**crashDumpType**|Optioneel.  Hiermee configureert u Azure Diagnostics voor het verzamelen van dumpbestanden voor foutopsporing mini of volledige loopt vast.|  
|**directoryQuotaPercentage**|Optioneel.  Hiermee configureert u het percentage van de **overallQuotaInMB** moet worden gereserveerd voor crashdumps op de virtuele machine.|  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Vereist. Definieert de configuratiewaarden voor elk proces.<br /><br /> Het volgende kenmerk is ook vereist:<br /><br /> **Procesnaam** -de naam van het proces dat u wilt dat Azure Diagnostics voor het verzamelen van een crashdump voor.|  

## <a name="directories-element"></a>Mappen-Element
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mappen*

 Zorgt ervoor dat de inhoud van een map, IIS kan geen toegang tot aanvraag logboeken en/of IIS-logboeken.  

 Optionele **scheduledTransferPeriod** kenmerk. Zie de uitleg eerder.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**IISLogs**|Met inbegrip van dit element in de configuratie kunt u het verzamelen van IIS-logboeken:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account moet worden gebruikt voor het opslaan van de IIS-logboeken.|   
|**FailedRequestLogs**|Met inbegrip van dit element in de configuratie kunt verzamelen van logboeken over mislukte aanvragen voor een IIS-site of toepassing. U moet ook traceringsopties onder inschakelen **system. WebServer** in **Web.config**.|  
|**Gegevensbronnen**|Een lijst met mappen om te controleren.|




## <a name="datasources-element"></a>Gegevensbronnen-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mappen - gegevensbronnen*

 Een lijst met mappen om te controleren.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Vereist. Vereist kenmerk:<br /><br /> **containerName** -de naam van de blob-container in uw Azure Storage-account dat moet worden gebruikt voor het opslaan van de logboekbestanden.|  





## <a name="directoryconfiguration-element"></a>DirectoryConfiguration-Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mappen - gegevensbronnen - DirectoryConfiguration-*

 Mogelijk bevat het **Absolute** of **LocalResource** element, maar niet beide.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**Absolute**|Het absolute pad naar de map om te controleren. De volgende kenmerken zijn vereist:<br /><br /> - **Pad** -het absolute pad naar de map om te controleren.<br /><br /> - **expandEnvironment** -configureert u of omgevingsvariabelen in het pad worden uitgevouwen.|  
|**LocalResource**|Het pad relatief ten opzichte van een lokale bron om te controleren. Vereiste kenmerken zijn:<br /><br /> - **Naam** -de lokale resource die de map voor het bewaken van bevat<br /><br /> - **relativePath** -het pad relatief ten opzichte van de naam die de map voor het bewaken van bevat|  



## <a name="etwproviders-element"></a>EtwProviders-Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders-*

 Hiermee configureert u verzamelen van ETW-gebeurtenissen uit de gebeurtenisbron en/of ETW Manifest op basis van de providers.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Hiermee configureert u verzamelen van gebeurtenissen die zijn gegenereerd op basis van [EventSource klasse](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Vereist kenmerk:<br /><br /> **provider** -de naam van de klasse van de gebeurtenis gebeurtenisbron.<br /><br /> Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -het minimale ernstniveau om over te dragen naar uw opslagaccount.<br /><br /> - **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/xml/schema_dtypes_date.asp) |  
|**EtwManifestProviderConfiguration**|Vereist kenmerk:<br /><br /> **provider** -de GUID van de gebeurtenisprovider<br /><br /> Optionele kenmerken zijn:<br /><br /> - **scheduledTransferLogLevelFilter** -het minimale ernstniveau om over te dragen naar uw opslagaccount.<br /><br /> - **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/xml/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*

 Hiermee configureert u verzamelen van gebeurtenissen die zijn gegenereerd op basis van [EventSource klasse](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**DefaultEvents**|Optionele kenmerk:<br/><br/> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration-Element  
 *Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**DefaultEvents**|Optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  
|**Gebeurtenis**|Vereist kenmerk:<br /><br /> **id** -de id van de gebeurtenis.<br /><br /> Optionele kenmerk:<br /><br /> **eventDestination** -de naam van de tabel voor het opslaan van de gebeurtenissen in|  



## <a name="metrics-element"></a>Element van de metrische gegevens  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - metrische gegevens*

 Hiermee kunt u voor het genereren van een tabel met prestaties teller die is geoptimaliseerd voor snelle query's. Elk prestatiemeteritem dat is gedefinieerd in de **PerformanceCounters** element wordt opgeslagen in de metrische gegevenstabel voor naast het prestatiemeteritem-tabel.  

 De **resourceId** kenmerk is vereist.  De resource-ID van de virtuele Machine of virtuele-Machineschaalset implementeert u om de Azure Diagnostics. Krijgen de **resourceID** uit de [Azure-portal](https://portal.azure.com). Selecteer **Bladeren** -> **resourcegroepen** -> **< naam\>**. Klik op de **eigenschappen** tegel en kopieer de waarde van de **ID** veld.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**MetricAggregation**|Vereist kenmerk:<br /><br /> **scheduledTransferPeriod** -het interval tussen de geplande overdrachten naar de opslag naar boven afgerond op de dichtstbijzijnde minuut. De waarde is een [XML "Duur van het gegevenstype."](http://www.w3schools.com/xml/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters-Element  
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters-*

 Kunt u het verzamelen van prestatiemeteritems.  

 Optionele kenmerk:  

 Optionele **scheduledTransferPeriod** kenmerk. Zie de uitleg eerder.

|Onderliggend Element|Beschrijving|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|De volgende kenmerken zijn vereist:<br /><br /> - **counterSpecifier** -de naam van het prestatiemeteritem. Bijvoorbeeld `\Processor(_Total)\% Processor Time`. Als u een lijst met prestatiemeteritems op uw host, voert u de opdracht `typeperf`.<br /><br /> - **sampleRate** -hoe vaak het item dat moet worden verzameld.<br /><br /> Optionele kenmerk:<br /><br /> **eenheid** -de eenheid van de teller.|
|**sinks** | In 1.5 toegevoegd. Optioneel. Verwijst naar een sink-locatie voor het verzenden van diagnostische gegevens ook. Azure Monitor of Event Hubs.|    




## <a name="windowseventlog-element"></a>WindowsEventLog-Element
 *Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*

 Kunt u het verzamelen van de Windows-gebeurtenislogboeken.  

 Optionele **scheduledTransferPeriod** kenmerk. Zie de uitleg eerder.  

|Onderliggend Element|Beschrijving|  
|-------------------|-----------------|  
|**Gegevensbron**|De Windows-gebeurtenislogboeken te verzamelen. Vereist kenmerk:<br /><br /> **naam** : de XPath-query met een beschrijving van de windows-gebeurtenissen te verzamelen. Bijvoorbeeld:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> Geef voor het verzamelen van alle gebeurtenissen, ' * '|  




## <a name="logs-element"></a>Logboeken-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Logboeken*

 Aanwezig zijn in versie 1.0 en 1.1. Ontbreekt in 1.2. Terug in 1.3 toegevoegd.  

 Definieert de configuratie van de buffer voor basic Logboeken in Azure.  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**unsignedInt**|Optioneel. Hiermee geeft u de maximale hoeveelheid opslag bestandssysteem dat beschikbaar is voor de opgegeven gegevens.<br /><br /> De standaardwaarde is 0.|  
|**scheduledTransferLogLevelFilterr**|**Tekenreeks**|Optioneel. Hiermee geeft u het minimale ernstniveau voor logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**, waarop alle logboeken gegevensoverdrachten. Andere mogelijke waarden (in volgorde van meest naar minst informatie) zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **Kritieke**.|  
|**scheduledTransferPeriod**|**Duur**|Optioneel. Hiermee geeft u het interval tussen geplande overdracht van gegevens, naar boven afgerond op de dichtstbijzijnde minuut.<br /><br /> De standaardwaarde is PT0S.|  
|**sinks** |**Tekenreeks**| In 1.5 toegevoegd. Optioneel. Verwijst naar een sink-locatie voor het verzenden van diagnostische gegevens ook. Bijvoorbeeld, Application Insights of Event Hubs.|  

## <a name="dockersources"></a>DockerSources
 *Structuur: De hoofd - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources-*

 In 1.9 toegevoegd.

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**Statistieken**|Geeft het systeem voor het verzamelen van statistieken voor Docker-containers|  

## <a name="sinksconfig-element"></a>SinksConfig-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*

 Een lijst met locaties voor het verzenden van diagnostische gegevens en de configuratie die is gekoppeld aan deze locaties.  

|De naam van element|Beschrijving|  
|------------------|-----------------|  
|**Sink**|Zie de beschrijving elders op deze pagina.|  

## <a name="sink-element"></a>Sink-Element
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink*

 In versie 1.5 toegevoegd.  

 Locaties voor het verzenden van diagnostische gegevens naar definieert. De Application Insights-service.  

|Kenmerk|Type|Beschrijving|  
|---------------|----------|-----------------|  
|**De naam**|tekenreeks|Een tekenreeks die de sinkname identificeren.|  

|Element|Type|Beschrijving|  
|-------------|----------|-----------------|  
|**Application Insights**|tekenreeks|Alleen gebruikt wanneer gegevens worden verzonden naar Application Insights. De Instrumentatiesleutel voor een actieve Application Insights-account dat u toegang tot hebt bevatten.|  
|**kanalen**|tekenreeks|Één voor elke aanvullende filters die u streamen|  

## <a name="channels-element"></a>Kanalen-Element  
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanalen*

 In versie 1.5 toegevoegd.  

 Filters voor gegevensstromen van logboekgegevens die zijn doorgegeven via een sink definieert.  

|Element|Type|Beschrijving|  
|-------------|----------|-----------------|  
|**Kanaal**|tekenreeks|Zie de beschrijving elders op deze pagina.|  

## <a name="channel-element"></a>Kanaal-Element
 *Structuur: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanalen - kanaal*

 In versie 1.5 toegevoegd.  

 Locaties voor het verzenden van diagnostische gegevens naar definieert. De Application Insights-service.  

|Kenmerken|Type|Beschrijving|  
|----------------|----------|-----------------|  
|**logLevel**|**Tekenreeks**|Hiermee geeft u het minimale ernstniveau voor logboekvermeldingen die worden overgedragen. De standaardwaarde is **Undefined**, waarop alle logboeken gegevensoverdrachten. Andere mogelijke waarden (in volgorde van meest naar minst informatie) zijn **uitgebreid**, **informatie**, **waarschuwing**, **fout**, en **Kritieke**.|  
|**De naam**|**Tekenreeks**|Een unieke naam van het kanaal om te verwijzen naar|  


## <a name="privateconfig-element"></a>PrivateConfig-Element
 *Structuur: Root - DiagnosticsConfiguration - PrivateConfig*

 Toegevoegd in versie 1.3.  

 Optioneel  

 Slaat de persoonlijke gegevens van het opslagaccount (naam, sleutel en -eindpunt). Deze informatie wordt verzonden naar de virtuele machine, maar kan niet worden opgehaald uit.  

|Onderliggende elementen|Beschrijving|  
|--------------------|-----------------|  
|**StorageAccount**|De storage-account te gebruiken. De volgende kenmerken zijn vereist<br /><br /> - **naam** -de naam van het storage-account.<br /><br /> - **sleutel** -de sleutel tot het opslagaccount.<br /><br /> - **eindpunt** -het eindpunt voor toegang tot het opslagaccount. <br /><br /> -**sasToken** (toegevoegd 1.8.1)-kunt u een SAS-token in plaats van een opslagaccountsleutel in de persoonlijke configuratie. Indien opgegeven, wordt de opslagaccountsleutel genegeerd. <br />Vereisten voor het SAS-Token: <br />-Ondersteunt alleen SAS-token van account <br />- *b*, *t* servicetypen zijn vereist. <br /> - *een*, *c*, *u*, *w* machtigingen zijn vereist. <br /> - *c*, *o* resourcetypen zijn vereist. <br /> -Ondersteunt alleen het HTTPS-protocol <br /> -De starten en verlooptijd moet geldig zijn.|  


## <a name="isenabled-element"></a>IsEnabled-Element  
 *Structuur: Root - DiagnosticsConfiguration - IsEnabled*

 Booleaanse waarde. Gebruik `true` de diagnostische gegevens inschakelen of `false` om uit te schakelen van de diagnostische gegevens.
