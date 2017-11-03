---
title: Verzamelen van Nagios en Zabbix waarschuwingen in OMS Log Analytics | Microsoft Docs
description: Nagios en Zabbix zijn open-source hulpprogramma's voor controle. U kunt waarschuwingen van deze hulpprogramma's om te kunnen analyseren ze samen met waarschuwingen uit andere bronnen in logboekanalyse verzamelen.  Dit artikel wordt beschreven hoe u configureert de OMS-Agent voor Linux voor het verzamelen van waarschuwingen van deze systemen.
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
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 0b64c32e1031e704d50aab0b38eaea41e27d134b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Waarschuwingen verzamelen van Nagios en Zabbix in logboekanalyse van OMS-Agent voor Linux 
[Nagios](https://www.nagios.org/) en [Zabbix](http://www.zabbix.com/) open-source hulpprogramma's voor controle zijn.  U kunt waarschuwingen van deze hulpprogramma's in logboekanalyse verzamelen om te kunnen analyseren ze samen met [waarschuwingen uit andere bronnen](log-analytics-alerts.md).  Dit artikel wordt beschreven hoe u configureert de OMS-Agent voor Linux voor het verzamelen van waarschuwingen van deze systemen.
 
## <a name="configure-alert-collection"></a>Verzamelen van waarschuwingen configureren

### <a name="configuring-nagios-alert-collection"></a>Nagios verzamelen van waarschuwingen configureren
Voer de volgende stappen uit op de server Nagios om waarschuwingen te verzamelen.

1. De gebruiker verlenen **omsagent** leestoegang tot het logboekbestand Nagios (dat wil zeggen `/var/log/nagios/nagios.log`). Ervan uitgaande dat het bestand nagios.log is eigendom van de groep `nagios`, kunt u de gebruiker toevoegen **omsagent** naar de **nagios** groep. 

    sudo usermod - a -G nagios omsagent

2.  Wijzig het configuratiebestand op (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Zorg ervoor dat de volgende vermeldingen zijn aanwezig en niet opmerkingen uit:  

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

3. De daemon omsagent starten

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Zabbix verzamelen van waarschuwingen configureren
Voor het verzamelen van waarschuwingen van een Zabbix-server, moet u een gebruiker en het wachtwoord in te geven *leesbare tekst*. Dit is niet ideaal, maar we raden u maken van de gebruiker en verleen machtigingen voor het bewaken van onlu.

Voer de volgende stappen uit op de server Nagios om waarschuwingen te verzamelen.

1. Wijzig het configuratiebestand op (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Zorg ervoor dat de volgende vermeldingen zijn aanwezig en niet opmerkingen uit.  De gebruikersnaam en wachtwoord te wijzigen waarden voor uw omgeving Zabbix.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. De daemon omsagent starten

    sudo servicel /opt/microsoft/omsagent/bin/service_control opnieuw starten


## <a name="alert-records"></a>Waarschuwing records
U kunt waarschuwingen records ophalen uit Nagios en Zabbix met [Meld zoekopdrachten](log-analytics-log-searches.md) in logboekanalyse.

### <a name="nagios-alert-records"></a>Waarschuwing Nagios records

Waarschuwing die is verzameld door Nagios-records hebben een **Type** van **waarschuwing** en een **SourceSystem** van **Nagios**.  Ze hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*Een waarschuwing* |
| SourceSystem |*Nagios* |
| AlertName |De naam van de waarschuwing. |
| AlertDescription | Beschrijving van de waarschuwing. |
| AlertState | Status van de service of de host.<br><br>OK<br>WAARSCHUWING<br>OMHOOG<br>OMLAAG |
| Hostnaam | De naam van de host die de waarschuwing wordt gemaakt. |
| PriorityNumber | Het prioriteitsniveau van de waarschuwing. |
| StateType | Het type van de status van de waarschuwing.<br><br>SOFT - probleem dat niet opnieuw is gecontroleerd.<br>HARDE - probleem dat is een opgegeven aantal keren opnieuw gecontroleerd.  |
| TimeGenerated |De datum en tijd waarop de waarschuwing is gemaakt. |


### <a name="zabbix-alert-records"></a>Waarschuwing Zabbix-records
Waarschuwing die is verzameld door Zabbix-records hebben een **Type** van **waarschuwing** en een **SourceSystem** van **Zabbix**.  Ze hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*Een waarschuwing* |
| SourceSystem |*Zabbix* |
| AlertName | De naam van de waarschuwing. |
| AlertPriority | Ernst van de waarschuwing.<br><br>niet geclassificeerd<br>Informatie<br>Waarschuwing<br>Gemiddelde<br>Hoog<br>noodherstel  |
| AlertState | Status van de waarschuwing.<br><br>0 - status is bijgewerkt.<br>1 - status is onbekend.  |
| AlertTypeNumber | Hiermee geeft u op of in een waarschuwing meerdere gebeurtenissen van het probleem kan worden gegenereerd.<br><br>0 - status is bijgewerkt.<br>1 - status is onbekend.    |
| Opmerkingen | Aanvullende opmerkingen voor waarschuwing. |
| Hostnaam | De naam van de host die de waarschuwing wordt gemaakt. |
| PriorityNumber | Waarde die aangeeft ernst van de waarschuwing.<br><br>0 - niet geclassificeerd<br>1 - informatie<br>2 - waarschuwing<br>3 - gemiddelde<br>4 - hoog<br>5 - noodherstel |
| TimeGenerated |De datum en tijd waarop de waarschuwing is gemaakt. |
| TimeLastModified |Datum en tijd die de status van de waarschuwing voor het laatst is gewijzigd. |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [waarschuwingen](log-analytics-alerts.md) in logboekanalyse.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren. 
