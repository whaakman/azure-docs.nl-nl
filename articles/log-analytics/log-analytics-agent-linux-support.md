---
title: Problemen met Azure Log Analytics Linux-Agent oplossen | Microsoft Docs
description: Beschrijf de symptomen, de oorzaken en de oplossing voor de meest voorkomende problemen met de Log Analytics Linux-agent.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: baa81a52d4b007cd690a2b01df642cd3775f7d6b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044133"
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Het oplossen van problemen met de Linux-agent voor Log Analytics

Dit artikel bevat informatie over het oplossen van fouten die u mogelijk ondervindt met de Linux-agent voor Log Analytics en duidt op mogelijke oplossingen om op te lossen.

## <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Probleem: Kan geen verbinding maken via proxy naar Log Analytics

### <a name="probable-causes"></a>Waarschijnlijke oorzaken
* De proxy die is opgegeven tijdens de voorbereiding is onjuist
* De Log Analytics en Azure Automation-Service-eindpunten zijn niet opgenomen in de whitelist in uw datacenter 

### <a name="resolutions"></a>Oplossingen
1. Reonboard naar de Log Analytics-service met de OMS-Agent voor Linux met behulp van de volgende opdracht uit met de optie `-v` ingeschakeld. Hiermee wordt uitgebreide uitvoer van de agent verbinding te maken via de proxy voor de OMS-Service. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Raadpleeg de sectie [proxy-instellingen bijwerken](log-analytics-agent-manage.md#update-proxy-settings) om te controleren of de agent kan communiceren via een proxyserver goed is geconfigureerd.    
* Controleer of de volgende Log Analytics-service-eindpunten opgenomen in de whitelist zijn:

    |Agentresource| Poorten | Richting |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Poort 443| Binnenkomend en uitgaand |  
    |*.oms.opinsights.azure.com | Poort 443| Binnenkomend en uitgaand |  
    |*.blob.core.windows.net | Poort 443| Binnenkomend en uitgaand |  
    |*.azure-automation.net | Poort 443| Binnenkomend en uitgaand | 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Probleem: U ontvangt een 403-fout bij het vrijgeven

### <a name="probable-causes"></a>Waarschijnlijke oorzaken
* Datum en tijd is niet correct op Linux-Server 
* Werkruimte-ID en Werkruimtesleutel gebruikt zijn niet juist

### <a name="resolution"></a>Oplossing

1. Controleer de tijd op uw Linux-server met de datum van de opdracht. Als de tijd +/-15 minuten na de huidige tijd is, mislukt onboarding. Op juiste dit bijwerken de datum en/of de tijdzone van de Linux-server. 
2. Controleer of dat u de nieuwste versie van de OMS-Agent voor Linux hebt geïnstalleerd.  De nieuwste versie wordt nu waarschuwt u als tijdverschilbereik wordt veroorzaakt door het onboarding-fout.
3. Reonboard met behulp van de juiste werkruimte-ID en Werkruimtesleutel na de installatie-instructies eerder in dit onderwerp.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Probleem: Ziet u een 500 en 404-fout in het logboekbestand direct na de onboarding
Dit is een bekend probleem dat zich op de eerste uploaden van gegevens van Linux in een Log Analytics-werkruimte voordoet. Dit heeft geen invloed op gegevens die worden verzonden of service-ervaring.

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Probleem: U ziet geen gegevens in Azure portal

### <a name="probable-causes"></a>Waarschijnlijke oorzaken

- Onboarding van de Log Analytics-service is mislukt
- Verbinding met de Log Analytics-service is geblokkeerd
- OMS-Agent voor Linux-gegevens een back-up

### <a name="resolutions"></a>Oplossingen
1. Controleer of onboarding van de service Log Analytics geslaagd is door te controleren of het volgende bestand bestaat: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Met behulp van Reonboard de `omsadmin.sh` opdrachtregelinstructies
3. Als u een proxy gebruikt, raadpleegt u de stappen voor het oplossen van proxy die eerder is verkregen.
4. In sommige gevallen, als de OMS-Agent voor Linux kan niet met de service communiceren gegevens op de agent is in de wachtrij voor de volledige buffergrootte, 50 MB. De OMS-Agent voor Linux moet opnieuw worden gestart door de volgende opdracht uit: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Dit probleem is opgelost in agent versie 1.1.0-28 en hoger.

