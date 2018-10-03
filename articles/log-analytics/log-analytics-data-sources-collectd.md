---
title: Gegevens verzamelen van verzamelde in OMS Log Analytics | Microsoft Docs
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
ms.openlocfilehash: eb053ef8fc66ff9d71a9576b71eb4edfcd688638
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041287"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Gegevens verzamelen van verzamelde op Linux-agents in Log Analytics
[Verzamelde](https://collectd.org/) is een open-source Linux-daemonwijzigingen die periodiek metrische gegevens voor prestaties van toepassingen en gegevens op systeem verzamelt. Van de voorbeeldtoepassingen bevatten de Java Virtual Machine (JVM), MySQL-Server en Nginx. In dit artikel bevat informatie over het verzamelen van prestatiegegevens van verzamelde in Log Analytics.

Een volledige lijst met beschikbare invoegtoepassingen kan worden gevonden op [tabel van invoegtoepassingen](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Overzicht van de verzamelde](media/log-analytics-data-sources-collectd/overview.png)

De volgende verzamelde-configuratie is opgenomen in de OMS-Agent voor Linux route verzamelde gegevens naar de OMS-Agent voor Linux.

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

De configuratie van de verzamelde gebruikmaakt van de`write_http` invoegtoepassing metrische prestatiegegevens via poort 26000 verzenden naar OMS-Agent voor Linux. 

> [!NOTE]
> Deze poort kan worden geconfigureerd met een aangepaste poort indien nodig.

De OMS-Agent voor Linux ook luistert op poort 26000 voor verzamelde metrische gegevens en vervolgens geconverteerd naar OMS schema metrische gegevens. Hieronder volgt de OMS-Agent voor Linux-configuratie `collectd.conf`.

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
- OMS-Agent voor Linux v1.1.0-217 of hoger is vereist voor het verzamelde metrische gegevens verzamelen.


## <a name="configuration"></a>Configuratie
Hier volgen de basisstappen voor het verzamelen van verzamelde gegevens configureren in Log Analytics.

1. Verzamelde om gegevens te verzenden naar de OMS-Agent voor Linux met behulp van de invoegtoepassing write_http configureren.  
2. De OMS-Agent voor Linux om te luisteren naar de verzamelde gegevens op de juiste poort configureren.
3. Verzamelde en OMS-Agent voor Linux opnieuw gestart.

### <a name="configure-collectd-to-forward-data"></a>Verzamelde voor het doorsturen van gegevens configureren 

1. Het routeren van verzamelde gegevens naar de OMS-Agent voor Linux, `oms.conf` moet worden toegevoegd aan de verzamelde configuratiemap. Het doel van dit bestand is afhankelijk van de Linux-distributie van uw computer.

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

3. Verzamelde en OMS-Agent voor Linux opnieuw met de volgende opdrachten.

    sudo-service verzamelde sudo /opt/microsoft/omsagent/bin/service_control opnieuw starten

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a>Verzamelde metrische gegevens naar Log Analytics-schema-conversie
Voor het onderhouden van een vertrouwde model tussen metrische gegevens over infrastructuur al zijn verzameld door OMS-Agent voor Linux en de nieuwe metrische gegevens die worden verzameld door verzamelde de schematoewijzing van het volgende wordt gebruikt:

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
* Meer informatie over [zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren. 
* Gebruik [aangepaste velden](log-analytics-custom-fields.md) parseren van gegevens van syslog-records in afzonderlijke velden.

