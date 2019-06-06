---
title: Wat is nieuw? Releaseopmerkingen - Azure Active Directory | Microsoft Docs
description: Ontdek wat er nieuw in Azure Active Directory, zoals de meest recente release-opmerkingen worden bekende problemen, oplossingen voor problemen, afgeschafte functies en toekomstige wijzigingen.
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6a66aefec69a0551f85b11a380c90d1915bd776
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66474201"
---
# <a name="whats-new-in-azure-active-directory"></a>Wat is er nieuw in Azure Active Directory?

>Blijf op de hoogte over wanneer u terug naar deze pagina voor updates kopiëren en plakken van deze URL: `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` in uw ![RSS-feed lezer pictogram](./media/whats-new/feed-icon-16x16.png) reader-feed.

Azure AD ontvangt verbeteringen regelmatig. Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

- De meest recente versies
- Bekende problemen
- Opgeloste fouten
- Afgeschafte functies
- Plannen voor wijzigingen

Deze pagina wordt maandelijks bijgewerkt, dus regelmatig bezoekt. Als u naar items die ouder dan zes maanden zoekt zijn, kunt u vinden in de [archief voor de wat is er nieuw in Azure Active Directory](whats-new-archive.md).

---

## <a name="may-2019"></a>Mei 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Service wijzigen: Toekomstige ondersteuning voor alleen de protocollen TLS 1.2 op de service voor toepassingsproxy

**Type:** Plan voor wijziging  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Toegangsbeheer

U kunt de best mogelijke codering voor onze klanten, zijn we mailtoegang beperken tot alleen de protocollen TLS 1.2 op de Application Proxy-service. Deze wijziging wordt geleidelijk worden geïmplementeerd voor klanten die al alleen TLS 1.2-protocollen, zodat u al dan niet mogen zien welke wijzigingen zijn.

Afschaffing van TLS 1.0 en 1.1 TLS zijn uitgevoerd op 31 augustus 2019, maar bieden we extra van tevoren, zodat u tijd om voor te bereiden voor deze wijziging hebt. Om voor te bereiden voor deze wijziging Zorg ervoor dat uw combinaties van client / server- en browser-server, met inbegrip van alle clients die uw gebruikers gebruiken voor toegang tot apps die zijn gepubliceerd via toepassingsproxy, worden bijgewerkt voor het gebruik van het TLS 1.2-protocol voor het onderhouden van de verbinding met de toepassing Proxy-service. Zie voor meer informatie, [toevoegen van een on-premises toepassing voor externe toegang via Application Proxy in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin).

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Het rapport gebruik en informatie gebruiken om uw gegevens met betrekking tot app aanmelden weer te geven

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** Bewaking en rapportage

U kunt nu het rapport gebruiks- en inzichten, zich in de **bedrijfstoepassingen** gebied van de Azure-portal om een toepassingsgerichte weergave van uw aanmelding gegevens, inclusief informatie over:

- Top apps voor uw organisatie gebruikt

- Apps met de meeste mislukte aanmeldingen

- Bovenste aanmelden fouten voor elke app

Zie voor meer informatie over deze functie [gebruiks- en inzichten rapport in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report)

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Automatiseer uw gebruikers inrichten voor cloud-apps met behulp van Azure AD

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** Bewaking en rapportage

Volg deze nieuwe zelfstudies voor het gebruik van de Azure AD-inrichtingsservice om automatisch te maken, verwijderen, en het bijwerken van gebruikersaccounts voor de volgende cloud-gebaseerde apps:

- [Comeet](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [DynamicSignal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [KeeperSecurity](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

U kunt ook volgen dit nieuwe [Dropbox zelfstudie](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial), waarmee u informatie over het inrichten van de groepsobjecten worden weergegeven.

Zie voor meer informatie over hoe uw organisatie via het inrichten van geautomatiseerde gebruikersaccounts beter kunt beveiligen, [automatisch gebruikers inrichten voor SaaS-toepassingen met Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>Beveiligde score identiteit is nu beschikbaar in Azure AD (algemene beschikbaarheid)

**Type:** Nieuwe functie  
**Categorie van de service:** N/A  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

U kunt nu controleren en verbeteren van uw identiteit beveiligingspostuur met behulp van de identiteit van de score functie beveiligen in Azure AD. De identiteit van de beveiligde score functie gebruikt een dashboard om u te helpen:

- Objectief meten het beveiligingspostuur van uw identiteit, op basis van een score 1 tot 223 op.

- Uw identiteit verbeteringen in de beveiliging plannen

- Het succes van uw verbeteringen in de beveiliging controleren

Zie voor meer informatie over de identiteit score beveiligingsfunctie [wat is de beveiligde score identiteit in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score).

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Nieuwe App-registraties ervaring is nu beschikbaar (algemene beschikbaarheid)

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Ervaring voor ontwikkelaars

De nieuwe [App-registraties](https://aka.ms/appregistrations) ervaring is nu algemeen beschikbaar. Deze nieuwe ervaring omvat de belangrijkste functies die u bekend met van de Azure-portal en de toepassing-registratieportal bent en verbeterd via:

- **Betere app-beheer.** In plaats van uw apps in andere portals ziet, kunt u nu al uw apps op één locatie bekijken.

- **Eenvoudiger app-registratie.** Uit de verbeterde navigatie-ervaring in de manier waarop nieuwe machtiging selectie is het nu gemakkelijker om te registreren en beheren van uw apps.

- **Meer gedetailleerde informatie.** U vindt meer informatie over uw app, inclusief snelstartgidsen en nog veel meer.

Zie voor meer informatie, [identiteitsplatform van Microsoft](https://docs.microsoft.com/azure/active-directory/develop/) en de [App-registraties ervaring is nu algemeen beschikbaar.](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) aankondiging blog.

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Nieuwe mogelijkheden beschikbaar in de riskante gebruikers-API voor Identity Protection

**Type:** Nieuwe functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

We zijn trots aan u kunt nu de riskante gebruikers-API gebruiken om te halen van de geschiedenis van gebruikers risico, negeren riskante gebruikers en om te bevestigen van gebruikers, zoals aangetast. Deze wijziging kunt u efficiënter werken de status van de risico's van uw gebruikers en inzicht in de geschiedenis van hun risico.

Zie voor meer informatie de [riskante gebruikers API-referentiedocumentatie](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - mei 2019

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie

In mei 2019, hebben we deze 21 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [echte koppelingen](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kianda](https://app.kianda.com/sso/OpenID/AzureAD/), [eenvoudige aanmelding](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [Braze](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [Displayr](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [Templafy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo verkoop betrekken](https://toutapp.com/login), [ACLP](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [Meta4 globale HR](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), [Quantum werkplek](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [kobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [webMethods API Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [RedFlag](https://pocketstop.com/redflag/), [Whatfix](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [Besturingselement](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [JOBHUB](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [NEOGOV](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), [Foodee](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial), [MyVR](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Ervaringen met verbeterde groepen maken en beheren in de Azure AD-portal

**Type:** Nieuwe functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

We hebben verbeteringen aangebracht in de ervaringen met betrekking tot groepen in de Azure AD-portal. Deze verbeteringen kunnen beheerders om lijsten met groepen, leden lijsten, beter te beheren en om het maken van extra opties te bieden.

Verbeteringen zijn onder andere:

- Standaardfilters toepassen door het lidmaatschapstype en het groepstype.

- Toevoeging van nieuwe kolommen zoals bron- en e-mailadres.

- Meervoudige selectie groepen, leden en de eigenaar van een lijst met voor eenvoudig verwijderen.

- De mogelijkheid om te kiezen van een e-mailadres en eigenaren toevoegen tijdens het maken van beveiligingsgroepen.

Zie voor meer informatie, [een basic-groep maken en leden met Azure Active Directory toevoegen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Een naamgevingsbeleid voor Office 365-groepen configureren in Azure AD-portal (algemene beschikbaarheid)

**Type:** Gewijzigde functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

Beheerders kunnen nu een naamgevingsbeleid voor Office 365-groepen, met behulp van de Azure AD-portal configureren. Deze wijziging helpt bij het afdwingen van consistente naamconventies voor Office 365-groepen gemaakt of bewerkt door gebruikers in uw organisatie.

U kunt naamgevingsbeleid voor Office 365-groepen op twee verschillende manieren configureren:

- Voorvoegsels of achtervoegsels, die automatisch worden toegevoegd aan de naam van een groep definiëren.

- Upload een aangepaste set geblokkeerde woorden voor uw organisatie, deze zijn niet toegestaan in de namen van groepen (bijvoorbeeld "CEO, salarisadministratie, HR").

Zie voor meer informatie, [naamgeving van beleid voor Office 365-groepen afdwingen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Microsoft Graph API-eindpunten zijn nu beschikbaar voor Azure AD-activiteitenlogboeken (algemene beschikbaarheid)

**Type:** Gewijzigde functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

We kondigen de algemene beschikbaarheid van ondersteuning voor Microsoft Graph API-eindpunten voor Azure AD-activiteit de functie voor Logboeken. Met deze release kunt u nu gebruik versie 1.0 van de Azure AD controleren Logboeken, evenals de logboeken aanmelden API's.

Zie voor meer informatie, [overzicht van Azure AD audit log API](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0).

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Beheerders kunnen nu voorwaardelijke toegang gebruiken voor het gecombineerde registratieproces (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging  

Beheerders kunnen nu beleidsregels voor voorwaardelijke toegang voor gebruik door de registratiepagina van de gecombineerde maken. Dit omvat het toepassen van beleid voor het toestaan van inschrijving als:

- Gebruikers zijn op een vertrouwd netwerk.

- Gebruikers zijn een laag risico aanmelden.

- Gebruikers zijn op een beheerd apparaat.

- Gebruikers komen overeen met de voorwaarden van de organisatie van gebruik (gebruiksvoorwaarden).

Voor meer informatie over voorwaardelijke toegang en het wachtwoord opnieuw instellen, ziet u de [voorwaardelijke toegang voor de Azure AD MFA gecombineerd en wachtwoord opnieuw instellen van registratie-ervaring-blogbericht](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348). Zie voor meer informatie over beleid voor voorwaardelijke toegang voor het gecombineerde registratieproces [beleid voor voorwaardelijke toegang voor de registratie van de gecombineerde](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration). Zie voor meer informatie over de Azure AD-termen van functie gebruiken, [Azure Active Directory gebruiksrechtovereenkomst gebruiken functie](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

## <a name="april-2019"></a>April 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-in-refreshed-azure-ad-identity-protection"></a>Nieuwe Azure AD-bedreigingsdetectie intelligence is nu beschikbaar in vernieuwd Azure AD Identity Protection

**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD-identiteitsbeveiliging  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

Azure AD intelligence detectie van bedreigingen is nu beschikbaar in de vernieuwd Azure AD Identity Protection. Deze nieuwe functionaliteit helpt u om aan te geven van gebruikersactiviteit die ongebruikelijke voor een specifieke gebruiker zijn of die is consistent met bekende aanvalspatronen op basis van de interne en externe bedreigingsinformatie van Microsoft.

Zie voor meer informatie over de vernieuwde versie van Azure AD Identity Protection, de [vier belangrijke verbeteringen in de Azure AD Identity Protection worden nu in openbare preview](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) blog en de [wat is Azure Active Directory Identity Protection (vernieuwd)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) artikel. Zie voor meer informatie over Azure AD-bedreigingsdetectie intelligence, de [risicogebeurtenissen voor Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/risk-events-reference#azure-ad-threat-intelligence) artikel.

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Beheer van de rechten van een Azure AD is nu beschikbaar (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Identiteitsbeheer  
**Product-mogelijkheid:** Identiteitsbeheer

Beheer van rechten voor Azure AD, nu helpt in openbare preview, klanten bij het delegeren van beheer van toegang tot pakketten, waarmee wordt gedefinieerd hoe werknemers als zakelijke partners toegang kunnen aanvragen, die moet goedkeuren en hoe lang ze toegang hebben. Toegang tot pakketten kunnen lidmaatschap in Azure AD en Office 365-groepen, roltoewijzingen in bedrijfstoepassingen en roltoewijzingen voor SharePoint Online-sites beheren. Meer informatie over het beheer van rechten op de [overzicht van Azure AD recht management](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview). Zie voor meer informatie over de mogelijkheden van Azure AD Identity Governance-functies, zoals Privileged Identity Management, beoordelingen en gebruiksvoorwaarden, [wat is Azure AD Identity Governance?](../governance/identity-governance-overview.md).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>Een naamgevingsbeleid voor Office 365-groepen configureren in Azure AD-portal (preview-versie)

**Type:** Nieuwe functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

Beheerders kunnen nu een naamgevingsbeleid voor Office 365-groepen, met behulp van de Azure AD-portal configureren. Deze wijziging helpt bij het afdwingen van consistente naamconventies voor Office 365-groepen gemaakt of bewerkt door gebruikers in uw organisatie.

U kunt naamgevingsbeleid voor Office 365-groepen op twee verschillende manieren configureren:

- Voorvoegsels of achtervoegsels, die automatisch worden toegevoegd aan de naam van een groep definiëren.

- Upload een aangepaste set geblokkeerde woorden voor uw organisatie, deze zijn niet toegestaan in de namen van groepen (bijvoorbeeld "CEO, salarisadministratie, HR").

Zie voor meer informatie, [naamgeving van beleid voor Office 365-groepen afdwingen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure AD-activiteitenlogboeken zijn nu beschikbaar in Azure Monitor (algemene beschikbaarheid)

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

Adres van uw feedback over visualisaties met de activiteitenlogboeken van Azure AD, zodat introduceren we een nieuwe functie voor inzichten in Log Analytics. Deze functie kunt u inzichten over uw Azure AD-resources met behulp van onze interactieve sjablonen, met de naam werkmappen. Deze vooraf gemaakte werkmappen kunnen Geef details op voor apps of gebruikers, en omvatten:

- **Aanmeldingen.** Bevat informatie voor apps en gebruikers, inclusief locatie-aanmelding, het besturingssysteem in gebruik of browserclient en versie en het aantal geslaagde of mislukte aanmeldingen.

- **Verouderde verificatie en voorwaardelijke toegang.** Bevat details voor apps en gebruikers met verouderde-verificatie, waaronder multi-factor Authentication gebruik geactiveerd door beleid voor voorwaardelijke toegang apps met behulp van beleid voor voorwaardelijke toegang enzovoort.

- **Analyse van aanmelding mislukt.** Helpt u om te bepalen als uw aanmelding bij fouten vanwege een gebruikersactie, problemen met beleid of uw infrastructuur optreden.

- **Aangepaste rapporten.** U kunt nieuwe maken of bewerken van de bestaande werkmappen om u te helpen bij het aanpassen van de functie voor inzichten voor uw organisatie.

Zie voor meer informatie, [over het gebruik van Azure Monitor werkmappen voor Azure Active Directory-rapporten](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - April 2019

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie

In April 2019, hebben we deze 21 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [HRworks Single Sign-On](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [Percolate](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [MobiControl](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [ Shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [Benchling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [EduBrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [RStudio Verbinding maken met](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel verbinding](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba Cloud (Role-based SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [Certent vermogen Management](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [Sectigo Certificate Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [GreenOrbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [Workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [ SurveyMonkey Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [Indiggo](https://indiggolead.com/)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>Nieuwe toegangsbeoordelingen frequentieoptie en meerdere een cloudrol kiezen

**Type:** Nieuwe functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Identiteitsbeheer

Nieuwe updates in Azure AD-toegangsbeoordelingen kunt u naar:

- Wijzig de frequentie van de toegang beoordeelt voor **semi annually**, naast de bestaande opties van wekelijks, maandelijks, per kwartaal en per jaar.

- Selecteer meerdere Azure AD en Azure-resourcerollen bij het maken van een eenmalige toegang controleren. In dit geval alle functies zijn geconfigureerd met dezelfde instellingen en alle revisoren op hetzelfde moment op de hoogte worden gesteld.

Zie voor meer informatie over het maken van een toegangscontrole [maken van een toegangscontrole van groepen of toepassingen in Azure AD-toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD Connect e waarschuwing systemen worden overgezet, nieuw e-mailadres afzendergegevens voor bepaalde klanten verzenden

**Type:** Gewijzigde functie  
**Categorie van de service:** AD synchroniseren  
**Product-mogelijkheid:** Platform

Azure AD Connect wordt momenteel het overstappen van onze systemen van de e-waarschuwing mogelijk sommige klanten met een nieuwe e-mailadres afzender. Om dit op te lossen, moet u toevoegen `azure-noreply@microsoft.com` aan lijst voor toestaan van uw organisatie of u het niet mogelijk om te blijven ontvangen van belangrijke waarschuwingen vanuit uw Office 365, Azure of uw Sync-services.

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>UPN-achtervoegsel wijzigingen zijn nu succesvolle tussen federatieve domeinen in Azure AD Connect

**Type:** Opgelost  
**Categorie van de service:** AD synchroniseren  
**Product-mogelijkheid:** Platform

U kunt nu is wijzigen van een gebruiker UPN-achtervoegsel van een federatieve domein aan een andere federatieve domein in Azure AD Connect. Deze oplossing moet u niet meer het foutbericht FederatedDomainChangeError optreden tijdens de synchronisatiecyclus of ontvangt een melding e-mailbericht aangeeft, betekent ' kan niet aan dit object in Azure Active Directory niet bijwerken omdat het kenmerk [ FederatedUser.UserPrincipalName] is niet geldig. Werk de waarde in uw lokale adreslijstservices'.

Zie voor meer informatie, [het oplossen van fouten tijdens synchronisatie](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror).

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Verbeterde beveiliging met behulp van het appbeleid-beveiliging op basis van voorwaardelijke toegang in Azure AD (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

App-beveiliging op basis van voorwaardelijke toegang is nu beschikbaar met behulp van de **app-beveiliging vereisen** beleid. Dit nieuwe beleid helpt bij het beveiligen van uw organisatie te helpen om te voorkomen dat:

- Gebruikers toegang krijgen tot apps zonder een Microsoft Intune-licentie.

- Gebruikers wordt niet lukt om een beveiligingsbeleid voor apps van Microsoft Intune.

- Gebruikers toegang krijgen tot apps zonder een geconfigureerde Microsoft Intune app-beveiligingsbeleid.

Zie voor meer informatie, [hoe u beveiligingsbeleid voor apps vereist voor toegang tot cloud-Apps met voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Nieuwe ondersteuning voor Azure AD eenmalige aanmelding en voorwaardelijke toegang in Microsoft Edge (preview-versie)

**Type:** Nieuwe functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

We hebben onze Azure AD-ondersteuning voor Microsoft Edge, met inbegrip van nieuwe ondersteuning voor eenmalige aanmelding en voorwaardelijke toegang voor Azure AD verbeterd. Als u Microsoft Intune Managed Browser eerder hebt gebruikt, kunt u Microsoft Edge in plaats daarvan nu gebruiken.

Zie voor meer informatie over het instellen en beheren van uw apparaten en apps met behulp van voorwaardelijke toegang, [vereisen beheerde apparaten voor toegang tot cloud-Apps met voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) en [vereisen goedgekeurde client-apps voor de cloud apptoegang met voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Zie voor meer informatie over het beheren van toegang met behulp van Microsoft Edge met Microsoft Intune-beleid [internettoegang beheren met een Microsoft Intune-beleid beveiligd browser](https://docs.microsoft.com/intune/app-configuration-managed-browser).

---

## <a name="march-2019"></a>Maart 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Identiteitservaring-Framework en aangepast beleid ondersteuning in Azure Active Directory B2C is nu beschikbaar is (GA)

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C

U kunt nu maken met aangepaste beleidsregels in Azure AD B2C, met inbegrip van de volgende taken die worden ondersteund op schaal en onder onze SLA voor Azure:

- Maken en uploaden van aangepaste verificatie gebruiker reizen met behulp van aangepast beleid.

- Gebruikers reizen stapsgewijze als worden uitgewisseld tussen claims-providers wordt beschreven.

- Definieer Voorwaardelijke vertakkingen in gebruiker reizen.

- Transformeren en claims voor gebruik in realtime beslissingen en communicatie toewijzen.

- Services REST API's gebruiken in uw aangepaste verificatie gebruiker reizen. Bijvoorbeeld: met e-mailproviders, referentiematerialen en eigen autorisatie-systemen.

- Federeren met id-providers die compatibel met het protocol OpenIDConnect zijn. Bijvoorbeeld, met meerdere tenants Azure AD, providers van sociale netwerken account of tweeledige verificatie providers.

Zie voor meer informatie over het maken van aangepaste beleidsregels [opmerkingen voor ontwikkelaars voor aangepast beleid in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-developer-notes-custom) en lezen [van Alex Simon blogbericht, met inbegrip van casestudy](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-custom-policies-to-build-your-own-identity-journeys/ba-p/382791).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2019"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - maart 2019

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie

In maart 2019, hebben we deze 14 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[ISEC7 mobiele Exchange gemachtigde](https://www.isec7.com/english/), [MediusFlow](https://office365.cloudapp.mediusflow.com/), [ePlatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [Fulcrum](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [ExcelityGlobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Controle systeem op basis van een uitleg](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [Lean](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [Powerschool prestaties zaken](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [Cinode](https://cinode.com/), [Iris Intranet](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [Empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit horizon](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [TAA](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Nieuwe Zscaler en connectors in de galerie van Azure AD - maart 2019 inrichting Atlassian

**Type:** Nieuwe functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** 3e partij integratie

Automatiseer het maken, bijwerken en verwijderen van gebruikersaccounts voor de volgende apps:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Bèta](https://aka.ms/ZscalerBetaProvisioning), [Zscaler één](https://aka.ms/ZscalerOneProvisioning), [Zscaler twee](https://aka.ms/ZscalerTwoProvisioning), [Zscaler drie](https://aka.ms/ZscalerThreeProvisioning), [Zscaler ZSCloud ](https://aka.ms/ZscalerZSCloudProvisioning), [Atlassian Cloud](https://aka.ms/atlassianCloudProvisioning)

Zie voor meer informatie over hoe uw organisatie via het inrichten van geautomatiseerde gebruikersaccounts beter kunt beveiligen, [automatisch gebruikers inrichten voor SaaS-toepassingen met Azure AD](https://aka.ms/ProvisioningDocumentation).

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Terugzetten en beheren van de verwijderde Office 365-groepen in de Azure AD-portal

**Type:** Nieuwe functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

U kunt nu weergeven en beheren van de verwijderde Office 365-groepen van de Azure AD-portal. Deze wijziging helpt u om te zien welke groepen zijn beschikbaar om te herstellen, samen met zodat u permanent verwijderen van alle groepen die niet meer nodig door uw organisatie hebt.

Zie voor meer informatie, [terugzetten is verlopen of verwijderde groepen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore).

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Eenmalige aanmelding is nu beschikbaar voor Azure AD-SAML-beveiligde on-premises toepassingen via Application Proxy (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Toegangsbeheer

Nu kunt u een eenmalige aanmelding (SSO)-ervaring bieden voor on-premises, SAML-verificatie-apps, samen met externe toegang tot deze apps via Application Proxy. Zie voor meer informatie over het instellen van de SAML SSO met uw on-premises toepassingen [SAML eenmalige aanmelding voor on-premises toepassingen met Application Proxy (Preview)](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>Client-apps in een lus van de aanvraag worden onderbroken als u wilt de betrouwbaarheid en gebruikerservaring verbeteren

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie

Client-apps kunnen honderden de dezelfde aanmeldingsaanvragen onjuist geven gedurende een korte periode. Deze aanvragen of ze voltooid of niet zijn, alle dragen bij aan een slechte ervaring en verhoogde werkbelastingen voor de id-provider, latentie voor alle gebruikers vergroten en de beschikbaarheid van de IDP verminderen.

Deze update verzendt een `invalid_grant` fout: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request` op client-apps die het verlenen van dupliceeraanvragen meerdere keren gedurende een korte periode, buiten het bereik van de normale werking. Client-apps die dit probleem ondervindt, moeten een interactieve prompt, vereisen dat de gebruiker zich opnieuw aanmelden weergegeven. Zie voor meer informatie over deze wijziging en over het oplossen van uw app als deze fout wordt aangetroffen, [wat is er nieuw voor verificatie?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted).

---

### <a name="new-audit-logs-user-experience-now-available"></a>Nieuwe auditlogboeken gebruikerservaring nu beschikbaar

**Type:** Gewijzigde functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

We hebben een nieuwe Azure AD gemaakt **auditlogboeken** pagina om leesbaarheid en hoe u zoeken naar uw gegevens te verbeteren. Om te zien van de nieuwe **auditlogboeken** weergeeft, schakelt **auditlogboeken** in de **activiteit** sectie van Azure AD.

![Nieuwe Audit logs pagina, met voorbeeld-info](media/whats-new/audit-logs-page.png)

Voor meer informatie over de nieuwe **auditlogboeken** pagina, Zie [Controleactiviteitenrapporten in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs).

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Nieuwe waarschuwingen en richtlijnen om te voorkomen dat onbedoelde beheerder vergrendeling van onjuist geconfigureerde beleid voor voorwaardelijke toegang

**Type:** Gewijzigde functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

We hebben om te voorkomen dat beheerders per ongeluk zichzelf buitensluiten buiten hun eigen tenants via onjuist geconfigureerde beleid voor voorwaardelijke toegang, nieuwe waarschuwingen en bijgewerkte richtlijnen gemaakt in Azure portal. Zie voor meer informatie over de nieuwe richtlijnen [wat serviceafhankelijkheden in Azure Active Directory voor voorwaardelijke toegang zijn](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Verbeterde eindgebruikers voorwaarden van de ervaringen gebruik op mobiele apparaten

**Type:** Gewijzigde functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

We hebben onze bestaande voorwaarden van gebruik in om u te helpen verbeteren van hoe u bekijken en instemmen met de gebruiksvoorwaarden op een mobiel apparaat bijgewerkt. U kunt nu in-en uitzoomen, gaat u terug, downloaden de informatie en hyperlinks selecteren. Zie voor meer informatie over de bijgewerkte gebruiksvoorwaarden [Azure Active Directory gebruiksrechtovereenkomst gebruiken functie](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users).

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Nieuwe Azure AD-activiteitenlogboeken downloaden beschikbaar is

**Type:** Gewijzigde functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

U kunt nu grote hoeveelheden activiteitenlogboeken downloaden rechtstreeks vanuit de Azure portal. Deze update kunt u:

- Download tot 250.000 rijen.

- Ontvang een melding nadat het downloaden is voltooid.

- De bestandsnaam van uw aanpassen.

- Bepaal de uitvoerindeling, JSON of CSV.

Zie voor meer informatie over deze functie [Quick Start: Download een controlerapport met behulp van de Azure portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report)

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Belangrijke wijziging: Updates voor evaluatie van voorwaarden door Exchange ActiveSync (EAS)

**Type:** Plan voor wijziging  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Toegangsbeheer

We hoe de volgende voorwaarden worden geëvalueerd door Exchange ActiveSync (EAS) bijgewerkt:

- De locatie van de gebruiker, op basis van land, regio of IP-adres

- Aanmeldingsrisico

- Apparaatplatform

Als u deze voorwaarden in uw beleid voor voorwaardelijke toegang eerder hebt gebruikt, Let erop dat de voorwaarde-gedrag kan worden gewijzigd. Bijvoorbeeld als u eerder de voorwaarde van de gebruiker-locatie in een beleid hebt gebruikt, vindt u mogelijk het beleid nu wordt overgeslagen op basis van de locatie van de gebruiker.

---

## <a name="february-2019"></a>Februari 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Configureerbare Azure AD-SAML-tokenversleuteling (openbare preview) 

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO

Nu kunt u elke ondersteunde SAML-app voor het ontvangen van versleutelde SAML-tokens. Wanneer geconfigureerd en gebruikt in combinatie met een app, worden de verzonden SAML-asserties ondertekend met behulp van een openbare sleutel die is verkregen van een certificaat dat is opgeslagen in Azure AD versleuteld in Azure AD.

Zie voor meer informatie over het configureren van de SAML-tokenversleuteling [configureren van Azure AD-SAML-tokenversleuteling](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Een toegangsbeoordeling groepen of apps met behulp van Azure AD-Toegangsbeoordelingen maken

**Type:** Nieuwe functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Beheer

U kunt nu meerdere groepen opnemen of apps in één Azure AD toegang tot revisie voor lidmaatschap van groep of app-toewijzing. Toegangsbeoordelingen met meerdere groepen of apps worden ingesteld met behulp van dezelfde instellingen en alle opgenomen revisoren krijgt een melding op hetzelfde moment.

Voor meer informatie over het maken van een toegangscontrole met behulp van Azure AD-Toegangsbeoordelingen kunt u vinden [een toegangsbeoordeling van groepen of toepassingen maken in Azure AD-Toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - februari 2019

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In februari 2019, hebben we deze 27 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT VINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [ IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), Machtiging klikken, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [seismische ](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [Delen een droom](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [webMethods integratie Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [kennis overal LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [Organisatie-eenheid Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope gegevens](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [Netop Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [PureCloud door Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [ClickUp productiviteit Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="enhanced-combined-mfasspr-registration"></a>Verbeterde gecombineerde MFA/SSPR-registratie

**Type:** Gewijzigde functie  
**Categorie van de service:** Selfservice voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie

In reactie op feedback van klanten, hebben we de gecombineerde MFA/SSPR-registratie preview-ervaring, uitgebreid uw gebruikers helpen bij het sneller hun beveiligingsgegevens registreren voor MFA en SSPR. 

**Om in te schakelen op de verbeterde ervaring voor uw gebruikers nu als volgt te werk:**

1. Als een globale beheerder of Gebruikerbeheerder, moet u zich aanmelden bij de Azure-portal en gaat u naar **Azure Active Directory > Instellingen > instellingen voor toegang tot deelvenster preview-functies beheren**. 

2. In de **gebruikers dat toegang heeft tot de preview-functies voor het registreren en beheren van beveiligingsgegevens: vernieuwen** optie, kiest u om in te schakelen op de functies voor een **geselecteerde groep gebruikers** of voor **alle gebruikers** .

Het volgende aantal weken duurt, zullen we de mogelijkheid om in te schakelen op de oude gecombineerde MFA/SSPR-registratie preview-ervaring voor tenants die nog niet ingeschakeld hebt op verwijderen.

**Als u wilt zien als het besturingselement voor uw tenant, worden verwijderd, de volgende stappen uit:**

1. Als een globale beheerder of Gebruikerbeheerder, moet u zich aanmelden bij de Azure-portal en gaat u naar **Azure Active Directory > Instellingen > instellingen voor toegang tot deelvenster preview-functies beheren**.  

2. Als de **gebruikers die de preview-functies gebruiken kunnen voor het registreren en beheren van beveiligingsgegevens** optie is ingesteld op **geen**, de optie wordt verwijderd uit uw tenant.

Preview-ervaring voor gebruikers, ongeacht of u eerder hebt ingeschakeld op de oude gecombineerde MFA/SSPR-registratie of niet, de oude ervaring worden uitgeschakeld op een later tijdstip. Vanwege die sterk het is raadzaam dat u naar de nieuwe, verbeterde ervaring zo snel mogelijk verplaatsen.

Zie voor meer informatie over de verbeterde registratie-ervaring, de [Cool verbeteringen in de Azure AD MFA gecombineerd en registratie-ervaring voor wachtwoordherstel](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271).

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Bijgewerkte beleid beheerervaring voor gebruikersstromen

**Type:** Gewijzigde functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C

We hebben het beleid maken en beheren van het proces voor het gebruikersstromen (voorheen bekend als, ingebouwde beleidsregels) eenvoudiger bijgewerkt. Deze nieuwe ervaring is nu de standaard voor al uw Azure AD-tenants.

U kunt aanvullende feedback en suggesties geven met behulp van de glimlach of Boog omlaag pictogrammen in de **Stuur ons feedback** gebied aan de bovenkant van het scherm.

Zie voor meer informatie over de nieuwe beheerervaring van beleid, de [JavaScript-aanpassingen en veel meer nieuwe functies van Azure AD B2C heeft nu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blog.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Kies specifieke pagina element versies geleverd door Azure AD B2C

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C

U kunt nu een specifieke versie van de pagina-elementen die is geleverd door Azure AD B2C. Als u een specifieke versie selecteert, kunt u uw wijzigingen testen voordat ze op een pagina weergegeven en u kunt voorspelbaar gedrag. Bovendien kunt u nu zich om af te dwingen van specifieke paginaversies om toe te staan van JavaScript-aanpassingen. Als u wilt deze functie inschakelen, gaat u naar de **eigenschappen** pagina in uw gebruikersstromen.

Zie voor meer informatie over het kiezen van specifieke versies van pagina-elementen, de [JavaScript-aanpassingen en veel meer nieuwe functies van Azure AD B2C heeft nu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blog.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>Vereisten voor wachtwoorden kunnen worden geconfigureerd door eindgebruikers voor B2C (GA)

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C

U kunt nu van uw organisatie wachtwoordcomplexiteit instellen voor uw eindgebruikers, in plaats van gebruik te maken van uw eigen Azure AD-wachtwoordbeleid. Uit de **eigenschappen** blade van uw stromen (voorheen bekend als de ingebouwde beleidsregels), kunt u een wachtwoordcomplexiteit van **eenvoudige** of **sterke**, of u kunt Maak een **aangepaste** set vereisten.

Zie voor meer informatie over de vereiste configuratie van wachtwoord complexiteit [complexiteitsvereisten voldoen voor wachtwoorden configureren in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Nieuwe standaardsjablonen voor aangepaste huisstijl authenticatie-ervaringen

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C

U kunt onze nieuwe standaardsjablonen, zich op de **pagina-indelingen** blade van uw gebruikersstromen (voorheen bekend als ingebouwde beleidsregels), om te maken van een aangepaste huisstijl verificatie-ervaring voor uw gebruikers.

Zie voor meer informatie over het gebruik van de sjablonen [JavaScript-aanpassingen en veel meer nieuwe functies van Azure AD B2C heeft nu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595).

---

## <a name="january-2019"></a>Januari 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Active Directory B2B-samenwerking met behulp van verificatie eenmalige wachtwoordcode (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C

Verificatie van de eenmalige wachtwoordcode (OTP) hebben we geïntroduceerd voor B2B-gastgebruikers die via andere middelen, zoals Azure AD, een Microsoft-account (MSA) of Google Federatie kunnen niet worden geverifieerd. Deze nieuwe methode voor netwerkverificatie betekent dat gebruikers hoeft te maken van een nieuw Microsoft-account Gast. In plaats daarvan, hoewel een uitnodiging inwisselen of toegang tot een gedeelde bron, kunt aanvragen een gastgebruiker een tijdelijke code moet worden verzonden naar een e-mailadres. Met deze tijdelijke code kunt de gastgebruiker aan te melden bij blijven.

Zie voor meer informatie, [e eenmalige wachtwoordcode verificatie (preview)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) en de blog [Azure AD maakt delen en samenwerking naadloos voor elke gebruiker met een account](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949).

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Nieuwe Azure AD-toepassingsproxy cookie-instellingen

**Type:** Nieuwe functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Toegangsbeheer

We hebben drie nieuwe cookie-instellingen, beschikbaar voor uw apps die worden gepubliceerd via toepassingsproxy geïntroduceerd:

- **Gebruik alleen HTTP-cookie.** Stelt de **HTTPOnly** vlag aan de Application Proxy toegangs- en sessiebeleid cookies. Deze instelling inschakelt, biedt extra voordelen, zoals het helpen om te voorkomen dat kopiëren of wijzigen van cookies met behulp van client-side '-scripts. We raden u deze vlag inschakelt (Kies **Ja**) voor de extra voordelen.

- **Veilige cookie gebruiken.** Stelt de **Secure** vlag aan de Application Proxy toegangs- en sessiebeleid cookies. Deze instelling inschakelt, biedt de voordelen van de extra beveiliging, door ervoor te zorgen dat cookies worden alleen verzonden via TLS beveiligde kanalen, zoals HTTPS. We raden u deze vlag inschakelt (Kies **Ja**) voor de extra voordelen.

- **Permanente cookie gebruiken.** Hiermee voorkomt dat toegang cookies verlopen wanneer de webbrowser wordt gesloten. Deze cookies laatste gedurende de levensduur van het toegangstoken. Cookies worden echter opnieuw ingesteld als de vervaltijd is bereikt of als de gebruiker de cookie voor het handmatig verwijdert. We raden u aan de standaardinstelling **Nee**, alleen de instelling voor oudere apps die geen cookies tussen processen delen inschakelen.

Zie voor meer informatie over de nieuwe cookies [Cookie-instellingen voor toegang tot on-premises toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - januari 2019

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In januari 2019, hebben we deze 35 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Talent palet](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco overkoepelende](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Beheerder van Internet](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [vervaldatum herinnering](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Viewer](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [term](https://app.verb.net/login), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Pavaso digitale sluit](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [ Cloud Service PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO-systeem](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [Are voor Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [K2 voor Office 365](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn veerboot ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Verbeteringen van Azure AD Identity Protection (preview-versie)

**Type:** Gewijzigde functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

We zijn trots te kunnen aankondigen dat er de volgende verbeteringen is toegevoegd aan de aanbieding van Azure AD Identity Protection preview-versie, met inbegrip van:

- Een bijgewerkte en meer geïntegreerde gebruikersinterface

- Aanvullende API's

- Verbeterde risico-evaluatie van machine Learning

- Uitlijning van de gehele product voor riskante gebruikers en riskante aanmeldingen

Zie voor meer informatie over de verbeteringen voor [wat is Azure Active Directory Identity Protection (vernieuwd)?](https://aka.ms/IdentityProtectionDocs) voor meer informatie en deel uw ideeën tot en met de stappen in het product.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>Nieuwe App Lock-functie voor de Microsoft Authenticator-app op iOS en Android-apparaten

**Type:** Nieuwe functie  
**Categorie van de service:** Microsoft Authenticator-App  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

Uw wachtwoordcodes eenmalig, app-gegevens en app-instellingen om veiliger te houden, kunt u de App-Lock-functie in de Microsoft Authenticator-app inschakelen. Inschakelen van App-vergrendeling betekent dat u waarschijnlijk gevraagd verifiëren met behulp van uw PINCODE of biometrische telkens wanneer u de Microsoft Authenticator-app opent.

Zie voor meer informatie de [Microsoft Authenticator-app Veelgestelde vragen over](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Verbeterde Azure AD Privileged Identity Management (PIM) exporteren mogelijkheden

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management

Privileged Identity Management (PIM)-beheerders kunnen nu alle actieve en in aanmerking komende roltoewijzingen voor een specifieke bron, waaronder roltoewijzingen voor alle onderliggende resources exporteren. Voorheen was het moeilijk voor beheerders om een volledige lijst van roltoewijzingen voor een abonnement en ze had roltoewijzingen voor elke specifieke resource te exporteren.

Zie voor meer informatie, [activiteit en audit geschiedenis weergeven voor Azure-resource-rollen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

## <a name="novemberdecember-2018"></a>November/December 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Gebruikers verwijderd uit synchronisatie bereik niet langer switch naar alleen-cloud-accounts

**Type:** Opgelost  
**Categorie van de service:** Gebruikersbeheer  
**Product-mogelijkheid:** Directory

>[!Important]
>We hebben op basis van en inzicht in uw frustraties vanwege deze oplossing. Dus hebben we hersteld deze wijziging tot aan het moment dat we kunnen de oplossing voor u eenvoudiger te implementeren in uw organisatie.

We hebben een bug waarin de vlag DirSyncEnabled van een gebruiker zou worden ten onrechte overgeschakeld naar de vaste **False** wanneer het Active Directory Domain Services (AD DS)-object is uitgesloten van synchronisatie-bereik en klikt u vervolgens verplaatst naar de Prullenbak in Azure AD op de volgende synchronisatiecyclus. Als gevolg van deze oplossing wordt als de gebruiker is uitgesloten van het bereik van gegevenssynchronisatie en daarna hersteld op basis van Azure AD Recycle Bin account van de gebruiker blijft als die zijn gesynchroniseerd vanaf on-premises AD, zoals verwacht en kan niet worden beheerd in de cloud, omdat de bron van bronrecords authority (SoA) blijft on-premises AD.

Voordat u deze oplossing is het een probleem wanneer de vlag DirSyncEnabled is overgeschakeld naar de waarde False. Het heeft de verkeerde indruk dat deze accounts zijn geconverteerd naar alleen-cloud-objecten en dat de accounts in de cloud kunnen worden beheerd. De accounts bewaard echter nog steeds hun SoA als on-premises en alle gesynchroniseerde eigenschappen (kenmerken) die afkomstig zijn van on-premises AD. Dit probleem veroorzaakt meerdere problemen in Azure AD en andere cloudwerkbelastingen (zoals Exchange Online) die u verwacht dat deze accounts behandelen als gesynchroniseerd uit Active Directory, maar zijn nu gedraagt, zoals accounts alleen in de cloud.

Op dit moment is de enige manier om echt een gesynchroniseerd van AD-account converteren naar alleen-cloud-account door het uitschakelen van DirSync op tenantniveau, waarvan een back endbewerking om over te dragen van de SoA wordt geactiveerd. Dit type SoA wijziging vereist (maar is niet beperkt tot) het opschonen van alle de on-premises gerelateerde kenmerken (zoals LastDirSyncTime en de kenmerken van) en een signaal verzenden naar andere cloudworkloads naar de respectieve object wordt omgezet in een alleen-cloud-account te hebben .

Deze oplossing is als gevolg daarvan wordt voorkomen dat directe updates op het kenmerk ImmutableID van een gebruiker gesynchroniseerd uit Active Directory dat in sommige scenario's in het verleden vereist zijn. Standaard de ImmutableID van een object in Azure AD, zoals de naam al aangeeft, is bedoeld om onveranderbaar zijn. Nieuwe functies die zijn geïmplementeerd in Azure AD Connect Health en Azure AD Connect-synchronisatie-client zijn beschikbaar voor dergelijke scenario's:

- **Grootschalige ImmutableID-update voor een groot aantal gebruikers in een gefaseerde benadering**
  
  Bijvoorbeeld, moet u een langdurige migratie van AD DS tussen forests. Oplossing: Gebruik Azure AD Connect **Bronanker configureren** en wanneer de gebruiker worden gemigreerd, kopieert u de bestaande ImmutableID-waarden van Azure AD naar ms-DS-consistentie-Guid-kenmerk van de lokale AD DS-gebruiker van het nieuwe forest. Zie voor meer informatie, [met behulp van ms-DS-ConsistencyGuid as sourceAnchor](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor).

- **Grootschalige ImmutableID updates voor veel gebruikers in één keer**

  Bijvoorbeeld bij het implementeren van Azure AD Connect u een fout maakt, en nu moet u het kenmerk SourceAnchor wijzigen. Oplossing: Uitschakelen van DirSync op het tenantniveau van de en schakel alle ongeldige ImmutableID-waarden. Zie voor meer informatie, [uitschakelen voor adreslijstsynchronisatie van Office 365](/office365/enterprise/turn-off-directory-synchronization).

- **Rematch on-premises gebruiker met een bestaande gebruiker in Azure AD** bijvoorbeeld een gebruiker die opnieuw worden gemaakt in AD DS is een duplicaat in Azure AD-account in plaats van het met een bestaande Azure AD-account (zwevende object) rematching genereert. Oplossing: Azure AD Connect Health gebruiken in Azure portal opnieuw toewijzen van anker/ImmutableID van de bron. Zie voor meer informatie, [zwevende object scenario](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario).

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Belangrijke wijziging: Updates voor de controle en het schema van de logboeken voor aanmelden via Azure Monitor

**Type:** Gewijzigde functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

We zijn momenteel log-streams voor zowel de controle en meld u via Azure Monitor, publiceren, zodat u de logboekbestanden naadloos met uw SIEM-hulpprogramma's of met Log Analytics integreren kunt. Op basis van uw feedback en ter voorbereiding van de aankondiging van de algemene beschikbaarheid van deze functie, doorvoeren we de volgende wijzigingen om onze schema. Deze wijzigingen in het schema en de bijbehorende documentatie-updates wordt uitgevoerd op de eerste week van januari.

#### <a name="new-fields-in-the-audit-schema"></a>Nieuwe velden in het schema controleren
We voegen een nieuwe toe **bewerkingstype** veld voor het type bewerking uitgevoerd op de resource. Bijvoorbeeld, **toevoegen**, **Update**, of **verwijderen**.

#### <a name="changed-fields-in-the-audit-schema"></a>Gewijzigde velden in het schema controleren
De volgende velden wijzigt in het controle-schema:

|Veldnaam|Wat is gewijzigd|Oude waarden|Nieuwe waarden|
|----------|------------|----------|----------|
|Category|Dit is de **servicenaam** veld. Het is nu de **Audit categorieën** veld. **Servicenaam** is gewijzigd in de **loggedByService** veld.|<ul><li>Account inrichten</li><li>Hoofddirectory</li><li>Self-service voor wachtwoord opnieuw instellen</li></ul>|<ul><li>Gebruikersbeheer</li><li>Groepsbeheer</li><li>App-beheer</li></ul>|
|targetResources|Bevat **TargetResourceType** op het hoogste niveau.|&nbsp;|<ul><li>Beleid</li><li>App</li><li>Gebruiker</li><li>Groep</li></ul>|
|loggedByService|Hier wordt de naam van de service die het auditlogboek gegenereerd.|Null|<ul><li>Account inrichten</li><li>Hoofddirectory</li><li>Self-service voor wachtwoord opnieuw instellen</li></ul>|
|Resultaat|Geeft het resultaat van de auditlogboeken. Voorheen was dit is geïnventariseerd, maar laten we nu zien de werkelijke waarde.|<ul><li>0</li><li>1</li></ul>|<ul><li>Geslaagd</li><li>Fout</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>Gewijzigde velden in het schema voor aanmelden
De volgende velden wilt in het schema voor aanmelden wijzigen:

|Veldnaam|Wat is gewijzigd|Oude waarden|Nieuwe waarden|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|Dit is de **conditionalaccessPolicies** veld. Het is nu de **appliedConditionalAccessPolicies** veld.|Geen wijziging|Geen wijziging|
|conditionalAccessStatus|Geeft het resultaat van de beleidsstatus van voorwaardelijke toegang bij het aanmelden. Voorheen was dit is geïnventariseerd, maar laten we nu zien de werkelijke waarde.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Geslaagd</li><li>Fout</li><li>Niet toegepast</li><li>Uitgeschakeld</li></ul>|
|appliedConditionalAccessPolicies: resultaat|Geeft het resultaat van de afzonderlijke voorwaardelijke toegang Beleidsstatus bij het aanmelden. Voorheen was dit is geïnventariseerd, maar laten we nu zien de werkelijke waarde.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Geslaagd</li><li>Fout</li><li>Niet toegepast</li><li>Uitgeschakeld</li></ul>|

Zie voor meer informatie over het schema [interpreteren van de Azure AD-auditlogboeken schema beschikbaar zijn in Azure Monitor (preview)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Identity Protection verbeteringen in de bewaakte machine learning-model en de risico-engine van score

**Type:** Gewijzigde functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Risicoscores

Verbeteringen in de Identity Protection-gerelateerde gebruiker en meld u risico-evaluatie-engine kunnen helpen om gebruiker risico nauwkeurigheid en dekking te verbeteren. Beheerders kunnen u ziet dat risiconiveau van de gebruiker niet meer rechtstreeks is gekoppeld aan het risiconiveau van specifieke detecties, en of er een toename van het aantal en het niveau van riskante aanmelding-gebeurtenissen.

Detecties van risico's worden nu geëvalueerd door de bewaakte machine learning-model, welke gebruikers risico's berekent met behulp van een patroon van detecties en extra functies van de aanmeldingen van de gebruiker. Op basis van dit model, kan de beheerder van de gebruikers met een hoog risicoscores, zoeken, zelfs als detecties die zijn gekoppeld aan deze gebruiker van laag of Gemiddeld risico zijn. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Beheerders kunnen hun eigen wachtwoord met behulp van de Microsoft Authenticator-app (preview-versie) in opnieuw instellen

**Type:** Gewijzigde functie  
**Categorie van de service:** Selfservice voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie

Azure AD-beheerders kunnen nu opnieuw instellen voor hun eigen wachtwoord met behulp van de Microsoft Authenticator-app-meldingen of een code van een mobiele verificator-app of de hardware-token. Om hun eigen wachtwoord opnieuw instellen, is beheerders nu mogelijk gebruik van twee van de volgende methoden:

- Microsoft Authenticator-app-melding

- Andere mobiele verificator-app / Hardware token code

- Email

- Telefonische oproep

- Sms-bericht

Zie voor meer informatie over het gebruik van de Microsoft Authenticator-app opnieuw instellen van wachtwoorden [Azure AD Self-service voor wachtwoord opnieuw instellen - mobiele app en SSPR (Preview)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr-preview)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Nieuwe Cloud-Apparaatbeheerder voor Azure AD-rol (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Device Registration service en beheer  
**Product-mogelijkheid:** Toegangsbeheer

Beheerders kunnen gebruikers toewijzen aan de nieuwe rol in de Cloud-Apparaatbeheerder cloud apparaat beheerderstaken uit te voeren. Gebruikers die zijn toegewezen de beheerdersrol van Cloud-apparaat kunnen inschakelen, uitschakelen en verwijderen van apparaten in Azure AD, samen met kunnen Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal worden gelezen.

Zie voor meer informatie over rollen en machtigingen, [beheerdersrollen toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Uw apparaten beheren met de tijdstempel van de nieuwe activiteit in Azure AD (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Device Registration service en beheer  
**Product-mogelijkheid:** Levenscyclus van Apparaatbeheer

We realiseren ons dat na verloop van tijd moet u vernieuwen en is voor uw organisatie apparaten buiten gebruik stellen in Azure AD, om te voorkomen dat verouderde apparaten in uw omgeving. Om te helpen met dit proces, werkt nu Azure AD uw apparaten met een nieuwe activiteit timestamp, helpen u bij het beheren van de levenscyclus van uw apparaat.

Zie voor meer informatie over het verkrijgen en gebruiken van deze timestamp [How To: De verouderde apparaten beheren in Azure AD](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Beheerders kunnen vereisen dat gebruikers de gebruiksvoorwaarden op elk apparaat accepteren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer
 
Beheerders kunnen nu inschakelen op de **vereisen dat gebruikers accepteren op elk apparaat** optie uw gebruikers moeten accepteer de gebruiksvoorwaarden op elk apparaat dat wordt gebruikt op uw tenant.

Zie voor meer informatie de [Per apparaat en de gebruiksvoorwaarden gedeelte van de voorwaarden van de Azure Active Directory van functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Beheerders kunnen een gebruiksrechtovereenkomst verloopt op basis van een terugkerend schema configureren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer
 

Beheerders kunnen nu inschakelen op de **verlopen toestemmingen** optie om te maken van een gebruiksrechtovereenkomst verlopen voor alle gebruikers op basis van de opgegeven terugkerende planning. Het schema mag per jaar, bi per jaar, kwartaal of per maand. Nadat de gebruiksvoorwaarden is verlopen, moeten gebruikers accepteren.

Zie voor meer informatie de [toevoegen van de voorwaarden van het gedeelte van de voorwaarden van de Azure Active Directory van functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Beheerders kunnen een gebruiksrechtovereenkomst verloopt op basis van de planning van elke gebruiker configureren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

Beheerders kunnen nu opgeven voor een duur van die gebruiker moet de gebruiksvoorwaarden accepteren. Beheerders kunnen bijvoorbeeld opgeven dat een gebruiksrechtovereenkomst om de 90 dagen door gebruikers moeten accepteren.

Zie voor meer informatie de [toevoegen van de voorwaarden van het gedeelte van de voorwaarden van de Azure Active Directory van functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Nieuwe Azure AD Privileged Identity Management (PIM) e-mailberichten voor Azure Active Directory-rollen

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Klanten die gebruikmaken van Azure AD Privileged Identity Management (PIM) kunnen nu ontvangen voor een wekelijkse e-mail met de samenvatting, met inbegrip van de volgende informatie voor de afgelopen zeven dagen:

- Overzicht van de bovenste in aanmerking komende en permanente roltoewijzingen

- Aantal gebruikers, rollen activeren

- Aantal gebruikers dat is toegewezen aan rollen in PIM

- Aantal gebruikers dat is toegewezen aan rollen buiten PIM

- Aantal gebruikers "en-klare permanente" in PIM

Zie voor meer informatie over PIM en de beschikbare e-mailmeldingen [e-mailmeldingen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications).

---

### <a name="group-based-licensing-is-now-generally-available"></a>Groepslicenties is nu algemeen beschikbaar

**Type:** Gewijzigde functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Directory

Groepslicenties is uit de openbare preview en is nu algemeen beschikbaar. Als onderdeel van deze algemene versie is uitgebracht, we deze functie beter schaalbaar hebt gemaakt en de mogelijkheid om te opnieuw verwerken licentieverlening toewijzingen op basis van een groep voor één gebruiker en de mogelijkheid om met Groepslicenties met Office 365 E3/A3-licenties hebt toegevoegd.

Zie voor meer informatie over Groepslicenties [wat is licentieverlening in Azure Active Directory op basis van groep?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - November 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In November 2018, hebben we deze 26 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[CoreStack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [grijs Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [eHour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [DriveDollar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview), [Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [oneindige Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [Alaya](https://alayagood.com/en/demo/), [ HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy voor bedrijven centrale 365](https://accounting.zenegy.com/), [Everbridge lid Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [Plex Apps: Klassieke Test](https://test.plexonline.com/signon), [Plex Apps – klassiek](https://www.plexonline.com/signon), [Plex Apps - UX Test](https://test.cloud.plex.com/sso), [Plex Apps – UX](https://cloud.plex.com/sso), [Plex Apps – IAM](https://accounts.plex.com/), [KNUTSELEN - kinderopvang Records, aanwezigheid en financiële volgsysteem](https://getcrafts.ca/craftsregistration) 

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---