---
title: Bewaking in Microsoft Azure | Microsoft Docs
description: Opties wanneer u wilt bewaken alles in Microsoft Azure. Monitor voor Azure, Application Insights en Log Analytics
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: robb
ms.openlocfilehash: c34211e0c55c10defaa32f1e0a2195514ff3ae5f
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Overzicht van de bewaking in Microsoft Azure
Dit artikel bevat een overzicht van de hulpprogramma's en services die zijn betrokken bij holistische bewaking van Microsoft Azure. Van toepassing op:
- Azure-services gebruiken om Azure-infrastructuur en toepassingen te bewaken
- Met behulp van Azure services voor hybride monitor en niet-Azure-infrastructuur en toepassingen
- Niet-Azure-services gebruiken om Azure-infrastructuur en toepassingen te bewaken

Dit artikel wordt beschreven voor de verschillende producten en services die beschikbaar zijn en hoe ze samenwerken. Dit kan helpen u om te bepalen welke programma's zijn het meest geschikt is voor u in welke gevallen.  

## <a name="why-use-azures-monitoring-services"></a>Waarom Azure bewaking services gebruiken?

Prestatieproblemen in uw cloud-app kunnen invloed hebben op uw bedrijf. Degradations kan met meerdere onderdelen onderling verbonden en releases vaak gebeuren op elk gewenst moment. En als u een app ontwikkelt, uw gebruikers problemen die u hebt niet vinden in het testen van meestal te detecteren. U moet weten over deze problemen direct en hulpprogramma's voor het opsporen en oplossen van de problemen hebben. Problemen in uw toepassing kunnen bovendien leiden van de onderliggende infrastructuur waarop deze toepassingen worden uitgevoerd, zodat een holistische weergave van uw toepassing en de infrastructuur die essentieel is voor bewaking van uw Azure-omgeving. Microsoft Azure is een reeks hulpmiddelen voor het opsporen en oplossen van dergelijke problemen.

## <a name="how-do-i-monitor-my-azure-environment"></a>Hoe bewaak ik mijn Azure-omgeving

Er zijn een reeks hulpmiddelen voor het bewaken van uw Azure-omgeving, vanuit de toepassingscode die in Azure wordt uitgevoerd tot de services en de infrastructuur voor uw toepassing te hosten. Deze hulpprogramma's werken samen om uitgebreide bewaking bieden en omvatten:

-   **Monitor voor Azure** -de Azure-service die wordt uitgevoerd als een geconsolideerde pijplijn voor alle bewakingsgegevens van Azure-services. Deze hebt u toegang tot de maatstaven voor prestaties en gebeurtenissen die worden beschreven van de werking van de Azure-infrastructuur en alle Azure-services die u gebruikt. Monitor voor Azure is een controle pijplijn gegevens voor uw Azure-omgeving en die gegevens rechtstreeks in Log Analytics, evenals 3e hulpprogramma's van leveranciers kunt u meer inzicht krijgen in die gegevens en combineren met gegevens van lokale of andere cloudbronnen biedt.

-   **Application Insights** -de Azure-service die application performance monitoring en de gebruiker analytics biedt. Hiermee bewaakt u de code die u hebt geschreven en de toepassingen die u hebt geïmplementeerd in Azure, on-premises of andere clouds. Door het instrumenteren van uw toepassing met de Application Insights-SDK als u krijgt toegang tot grote hoeveelheden gegevens met inbegrip van reactietijden van afhankelijkheden, uitzondering traceringen foutopsporing momentopnamen en profielen kan worden uitgevoerd. Het biedt krachtige hulpprogramma's voor het analyseren van deze toepassingstelemetrie bij de ontwikkeling en het besturingssysteem van uw toepassing. Nauw worden geïntegreerd met Visual Studio, zodat u kunt direct aan de probleem (s) van code zodat u kunt dit verhelpen en biedt gebruiksanalyse voor het analyseren van uw toepassingen voor productmanagers ook gebruik van de klant.

-   **Meld u Analytics** -voorheen bekend als OMS Log Analytics, is een Azure-service die logboek en meetwaarde op die gegevens uit Azure-services (via Azure Monitor), Azure Virtual machines, en on-premises of andere cloudinfrastructuur opgenomen en biedt flexibele logboek zoeken en out van het vak analytics boven op deze gegevens. Het biedt uitgebreide hulpprogramma's om gegevens te analyseren alle bronnen, kunt u complexe query's via alle logboeken en proactief kan waarschuwing bij de opgegeven voorwaarden.  U kunt zelfs aangepaste gegevens verzamelen in de centrale opslagplaats, zodat u kunt opvragen en visualiseren van deze. U kunt ook te profiteren van het logboek analytische ingebouwde oplossingen onmiddellijk om inzicht te krijgen in de beveiliging en functionaliteit van uw infrastructuur.

## <a name="accessing-monitoring-in-the-azure-portal"></a>Toegang tot de bewaking in de Azure portal
Alle Azure-bewaking services zijn nu beschikbaar in één UI deelvenster. Zie voor meer informatie over toegang krijgen tot dit gebied [aan de slag met Azure Monitor](monitoring-get-started.md). 

U kunt bewaking functies voor specifieke Azure-resources ook openen door te selecteren die bronnen en inzoomen op hun controle-opties. 

## <a name="examples-of-when-to-use-which-tool"></a>Voorbeelden van wanneer u welke hulpprogramma 

De volgende secties ziet u enkele algemene scenario's en welke hulpprogramma's moeten samen worden gebruikt. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Scenario 1: Fix fouten in een Azure-toepassing in ontwikkeling   

**De beste oplossing is Application Insights, Azure bewaken en Visual Studio samen gebruiken**

Azure biedt nu de kracht van de Visual Studio-foutopsporing in de cloud. Azure-Monitor voor het verzenden van telemetrie naar Application Insights configureren. Visual Studio Application Insights-SDK opnemen in uw toepassing inschakelen. Eenmaal in Application Insights, kunt u de kaart toepassing visueel te ontdekken welke onderdelen van uw actieve toepassing wel of niet in orde zijn. Voor de onderdelen die niet in orde, zijn fouten en uitzonderingen al beschikbaar voor onderzoek. U kunt de verschillende analytics in Application Insights dieper. Als u niet zeker weet over de fout, kunt u de foutopsporingsfunctie als u wilt traceren in code en de pincode van een probleem verder. 

Zie voor meer informatie [Web-Apps bewaken](../application-insights/app-insights-azure-web-apps.md) en verwijzen naar de inhoudsopgave links voor instructies over de verschillende typen apps en talen.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Scenario 2: fouten opsporen in een webtoepassing Azure .NET voor fouten die alleen in productie weergeven 

> [!NOTE]
> Deze functies zijn Preview-versie. 

**De beste optie is het gebruik van Application Insights en als mogelijke Visual Studio voor de volledige foutopsporing optreden.**

Gebruik de Application Insights momentopname foutopsporing fouten opsporen in uw app. Wanneer een bepaalde drempelwaarde voor fout met productieonderdelen optreedt, het systeem automatisch wordt vastgelegd telemetrie in windows van keer aangeroepen "momentopnamen." De hoeveelheid vastgelegd is voor een productie-cloud veilig omdat deze niet te beïnvloeden klein genoeg maar aanzienlijke om toe te staan tracering.  Het systeem kan meerdere momentopnamen vastleggen. U kunt zoeken op een punt in tijd in de Azure portal of Visual Studio gebruiken voor de volledige functionaliteit. Met Visual Studio kunnen ontwikkelaars doorlopen die momentopname alsof ze zijn foutopsporing in realtime. Lokale variabelen, parameters, geheugen en frames zijn allemaal beschikbaar. Ontwikkelaars moeten toegangsrechten voor deze productiegegevens via een [RBAC-rol](../active-directory/role-based-access-built-in-roles.md).  

Zie voor meer informatie [foutopsporing in momentopname](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Scenario 3: fouten opsporen in een Azure-toepassing die gebruikmaakt van containers of microservices 

**Hetzelfde als scenario 1. Application Insights, Azure bewaken en Visual Studio samen gebruiken**

Application Insights ondersteunt ook verzamelen telemetrie uit de processen die worden uitgevoerd binnen de containers en microservices (Kubernetes, Docker, Azure Service Fabric). Voor meer informatie [deze video bekijken in het foutopsporingsprogramma containers en microservices](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Scenario 4: prestaties van de oplossing problemen in uw Azure-toepassing

De [Application Insights profiler](../application-insights/app-insights-profiler.md) is ontworpen voor het oplossen van problemen. U kunt identificeren en oplossen van prestatieproblemen voor toepassingen die worden uitgevoerd in App Services (Web-Apps, Logic Apps, Mobile Apps, API-Apps, Apps van de functie) en andere bronnen zoals virtuele Machines, virtuele-machineschaalsets (VMSS), Cloud Services berekenen en Service Fabric. 

> [!NOTE]
> De mogelijkheid aan virtuele Machines, virtuele-machineschaalsets (VMSS), Cloud Services en -Services Fabric-profiel is een Preview-versie.   

Bovendien u proactief een melding per e-mail over bepaalde soorten fouten, zoals laadtijden voor pagina langzaam, door het hulpprogramma Slimme detectie.  U hoeft niet te geen instellingen op dit hulpprogramma. Zie voor meer informatie [Slimme detectie - afwijkingen](../application-insights/app-insights-proactive-performance-diagnostics.md).



## <a name="next-steps"></a>Volgende stappen
Meer informatie over

* [Azure Monitor in een video Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Aan de slag met Azure Monitor](monitoring-get-started.md)
* [Azure Diagnostics](../azure-diagnostics.md) als u probeert te analyseren van problemen in uw Cloud-Service, de virtuele Machine virtuele machine instellen of de Service Fabric-toepassing schalen.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) als u te diagnostische problemen in uw App Service-Web-app probeert.
* [Meld u Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) en de [Operations Management Suite](https://www.microsoft.com/oms/) bewakingsoplossing productie
