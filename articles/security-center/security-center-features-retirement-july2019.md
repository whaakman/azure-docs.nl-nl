---
title: Buiten gebruik stellen van Security Center-functies (juli 2019) | Microsoft Docs
description: Dit artikel wordt uitgelegd functies in Security Center die wordt beëindigd op 31 juli 2019.
services: security-center
author: yoavfrancis
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.date: 4/16/2019
ms.author: yoafr
ms.openlocfilehash: 614dabe842c7fe99da3ddb486c27003c79382ea1
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231390"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Buiten gebruik stellen van Security Center-functies (2019 juli)

We hebben aangebracht verschillende [verbeteringen](https://azure.microsoft.com/updates/?product=security-center) naar de Azure Security Center in de afgelopen 6 maanden.  
Met de verbeterde mogelijkheden worden een aantal redundante functies, evenals de bijbehorende API's, verwijderd uit het Security Center op 31 juli 2019.  

De meeste van de buiten gebruik gestelde functies kan worden vervangen door nieuwe functies in Azure Security Center of Log Analytics; en een aantal functies kunnen worden geïmplementeerd met behulp van [Azure Sentinel (preview)](https://azure.microsoft.com/services/azure-sentinel/), onlangs aangekondigd.

De lijst met functies die buiten gebruik gesteld van het Security Center bevat:

- [Gebeurtenissendashboard](#menu_events)
- [Menu-zoekitem](#menu_search)
- [Klassieke Identity & Access weergavekoppeling op identiteits- en toegangsbeheer (preview)](#menu_classicidentity)
- [Beveiligingsgebeurtenissen toewijzen knop op beveiliging waarschuwingen kaart (preview)](#menu_securityeventsmap)
- [Aangepaste waarschuwingsregels (preview)](#menu_customalerts)
- [Knop in threat protection beveiligingswaarschuwingen onderzoeken](#menu_investigate)
- [Een subset van beveiligingsoplossingen](#menu_solutions)
- [Beveiligingsconfiguraties voor beveiligingsbeleid bewerken](#menu_securityconfigurations)
- [Beveiliging en audit-dashboard (oorspronkelijk hebt gebruikt in OMS-portal) voor Log Analytics-werkruimten.](#menu_securityomsdashboard)

De hieronder vindt u gedetailleerde informatie voor elke buiten gebruik gestelde functie en de stappen die u kunt uitvoeren met behulp van de functies van de vervanging.

## Gebeurtenissendashboard<a name="menu_events"></a>
Security Center maakt gebruik van de Microsoft Monitoring Agent voor het verzamelen van verschillende aan beveiliging gerelateerde configuraties en gebeurtenissen van uw machines en slaat deze gebeurtenissen in uw werkruimte(n). De [gebeurtenissendashboard](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) kunnen deze gegevens weergeven en vindt u in feite een ander toegangspunt met Log Analytics.

Het gebeurtenissendashboard voortaan wordt beëindigd:

![Selectiescherm van gebeurtenissen werkruimte][1]

Het gebeurtenissendashboard die wordt weergegeven wanneer een gebruiker in een werkruimte wordt ook stopgezet:

![Gebeurtenissendashboard][2]

### <a name="events-dashboard---new-experience"></a>Gebeurtenissendashboard - nieuwe ervaring

Klanten worden aangeraden systeemeigen mogelijkheden van Log Analytics gebruiken om te bekijken van gebeurtenissen die aandacht vereisen in hun werkruimte.
Als u aangepaste gebeurtenissen die aandacht vereisen al hebt gemaakt vanuit Security Center, dit zijn toegankelijk via Log analytics -> Selecteer werkruimte -> opgeslagen zoekopdrachten. Uw gegevens wordt niet verloren gaan of gewijzigd. Systeemeigen gebeurtenissen die aandacht vereisen, zijn ook toegankelijk in hetzelfde scherm.

![Werkruimte opgeslagen zoekopdrachten][3]

## Menu-zoekitem<a name="menu_search"></a>
Azure Security Center gebruikt momenteel Azure Monitor zoeken in Logboeken om te halen en je beveiligingsgegevens te analyseren. Dit scherm is in feite fungeert als een gevel naar Log Analytics "[zoeken](https://docs.microsoft.com/azure/security-center/security-center-search)" pagina – zodat gebruikers een voor de zoekquery's uitvoeren op de geselecteerde werkruimte. Dit zoekvenster voortaan wordt beëindigd:

![Zoekpagina][4]

### <a name="search-menu-entry---new-experience"></a>Menu-item - nieuwe ervaring zoeken

Klanten worden aangeraden om te gebruiken **Log Analytics** systeemeigen mogelijkheden voor het uitvoeren van zoekopdrachten op hun werkruimten. Om dit te doen, kunnen ze gaat u naar Log analytics in Azure en selecteer: 'Logs':

![Pagina voor log Analytics-Logboeken][5]

## Klassieke Identity & Access (Preview)<a name="menu_classicidentity"></a>
De ervaring voor 'Klassieke' identiteit en toegang in Security Center heeft een manier voor klanten die u wilt weergeven van hun identiteit en toegang tot gerelateerde gegevens in log analytics worden opgegeven. Dit is gedaan door de onderstaande klikt op te volgen:

Klik op "Klassieke weergave-Identity & Access"

![Pagina id][6]

Samen met het scherm op deze pagina openen wilt, 'dashboard identiteit en toegang':

![Pagina van de id - werkruimte kunt selecteren.][7]

Klik in de werkruimte 'Identiteit en toegang' log analytics wordt het dashboard geopend waar klanten kunnen bekijken van de identiteit en toegang tot gegevens in hun werkruimte:

![Pagina van de id - dashboard][8]

Alle bovenstaande drie schermen voortaan wordt beëindigd. Uw gegevens worden blijven beschikbaar in de log analytics-beveiligingsoplossing en niet gewijzigd of verwijderd.
De oorspronkelijke Log Analytics-query's gebruikt voor het vullen van het dashboard identiteit en toegang zijn beschikbaar in Security Center [GitHub-opslagplaats](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards).

### <a name="classic-identity--access-preview---new-experience"></a>Klassieke Identity & Access (Preview) - nieuwe ervaring
Terwijl de Log analytics-dashboard insights op een opgegeven werkruimte alleen opgegeven heeft, de systeemeigen mogelijkheden van Security Center bieden inzicht in alle abonnementen en alle werkruimten die zijn gekoppeld aan deze in een eenvoudig-werken met weergave waarmee u zich richten op wat van belangrijk, op basis van de beveiligde score van uw Identity & access aanbeveling(en) zijn.
Alle functies van de identiteit en toegang tot Log analytics-dashboard kunnen worden bereikt door het selecteren van "Identity & access (Preview)" in Security Center:

![Pagina met id - klassieke ervaring buiten gebruik stellen][9]

## Beveiligingsgebeurtenissen toewijzen<a name="menu_securityeventsmap"></a>
Security Center biedt u een [kaart](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) die helpt u beveiligingsrisico's op basis van de omgeving identificeren. De knop 'Ga naar security gebeurtenissen kaart' in die map leidt tot een dashboard waarmee onbewerkte beveiligingsgebeurtenissen bekijken op de geselecteerde werkruimte.
De knop, samen met het dashboard per werkruimte wordt verwijderd na de afschaffing.

![Beveiligingswaarschuwingen kaart - knop][10]

Vandaag wordt het dashboard Bedreigingsinformatie geopend wanneer u op 'Ga naar security gebeurtenissen kaart klikt'. Het dashboard bedreigingsinformatie wordt beëindigd.  

![Het dashboard Bedreigingsinformatie][11]

Als u een werkruimte om hiervoor het dashboard bedreigingsinformatie weer te geven, de beveiligingswaarschuwingen map(Preview) scherm *in Log Analytics* wordt geopend. Dit scherm wordt beëindigd.

![Kaart van de beveiliging waarschuwingen in Log Analytics][12]

Uw bestaande gegevens zal blijven beschikbaar in de log analytics-beveiligingsoplossing en niet worden gewijzigd of verwijderd.
De oorspronkelijke Log Analytics-query's gebruikt voor het vullen van het dashboard Bedreigingsinformatie zijn beschikbaar in Security Center [GitHub-opslagplaats](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards).

### <a name="security-events-map---new-experience"></a>Beveiliging-kaart voor gebeurtenissen - nieuwe ervaring
We raden onze klanten gebruiken de waarschuwingsfunctie van de kaart die is ingebouwd in Security Center - "beveiligingswaarschuwingen toewijzen (Preview)". Dit biedt een optimale ervaring en werkt met alle abonnementen en bijbehorende werkruimten, zodat een macro weergeven in uw omgeving, en niet gericht op één werkruimte.

## Aangepaste waarschuwingsregels (Preview)<a name="menu_customalerts"></a>
De ervaring voor aangepaste waarschuwingen worden [buiten gebruik gesteld](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) en met 30 juni 2019, vanwege buiten gebruik stellen van de onderliggende infrastructuur is gebouwd op. In de periode totdat afschaffing gebruikers kunnen bestaande regels voor aangepaste waarschuwingen bewerken, maar is niet mogelijk om toe te voegen nieuwe labels. Gebruikers wordt aangeraden om in te schakelen [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) waarschuwingen voor activiteitenlogboeken met één muisklik onboarding om automatisch te migreren van hun bestaande waarschuwingen en nieuwe te maken, of hun waarschuwingen met Azure Monitor ook opnieuw maken.

Start Azure Sentinel wilt houden van uw bestaande waarschuwingen en migreren naar Azure Sentinel. Selecteer de werkruimte waar uw aangepaste waarschuwingen worden opgeslagen als eerste stap en selecteer vervolgens het menu-item 'Analytics' voor het migreren van uw waarschuwingen automatisch.

![Aangepaste waarschuwingen][13]

Klanten die geen interesse in onboarding naar Azure Sentinel worden uitgevers aangemoedigd opnieuw hun om waarschuwingen te maken met Azure Monitor log-waarschuwingen. Zie voor instructies: [maken, weergeven en beheren van waarschuwingen met behulp van Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log). Raadpleeg voor instructies over het maken van waarschuwingen voor logboeken: [Waarschuwingen voor activiteitenlogboeken in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

Zie voor meer informatie over aangepaste waarschuwingen buiten gebruik stellen, de [Security Center aangepaste waarschuwingen documentatie](https://docs.microsoft.com/azure/security-center/security-center-custom-alert).

## Beveiligingsonderzoek waarschuwingen<a name="menu_investigate"></a>
[De onderzoeksfunctie](https://docs.microsoft.com/azure/security-center/security-center-investigation) in Security Center kunt u sorteren, inzicht in het bereik en bijhouden van de hoofdoorzaak van een mogelijke beveiligingsincident. Deze functie wordt verwijderd uit Security Center als deze is vervangen door een verbeterde ervaring in [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/).

![Beveiligingsincident][14]

Wanneer u op de knop 'Onderzoeken' hierboven klikt, wordt de 'Onderzoeksdashboard (Preview)"in Log Analytics wordt geopend. Het Dashboard wordt beëindigd.  
Uw bestaande gegevens zal blijven beschikbaar in de Log Analytics-beveiligingsoplossing en niet worden gewijzigd of verwijderd.

![Het dashboard verkennen in Log Analytics][15]

### <a name="investigation---new-experience"></a>Onderzoek - nieuwe ervaring

We raden onze klanten te onboarden [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) voor een uitgebreide onderzoeksmethode, mogelijk gemaakt door de functie jacht waarschuwingen. Azure Sentinel biedt krachtige jacht Zoek- en query's om te zoeken op beveiligingsrisico's in uw organisatie gegevensbronnen.  

## Subset van beveiligingsoplossingen<a name="menu_solutions"></a>

Security Center biedt de mogelijkheid om in te schakelen [geïntegreerde beveiligingsoplossingen in Azure](https://docs.microsoft.com/azure/security-center/security-center-partner-integration). De volgende partneroplossingen zou worden verwijderd en worden ondersteund in [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/), samen met nog meer gegevensbronnen.

- De volgende generatie firewall- en Web application Firewall-oplossingen (Azure Sentinel [documentatie](https://docs.microsoft.com/azure/sentinel/connect-data-sources))
- Integratie van beveiligingsoplossingen die ondersteuning bieden voor de Common event format (CEF) (Azure Sentinel [documentatie](https://docs.microsoft.com/azure/sentinel/connect-common-event-format))
- Microsoft Advanced Threat Analytics (Azure Sentinel [documentatie](https://docs.microsoft.com/azure/sentinel/connect-azure-atp))
- Azure AD Identity Protection (Azure Sentinel [documentatie](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection))

Na de afschaffing pas gebruikers weer nieuwe toevoegen of wijzigen van bestaande verbonden oplossingen van de typen die hierboven vermeld, vanuit de gebruikersinterface en de API.
De bestaande verbonden oplossingen worden aangemoedigd om te verplaatsen naar Azure Sentinel aan het einde van deze periode.

![Beveiliging centers oplossingen][16]

## Beveiligingsconfiguraties voor beveiligingsbeleid bewerken<a name="menu_securityconfigurations"></a>
Azure Security Center bewaakt beveiligingsconfiguraties door het toepassen van een set [meer dan 150 aanbevolen regels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor het beperken van het besturingssysteem, met inbegrip van regels die betrekking hebben op firewalls, controle, wachtwoordbeleid en meer. Als een virtuele machine hebben een kwetsbare configuratie wordt gevonden, wordt door Security Center een beveiligingsaanbeveling gegenereerd. De [bewerkingsscherm security configuration](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) kunnen klanten om aan te passen de standaard OS-beveiligingsconfiguratie in Security Center.

Deze functie is in preview en wordt beëindigd.

![Beveiligingsconfiguraties bewerken][17]

### <a name="edit-security-configurations---new-experience"></a>Beveiligingsconfiguraties - nieuwe ervaring bewerken

Security Center biedt ondersteuning voor de [configuratie gastagent](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) in de nabije toekomst, zodat een veel uitgebreider functieset - ondersteuning voor aanvullende besturingssystemen en integratie met gast-configuratie met de Azure-beleid (inclusief in de Gast-beleid genoemd). Dit biedt ook de mogelijkheid om te bepalen op schaal en het automatisch toepassen op nieuwe resources.

## Beveiliging en audit dashboard (oorspronkelijk hebt gebruikt in OMS-portal) voor Log Analytics-werkruimten<a name="menu_securityomsdashboard"></a>

De Security-dashboard in Log analytics biedt een overzicht van de per-werkruimte van belangrijke beveiligingsgebeurtenissen en bedreigingen, een bedreigingsinformatiekaart en evaluatie van de identiteit en toegang van beveiligingsgebeurtenissen die zijn opgeslagen in de werkruimte. Het dashboard wordt voortaan worden verwijderd. Zoals we al in de gebruikersinterface van het dashboard aanbevolen, wordt onze gebruikers aangeraden gebruik van Azure Security Center gaan.

![Log analytics-security-dashboard][18]

### <a name="security--audit-dashboard---new-experience"></a>Dashboard beveiliging en audit - nieuwe ervaring
Onze klanten wordt aangeraden gebruik van Azure Security Center, waarmee u de dezelfde beveiligingsoverzicht voor meerdere abonnementen en werkruimten die zijn gekoppeld, samen met een uitgebreidere reeks functies.

De oorspronkelijke Log Analytics-query's gebruikt voor het vullen van de beveiliging en controle uitvoeren op dashboard beschikbaar zijn in Security Center [GitHub-opslagplaats](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/)
- Meer informatie over [Azure Sentinel](https://docs.microsoft.com/azure/sentinel)

<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png
