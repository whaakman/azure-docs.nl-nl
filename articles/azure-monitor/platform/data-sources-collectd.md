---
title: Gegevens verzamelen van verzamelde in Log Analytics | Microsoft Docs
description: Verzamelde is een open-source Linux-daemonwijzigingen waarmee periodiek gegevens worden verzameld van toepassingen en gegevens op systeem.  In dit artikel bevat informatie over het verzamelen van gegevens van verzamelde in Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 1a6ccb42e00431c16ce7767f19d6cdc9cbb3cffe
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52841690"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Gegevens verzamelen van verzamelde op Linux-agents in Log Analytics
[Verzamelde](https://collectd.org/) is een open-source Linux-daemonwijzigingen die periodiek metrische gegevens voor prestaties van toepassingen en gegevens op systeem verzamelt. Van de voorbeeldtoepassingen bevatten de Java Virtual Machine (JVM), MySQL-Server en Nginx. In dit artikel bevat informatie over het verzamelen van prestatiegegevens van verzamelde in Log Analytics.

Een volledige lijst met beschikbare invoegtoepassingen kan worden gevonden op [tabel van invoegtoepassingen](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Overzicht van de verzamelde](media/data-sources-collectd/overview.png)

De volgende verzamelde-configuratie is opgenomen in de Log Analytics-agent voor Linux verzamelde gegevens routeren naar de Log Analytics-agent voor Linux.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Bovendien, als een versies van verzamelde voordat 5.5 gebruik in plaats daarvan de volgende configuratie.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

De configuratie van de verzamelde gebruikmaakt van de`write_http` invoegtoepassing metrische prestatiegegevens verzenden via poort 26000 naar Log Analytics-agent voor Linux. 

> [!NOTE]
> Deze poort kan worden geconfigureerd met een aangepaste poort indien nodig.

De Log Analytics-agent voor Linux ook luistert op poort 26000 voor verzamelde metrische gegevens en vervolgens geconverteerd naar Log Analytics-schema metrische gegevens. Hieronder volgt de Log Analytics-agent voor Linux-configuratie `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Ondersteunde versies
- Log Analytics biedt momenteel ondersteuning verzamelde versie 4.8 en hoger.
- Log Analytics-agent voor Linux v1.1.0-217 of hoger is vereist voor het verzamelde metrische gegevens verzamelen.


## <a name="configuration"></a>Configuratie
Hier volgen de basisstappen voor het verzamelen van verzamelde gegevens configureren in Log Analytics.

1. Verzamelde om gegevens te verzenden naar de Log Analytics-agent voor Linux met behulp van de invoegtoepassing write_http configureren.  
2. De Log Analytics-agent voor Linux om te luisteren naar de verzamelde gegevens op de juiste poort configureren.
3. Verzamelde en Log Analytics-agent voor Linux opnieuw te starten.

### <a name="configure-collectd-to-forward-data"></a>Verzamelde voor het doorsturen van gegevens configureren 

1. Het routeren van verzamelde gegevens naar de Log Analytics-agent voor Linux, `oms.conf` moet worden toegevoegd aan de verzamelde configuratiemap. Het doel van dit bestand is afhankelijk van de Linux-distributie van uw computer.

    Als uw verzamelde config directory bevindt zich in /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Als uw verzamelde config directory bevindt zich in /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Voor versies van verzamelde voordat 5.5 u moet wijzigen van de labels in `oms.conf` zoals hierboven.
    >

2. Kopieer collectd.conf naar de gewenste werkruimte omsagent configuratiemap.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Verzamelde en Log Analytics-agent voor Linux met de volgende opdrachten opnieuw starten.

    sudo-service verzamelde sudo /opt/microsoft/omsagent/bin/service_control opnieuw starten

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a>Verzamelde metrische gegevens naar Log Analytics-schema-conversie
Voor het onderhouden van een vertrouwde model tussen metrische gegevens over infrastructuur al zijn verzameld door Log Analytics-agent voor Linux en de nieuwe metrische gegevens die worden verzameld door verzamelde de schematoewijzing van het volgende wordt gebruikt:

| Veld van de verzamelde metrische gegevens | Log Analytics-veld |
|:--|:--|
| host | Computer |
| Invoegtoepassing | Geen |
| plugin_instance | Instantienaam<br>Als **plugin_instance** is *null* vervolgens InstanceName = "*_Totaal*" |
| type | Objectnaam |
| type_instance | CounterName<br>Als **type_instance** is *null* vervolgens CounterName =**leeg** |
| dsnames] | CounterName |
| dstypes | Geen |
| waarden] | CounterValue |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren. 
* Gebruik [aangepaste velden](../../log-analytics/log-analytics-custom-fields.md) parseren van gegevens van syslog-records in afzonderlijke velden.

