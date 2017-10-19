---
title: Uw beveiligingsproducten koppelen aan de beveiligings- en controleoplossing van de Operations Management Suite (OMS) | Microsoft Docs
description: Met dit document kunt u uw beveiligingsproducten koppelen aan de beveiligings- en controleoplossing van de Operations Management Suite met behulp van Common Event Format.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 710a1fe0ce2b7a1841187cf75f4ffb090cc161e5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connecting-your-security-products-to-the-operations-management-suite-oms-security-and-audit-solution"></a>Uw beveiligingsproducten koppelen aan de beveiligings- en controleoplossing van de Operations Management Suite (OMS) 
Met dit document kunt u uw beveiligingsproducten koppelen aan de beveiligings- en controleoplossing van de OMS. De volgende bronnen worden ondersteund:

- Common Event Format-gebeurtenissen (CEF)
- Cisco ASA-gebeurtenissen


## <a name="what-is-cef"></a>Wat is CEF?
Common Event Format (CEF) is een standaardindeling in de branche voor Syslog-berichten. De indeling wordt door veel leveranciers van beveiligingsproducten gebruikt om gebeurtenisinteroperabiliteit tussen verschillende platforms mogelijk te maken. De beveiligings- en controleoplossing van OMS biedt ondersteuning voor gegevensopname met CEF. Daardoor kunt u uw beveiligingsproducten koppelen aan OMS Security. 

Als u uw gegevensbron koppelt aan OMS kunt u profiteren van de volgende mogelijkheden die onderdeel uitmaken van dit platform:

- Zoeken en correlatie
- Controleren
- Waarschuwing
- Bedreigingsinformatie
- Problemen die aandacht vereisen

## <a name="collection-of-security-solution-logs"></a>Logboeken van beveiligingsoplossingen verzamelen

OMS Security biedt ondersteuning voor het verzamelen van logboeken middels CIS via Syslogs en [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/)-logboeken. In dit voorbeeld is de bron (de computer die de logboeken genereert) een Linux-computer met syslog-ng daemon. De bestemming is OMS Security. U moet de volgende taken uitvoeren om de Linux-computer voor te bereiden:

- Download de OMS-agent voor Linux (versie 1.2.0-25 of hoger).
- Volg de sectie **Korte installatiehandleiding** in [dit artikel](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) om de agent in uw werkruimte te installeren en vrij te geven.

Normaal gesproken wordt de agent op een andere computer geïnstalleerd dan op de computer waarop de logboeken worden gegenereerd. De volgende stappen zijn normaal nodig om de logboeken door te sturen naar de computer met de agent:

- Configureer het product/apparaat waarop de logboeken worden gemaakt zodanig dat de vereiste gebeurtenissen worden doorgestuurd naar de syslog-daemon (rsyslog of syslog-ng) op de computer met de agent.
- Stel in dat de syslog-daemon op de computer met de agent berichten mag ontvangen vanaf een extern systeem.

Op de computer met de agent moeten de gebeurtenissen vanuit de syslog-daemon worden verzonden naar lokale UDP-poort 25226. De agent houdt in de gaten of er gebeurtenissen binnenkomen via deze poort. Het volgende is een voorbeeldconfiguratie voor het verzenden van alle gebeurtenissen van het lokale systeem naar de agent (u kunt de configuratie aanpassen zodat deze aansluit op uw lokale instellingen):

1. Open het terminalvenster en ga naar de map */etc/syslog-ng/* 
2. Maak een nieuw bestand (*security-config-omsagent.conf*) en voeg de volgende inhoud toe: OMS_facility = local4
    
    filter f_local4_oms { facility(local4); };

    destination security_oms { tcp("127.0.0.1" port(25226)); };

    log { source(src); filter(f_local4_oms); destination(security_oms); };
    
3. Download het bestand *security_events.conf* en plaats het in */etc/opt/microsoft/omsagent/conf/omsagent.d/* op de computer met de OMS-agent.
4. Voer de volgende opdracht in om de syslog-daemon opnieuw op te starten: *Voer voor syslog-ng het volgende uit:*
    
    ```
    sudo service rsyslog restart
    ```

    *Voer voor rsyslog het volgende uit:*
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. Voer de onderstaande opdracht in om de OMS-agent opnieuw op te starten:

    *Voer voor syslog-ng het volgende uit:*
    
    ```
    sudo service omsagent restart
    ```

    *Voer voor rsyslog het volgende uit:*
    
    ```
    systemctl restart omsagent
    ```
6. Voer de onderstaande opdracht in en bekijk het resultaat om te controleren of er fouten in het logboek van de OMS-agent staan:

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a>Verzamelde beveiligingsgebeurtenissen controleren

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

Wanneer de configuratie is voltooid, wordt de beveiligingsgebeurtenis opgenomen door OMS Security. Als u de gebeurtenissen wilt visualiseren, opent u Zoeken in logboeken en voert u de opdracht *Type=CommonSecurityLog* in in het zoekveld. Druk vervolgens op ENTER. In het volgende voorbeeld ziet u het resultaat van deze opdracht. In dit geval zijn in OMS Security al beveiligingslogboeken van meerdere leveranciers opgenomen:
   
![Basislijnevaluatie in Beveiliging en controle in OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

U kunt deze zoekopdracht verfijnen voor één enkele leverancier om bijvoorbeeld online Cisco-logboeken te visualiseren. Typ daarvoor: *Type=CommonSecurityLog DeviceVendor=Cisco*. CommonSecurityLog bevat vooraf gedefinieerde velden voor eventuele CEF-headers, waaronder de basisextensies. Alle andere extensies, of dat nu aangepaste extensies zijn of niet, worden ingevoerd in het veld AdditionalExtensions. U kunt de functie Aangepaste velden gebruiken voor speciale velden. 

### <a name="accessing-computers-missing-baseline-assessment"></a>Computers openen waarop de evaluatie van de basislijn ontbreekt
OMS ondersteunt het basislijnprofiel van het domeinlid op Windows Server 2008 R2 tot en met Windows Server 2012 R2. De basislijn voor Windows Server 2016 is nog niet helemaal klaar en wordt toegevoegd zodra deze is gepubliceerd. Alle andere besturingssystemen die via basislijnevaluatie in Beveiliging en controle in OMS zijn gescand, worden weergegeven onder de sectie **Computers waarop de evaluatie van de basislijn ontbreekt**.

## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe u uw CEF-oplossing koppelt aan OMS. Raadpleeg de volgende artikelen voor meer informatie over OMS Beveiliging:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md)
* [Resources bewaken in de oplossing Beveiliging en controle van Operations Management Suite ](oms-security-monitoring-resources.md)

