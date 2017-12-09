---
title: Configureren van Azure Diagnostics om gegevens te verzenden naar Application Insights | Microsoft Docs
description: De openbare configuratie van Azure Diagnostics om gegevens te verzenden naar Application Insights bijwerken.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: f9e12c3e-c307-435e-a149-ef0fef20513a
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2016
ms.author: robb
ms.openlocfilehash: 7dd8c6e1fbfba2587aadb3410c3a769b57e06001
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a>Cloud-Service, virtuele Machine of Service Fabric diagnostische gegevens verzenden naar Application Insights
De extensie Azure Diagnostics-cloudservices, virtuele Machines, virtuele-Machineschaalsets en Service Fabric alle gebruiken om gegevens te verzamelen.  Azure diagnostics verzendt gegevens naar Azure Storage-tabellen.  U kunt echter ook alle pipe of een subset van de gegevens naar andere locaties met de extensie Azure Diagnostics 1.5 of hoger.

In dit artikel wordt beschreven hoe gegevens van de extensie Azure Diagnostics verzenden naar Application Insights.

## <a name="diagnostics-configuration-explained"></a>Configuratie van diagnostische uitgelegd
De extensie Azure diagnostics 1.5 geïntroduceerd Put, die zijn extra locaties waar u diagnostische gegevens kunt verzenden.

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

- De **ApplicationInsights** element geeft de instrumentatiesleutel van de Application insights-resource waarnaar de gegevens van Azure diagnostics is verzonden.
    - Als u een bestaande Application Insights-resource niet hebt, raadpleegt u [Maak een nieuwe Application Insights-resource](../application-insights/app-insights-create-new-resource.md) voor meer informatie over het maken van een resource en de instrumentatiesleutel ophalen.
    - Als u een Cloudservice met de Azure SDK 2.8 of hoger ontwikkelt, wordt deze instrumentatiesleutel automatisch ingevuld. De waarde is gebaseerd op de **APPINSIGHTS_INSTRUMENTATIONKEY** service van configuratie-instelling wanneer het verpakken van de Cloud Services-project. Zie [gebruik Application Insights met Azure Diagnostics problemen met Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).

- De **kanalen** element bevat een of meer **kanaal** elementen.
    - De *naam* kenmerk unieke verwijst naar dat kanaal.
    - De *loglevel* kenmerk kunt u het logboekniveau waarmee het kanaal opgeven. De beschikbare logboekniveaus in volgorde van meest naar minst informatie zijn:
        - Uitgebreid
        - Informatie
        - Waarschuwing
        - Fout
        - Kritiek

Een kanaal dat fungeert als een filter, en kunt u specifieke logboekniveaus verzenden naar de doel-sink selecteren. U kan bijvoorbeeld uitgebreide logboeken verzamelen en ze verzenden naar de opslag, maar alleen fouten verzenden naar de sink.

De volgende afbeelding ziet u deze relatie.

![De openbare configuratie voor diagnostische gegevens](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

De volgende afbeelding ziet u de configuratiewaarden en hoe ze werken. U kunt meerdere put opnemen in de configuratie op verschillende niveaus in de hiërarchie. De sink op het hoogste niveau fungeert als een algemene instelling en die is opgegeven in de afzonderlijke element fungeert als een onderdrukking die algemene instelling.

![Diagnostische gegevens Sinks configuratie met Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Volledige sink configuratievoorbeeld
Hier volgt een compleet voorbeeld van de openbare configuratie voor het bestand dat
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

### <a name="send-all-the-data-that-is-being-collected-by-azure-diagnostics"></a>Alle gegevens worden verzameld door Azure diagnostics verzenden

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

### <a name="send-verbose-application-logs-to-application-insights"></a>Uitgebreide toepassingslogboeken naar Application Insights verzenden

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

- **Kanalen zich alleen aanmelden type niet prestatiemeteritems als.** Als u een kanaal met een element van de teller prestaties opgeeft, wordt dit genegeerd.
- **Het logboekniveau voor een kanaal kan niet groter zijn dan het logboekniveau voor wat wordt er door Azure diagnostics wordt verzameld.** Bijvoorbeeld, u kan niet toepassingslogboek fouten in het element logboeken verzamelen en verzend uitgebreid logboeken voor de toepassing inzicht sink. De *scheduledTransferLogLevelFilter* kenmerk moet altijd gelijk verzamelen of meer logboeken dan de logboeken u probeert te verzenden naar een sink.
- **U kunt geen blob-gegevens verzameld door de extensie voor Azure diagnostische gegevens naar Application Insights verzenden.** Bijvoorbeeld, iets opgegeven onder het *mappen* knooppunt. Voor crashdumps de crashdump werkelijke is verzonden naar de blob storage en alleen een melding dat de crashdump is gegenereerd naar Application Insights wordt verzonden.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [uw Azure diagnostics-gegevens weergeven](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) in Application Insights.
* Gebruik [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) de extensie Azure diagnostics voor uw toepassing inschakelen.
* Gebruik [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) de extensie Azure diagnostics voor uw toepassing inschakelen
