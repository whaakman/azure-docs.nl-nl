---
title: Verzamelen van Nagios en Zabbix-waarschuwingen in Log Analytics | Microsoft Docs
description: Nagios en Zabbix zijn open-source hulpprogramma's voor controle. U kunt waarschuwingen van deze hulpprogramma's verzamelen in Log Analytics om ze te analyseren en met waarschuwingen van andere bronnen.  In dit artikel wordt beschreven hoe u de Log Analytics-agent voor Linux voor het verzamelen van waarschuwingen van deze systemen configureren.
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
ms.date: 04/13/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 3331ed7775cd3027f1262b195c6230fbea742497
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336834"
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-log-analytics-agent-for-linux"></a>Waarschuwingen verzamelen van Nagios en Zabbix in Log Analytics van Log Analytics-agent voor Linux 
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]
[Nagios](https://www.nagios.org/) en [Zabbix](http://www.zabbix.com/) zijn open-source hulpprogramma's voor controle. U kunt waarschuwingen van deze hulpprogramma's verzamelen in Log Analytics om te kunnen analyseren ze samen met [waarschuwingen uit andere bronnen](../../monitoring-and-diagnostics/monitoring-overview-alerts.md).  In dit artikel wordt beschreven hoe u de Log Analytics-agent voor Linux voor het verzamelen van waarschuwingen van deze systemen configureren.
 
## <a name="prerequisites"></a>Vereisten
De Log Analytics-agent voor Linux ondersteunt verzamelen meldingen van Nagios tot versie 4.2.x en Zabbix maximaal versie 2.x.

## <a name="configure-alert-collection"></a>Verzamelen van waarschuwingen configureren

### <a name="configuring-nagios-alert-collection"></a>Nagios verzamelen van waarschuwingen configureren
Voor het verzamelen van waarschuwingen, moet u de volgende stappen uitvoeren op de Nagios-server.

1. Verleen de gebruiker **omsagent** leestoegang tot het logboekbestand Nagios `/var/log/nagios/nagios.log`. Ervan uitgaande dat het bestand nagios.log eigendom is van de groep `nagios`, kunt u de gebruiker toevoegen **omsagent** naar de **nagios** groep. 

    sudo usermod - a -G nagios omsagent

2.  Wijzig het configuratiebestand op `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Zorg ervoor dat de volgende vermeldingen zijn aanwezig en geen opmerkingen uit:  

        <source>  
          type tail  
          #Update path to point to your nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. Start de omsagent-daemon opnieuw

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Verzamelen van Zabbix-waarschuwingen configureren
Voor het verzamelen van waarschuwingen van een Zabbix-server, die u nodig hebt om op te geven van een gebruiker en het wachtwoord in *ongecodeerd*.  Hoewel niet ideaal zijn, adviseren we u een Zabbix-gebruiker met alleen-lezen machtigingen om af te vangen relevante waarschuwingen maken.

Voor het verzamelen van waarschuwingen op de Nagios-server, moet u de volgende stappen uitvoeren.

1. Wijzig het configuratiebestand op `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Zorg ervoor dat de volgende vermeldingen zijn aanwezig en geen opmerkingen uit.  De gebruikersnaam en het wachtwoord wijzigen naar waarden voor uw Zabbix-omgeving.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Start de omsagent-daemon opnieuw

    `sudo sh /opt/microsoft/omsagent/bin/service_control restart`


## <a name="alert-records"></a>Waarschuwing records
U kunt waarschuwingen records van Nagios en Zabbix ophalen met behulp van [zoekopdrachten](../../log-analytics/log-analytics-queries.md) in Log Analytics.

### <a name="nagios-alert-records"></a>Nagios-waarschuwing records

Ontvang een waarschuwing records die zijn verzameld door Nagios hebben een **Type** van **waarschuwing** en een **SourceSystem** van **Nagios**.  Ze hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*Ontvang een waarschuwing* |
| SourceSystem |*Nagios* |
| AlertName |Naam van de waarschuwing. |
| AlertDescription | Beschrijving van de waarschuwing. |
| AlertState | De status van de service of de host.<br><br>OK<br>WAARSCHUWING<br>OMHOOG<br>OMLAAG |
| Hostnaam | Naam van de host die de waarschuwing hebt gemaakt. |
| PriorityNumber | Prioriteit van de waarschuwing. |
| StateType | Het type van de status van de waarschuwing.<br><br>VOORLOPIG - probleem dat niet opnieuw is gecontroleerd.<br>HARDE - probleem dat is een opgegeven aantal keren opnieuw gecontroleerd.  |
| TimeGenerated |De datum en tijd waarop de waarschuwing is gemaakt. |


### <a name="zabbix-alert-records"></a>Waarschuwing Zabbix-records
Ontvang een waarschuwing die worden verzameld door Zabbix records hebben een **Type** van **waarschuwing** en een **SourceSystem** van **Zabbix**.  Ze hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*Ontvang een waarschuwing* |
| SourceSystem |*Zabbix* |
| AlertName | Naam van de waarschuwing. |
| AlertPriority | Ernst van de waarschuwing.<br><br>niet-geclassificeerd<br>informatie<br>waarschuwing<br>gemiddeld<br>Hoog<br>na noodgevallen  |
| AlertState | Status van de waarschuwing.<br><br>0 - status is bijgewerkt.<br>1 - status is onbekend.  |
| AlertTypeNumber | Hiermee geeft u op of in een waarschuwing meerdere gebeurtenissen van het probleem kan worden gegenereerd.<br><br>0 - status is bijgewerkt.<br>1 - status is onbekend.    |
| Opmerkingen | Aanvullende opmerkingen voor de waarschuwing. |
| Hostnaam | Naam van de host die de waarschuwing hebt gemaakt. |
| PriorityNumber | De waarde die aangeeft van ernst van de waarschuwing.<br><br>0 - niet geclassificeerd<br>1 - gegevens<br>2 - waarschuwing<br>3 - gemiddelde<br>4 - hoog<br>5 - noodherstel |
| TimeGenerated |De datum en tijd waarop de waarschuwing is gemaakt. |
| TimeLastModified |Datum en tijd die de status van de waarschuwing voor het laatst is gewijzigd. |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [waarschuwingen](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) in Log Analytics.
* Meer informatie over [zoekopdrachten](../../log-analytics/log-analytics-queries.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren. 
