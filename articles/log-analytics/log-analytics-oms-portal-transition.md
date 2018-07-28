---
title: OMS-portal naar Azure verplaatst | Microsoft Docs
description: De OMS-portal wordt sunsetted met alle functionaliteit verplaatst naar de Azure portal. Dit artikel bevat informatie over deze overgang.
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
ms.date: 07/27/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 1a8ccc818cafac4867cb533c83f297af61a21836
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309099"
---
# <a name="oms-portal-moving-to-azure"></a>OMS-portal naar Azure verplaatsen
Een stukje feedback herhaaldelijk op basis van Log Analytics-klanten is de noodzaak voor een enkele gebruikerservaring bewaken en beheren van zowel on-premises en Azure-workloads. U weet waarschijnlijk de Azure-portal is de hub voor alle Azure-services en biedt een uitgebreide beheerervaring, met mogelijkheden zoals dashboards om vast te zetten resources, intelligent zoeken naar resources zoeken en labels voor resourcebeheer. Om te consolideren en de controle en beheer de werkstroom te stroomlijnen, begonnen we met het toevoegen van de OMS-portal-mogelijkheden in Azure portal. We kondigen met trots de meeste van de functies van de OMS-portal zijn nu onderdeel van de Azure-portal. Enkele van de nieuwe functies zoals Traffic Analytics zijn in feite alleen beschikbaar in Azure portal. Er zijn slechts een paar hiaten resterende met inbegrip van een aantal oplossingen die zich nog steeds in het proces moet worden verplaatst naar Azure portal. Als u deze functies niet gebruikt, wordt u mogelijk zijn om uit te voeren van alles wat die je mee bezig was in de OMS-portal met de Azure-portal en meer. Als u dit nog niet hebt gedaan, raden wij aan dat u start met behulp van de Azure-portal vandaag nog! 

We verwachten dat de resterende onderbrekingen tussen de twee portals door augustus 2018 sluiten. Op basis van feedback van klanten, zullen we de tijdlijn voor de OMS-portal sunsetting communiceren. We zijn trots te verplaatsen naar de Azure portal en de overgang naar eenvoudig worden verwacht. Maar we begrijpen dat wijzigingen zijn moeilijk en kunnen worden ervaren. Verzenden van eventuele vragen, feedback of opmerkingen aan **LAUpgradeFeedback@microsoft.com**. De rest van dit artikel gaat over de belangrijkste scenario's, de huidige hiaten en het schema voor deze overgang. 

## <a name="progress"></a>Voortgang
Hieronder vindt u updates die zijn uitgevoerd sinds de vorige versies van dit artikel.

### <a name="july-27"></a>27 juli

- [DNS Analytics](log-analytics-dns.md) is nu volledig functioneel in Azure portal.
- [Updatebeheer](../automation/automation-update-management.md) is bijgewerkt om te worden volledig functioneel in Azure portal. Zie [uw OMS-Update-implementaties te migreren naar Azure](../automation/migrate-oms-update-deployments.md) voor meer informatie.
- [Waarschuwingen](#changes-to-alerts) nu volledig zijn uitgebreid in de Azure-portal.
- [Aangepaste logboeken preview-functie](log-analytics-data-sources-custom-logs.md) is nu automatisch ingeschakeld voor alle werkruimten.

## <a name="what-will-change"></a>Wat verandert er? 
De volgende wijzigingen zijn met de afschaffing van de OMS-portal wordt aangekondigd. Elk van deze wijzigingen wordt beschreven in de secties hieronder in meer detail.

- U kunt zich alleen in de Azure-portal een nieuwe werkruimte maakt.
- De nieuwe ervaring voor waarschuwingen te beheren, wordt vervangen door de oplossing voor beheer van de waarschuwing.
- Beheer van de gebruiker toegang wordt uitgevoerd in Azure portal met op rollen gebaseerd toegangsbeheer in Azure.
- De Application Insights-Connector is niet langer vereist omdat dezelfde functionaliteit kan worden ingeschakeld via meerdere werkruimten query's.
- De mobiele OMS-App wordt afgeschaft. 
- De NSG-oplossing wordt met uitgebreide functionaliteit die beschikbaar zijn via Traffic Analytics-oplossing vervangen.
- Nieuwe verbindingen van System Center Operations Manager naar Log Analytics vereist bijgewerkte management packs.


## <a name="current-known-gaps"></a>Huidige bekende hiaten 
Er zijn momenteel enkele functionaliteit hiaten waarvoor u nog steeds gebruikmaken van de OMS-Portal. Deze ruimten zijn wordt gesloten en dit document wordt op de juiste wijze worden bijgewerkt. U moet ook verwijzen naar [Azure Updates](https://azure.microsoft.com/updates/?product=log-analytics) voor continue aankondigingen over extensies en wijzigingen.

- De volgende oplossingen zijn nog niet volledig functioneel in Azure portal. U moet echter ook doorgaan met deze oplossingen in de klassieke portal totdat ze zijn bijgewerkt.

    - Windows Analytics-oplossingen ([gereedheid voor Upgrade](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [Apparaatstatus](https://docs.microsoft.com/windows/deployment/update/device-health-monitor), en [Updatenaleving](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Voor toegang tot Log Analytics-resource in Azure, de gebruiker moet toegang worden verleend via [Azure op rollen gebaseerde toegang](#user-access-and-role-migration).


## <a name="what-should-i-do-now"></a>Wat moet ik nu doen?  
U moet verwijzen naar [Veelgestelde vragen voor overgang van OMS-portal naar Azure-portal voor gebruikers van de Log Analytics](../log-analytics/log-analytics-oms-portal-faq.md) voor informatie over het overstappen naar de Azure-portal. Als de [hiaten die hierboven worden beschreven](#current-known-gaps) gelden niet voor uw omgeving, en vervolgens moet u rekening houden met behulp van Azure portal als uw primaire ervaring starten. Alle feedback, vragen of opmerkingen aan verzenden LAUpgradeFeedback@microsoft.com.

## <a name="new-workspaces"></a>Nieuwe werkruimten
Vanaf juli 29 kunt wordt u niet langer om nieuwe werkruimten met behulp van de OMS-portal te maken. Volg de instructies in [een Log Analytics-werkruimte maken in Azure portal](log-analytics-quick-create-workspace.md) een nieuwe werkruimte maken in Azure portal.

## <a name="changes-to-alerts"></a>Wijzigingen van waarschuwingen 

### <a name="alert-extension"></a>Waarschuwing-extensie  

> [!NOTE]
> Waarschuwingen zijn nu volledig uitgebreid naar Azure portal. Bestaande waarschuwingsregels kunnen worden weergegeven in de OMS-portal, maar ze kunnen alleen worden beheerd in Azure portal.

Waarschuwingen zijn momenteel wordt [uitgebreid naar Azure portal](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Zodra dit voltooid is, zijn acties voor waarschuwingen alleen worden beschikbaar in Azure portal. Bestaande-waarschuwingen blijven worden vermeld in de OMS-portal. Als u waarschuwingen programmatisch met behulp van de Log Analytics Alert REST API of Log Analytics-waarschuwing Resourcesjabloon opent, moet u actiegroepen gebruiken in plaats van acties in uw API-aanroepen, Azure Resource Manager-sjablonen en PowerShell-opdrachten.

### <a name="alert-management-solution"></a>Oplossing voor waarschuwing
In plaats van de [waarschuwingenbeheeroplossing](log-analytics-solution-alert-management.md), kunt u [van Azure Monitor unified interface waarschuwingen](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) te visualiseren en waarschuwingen beheren. Deze nieuwe ervaring aggregeert waarschuwingen uit meerdere bronnen binnen Azure inclusief waarschuwingen van Log Analytics. U kunt distributies van uw waarschuwingen bekijken, profiteer van geautomatiseerde groeperen van gerelateerde waarschuwingen via slimme groepen en weergeven van waarschuwingen voor meerdere abonnementen terwijl geavanceerde filters zijn toegepast. Al deze functies zijn beschikbaar in Preview-versie 4 juni 2018 beginnen. Het beheersysteem voor de waarschuwing is niet beschikbaar in de Azure portal. 

De gegevens die zijn verzameld door de oplossing van waarschuwingen te beheren (records met een type van de waarschuwing) blijft in Log Analytics, zolang de oplossing voor de werkruimte is geïnstalleerd. Vanaf augustus 2018, wordt streaming van waarschuwingen van geïntegreerde waarschuwingen in werkruimten ingeschakeld, vervangt deze mogelijkheid. Sommige wijzigingen in het schema worden verwacht en op een later tijdstip worden aangekondigd.

## <a name="user-access-and-role-migration"></a>Toegang en de rol gebruikersmigratie
Beheer van Azure portal toegang uitgebreidere en krachtiger zijn dan de toegangsbeheer in de OMS-Portal, maar het aantal conversies is vereist. Zie [werkruimten beheren](log-analytics-manage-access.md#manage-accounts-and-users) voor meer informatie over toegangsbeheer in Log Analytics.

Vanaf 30 juli automatische conversie van de machtigingen voor toegangsbeheer van de OMS wordt-portal voor Azure-portal machtigingen gestart. Zodra de conversie is voltooid, wordt de beheersectie van de OMS-Portal-gebruiker gebruikers gerouteerd naar de Access control (IAM) in Azure. 

Tijdens de conversie moet het systeem elke gebruiker of beveiligingsgroep die machtigingen in de OMS-portal heeft te controleren en bepalen of het hetzelfde niveau of machtigingen in Azure heeft. Als machtigingen ontbreken, deze wordt de volgende rollen toewijzen voor de relevante werkruimten en -oplossingen.

| OMS-portal machtiging | Azure-rol |
|:---|:---|
| ReadOnly | Lezer van Log Analytics |
| Inzender | Inzender van Log Analytics |
| Beheerder | Eigenaar |

Om ervoor te zorgen dat geen overmatige machtigingen zijn toegewezen aan gebruikers, worden deze machtigingen op het niveau van de resource niet op de automatisch toegewezen door het systeem. Werkruimte-beheerders toewijzen als gevolg hiervan moeten handmatig zelf _eigenaar_ of _Inzender_ rollen op resource groep of abonnement op het niveau van de volgende acties uit te voeren.

- Oplossingen toevoegen of verwijderen
- Nieuwe aangepaste weergaven definiëren
- Waarschuwingen beheren 

In sommige gevallen kan de automatische conversie machtiging kan niet worden toegepast en de beheerder om machtigingen te handmatig wordt gevraagd.

## <a name="oms-mobile-app"></a>Mobiele OMS-App
De mobiele OMS-app worden sunsetted samen met de OMS-portal. In plaats van de mobiele OMS-app voor toegang tot informatie over uw IT-infrastructuur, dashboards en opgeslagen query's, kunt u de Azure-portal rechtstreeks openen vanuit uw browser in uw mobiele apparaat. Als u waarschuwingen, moet u configureren [Azure-actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md) voor het ontvangen van meldingen in de vorm van SMS- of een telefoongesprek

## <a name="application-insights-connector-and-solution"></a>Application Insights-Connector en oplossing
[Application Insights-Connector](log-analytics-app-insights-connector.md) biedt een manier om Application Insights-gegevens in een Log Analytics-werkruimte. De duplicatie van deze gegevens is vereist voor het inschakelen van zichtbaarheid in de infrastructuur-en toepassingsgegevens.

Met de ondersteuning van [query's voor meerdere bronnen](log-analytics-cross-workspace-search.md), er is niet langer in deze behoefte te dupliceren van gegevens. De bestaande Application Insights-oplossing wordt als zodanig worden afgeschaft. Vanaf juli kunt pas u weer nieuwe Application Insights-resources koppelen aan Log Analytics-werkruimten. Bestaande koppelingen en dashboards blijft functioneren tot November 2018.


## <a name="azure-network-security-group-analytics"></a>Analyse van Azure-netwerkbeveiligingsgroep
De [oplossing Azure Network Security Group Analytics](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) wordt vervangen met onlangs geopende [Traffic Analytics](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) waarmee u inzicht in gebruikers en toepassingen op cloudnetwerken. Traffic Analytics kunt u controle-activiteit op het netwerk, beveiligde toepassingen en gegevens van uw organisatie, werkbelasting prestaties te optimaliseren en blijven voldoen. 

Deze oplossing analyseert Stroomlogboeken logboeken en biedt inzicht in de volgende.

- Verkeer stroomt tussen uw netwerken tussen Azure en Internet, openbare cloud-regio's, VNETs en subnetten.
- Toepassingen en protocollen in uw netwerk, zonder de noodzaak voor sniffers of toegewezen stroom verzameling apparaten.
- Belangrijke gebruikers, intensieve toepassingen, VM-gesprekken in de cloud, verkeer hotspots.
- Bronnen en bestemmingen van verkeer in vnet's, tussen relaties tussen kritische zakelijke services en toepassingen.
- Beveiliging met inbegrip van schadelijk verkeer, poorten openen op het Internet, toepassingen of virtuele machines die probeert toegang tot Internet.
- Gebruik van capaciteit, waarmee u problemen met inrichten of ondermaats gebruik elimineren.

U kunt blijven zijn afhankelijk van instellingen voor diagnostische gegevens voor het verzenden van NSG-logboeken naar Log Analytics, zodat uw bestaande zoekopdrachten, waarschuwingen, dashboards opgeslagen blijven werken. Klanten die de oplossing al hebt geïnstalleerd kunnen echter ook doorgaan met het tot nadere kennisgeving. Vanaf 15 augustus, de Network Security Group Analytics-oplossing wordt verwijderd uit de marketplace en beschikbaar gesteld via de community als een [Azure QuickStart-sjabloon](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Als u hebt [uw Operations Manager-beheergroep verbonden met Log Analytics](log-analytics-om-agents.md), en vervolgens deze blijven werken zonder te wijzigen. Voor nieuwe verbindingen echter, u moet volgen de richtlijnen in [Microsoft System Center Operations Manager Management Pack voor het configureren van Operations Management Suite](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/).

## <a name="next-steps"></a>Volgende stappen
- Zie [Veelgestelde vragen voor overgang van OMS-portal naar Azure-portal voor gebruikers van de Log Analytics](log-analytics-oms-portal-faq.md) voor hulp bij het verplaatsen van de OMS-portal naar de Azure-portal.