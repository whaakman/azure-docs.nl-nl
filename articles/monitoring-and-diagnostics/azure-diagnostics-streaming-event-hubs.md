---
title: Azure Diagnostics-gegevens naar Event Hubs Stream
description: Azure Diagnostics configureren met Event Hubs-end-to-end, met inbegrip van richtlijnen voor algemene scenario's.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 07/13/2017
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: 123005a32315ba3972ed4f1c1784d3ac9460b101
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958947"
---
# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Streaming van Azure Diagnostics-gegevens in het dynamische pad met behulp van Event Hubs
Diagnostische gegevens van Azure biedt flexibele manieren voor het verzamelen van Logboeken en metrische gegevens van virtuele machines (VM's) voor cloud services en resultaten overbrengen naar Azure Storage. Vanaf het tijdsbestek van maart 2016 (SDK 2.9 gebruikt), kunt u diagnostische gegevens verzenden naar aangepaste gegevensbronnen en gegevensoverdracht snelpad binnen enkele seconden met behulp van [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).

Ondersteunde gegevenstypen zijn onder andere:

* ETW-gebeurtenissen (Event Tracing for Windows)
* Prestatiemeteritems
* Windows-gebeurtenislogboeken
* Toepassingslogboeken
* Logboeken van Azure Diagnostics-infrastructuur

In dit artikel wordt beschreven hoe u Azure Diagnostics configureren met Event Hubs van begin tot eind. Ook worden richtlijnen gegeven voor de volgende algemene scenario's:

* Over het aanpassen van de logboeken en metrische gegevens die worden verzonden naar Event Hubs
* Configuraties in elke omgeving wijzigen
* Event Hubs streaminggegevens weergeven
* Problemen oplossen met de verbinding  

## <a name="prerequisites"></a>Vereisten
Event Hubs receieving gegevens van Azure Diagnostics wordt ondersteund in de Cloud Services, virtuele machines, Virtual Machine Scale Sets en Service Fabric starten in de Azure SDK 2.9 gebruikt en de bijbehorende Azure-hulpprogramma's voor Visual Studio.

* Azure Diagnostics-extensie 1.6 ([Azure SDK voor .NET 2.9 of hoger](https://azure.microsoft.com/downloads/) gericht op deze standaard)
* [Visual Studio 2013 of hoger](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* Bestaande configuraties van Azure Diagnostics in een toepassing met behulp van een *.wadcfgx* bestands- en een van de volgende methoden:
  * Visual Studio: [diagnostische gegevens configureren voor Azure Cloudservices en Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
  * Windows PowerShell: [diagnostische gegevens in Azure Cloud Services met behulp van PowerShell inschakelen](../cloud-services/cloud-services-diagnostics-powershell.md)
* Event Hubs-naamruimte is ingericht per artikel [aan de slag met Event Hubs](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Verbinding maken met Azure Diagnostics naar Event Hubs-sink
Standaard verzendt Azure Diagnostics altijd logboeken en metrische gegevens naar een Azure Storage-account. Een toepassing kan ook gegevens naar Event Hubs verzenden door toe te voegen een nieuwe **Sinks** sectie onder de **PublicConfig** / **WadCfg** element van de *. wadcfgx* bestand. In Visual Studio, de *.wadcfgx* bestand wordt opgeslagen in het volgende pad: **Cloudserviceproject** > **rollen** > **() RoleName)** > **diagnostics.wadcfgx** bestand.

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

In dit voorbeeld wordt de URL van de event hub is ingesteld op de volledig gekwalificeerde naamruimte van de event hub: Event Hubs-naamruimte + "/" + naam event hub.  

De event hub URL wordt weergegeven in de [Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885) op het dashboard van de Event Hubs.  

De **Sink** naam kan worden ingesteld op een willekeurige geldige tekenreeks, zolang de dezelfde waarde consistent wordt gebruikt in het configuratiebestand.

> [!NOTE]
> Mogelijk zijn er extra Put, zoals *applicationInsights* geconfigureerd in deze sectie. Azure Diagnostics kunt u een of meer sinks worden gedefinieerd als elke sink wordt ook aangegeven de **PrivateConfig** sectie.  
>
>

De Event Hubs-sink moet ook worden gedeclareerd en gedefinieerd in de **PrivateConfig** sectie van de *.wadcfgx* config-bestand.

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

De `SharedAccessKeyName` waarde moet overeenkomen met een Shared Access Signature (SAS)-sleutel en een beleid dat is gedefinieerd in de **Event Hubs** naamruimte. Blader naar het dashboard van de Event Hubs in de [Azure-portal](https://portal.azure.com), klikt u op de **configureren** tabblad en instellen van een beleid met de naam (bijvoorbeeld ' SendRule' genoemd) waarvoor *verzenden* machtigingen. De **StorageAccount** ook is gedeclareerd in **PrivateConfig**. Er is niet nodig voor het wijzigen van hier waarden als ze werken. In dit voorbeeld laten we de waarden leeg is, dit is een teken of een downstream asset de waarden worden ingesteld. Bijvoorbeeld, de *ServiceConfiguration.Cloud.cscfg* omgeving configuratiebestand Hiermee stelt u de omgeving nodig namen en sleutels.  

> [!WARNING]
> De Event Hubs SAS-sleutel wordt opgeslagen als tekst zonder opmaak in het *.wadcfgx* bestand. Deze sleutel wordt vaak het geval is, is ingecheckt met broncodebeheer of is beschikbaar als een activum in uw buildserver, zodat u deze zo nodig te beschermen. Het is raadzaam dat u een SAS-sleutel hier met *alleen verzenden* machtigingen zodat een kwaadwillende gebruiker kan naar de event hub schrijven, maar die niet worden luisteren naar deze of beheren.
>
>

## <a name="configure-azure-diagnostics-to-send-logs-and-metrics-to-event-hubs"></a>Azure Diagnostics voor het verzenden van Logboeken en metrische gegevens naar Event Hubs configureren
Zoals eerder zijn besproken, alle standaard en aangepaste diagnostische gegevens, dat wil zeggen, wordt metrische gegevens en zich aanmeldt, automatisch verzonden naar Azure Storage in het geconfigureerde interval. Met Event Hubs en eventuele aanvullende sink, kunt u een basis- of leaf-knooppunt in de hiërarchie moet worden verzonden naar de event hub. Dit omvat ETW-gebeurtenissen, prestatiemeteritems, Windows-gebeurtenislogboeken en toepassingslogboeken.   

Het is belangrijk om te overwegen het aantal gegevenspunten daadwerkelijk moeten worden overgebracht naar Event Hubs. Normaal gesproken ontwikkelaars gegevensoverdracht met lage latentie hot pad die moet worden gebruikt en geïnterpreteerd snel. Systemen die waarschuwingen of regels voor automatisch schalen bewaken zijn voorbeelden. Een ontwikkelaar kan ook een alternatieve analysis-store configureren of zoek store, bijvoorbeeld Azure Stream Analytics, Elasticsearch, een aangepaste bewakingssysteem of een favoriete bewakingssysteem van anderen.

Hier volgen enkele voorbeeldconfiguraties.

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

In het bovenstaande voorbeeld de sink is toegepast op de bovenliggende **PerformanceCounters** knooppunt in de hiërarchie, wat betekent alle onderliggende dat **PerformanceCounters** wordt verzonden naar Event Hubs.  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

In het vorige voorbeeld de sink is toegepast op slechts drie items: **aanvragen in wachtrij**, **aanvragen afgewezen**, en **% processortijd**.  

Het volgende voorbeeld laat zien hoe een ontwikkelaar de hoeveelheid verzonden gegevens moeten de belangrijke metrische gegevens die worden gebruikt voor de status van deze service kunt beperken.  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

In dit voorbeeld wordt de sink wordt toegepast op Logboeken en alleen te fout serviceniveau traceren is gefilterd.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Implementeren en bijwerken van een toepassings- en diagnostische gegevens over configuratie voor Cloud Services
Visual Studio biedt de eenvoudigste manier voor het implementeren van de toepassing en de configuratie van de Event Hubs-sink. Als u wilt weergeven en bewerken van het bestand, open de *.wadcfgx* -bestand in Visual Studio, bewerken en opslaan. Het pad is **Cloudserviceproject** > **rollen** > **(RoleName)** > **diagnostics.wadcfgx**.  

Op dit moment alle implementatie en implementatie bijwerken acties in Visual Studio, Visual Studio Team System, en alle opdrachten of scripts die zijn gebaseerd op MSBuild en gebruik de **/t: publiceren** doel bevatten de *.wadcfgx* tijdens het verpakken. Bovendien, implementeren updates en implementaties het bestand in Azure met behulp van de juiste Azure Diagnostics-agent-extensie op uw virtuele machines.

Nadat u de toepassing en Azure Diagnostics-configuratie hebt geïmplementeerd, ziet u onmiddellijk activiteit in het dashboard van de event hub. Hiermee wordt aangegeven dat u klaar bent om door te gaan met het bekijken van de gegevens van hot-pad in het hulpprogramma voor listener-client of analyse van uw keuze.  

In de volgende afbeelding ziet u het dashboard van de Event Hubs in orde verzenden van diagnostische gegevens naar de event hub vanaf enige tijd na 23: 00 uur. Als de toepassing is geïmplementeerd met een bijgewerkte *.wadcfgx* bestands- en de sink correct is geconfigureerd.

![][0]  

> [!NOTE]
> Wanneer u updates aanbrengt in het configuratiebestand van de Azure Diagnostics (.wadcfgx), wordt het aanbevolen dat u de updates voor de gehele toepassing, evenals de configuratie van de pushen met behulp van Visual Studio publiceren of een Windows PowerShell-script.  
>
>

## <a name="view-hot-path-data"></a>Gegevens in de hot-pad
Zoals eerder besproken, er worden veel gebruikt voor het luisteren naar en verwerken van Event Hubs-gegevens.

Een eenvoudige benadering is het maken van een kleine test-consoletoepassing om te luisteren naar de event hub en de uitvoerstroom afdrukken. U kunt de volgende code, die wordt uitgelegd in meer detail in plaatsen [aan de slag met Event Hubs](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)), in een consoletoepassing.  

Houd er rekening mee dat de consoletoepassing moet bevatten de [Event Processor Host NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Vervang de waarden in de punthaken in de **Main** functie met waarden voor uw resources.   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a>Event Hubs sinks oplossen
* De event hub wordt niet weergegeven voor binnenkomende of uitgaande gebeurtenisactiviteit zoals verwacht.

    Controleer of uw event hub is ingericht. Alle informatie over de verbinding in de **PrivateConfig** sectie van *.wadcfgx* moet overeenkomen met de waarden van uw resource, zoals te zien is in de portal. Zorg ervoor dat u een SAS-beleid gedefinieerd ('SendRule' in het voorbeeld) in de portal en die *verzenden* machtiging is verleend.  
* Nadat een update, wordt de event hub niet meer binnenkomende of uitgaande activiteit weergegeven.

    Eerst, zorg ervoor dat de gegevens van de event hub en de configuratie juist zijn, zoals hierboven is uitgelegd. Soms de **PrivateConfig** opnieuw wordt ingesteld in een implementatie-update. De aanbevolen oplossing is om te zorgen dat alle wijzigingen aan *.wadcfgx* in het project en push de update van een volledige toepassing. Als dit niet mogelijk is, zorgt u ervoor dat de update voor diagnostische gegevens over een complete pushes **PrivateConfig** die de SAS-sleutel bevat.  
* Ik heb geprobeerd de suggesties en de event hub is nog steeds niet werkt.

    Bekijk in de Azure Storage-tabel met Logboeken en fouten voor Azure Diagnostics zelf: **WADDiagnosticInfrastructureLogsTable**. Een optie is het gebruik van een hulpprogramma zoals [Azure Storage Explorer](http://www.storageexplorer.com) voor verbinding met dit opslagaccount wordt gebruikt, bekijkt u deze tabel en het toevoegen van een query voor de tijdstempel in de afgelopen 24 uur. U kunt het hulpprogramma gebruiken om te exporteren van een CSV-bestand en opent u het in een toepassing, zoals Microsoft Excel. Excel kunt u eenvoudig zoeken naar tekenreeksen met-kaart, zoals **EventHubs**, om te zien welke fout is gemeld.  

## <a name="next-steps"></a>Volgende stappen
• [Meer informatie over Event Hubs](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Bijlage: Voorbeeld van Azure Diagnostics-configuratie-bestand (.wadcfgx) voltooien
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

De aanvullende *ServiceConfiguration.Cloud.cscfg* voor dit voorbeeld uitziet.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

Equivalente JSON-instellingen voor virtuele machines is als volgt:

Openbare-instellingen:
```JSON
{
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                "DataSource": [
                    {
                        "name": "Application!*"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}

```

Beveiligde instellingen:
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](../event-hubs/event-hubs-about.md)
* [Een Event Hub maken](../event-hubs/event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
