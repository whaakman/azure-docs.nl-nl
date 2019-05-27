---
title: Buiten gebruik stellen van Security Center-functies (juli 2019) | Microsoft Docs
description: In dit artikel beschrijft de functies in Security Center die wordt beëindigd op 31 juli 2019.
services: security-center
author: yoavfrancis
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.date: 4/16/2019
ms.author: yoafr
ms.openlocfilehash: 069345f9c2d0fff0b580365153d8be13bb4ba204
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65952132"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Buiten gebruik stellen van Security Center-functies (2019 juli)

We hebben aangebracht verschillende [verbeteringen](https://azure.microsoft.com/updates/?product=security-center) naar Azure Security Center in de afgelopen zes maanden.
Met deze verbeterde mogelijkheden bent we enkele redundante functies en gerelateerde API's vanuit Security Center op 31 juli 2019 verwijderd.  

De meeste van deze retiring functies kan worden vervangen door nieuwe functionaliteit in Azure Security Center of Azure Log Analytics. Andere functies kunnen worden geïmplementeerd met behulp van [Azure Sentinel (preview)](https://azure.microsoft.com/services/azure-sentinel/).

Security Center-functies voor het buiten gebruik gesteld zijn onder andere:

- [Gebeurtenissendashboard](#menu_events)
- [Menu-zoekitem](#menu_search)
- [Klassieke Identity & Access weergavekoppeling op identiteits- en toegangsbeheer (preview)](#menu_classicidentity)
- [Beveiligingsgebeurtenissen toewijzen knop op beveiliging waarschuwingen kaart (preview)](#menu_securityeventsmap)
- [Aangepaste waarschuwingsregels (preview)](#menu_customalerts)
- [Knop in threat protection beveiligingswaarschuwingen onderzoeken](#menu_investigate)
- [Een subset van beveiligingsoplossingen](#menu_solutions)
- [Beveiligingsconfiguraties voor beveiligingsbeleid bewerken](#menu_securityconfigurations)
- [Dashboard beveiliging en controle (oorspronkelijk hebt gebruikt in OMS-portal) voor Log Analytics-werkruimten](#menu_securityomsdashboard)

In dit artikel bevat gedetailleerde informatie voor elke buiten gebruik gestelde functie en de stappen die u kunt ondernemen om de vervanging functies implementeren.

## Gebeurtenissendashboard<a name="menu_events"></a>

Security Center maakt gebruik van Microsoft Monitoring Agent voor het verzamelen van verschillende beveiliging gerelateerde configuraties en legt gebeurtenissen van uw machines. Deze gebeurtenissen worden opgeslagen in uw werkruimten. De [gebeurtenissendashboard](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) kunt u deze gegevens weergeven en biedt u een ingangspunt naar Log Analytics.

We zijn buiten gebruik stellen van het gebeurtenissendashboard die wordt weergegeven wanneer u een werkruimte selecteren:

![Gebeurtenissendashboard][2]

### <a name="events-dashboard---the-new-experience"></a>Gebeurtenissendashboard - de nieuwe ervaring

We aangemoedigd om u aan de systeemeigen mogelijkheden van Azure Log Analytics gebruiken om gebeurtenissen die aandacht vereisen in uw werkruimten weer te geven.

Als u aangepaste gebeurtenissen die aandacht vereisen in Security Center hebt gemaakt, wordt ze toegankelijk zijn. In Log Analytics, gaat u naar **werkruimte selecteren** > **opgeslagen zoekopdrachten**. Uw gegevens wordt niet verloren gaan of gewijzigd. Systeemeigen gebeurtenissen die aandacht vereisen, zijn ook toegankelijk in hetzelfde scherm in Log Analytics.

![Werkruimte opgeslagen zoekopdrachten][3]

## Menu-zoekitem<a name="menu_search"></a>

Azure Security Center gebruikt momenteel Azure Monitor zoeken in Logboeken om te halen en je beveiligingsgegevens te analyseren. Dit scherm fungeert als een venster op de pagina voor zoeken van Log Analytics, en waarmee gebruikers zoekquery's uitvoeren op de geselecteerde werkruimte. Zie voor meer informatie, [zoeken in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-search). We zijn buiten gebruik stellen van dit zoekvenster:

![Zoekpagina][4]

### <a name="search-menu-entry---the-new-experience"></a>Search menu-item - de nieuwe ervaring

We raden u aan de systeemeigen mogelijkheden van Azure Log Analytics gebruiken voor het uitvoeren van zoekquery's op uw werkruimten. Ga naar Azure Log Analytics en selecteer **logboeken**.

![Pagina voor log Analytics-Logboeken][5]

## Klassieke Identity & Access (Preview)<a name="menu_classicidentity"></a>

De klassieke Identity & Access-ervaring in Security Center ziet u momenteel een dashboard identiteit en toegang tot gegevens in Log Analytics. Dit dashboard weergeven:

1. Selecteer **weergeven klassieke Identity & Access**.

   ![Pagina id][6]

1. Weergave de **dashboard identiteit en toegang**.

    ![Pagina van de id - werkruimte kunt selecteren.][7]

1. Selecteer een werkruimte te openen de **Identity & Access** dashboard in Log Analytics weergeven identiteit en toegang tot gegevens in uw werkruimte.

   ![Pagina van de id - dashboard][8]

We zijn buiten gebruik stellen van alle drie schermen wordt weergegeven in de voorgaande stappen. Uw gegevens wordt niet blijven beschikbaar in de Log Analytics-beveiligingsoplossing en worden gewijzigd of verwijderd.

### <a name="classic-identity--access-preview---the-new-experience"></a>Klassieke Identity & Access (Preview) - de nieuwe ervaring

De Log Analytics-dashboard heeft laten zien van inzichten op één werkruimte. Echter bieden systeemeigen mogelijkheden van Security Center inzicht in alle abonnementen en alle werkruimten die zijn gekoppeld. U toegang hebt tot een eenvoudig-werken met weergave waarmee u zich richten op wat belangrijk met de aanbevelingen die worden beoordeeld op basis van hun beveiligde score is.

Alle functies van de **Identity & Access** dashboard in Log Analytics kan worden bereikt door het selecteren van **Identity & access (Preview)** in Security Center.

![Pagina met id - klassieke ervaring buiten gebruik stellen][9]

## Beveiligingsgebeurtenissen toewijzen<a name="menu_securityeventsmap"></a>

Security Center biedt u een [security waarschuwingen kaart](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) voor het identificeren van beveiligingsrisico's. De **gaat u naar beveiliging gebeurtenissen kaart** knop in die map opent u een dashboard waarmee u kunt de onbewerkte beveiligingsgebeurtenissen weergeven in de geselecteerde werkruimte.

We verwijderen bent de **gaat u naar beveiliging gebeurtenissen kaart** knop en het dashboard per werkruimte.

![Beveiligingswaarschuwingen kaart - knop][10]

Wanneer u selecteert de **gaat u naar beveiliging gebeurtenissen kaart** knop, u opent het dashboard bedreigingsinformatie. We zijn buiten gebruik stellen van het dashboard bedreigingsinformatie.  

![Het dashboard Bedreigingsinformatie][11]

Als u ervoor een werkruimte om hiervoor het dashboard bedreigingsinformatie weer te geven kiest, opent u het scherm overzicht (preview) van beveiliging waarschuwingen in Log Analytics. We zijn buiten gebruik stellen van dit scherm.

![Kaart van de beveiliging waarschuwingen in Log Analytics][12]

Uw bestaande gegevens wordt niet blijven beschikbaar in de Log Analytics-beveiligingsoplossing en worden gewijzigd of verwijderd.

### <a name="security-events-map---the-new-experience"></a>Beveiliging-kaart voor gebeurtenissen - de nieuwe ervaring

We raden u aan het gebruik van de waarschuwingen kaart functionaliteit die is ingebouwd in Security Center: **Beveiliging waarschuwingen kaart (Preview)**. Deze functionaliteit biedt een optimale ervaring en werkt met alle abonnementen en bijbehorende werkruimten. Het biedt u een weergave op hoog niveau in uw omgeving en is niet gericht op één werkruimte.

## Aangepaste waarschuwingsregels (Preview)<a name="menu_customalerts"></a>

We zijn [ervaring buiten gebruik stellen van de aangepaste waarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) op 30 juni 2019 omdat de onderliggende infrastructuur wordt buiten gebruik gesteld. Tot die tijd kun u de bestaande regels voor aangepaste waarschuwingen kunt bewerken, maar u niet kunnen nieuwe toevoegen. Het is raadzaam dat u inschakelt [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) automatisch migreren van uw bestaande waarschuwingen en nieuwe te maken. U kunt ook uw waarschuwingen maken met Azure Monitor log-waarschuwingen.

Houd uw bestaande waarschuwingen en migreren naar Azure Sentinel:

1. Open Azure Sentinel en selecteer de werkruimte waar uw aangepaste waarschuwingen worden opgeslagen.
1. Selecteer **Analytics** in het menu voor het migreren van uw waarschuwingen automatisch.

![Aangepaste waarschuwingen][13]

Als u niet betrokken bent bij het overstappen naar Azure Sentinel, raden we u aan uw waarschuwingen te maken met Azure Monitor log-waarschuwingen. Zie voor instructies [maken, weergeven en beheren van waarschuwingen via Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) en [waarschuwingen voor activiteitenlogboeken in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

Zie voor meer informatie over aangepaste waarschuwingen buiten gebruik stellen [regels voor aangepaste waarschuwingen in Azure Security Center (Preview)](https://docs.microsoft.com/azure/security-center/security-center-custom-alert).

## Beveiligingsonderzoek waarschuwingen<a name="menu_investigate"></a>

[De onderzoeksfunctie](https://docs.microsoft.com/azure/security-center/security-center-investigation) in Security Center helpt u een potentiële beveiligingsincident sorteren. De functie kunt u inzicht in het bereik van een incident en de onderliggende oorzaak opsporen. We deze functie van Security Center wilt verwijderen omdat deze wordt vervangen door een verbeterde ervaring in [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/).

![Beveiligingsincident][14]

Wanneer u selecteert de **onderzoeken** knop van een **beveiligingsincident** scherm, u opent de Onderzoeksdashboard (Preview) in Log Analytics. We zijn buiten gebruik stellen van het Dashboard.  

Uw bestaande gegevens wordt niet blijven beschikbaar in de Log Analytics-beveiligingsoplossing en worden gewijzigd of verwijderd.

![Het dashboard verkennen in Log Analytics][15]

### <a name="investigation---the-new-experience"></a>Onderzoek - de nieuwe ervaring

We raden u aan de overgang naar [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) voor een ervaring voor het uitgebreide onderzoeken. Azure Sentinel biedt krachtige zoek- en query's om te zoeken op beveiligingsrisico's in uw organisatie gegevensbronnen.  

## Subset van beveiligingsoplossingen<a name="menu_solutions"></a>

Security Center kunt inschakelen [geïntegreerde beveiligingsoplossingen in Azure](https://docs.microsoft.com/azure/security-center/security-center-partner-integration). We zijn buiten gebruik stellen van de volgende partneroplossingen uit Security Center. Deze oplossingen zijn ingeschakeld in [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) samen met een aantal extra gegevensbronnen.

- [Volgende generatie firewall- en web application firewall-oplossingen](https://docs.microsoft.com/azure/sentinel/connect-data-sources)
- [Integratie van beveiligingsoplossingen die ondersteuning bieden voor de Common Event Format (CEF)](https://docs.microsoft.com/azure/sentinel/connect-common-event-format)
- [Microsoft Advanced Threat Analytics](https://docs.microsoft.com/azure/sentinel/connect-azure-atp)
- [Azure AD Identity Protection](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection)

Na het buiten gebruik stellen, kunt u zich niet toevoegen of wijzigen van typen oplossingen die worden vermeld in de voorgaande lijst via de gebruikersinterface of de API.

Als u bestaande verbonden oplossingen hebt, raden we u om te verplaatsen naar Azure Sentinel.

![Beveiliging centers oplossingen][16]

## Beveiligingsconfiguraties voor beveiligingsbeleid bewerken<a name="menu_securityconfigurations"></a>

Azure Security Center bewaakt beveiligingsconfiguraties door het toepassen van een set [meer dan 150 aanbevolen regels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). voor het beperken van het besturingssysteem. Deze regels hebben betrekking op firewalls, controle, wachtwoordbeleid en meer. Als er wordt geconstateerd dat een computer een kwetsbare configuratie heeft, wordt er door Security Center een beveiligingsaanbeveling gegenereerd. De [bewerkingsscherm security configuration](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) kunnen klanten om aan te passen de standaard OS-beveiligingsconfiguratie in Security Center.

We zijn buiten gebruik stellen van deze preview-functie. Als u na de vervaldatum, wilt u uw beveiligingsconfiguraties teruggezet op hun standaardwaarden, kunt u doen via de API of Powershell gebruiken de [instructies te volgen](https://aka.ms/ascresetsecurityconfigurations)

![Beveiligingsconfiguraties bewerken][17]

### <a name="edit-security-configurations---the-new-experience"></a>Beveiligingsconfiguraties - de nieuwe ervaring bewerken

Wij van plan bent om in te schakelen van Security Center ter ondersteuning van de [configuratie gastagent](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration). Deze update kunt een veel uitgebreider functieset, inclusief ondersteuning voor meer besturingssystemen en integratie van Azure in de Gast-beleid voor de Gast-configuraties. Nadat deze wijzigingen zijn ingeschakeld, hebt u ook de mogelijkheid om te bepalen de configuraties op schaal en automatisch toegepast op nieuwe resources.

## Dashboard beveiliging en controle voor Log Analytics-werkruimten<a name="menu_securityomsdashboard"></a>

Het dashboard beveiliging en controle werd oorspronkelijk gebruikt in de OMS-portal. In Log Analytics biedt het dashboard een overzicht van de per-werkruimte van belangrijke beveiligingsgebeurtenissen en bedreigingen, een bedreigingsinformatiekaart en een evaluatie van de identiteit en toegang van beveiligingsgebeurtenissen die zijn opgeslagen in de werkruimte. We wilt het dashboard verwijderen. Zoals we al in de gebruikersinterface van het dashboard aanbevolen, we adviseren dat u de overgang naar Azure Security Center.

![Log Analytics-security-dashboard][18]

### <a name="security-and-audit-dashboard---the-new-experience"></a>Dashboard beveiliging en controle - de nieuwe ervaring

We adviseren u overschakelen naar Azure Security Center. Het overzicht van de dezelfde beveiliging biedt voor meerdere abonnementen en de werkruimten die zijn gekoppeld aan deze, plus een uitgebreidere functie ingesteld.

Krijgt u de oorspronkelijke Log Analytics-query's die in het dashboard beveiliging en controle in de [GitHub-opslagplaats](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards) voor Security Center.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/).
- Meer informatie over [Azure Sentinel](https://docs.microsoft.com/azure/sentinel).

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
