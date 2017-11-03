---
title: Gegevens verzamelen van CollectD in OMS Log Analytics | Microsoft Docs
description: CollectD is een open-source Linux-daemonwijzigingen waarmee periodiek gegevens worden verzameld van toepassingen en het niveau van systeemgegevens.  In dit artikel bevat informatie over het verzamelen van gegevens van CollectD in logboekanalyse.
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: magoedte
ms.openlocfilehash: a63b15ca5126b45451f0694c9ee75d7b67b1ceaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Gegevens verzamelen van CollectD op Linux-agents in Log Analytics
[CollectD](https://collectd.org/) is een open-source Linux-daemonwijzigingen die regelmatig maatstaven voor prestaties van toepassingen en het niveau van systeemgegevens verzamelt. Voorbeeldtoepassingen bevatten de Java Virtual Machine (JVM), MySQL-Server en Nginx. In dit artikel bevat informatie over het verzamelen van prestatiegegevens van CollectD in logboekanalyse.

Een volledige lijst met beschikbare invoegtoepassingen kan worden gevonden op [tabel van invoegtoepassingen](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Overzicht van CollectD](media/log-analytics-data-sources-collectd/overview.png)

De volgende CollectD-configuratie is opgenomen in de OMS-Agent voor Linux CollectD gegevens routeren naar de OMS-Agent voor Linux.

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Bovendien, als u een versie van collectD voordat 5.5 in plaats daarvan de volgende configuratie gebruiken.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

De configuratie van de CollectD gebruikt de standaardinstallatielocatie`write_http` invoegtoepassing prestaties metrische gegevens verzenden via poort 26000 naar OMS-Agent voor Linux. 

> [!NOTE]
> Deze poort kan worden geconfigureerd voor een aangepaste poort indien nodig.

De OMS-Agent voor Linux ook luistert op poort 26000 voor CollectD metrische gegevens en converteert ze met OMS schema metrische gegevens. Hieronder volgt de OMS-Agent voor Linux-configuratie `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Ondersteunde versies van
- Log Analytics ondersteunt momenteel CollectD versie 4.8 en hoger.
- OMS-Agent voor Linux v1.1.0-217 of hoger is vereist voor CollectD metrische verzameling.


## <a name="configuration"></a>Configuratie
Hieronder vindt u eenvoudige stappen voor het verzamelen van gegevens CollectD configureren in logboekanalyse.

1. CollectD om gegevens te verzenden naar de OMS-Agent voor Linux met behulp van de invoegtoepassing write_http configureren.  
2. De OMS-Agent voor Linux te luisteren naar de CollectD gegevens op de juiste poort configureren.
3. CollectD en OMS-Agent voor Linux opnieuw gestart.

### <a name="configure-collectd-to-forward-data"></a>CollectD voor het doorsturen van gegevens configureren 

1. Gegevens van de CollectD routeren naar de OMS-Agent voor Linux `oms.conf` moet worden toegevoegd aan de configuratiemap van CollectD. Het doel van dit bestand is afhankelijk van de Linux-distro van uw machine.

    Als uw CollectD config directory bevindt zich in /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Als uw CollectD config directory bevindt zich in /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Voor versies van CollectD voor 5.5 u moet de labels in wijzigen `oms.conf` zoals hierboven.
    >

2. Kopieer collectd.conf naar de gewenste werkruimte omsagent configuratiemap.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Start CollectD en OMS-Agent voor Linux opnieuw met de volgende opdrachten.

    sudo service collectd sudo /opt/microsoft/omsagent/bin/service_control opnieuw starten

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a>CollectD metrische gegevens voor de conversie van logboekanalyse schema
Voor het onderhouden van een vertrouwde model tussen infrastructuur metrische gegevens die al zijn verzameld door de OMS-Agent voor Linux en de nieuwe metrische gegevens die worden verzameld door CollectD de volgende schematoewijzing wordt gebruikt:

| CollectD metriek veld | Log Analytics-veld |
|:--|:--|
| host | Computer |
| Invoegtoepassing | Geen |
| plugin_instance | Exemplaarnaam<br>Als **plugin_instance** is *null* vervolgens InstanceName = "*_Totaal*' |
| type | Objectnaam |
| type_instance | CounterName<br>Als **type_instance** is *null* vervolgens CounterName =**leeg** |
| dsnames] | CounterName |
| dstypes | Geen |
| [] waarden | Tegenwaarde |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren. 
* Gebruik [aangepaste velden](log-analytics-custom-fields.md) parseren van gegevens van syslog-records in afzonderlijke velden.

