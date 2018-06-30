---
title: Verplaatsen naar Azure OMS-portal | Microsoft Docs
description: De OMS-portal wordt sunsetted met alle functionaliteit verplaatsen naar de Azure-portal. In dit artikel biedt details over deze overgang.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e47e8cbd209ea34317ca9b176a2c4b0fef10a2b2
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132969"
---
# <a name="oms-portal-moving-to-azure"></a>OMS-portal verplaatsen naar Azure
Bedankt dat u de OMS-portal. We uw ondersteuning aan te bevelen en doorgaan met intensief investeren in onze services voor bewaking en beheer. Een stukje feedback herhaaldelijk gehoord dat klanten is de noodzaak van een enkele gebruikerservaring bewaken en beheren van zowel on-premises en Azure werkbelastingen. U weet waarschijnlijk de Azure portal is de hub voor alle Azure-services en biedt een uitgebreide management ervaring met mogelijkheden, zoals dashboards voor resources, intelligent zoeken naar resources zoeken en labels voor resourcebeheer vastmaken. Om te consolideren en de bewaking en beheer werkstroom te stroomlijnen, begonnen we met de OMS-portal-mogelijkheden in de Azure-portal toevoegen. We zijn trots de meeste van de functies van de OMS-portal zijn nu deel uit van de Azure-portal. Enkele van de nieuwe functies zoals Traffic Manager zijn in feite is alleen beschikbaar in de Azure portal. Er zijn slechts enkele hiaten resterende, de meest impactful wordt vijf oplossingen die nog in het proces worden verplaatst naar de Azure-portal. Als u deze functies niet gebruikt, zich u voor het uitvoeren van alles wat die u deed in de OMS-portal met de Azure-portal en meer. We raden dat u aan de slag met Azure portal vandaag als u dit nog niet hebt gedaan. 

We verwachten de resterende onderbrekingen tussen de twee portals door augustus 2018 sluiten. Op basis van feedback van klanten, communiceert we de tijdlijn voor sunsetting OMS-portal. We zijn trots verplaatsen naar de Azure-portal en de overgang naar het eenvoudig berekend kunnen worden verwacht. Maar we begrijpen wijzigingen zijn moeilijk en kunnen verstoren. Verzenden van eventuele vragen, feedback of opmerkingen aan LAUpgradeFeedback@microsoft.com. De rest van dit artikel gaat over de belangrijkste scenario's, de huidige hiaten en het overzicht voor deze overgang. 


## <a name="what-will-change"></a>Wat er wordt gewijzigd? 
De volgende wijzigingen zijn met de afschaffing van OMS-portal wordt aangekondigd. Elk van deze wijzigingen wordt nader beschreven in de volgende secties beschreven.

- De nieuwe waarschuwing beheerervaring wordt vervangen door het beheersysteem voor waarschuwing.
- Beheer van de gebruiker toegang wordt uitgevoerd in de Azure-portal met behulp van op rollen gebaseerde toegangsbeheer van Azure.
- De Application Insights-Connector zijn niet langer vereist omdat dezelfde functionaliteit kan worden ingeschakeld via cross-werkruimte query's.
- De OMS mobiele App wordt afgeschaft. 
- Het NSG-oplossing wordt met uitgebreide functionaliteit beschikbaar via Traffic Analytics-oplossing vervangen.



## <a name="current-known-gaps"></a>Huidige bekende hiaten 
Er zijn momenteel enkele functionaliteit hiaten waarvoor u nog steeds de OMS-Portal te gebruiken. Deze ruimten zijn wordt gesloten en dit document wordt op de juiste wijze bijgewerkt. U moet ook verwijzen naar [Azure Updates](https://azure.microsoft.com/updates/?product=log-analytics) continu aankondigingen over uitbreidingen en wijzigingen.

- De volgende oplossingen zijn nog niet volledig functioneel in de Azure portal. U moet doorgaan naar deze oplossingen in de klassieke portal gebruiken totdat ze zijn bijgewerkt.

    - Analytics-oplossingen voor Windows ([gereedheid voor Upgrade](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [Apparaatstatus](https://docs.microsoft.com/windows/deployment/update/device-health-monitor), en [Updatenaleving](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [Analytische gegevens voor DNS](log-analytics-dns.md) 
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Voor toegang tot resources in Azure Log Analytics moet de gebruiker moet toegang worden verleend via [Azure op rollen gebaseerde toegang](#user-access-and-role-migration).
- Planningen van updates die zijn gemaakt met de OMS-portal mogelijk niet gereflecteerd in de geplande update-implementaties of bijwerken van de taakgeschiedenis van het dashboard van de Update management in Azure portal. Door dit gat wordt verwacht aan het einde van juni 2018 worden opgelost.
- Aangepaste logboeken preview-functie kan alleen worden ingeschakeld via de OMS-Portal. Aan het einde van juni 2018, wordt dit automatisch ingeschakeld voor alle werk spaties.
 
## <a name="what-should-i-do-now"></a>Wat moet ik nu doen?  
U moet verwijzen naar [Veelgestelde vragen voor de overgang van OMS-portal naar Azure-portal voor gebruikers van logboekanalyse](../log-analytics/log-analytics-oms-portal-faq.md) voor meer informatie over het overstappen naar de Azure-portal. Als de [hiaten hierboven beschreven](#current-known-gaps) niet van toepassing op uw omgeving, en vervolgens moet u rekening houden met behulp van Azure portal als uw primaire ervaring wordt gestart. Verzenden van eventuele feedback, vragen of opmerkingen aan LAUpgradeFeedback@microsoft.com.

## <a name="changes-to-alerts"></a>Wijzigingen van waarschuwingen 

### <a name="alert-extension"></a>Waarschuwing-uitbreiding  
Waarschuwingen zijn in de [uitgebreid naar de Azure-portal](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Zodra dit voltooid is, management acties op waarschuwingen alleen zijn beschikbaar in Azure-portal. Bestaande waarschuwingen blijven worden vermeld in de OMS-portal. Als u waarschuwingen programmatisch met behulp van de Log Analytics waarschuwing REST-API of Log Analytics waarschuwing Resource-sjabloon, moet u actiegroepen in plaats van acties in de API-aanroepen, sjablonen voor Azure Resource Manager en PowerShell-opdrachten gebruiken.

### <a name="alert-management-solution"></a>Oplossing voor beheer van waarschuwingen
In plaats van de [waarschuwing beheeroplossing](log-analytics-solution-alert-management.md), kunt u [van Azure Monitor unified waarschuwingsmethoden interface](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) te visualiseren en waarschuwingen beheren. Deze nieuwe ervaring aggregeert waarschuwingen uit meerdere bronnen binnen Azure inclusief logboek waarschuwingen van logboekanalyse. U kunt distributies van de waarschuwingen bekijken, te profiteren van geautomatiseerde groepering van gerelateerde berichten via een smartcard groepen en waarschuwingen weergeven voor meerdere abonnementen terwijl geavanceerde filters zijn toegepast. Al deze functies zijn beschikbaar in preview 4 juni 2018 wordt gestart. De oplossing voor beheer van waarschuwingen worden niet beschikbaar in de Azure-portal. 

De gegevens die worden verzameld door de oplossing voor beheer van waarschuwingen (records van het type waarschuwing) blijft in Log Analytics, zolang de oplossing is geïnstalleerd voor de werkruimte. Vanaf augustus 2018, wordt streamen van waarschuwingen van geïntegreerde waarschuwingen in werkruimten ingeschakeld, vervangt deze mogelijkheid. Sommige wijzigingen in het schema worden verwacht en worden aangekondigd op een later tijdstip.

## <a name="user-access-and-role-migration"></a>De migratie van de toegang en rol van de gebruiker
Toegang tot de Azure portal-management is uitgebreidere en krachtiger dan het beheer van toegang in de OMS-Portal, maar er enkele conversies nodig. Zie [werkruimten beheren](log-analytics-manage-access.md#manage-accounts-and-users) voor meer informatie over toegangsbeheer in logboekanalyse.

Vanaf juni 25 en tot juli, bepalen automatische conversie van de toegang tot machtigingen van de OMS-portal naar Azure-portal machtigingen wordt gestart. Zodra de conversie is voltooid, wordt de beheersectie van OMS-Portal-gebruiker gebruikers routeren voor toegangsbeheer (IAM) in Azure. 

Tijdens de conversie wordt het systeem elke gebruiker of beveiligingsgroep die gemachtigd in de OMS-portal is controleren en bepalen of er hetzelfde niveau of de machtigingen in Azure. Als machtigingen ontbreken, wordt deze de volgende rollen voor de relevante werkruimten en -oplossingen toewijzen.

| OMS-portal machtiging | Azure-functie |
|:---|:---|
| Alleen-lezen | Lezer van Log Analytics |
| Inzender | Inzender van Log Analytics |
| Beheerder | Eigenaar |

Om ervoor te zorgen dat geen overmatige machtigingen zijn toegewezen aan gebruikers, wordt deze machtigingen op het niveau van de resourcegroep niet op de automatisch toegewezen door het systeem. Beheerders van de werkruimte toewijzen als gevolg hiervan moeten handmatig zelf _eigenaar_ of _Inzender_ rollen op resource groep of -abonnement niveau van de volgende acties uit te voeren.

- Toevoegen of verwijderen van oplossingen
- Nieuwe aangepaste weergaven definiëren
- Waarschuwingen beheren 

In sommige gevallen kan krijgt de automatische conversie machtiging kan niet worden toegepast en de beheerder machtigingen handmatig toe te wijzen.

## <a name="oms-mobile-app"></a>OMS mobiele App
De mobiele app OMS worden sunsetted samen met de OMS-portal. In plaats van de mobiele app OMS voor toegang tot informatie over uw IT-infrastructuur, dashboards en opgeslagen query's, u kunt toegang tot de Azure-portal rechtstreeks vanuit de browser in uw mobiele apparaat. Als u waarschuwingen, moet u configureren [Azure actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md) om meldingen te ontvangen in de vorm van SMS of een telefoongesprek

## <a name="application-insights-connector-and-solution"></a>Application Insights-Connector en oplossing
[Application Insights-Connector](log-analytics-app-insights-connector.md) biedt een manier om gegevens van Application Insights in een werkruimte voor logboekanalyse. Deze gegevens worden gedupliceerd is vereist voor het inschakelen van zichtbaarheid tussen infrastructuur-en toepassingsgegevens.

Met de ondersteuning van [cross-resource query's](log-analytics-cross-workspace-search.md), is niet langer deze behoefte te dupliceren van gegevens. Hierdoor worden de bestaande Application Insights-oplossing afgeschaft. Vanaf juli kunt pas u weer nieuwe Application Insights-resources koppelen aan Log Analytics-werkruimten. Bestaande koppelingen en dashboards blijven functioneren tot November 2018.


## <a name="azure-network-security-group-analytics"></a>Analyse van Azure-netwerkbeveiligingsgroep
De [Azure Network Security groep Analytics-oplossing](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) wordt vervangen met onlangs geopende [Traffic Analytics](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) waarmee u inzicht krijgen in gebruikers- en activiteit van het cloud-netwerken. Verkeer Analytics kunt u controle-activiteit op het netwerk, beveiligde toepassingen en gegevens van uw organisatie, optimaliseren van de werkbelasting en blijven voldoen aan het beleid. 

Deze oplossing analyseert NSG stromen logboeken en biedt inzicht in de volgende.

- Verkeersstromen tussen uw netwerken tussen Azure en Internet, openbare cloud-regio, vnet's en subnetten.
- Toepassingen en protocollen die in uw netwerk, zonder de noodzaak voor sniffers of speciale stroom verzameling apparaten.
- Bovenste talkers, chatty toepassingen conversaties in de cloud, VM verkeer hotspots.
- Bronnen en doelen van verkeer tussen VNETs, onderlinge relaties tussen kritische zakelijke services en toepassingen.
- Beveiliging met inbegrip van schadelijk verkeer, poorten openen met het Internet, toepassingen of virtuele machines die toegang probeert te krijgen met Internet.
- Capaciteitsverbruik, waardoor u elimineren problemen van via het inrichten of ondermaats gebruik.

U kunt blijven zijn afhankelijk van de diagnostische instellingen voor het verzenden van de NSG-logboeken met logboekanalyse zodat uw bestaande zoekopdrachten, waarschuwingen, dashboards opgeslagen blijven werken. Klanten die de oplossing al hebt geïnstalleerd kunnen blijven gebruiken totdat de aankondiging. Vanaf juni 20 de netwerk-beveiligingsanalyse groep oplossing wordt verwijderd uit de marketplace en beschikbaar gesteld via de community als een [Snelstartsjabloon met de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="next-steps"></a>Volgende stappen
- Zie [Veelgestelde vragen voor de overgang van OMS-portal naar Azure-portal voor gebruikers van logboekanalyse](log-analytics-oms-portal-faq.md) voor hulp bij het verplaatsen van de OMS-portal naar de Azure-portal.