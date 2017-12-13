---
title: Problemen met Azure Log Analytics Linux Agent | Microsoft Docs
description: Beschrijf de symptomen, de oorzaken en de oplossing voor de meest voorkomende problemen met de Log Analytics Linux-agent.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: magoedte
ms.openlocfilehash: 4f9eaa330ba90052080fc18def974d5dec43c1e0
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Het oplossen van problemen met het Linux-agent voor logboekanalyse

Dit artikel bevat help het oplossen van problemen die u mogelijk ondervindt met de Linux-agent voor logboekanalyse en mogelijke oplossingen om op te lossen ze voorgesteld.

### <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Probleem: Kan geen verbinding maken via proxy met Log Analytics

#### <a name="probable-causes"></a>Mogelijke oorzaken
* De proxy die is opgegeven tijdens de voorbereiding is onjuist
* De Log Analytics en Azure Automation-Service-eindpunten zich niet wilt plaatsen in uw datacenter 

#### <a name="resolutions"></a>Oplossingen
1. Reonboard naar de Log Analytics-service met de OMS-Agent voor Linux met behulp van de volgende opdracht met de optie `-v` ingeschakeld. Hiermee worden uitgebreide uitvoer van de agent een verbinding via de proxy die de OMS-Service. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Raadpleeg de sectie [configuratie van de agent voor gebruik met een proxy of Gateway OMS](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway) om te controleren of u de agent communiceren via een proxyserver correct hebben geconfigureerd.    
* Controleer of de volgende Log Analytics-service-eindpunten goedgekeurde lijst zijn:

    |Agentresource| Poorten |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Poort 443|   
    |*.oms.opinsights.azure.com | Poort 443|   
    |ods.systemcenteradvisor.com | Poort 443|   
    |*.BLOB.Core.Windows.NET/ | Poort 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Probleem: U ontvangt een fout 403 bij een poging om vrij te geven

#### <a name="probable-causes"></a>Mogelijke oorzaken
* Datum en tijd is onjuist op Linux-Server 
* Werkruimte-ID en Werkruimtesleutel gebruikt, zijn niet juist

#### <a name="resolution"></a>Oplossing

1. Controleer de tijd op uw Linux-server met de datum van de opdracht. Als de tijd +/-15 minuten van huidige tijd is, mislukt de voorbereiding. Op juiste dit bijwerken de datum en/of de tijdzone van uw Linux-server. 
2. Controleer of dat u de nieuwste versie van de OMS-Agent voor Linux hebt geïnstalleerd.  De nieuwste versie nu waarschuwt u als tijdverschilbereik wordt veroorzaakt door de mislukte voorbereiding.
3. Reonboard juiste werkruimte-ID en Werkruimtesleutel na de installatie-instructies eerder in dit onderwerp.

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Probleem: U een 500 en 404-fout in het logboekbestand ziet direct na het voorbereiden
Dit is een bekend probleem dat op de eerste uploaden van gegevens van Linux in een werkruimte voor logboekanalyse optreedt. Dit heeft geen invloed op de gegevens worden verzonden of service-ervaring.

### <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Probleem: U ze niet alle gegevens in de Azure portal ziet

#### <a name="probable-causes"></a>Mogelijke oorzaken

- Voorbereidingen voor de Log Analytics-service is mislukt
- Verbinding met de Log Analytics-service is geblokkeerd
- OMS-Agent voor Linux-gegevens back-up is gemaakt

#### <a name="resolutions"></a>Oplossingen
1. Controleer als voorbereiding op de Log Analytics-service gelukt is door te controleren of het volgende bestand bestaat:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard met behulp van de `omsadmin.sh` opdrachtregelinstructies
3. Als u een proxy gebruikt, raadpleegt u de stappen voor het oplossen van proxy die eerder is verkregen.
4. In sommige gevallen wanneer de OMS-Agent voor Linux kan niet met de service communiceren, gegevens op de agent zich in de wachtrij voor de volledige buffergrootte, 50 MB. De OMS-Agent voor Linux moet opnieuw worden gestart met de volgende opdracht: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Dit probleem is opgelost in agent versie 1.1.0-28 en hoger.

### <a name="issue-omsagent-creates-excessive-number-of-user-process-on-computer-and-never-terminates-them"></a>Probleem: OMSAgent uitzonderlijk groot aantal gebruikersproces maakt op de computer en deze nooit wordt beëindigd
Wanneer u oplossingen voor die ondersteuning bieden voor het beheren van uw virtuele Linux-machines inschakelt, start het een aantal processen op de Linux-agent. Maar voordat het proces wordt beëindigd, een ander proces wordt gestart vanwege een bekend probleem. 

#### <a name="resolutions"></a>Oplossingen
Als u wilt wijzigen van het aantal gebruikersprocessen dat kan worden gegenereerd door de OMSAgent, moet u de agent via omsadmin.sh configureren.  Het aantal processen die worden gegenereerd door de standaardwaarde is 75. voordat u de limiet wijzigt, moet u eerst de volgende opdracht om te zien hoeveel OMSAgent processen die momenteel actief zijn uitvoeren: `ps aux | grep -E '^omsagent' | wc -l`.  
U kunt controleren wat de huidige limiet is ingesteld op met de volgende opdracht:`cat /etc/security/limits.conf | grep -E '^omsagent'`

Gebruik de volgende opdrachten voor het configureren van een aangepast proceslimiet ofwel of de limiet voor het proces weer instellen op de standaardwaarde.

1. De limiet voor het proces voor OMSAgent instellen: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh -n <specific number limit>`.<br>Houd er rekening mee dat de ondergrens die kan worden ingesteld op 5.  

2. De limiet voor het proces voor OMSAgent terug naar de standaardwaarde instellen:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh -N`

Controleer of de nieuwe instelling is toegepast door het uitvoeren van de volgende opdracht: `cat /etc/security/limits.conf | grep -E '^omsagent'`.  Als u de nieuwe configuratie is toegepast niet ziet, is het mogelijk omdat u de limiet voor het proces te laag ingesteld.  