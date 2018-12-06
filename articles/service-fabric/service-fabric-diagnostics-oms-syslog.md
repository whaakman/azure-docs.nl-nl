---
title: Gebeurtenissen van de Linux-cluster in Azure Service Fabric bewaken | Microsoft Docs
description: Informatie over het bewaken van Linux-Clustergebeurtenissen van Syslog
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2018
ms.author: srrengar
ms.openlocfilehash: ec2b623650818877930ac6b95a17ee264f07efdf
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52959528"
---
# <a name="service-fabric-linux-cluster-events-in-syslog"></a>Gebeurtenissen van de service Fabric Linux-cluster in Syslog

Service Fabric wordt aangegeven dat een reeks gebeurtenissen platform om u belangrijke activiteit in uw cluster te informeren. De volledige lijst van gebeurtenissen die worden weergegeven is beschikbaar [hier](service-fabric-diagnostics-event-generation-operational.md). Er zijn verschillende manieren waarmee deze gebeurtenissen kunnen worden gebruikt. In dit artikel gaan we bespreken hoe u Service Fabric voor het schrijven van deze gebeurtenissen tot Syslog configureert.

## <a name="introduction"></a>Inleiding

In de 6.4 release is de SyslogConsumer geïntroduceerd voor het verzenden van de Service Fabric-platformgebeurtenissen tot Syslog voor Linux-clusters. Eenmaal ingeschakeld, wordt automatisch gebeurtenissen overgebracht naar Syslog dat kan worden verzameld en verzonden door de Log Analytics-Agent.

Alle Syslog-gebeurtenis is 4-onderdelen
* Opslagruimte
* Identiteit
* Bericht
* Severity

De SyslogConsumer schrijft alle platform-gebeurtenissen met behulp van functie `Local0`. U kunt bijwerken naar een geldige faciliteitnaam door het veranderen van de configuratie van de configuratie. De identiteit die wordt gebruikt is `ServicFabric`. Het veld bericht bevat de volledige gebeurtenis geserialiseerd in JSON, zodat deze kan worden opgevraagd of die worden gebruikt door verschillende hulpprogramma's. 

## <a name="enable-syslogconsumer"></a>SyslogConsumer inschakelen

Om in te schakelen de SyslogConsumer, moet u een upgrade van uw cluster uitvoeren. De `fabricSettings` sectie moet worden bijgewerkt met de volgende code. Houd er rekening mee dat met deze code bevat alleen secties die betrekking hebben op SyslogConsumer

```json
    "fabricSettings": [
        {
            "name": "Diagnostics",
            "parameters": [
            {
                "name": "ConsumerInstances",
                "value": "AzureWinFabCsv, AzureWinFabCrashDump, AzureTableWinFabEtwQueryable, SyslogConsumer"
            }
            ]
        },
        {
            "name": "SyslogConsumer",
            "parameters": [
            {
                "name": "ProducerInstance",
                "value": "WinFabLttProducer"
            },
            {
            "name": "ConsumerType",
            "value": "SyslogConsumer"
            },
            {
                "name": "IsEnabled",
                "value": "true"
            }
            ]
        },
        {
            "name": "Common",
            "parameters": [
            {
                "name": "LinuxStructuredTracesEnabled",
                "value": "true"
            }
            ]
        }
    ],
```

Hier volgen de wijzigingen te belichten
1. In het algemene gedeelte is een nieuwe parameter met de naam `LinuxStructuredTracesEnabled`. **Dit is vereist voor het Linux-gebeurtenissen gestructureerde en geserialiseerd als deze is verzonden naar de Syslog.**
2. In de sectie diagnostische gegevens, een nieuwe ConsumerInstance: SyslogConsumer is toegevoegd. Hiermee wordt aangegeven het platform is er een andere gebruiker van de gebeurtenissen. 
3. De nieuwe sectie SyslogConsumer moet `IsEnabled` als `true`. Het is geconfigureerd om gebruik van de faciliteit Local0 automatisch. U kunt dit opheffen door een andere parameter toe te voegen.

```json
    {
        "name": "New LogFacility",
        "value": "<Valid Syslog Facility>"
    }
```

## <a name="log-analytics-integration"></a>Log Analytics-integratie
U vindt deze Syslog-gebeurtenissen in een hulpprogramma voor bewaking, zoals Log Analytics. U kunt een Log Analytics-werkruimte maken met behulp van de Azure Marketplace via deze [instructies]. (.. / azure-monitor/learn/quick-create-workspace.md) moet u ook de Log Analytics-agent toevoegen aan uw cluster voor het verzamelen en deze gegevens worden verzonden naar de werkruimte. Dit is dezelfde agent die wordt gebruikt voor het verzamelen van prestatiemeteritems. 

1. Navigeer naar de `Advanced Settings` blade

    ![Werkruimte-instellingen](media/service-fabric-diagnostics-oms-syslog/workspace-settings.png)

2. Klik op `Data`
3. Klik op `Syslog`
4. Local0 configureren als de mogelijkheid om bij te houden. U kunt een andere opslagruimte toevoegen als u deze in de instelling fabricSettings gewijzigd

    ![Syslog configureren](media/service-fabric-diagnostics-oms-syslog/syslog-configure.png)
5. Ga naar de queryverkenner door te klikken op `Logs` in de werkruimteresource om te gaan met het uitvoeren van query's

    ![Logboeken van de werkruimte](media/service-fabric-diagnostics-oms-syslog/workspace-logs.png)
6. U kunt een query naar de `Syslog` op zoek naar tabel `ServiceFabric` als de procesnaam. De query hieronder volgt een voorbeeld van hoe u de JSON in de gebeurtenis parseren en de inhoud ervan weer te geven

```kusto
    Syslog | where ProcessName == "ServiceFabric" | extend $payload = parse_json(SyslogMessage) | project $payload
```

![Syslog-query](media/service-fabric-diagnostics-oms-syslog/syslog-query.png)

Het bovenstaande voorbeeld is van een gebeurtenis NodeDown. U vindt de volledige lijst van gebeurtenissen [hier](service-fabric-diagnostics-event-generation-operational.md).

## <a name="next-steps"></a>Volgende stappen
* [De Log Analytics-agent implementeren](service-fabric-diagnostics-oms-agent.md) naar uw knooppunten om te verzamelen prestatiemeteritems en verzamelen van Logboeken voor uw containers en docker-statistieken
* Familiarized ophalen met de [zoeken en uitvoeren van query's](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van Log Analytics
* [Weergaveontwerper gebruiken om te maken van aangepaste weergaven in Log Analytics](../log-analytics/log-analytics-view-designer.md)
* Verwijzing voor informatie over het [Log Analytics-integratie met Syslog](../log-analytics/log-analytics-data-sources-syslog.md).
