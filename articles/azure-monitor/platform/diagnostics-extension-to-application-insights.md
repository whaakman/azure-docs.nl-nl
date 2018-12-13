---
title: Azure Diagnostics voor het verzenden van gegevens naar Application Insights configureren
description: De Azure Diagnostics openbare configuratie voor het verzenden van gegevens naar Application Insights bijwerken.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2016
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: b2399e47c9071bac159f407deac9755606763a67
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53326094"
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a>Cloud Service, virtuele Machine of Service Fabric diagnostische gegevens verzenden naar Application Insights
Cloudservices, virtuele Machines, Virtual Machine Scale Sets en Service Fabric alle gebruiken de Azure Diagnostics-extensie om gegevens te verzamelen.  Azure diagnostics verzendt gegevens naar Azure Storage-tabellen.  U kunt echter ook alle pipe of een subset van de gegevens naar andere locaties met behulp van Azure Diagnostics-extensie 1.5 of hoger.

Dit artikel wordt beschreven hoe u gegevens van de Azure Diagnostics-extensie naar Application Insights verzendt.

## <a name="diagnostics-configuration-explained"></a>Configuratie van diagnostische uitgelegd
De Azure diagnostics-extensie 1.5 geïntroduceerd Put, zijn extra locaties waar kunt u diagnostische gegevens verzenden.

Van de voorbeeldconfiguratie van een sink voor Application Insights:

```XML
<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "ApplicationInsights",
            "ApplicationInsights": "{Insert InstrumentationKey}",
            "Channels": {
                "Channel": [
                    {
                        "logLevel": "Error",
                        "name": "MyTopDiagData"
                    },
                    {
                        "logLevel": "Error",
                        "name": "MyLogData"
                    }
                ]
            }
        }
    ]
}
```
- De **Sink** *naam* kenmerk is een tekenreekswaarde die een unieke identificatie van de sink.

- De **ApplicationInsights** element Hiermee geeft u de instrumentatiesleutel van de Application insights-resource waarnaar de gegevens van Azure diagnostics is verzonden.
    - Als u een bestaande Application Insights-resource hebt, raadpleegt u [Maak een nieuwe Application Insights-resource](../../application-insights/app-insights-create-new-resource.md) voor meer informatie over het maken van een resource en de instrumentatiesleutel ophalen.
    - Als u een Cloudservice met de Azure SDK 2.8 of hoger ontwikkelt, wordt deze instrumentatiesleutel automatisch ingevuld. De waarde is gebaseerd op de **APPINSIGHTS_INSTRUMENTATIONKEY** service van configuratie-instelling bij het verpakken van de Cloud Service-project. Zie [Application Insights gebruiken met Cloudservices](../../application-insights/app-insights-cloudservices.md).

- De **kanalen** element bevat een of meer **kanaal** elementen.
    - De *naam* kenmerk een unieke verwijst naar dat kanaal.
    - De *loglevel* kenmerk kunt u het registratieniveau dat het kanaal kunt opgeven. De beschikbare logboekniveaus in volgorde van meest naar minst informatie zijn:
        - Uitgebreid
        - Informatie
        - Waarschuwing
        - Fout
        - Kritiek

Een kanaal dat fungeert als een filter en kunt u specifieke logboekniveaus verzenden naar de doel-sink selecteren. U kan bijvoorbeeld uitgebreide logboeken te verzamelen en ze verzenden naar opslag, maar alleen fouten te verzenden naar de sink.

De volgende afbeelding ziet u deze relatie.

![Diagnostische gegevens over openbare configuratie](media/diagnostics-extension-to-application-insights/AzDiag_Channels_App_Insights.png)

De volgende afbeelding bevat een overzicht van de configuratiewaarden en hoe deze werken. U kunt meerdere sinks opnemen in de configuratie op verschillende niveaus in de hiërarchie. De sink op het hoogste niveau fungeert als een algemene instelling en de versie die is opgegeven bij het afzonderlijke element fungeert als een onderdrukking voor die instelling.

![Diagnostische gegevens Sinks configureren met Application Insights](media/diagnostics-extension-to-application-insights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Voorbeeld van de configuratie voltooid sink
Hier volgt een compleet voorbeeld van de openbare configuratie die het bestand
1. alle fouten naar Application Insights verzendt (opgegeven in de **DiagnosticMonitorConfiguration** knooppunt)
2. verzendt ook uitgebreide niveau logboeken voor de toepassingslogboeken (opgegeven in de **logboeken** knooppunt).

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent to this channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent to this channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```
```JSON
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
        "overallQuotaInMB": 4096,
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent to this channel",
        "DiagnosticInfrastructureLogs": {
        },
        "PerformanceCounters": {
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                },
                {
                    "counterSpecifier": "\\Memory\\Available MBytes",
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
            "scheduledTransferLogLevelFilter": "Verbose",
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent to this channel"
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
                            "name": "MyTopDiagData"
                        },
                        {
                            "logLevel": "Verbose",
                            "name": "MyLogData"
                        }
                    ]
                }
            }
        ]
    }
}
```
In de vorige configuratie hebben de volgende regels in de volgende betekenis:

### <a name="send-all-the-data-that-is-being-collected-by-azure-diagnostics"></a>Alle gegevens die worden verzameld door Azure diagnostics verzenden

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-to-the-application-insights-sink"></a>Alleen foutenlogboeken verzenden naar de Application Insights-sink

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-to-application-insights"></a>Uitgebreide toepassingslogboeken verzenden naar Application Insights

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a>Beperkingen

- **Kanalen alleen aanmelden type en de prestaties van niet-items.** Als u een kanaal met een element van prestaties teller opgeeft, wordt dit genegeerd.
- **Het logboek-niveau voor een kanaal kan niet groter zijn dan het logboek-niveau voor wat wordt er door Azure diagnostics wordt verzameld.** Bijvoorbeeld, u kunt geen toepassingslogboek fouten in het element logboeken verzamelen en verzenden uitgebreid logboeken naar de Application Insights-sink. De *scheduledTransferLogLevelFilter* kenmerk moet altijd gelijk verzamelen of meer logboeken dan de logboeken u probeert te verzenden naar een sink.
- **U kunt geen blob-gegevens die worden verzameld door Azure diagnostics-extensie naar Application Insights verzenden.** Bijvoorbeeld: opgegeven onder de *mappen* knooppunt. Voor crashdumps de werkelijke crashdump wordt verzonden naar de blob storage en alleen een melding dat de crashdump is gegenereerd, wordt verzonden naar Application Insights.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [uw Azure diagnostics-gegevens weergeven](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) in Application Insights.
* Gebruik [PowerShell](../../cloud-services/cloud-services-diagnostics-powershell.md) om in te schakelen van de Azure diagnostics-extensie voor uw toepassing.
* Gebruik [Visual Studio](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) om in te schakelen van de Azure diagnostics-extensie voor uw toepassing
