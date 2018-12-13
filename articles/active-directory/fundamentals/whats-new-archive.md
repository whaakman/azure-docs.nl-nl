---
title: Archief voor de wat is er nieuw? -Azure Active Directory | Microsoft Docs
description: Wat is nieuw opmerkingen bij de release in het overzicht van de sectie van deze inhoudsset bevat zes maanden van de activiteit van. Na zes maanden worden de items verwijderd uit het belangrijkste artikel en in dit archiefartikel plaatsen.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: d727c9c3a9add701263e799dba778b37e637adef
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53194280"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Archief voor de wat is er nieuw? in Azure Active Directory

De primaire [wat is er nieuwe opmerkingen bij de release](whats-new.md) artikel bevat de laatste zes maanden van gegevens, hoewel dit artikel de oudere gegevens bevat.

Wat is nieuw opmerkingen bij de release bieden u informatie over:

- De meest recente versies
- Bekende problemen
- Opgeloste fouten
- Afgeschafte functies
- Plannen voor wijzigingen

---

## <a name="may-2018"></a>Mei 2018

### <a name="expressroute-support-changes"></a>Ondersteuning voor ExpressRoute-wijzigingen

**Type:** Plan voor wijziging  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Platform  

Software as a Service biedt, zoals Azure Active Directory (Azure AD) zijn ontworpen voor het beste werkt door te gaan rechtstreeks via het Internet, zonder ExpressRoute of andere persoonlijke VPN-tunnels. Als gevolg hiervan op **vanaf 1 augustus 2018**, we geen ondersteuning meer voor ExpressRoute voor Azure AD-services met behulp van openbare Azure-peering en Azure-community's in Microsoft-peering. Alle services die beïnvloed door deze wijziging mogelijk ziet u Azure AD-verkeer geleidelijk verschuiving van ExpressRoute met Internet.

Terwijl we onze ondersteuning, wijzigt ook weten we er zijn nog steeds situaties waarin u mogelijk wilt gebruiken een toegewezen set met circuits voor uw verificatieverkeer. Als gevolg hiervan blijft Azure AD voor de ondersteuning van per-tenant IP-bereikbeperkingen met behulp van ExpressRoute- en -services al op het Microsoft-peering met de community 'Andere Online Office 365-services'. Als uw services worden beïnvloed, maar u ExpressRoute nodig hebt, moet u het volgende doen:

- **Als u openbare Azure-peering.** Ga naar het Microsoft-peering en zich aanmelden voor de **andere Office 365 Online services (12076:5100)** community. Zie voor meer informatie over het verplaatsen van openbare Azure-peering voor het Microsoft-peering, de [verplaatsen van een openbare peering naar het Microsoft-peering](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) artikel.

- **Als u van Microsoft-peering gebruikmaakt.** Zich aanmelden voor de **andere Office 365 Online service (12076:5100)** community. Zie voor meer informatie over de vereisten voor routering, de [ondersteuning voor BGP-community's sectie](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) van het ExpressRoute-routering vereisten-artikel.

Als u moet echter ook doorgaan met toegewezen circuits, moet u om te communiceren met uw team van Microsoft-Account over het ophalen van autorisatie gebruik van de **andere Office 365 Online service (12076:5100)** community. De MS Office beheerde controleteam controleert of u of u deze circuits moet en zorg ervoor dat u inzicht in de technische gevolgen te houden. Niet-gemachtigde abonnementen bij het maken van routefilters voor Office 365 ontvangt een foutbericht weergegeven. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph-API's voor beheerscenario's voor de gebruiksvoorwaarden

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Ontwikkelaarservaring
 
We hebben Microsoft Graph-API's toegevoegd voor de beheerbewerking van Azure AD gebruiksrechtovereenkomst. U zijn kunt maken, bijwerken en verwijderen van de gebruiksvoorwaarden van object.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Azure AD-multitenant-eindpunt toevoegen als een id-provider in Azure AD B2C

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - Consumentenidentiteitsbeheer  
**Product-mogelijkheid:** B2B/B2C
 
Met behulp van aangepaste beleidsregels, kunt u de algemene Azure AD-eindpunt toevoegen als een id-provider in Azure AD B2C. Hiermee kunt u dat een single point of vermelding voor alle Azure AD-gebruikers die bij uw toepassingen aanmeldt zich. Zie voor meer informatie, [Azure Active Directory B2C: Toestaan dat gebruikers zich aanmelden met een id-provider van het Azure AD met meerdere tenants met behulp van aangepaste beleidsregels](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Interne URL's gebruikt voor toegang tot apps vanaf elke locatie met de extensie mijn Apps, aanmelding en de Azure AD-toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn apps  
**Product-mogelijkheid:** Eenmalige aanmelding
 
Gebruikers kunnen nu toegang tot toepassingen via de interne URL's ook buiten uw bedrijfsnetwerk met behulp van de mijn Apps beveiligde aanmelding-extensie voor Azure AD. Dit werkt met elke toepassing die u hebt gepubliceerd met behulp van Azure AD-toepassingsproxy in elke browser waarop ook de Browseruitbreiding van het toegangsvenster geïnstalleerd. De functionaliteit van URL-omleiding is automatisch ingeschakeld wanneer een gebruiker meldt zich aan bij de extensie. De extensie is beschikbaar voor downloaden op [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367), en [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory - gegevens in Europa voor Europa-klanten

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** GoLocal

Klanten in Europa vereisen hun gegevens om te blijven in Europa en niet gerepliceerd buiten de Europese datacenters voor vergadering privacy en de Europese wetgeving. Dit [artikel](https://go.microsoft.com/fwlink/?linkid=872328) bevat de specifieke details over welke identiteitsgegevens worden opgeslagen in Europa en bieden ook details van de gegevens die zijn opgeslagen buiten Europese datacenters. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Nieuwe gebruikers inrichten van SaaS-app integraties - mei 2018

**Type:** Nieuwe functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
Azure AD kunt u het maken, onderhoud en verwijderen van gebruikers-id's in SaaS-toepassingen, zoals Dropbox, Salesforce, ServiceNow en automatiseren. Voor mei 2018, hebben we ondersteuning voor de volgende toepassingen in de galerie van Azure AD-app inrichten van gebruikers toegevoegd:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Hoeksteen OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Zie voor een lijst van alle toepassingen die ondersteuning bieden voor het inrichten van gebruikers in de Azure AD-galerie, [ https://aka.ms/appstutorial ](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Azure AD-toegangsbeoordelingen van groepen en toegang tot de app biedt nu terugkerende beoordelingen

**Type:** Nieuwe functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Beheer
 
Toegangsbeoordeling voor groepen en apps is nu algemeen beschikbaar als onderdeel van Azure AD Premium P2.  Beheerders zich toegangsbeoordelingen van groepslidmaatschappen en -toewijzingen van de toepassing automatisch wordt herhaald met regelmatige intervallen, zoals maandelijkse of driemaandelijkse configureren.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD-activiteitenlogboeken (aanmeldingen en audit) zijn nu beschikbaar zijn via MS Graph

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Controleren en rapporteren
 
Azure AD activiteitenlogboeken, waaronder, aanmeldingen en auditlogboeken, zijn nu beschikbaar zijn via MS Graph. We hebben twee eindpunten via MS Graph voor toegang tot deze logboeken weergegeven. Bekijk onze [documenten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) voor programmatische toegang tot Azure AD Reporting API's aan de slag. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Verbeteringen aan het inwisselen van de B2B-ervaring en een organisatie verlaten

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C

**Just-in-time-inschrijving:** Wanneer u een resource delen met een gastgebruiker met B2B-API – u hoeft niet te verzenden van een speciale uitnodiging via e-mail. In de meeste gevallen de gastgebruiker hebben toegang tot de resource en zien wat de ervaring inwisselen just-in-tijd worden uitgevoerd. Er is geen meer impact vanwege gemiste e-mailberichten. Niet meer wordt gevraagd uw gastgebruikers 'Hebt u klikken op de koppeling inwisselen die het systeem naar u verzonden?'. Dit betekent dat zodra SPO maakt gebruik van de manager van de uitnodiging – bewolkt bijlagen de dezelfde standaard-URL voor alle gebruikers, interne en externe – in elke status van inschrijving hebben kunnen.

**De ervaring voor moderne inschrijving:** Niet meer gesplitst scherm inwisselen landingspagina. Gebruikers zien een moderne toestemming geven ervaring met de privacyverklaring van de uitnodigende organisatie, net als voor apps van derden.

**Gastgebruikers kunnen de organisatie verlaten:** Zodra de relatie van een gebruiker met een organisatie uitgevoerd is, kunnen ze zelf dienen de organisatie heeft verlaten. Aanroepen niet meer van de uitnodigende organisatie admin 'verwijderd', niet meer verhogen ondersteuningstickets.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - mei 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In mei 2018, hebben we deze 18 nieuwe apps met Federatie ondersteuning aan onze app-galerie toegevoegd:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty regelen, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [landen MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial),[まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [boog publicatie - eenmalige aanmelding ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [koppel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Nieuwe stapsgewijze implementatiehandleidingen voor Azure Active Directory

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Directory
 
Nieuwe, stapsgewijze richtlijnen over het implementeren van Azure Active Directory (Azure AD), inclusief selfservice voor wachtwoordherstel (SSPR), eenmalige aanmelding (SSO), voorwaardelijke toegang (CA), App-proxy, gebruikers inrichten, Active Directory Federation Services (ADFS) op Pass through-verificatie (PTA) en ADFS voor synchronisatie van wachtwoordhashes (WHS).

Als u de implementatiehandleidingen, gaat u naar de [identiteit implementatiehandleidingen](https://aka.ms/DeploymentPlans) op GitHub. Voor feedback over de implementatiehandleidingen, gebruikt u de [implementatie plannen feedbackformulier](https://aka.ms/deploymentplanfeedback). Hebt u vragen hebt over de implementatiehandleidingen, contact met ons op [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="enterprise-applications-search---load-more-apps"></a>Zakelijke toepassingen zoeken: meer Apps laden

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding
 
Problemen met het vinden van uw toepassingen / service-principals? We hebben de mogelijkheid om te laden meer toepassingen in uw zakelijke toepassingen op de lijst met alle toepassingen toegevoegd. Standaard, laten we zien 20 toepassingen. U kunt nu op klikt, **meer laden** om aanvullende toepassingen weer te geven. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>De versie van AADConnect een openbare preview-versie van de integratie met PingFederate bevat, mei belangrijke beveiligingsupdates, veel oplossingen voor problemen en nieuwe geweldige nieuwe hulpprogramma's voor probleemoplossing. 

**Type:** Gewijzigde functie  
**Categorie van de service:** AD Connect  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli
 
De versie van AADConnect een openbare preview-versie van de integratie met PingFederate bevat, mei belangrijke beveiligingsupdates, veel oplossingen voor problemen en nieuwe geweldige nieuwe hulpprogramma's voor probleemoplossing. U vindt de opmerkingen bij de release [hier](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD-toegangsbeoordelingen: automatisch toepassen

**Type:** Gewijzigde functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Beheer

Toegangsbeoordelingen voor groepen en apps zijn nu algemeen beschikbaar als onderdeel van Azure AD Premium P2. Een beheerder kan configureren zodat de wijzigingen van de revisor automatisch aan die groep of de app worden toegepast als de toegangsbeoordeling is voltooid. De beheerder kan ook opgeven wat gebeurt er met blijvende toegang van de gebruiker als revisoren niet reageren, toegang verwijderen, toegang houden of systeem aanbevelingen ophalen. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>ID-tokens kunnen niet meer worden geretourneerd met behulp van de query-response_mode voor nieuwe apps. 

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Apps die zijn gemaakt op of na 25 April 2018 niet meer mogelijk om aan te vragen een **id_token** met behulp van de **query** response_mode.  Hiermee wordt Azure AD-inline met de OIDC-specificaties en helpt de kwetsbaarheid van uw apps te beperken.  Apps die zijn gemaakt voor 25 April 2018 worden niet geblokkeerd met behulp van de **query** response_mode met een response_type van **id_token**.  De fout is geretourneerd, bij het aanvragen van een id_token van AAD, is **AADSTS70007: 'query' is geen ondersteunde waarde van 'response_mode' bij het aanvragen van een token**.

De **fragment** en **form_post** response_modes blijven werken - bij het maken van het nieuwe objecten voor toepassingen (bijvoorbeeld voor App-Proxy gebruik), zorg ervoor dat gebruik van een van deze response_modes voordat ze maakt een nieuwe toepassing.  

---
 
## <a name="april-2018"></a>April 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C Access Token zijn algemeen beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - Consumentenidentiteitsbeheer  
**Product-mogelijkheid:** B2B/B2C 

U hebt nu toegang tot Web-API's beveiligd door Azure AD B2C met-toegangstokens. De functie wordt verplaatst van de openbare preview-versie voor algemene beschikbaarheid. De gebruikersinterface-ervaring voor het configureren van Azure AD B2C-toepassingen en web-API's, is verbeterd en zijn andere kleine verbeteringen aangebracht.
 
Zie voor meer informatie, [Azure AD B2C: Aanvragen van toegangstokens](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Configuratie voor eenmalige aanmelding voor toepassingen op basis van SAML testen

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding

Bij het configureren van SAML gebaseerde SSO-toepassingen, u kunt de integratie van op de configuratiepagina testen. Als er een fout opgetreden tijdens het aanmelden, kunt u de fout in de test-ervaring bieden en Azure AD biedt u stappen voor het oplossen van dit specifieke probleem.

Zie voor meer informatie:

- [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
- [Fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD gebruiksrechtovereenkomst heeft nu per gebruiker melden

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 
Beheerders kunnen nu selecteert u een bepaalde gebruiksvoorwaarden en zien alle gebruikers die hebben ingestemd met de gebruiksvoorwaarden en wat datum/tijd het heeft plaatsgevonden.

Zie voor meer informatie de [functie van Azure AD gebruiksrechtovereenkomst](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: Riskant IP-adres voor beveiliging met AD FS extranet accountvergrendeling 

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Controleren en rapporteren

Connect Health nu ondersteunt de mogelijkheid voor het detecteren van IP adressen die groter zijn dan een drempel voor mislukte U/P aanmeldingen op basis van per uur of dagelijks. De mogelijkheden van deze functie zijn:

- Uitgebreide rapport weergeven met de IP-adres en het aantal mislukte aanmeldingen die zijn gegenereerd op basis van per uur/dag met aanpasbare drempelwaarde.
- Waarschuwingen op basis van e-mailbericht weergegeven wanneer een specifiek IP-adres heeft de drempelwaarde van mislukte U/P aanmeldingen op basis van per uur/dag overschreden.
- Een downloadoptie om een gedetailleerde analyse van de gegevens

Zie voor meer informatie, [rapport riskant IP-adres](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Eenvoudig app-configuratie met de van metagegevensbestand of URL

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding

Op de pagina van de Enterprise-toepassingen kunnen beheerders een SAML-metagegevensbestand als u wilt configureren op basis van SAML-aanmelding voor AAD-galerie en niet toepassing uploaden.

Bovendien kunt u URL voor federatieve metagegevens van Azure AD-toepassing voor het configureren van eenmalige aanmelding met de betreffende toepassing.

Zie voor meer informatie, [configureren van eenmalige aanmelding voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD gebruiksrechtovereenkomst gebruiken nu algemeen beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 

Azure AD gebruiksrechtovereenkomst hebt verplaatst van de openbare preview-versie naar algemeen beschikbaar.

Zie voor meer informatie de [functie van Azure AD gebruiksrechtovereenkomst](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Uitnodigingen voor B2B-gebruikers van bepaalde organisaties toestaan of blokkeren

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C
 

U kunt nu opgeven welke partnerorganisaties die u wilt delen en samenwerken met in Azure AD B2B-samenwerking. Om dit te doen, kunt u lijst maken met specifieke toestaan of weigeren van domeinen. Wanneer een domein is geblokkeerd met behulp van deze mogelijkheden, kunnen werknemers niet meer uitnodigingen verzenden naar mensen in dat domein.

Zo kunt u voor het beheren van toegang tot uw resources tijdens het inschakelen van een goede ervaring voor goedgekeurde gebruikers.

Deze functie B2B-samenwerking is beschikbaar voor alle klanten van Azure Active Directory en kan worden gebruikt in combinatie met Azure AD Premium-functies, zoals voorwaardelijke toegang en identity protection voor gedetailleerdere controle over wanneer en hoe de externe zakelijke gebruikers zich aanmelden in en toegang te krijgen.

Zie voor meer informatie, [uitnodigingen toestaan of blokkeren voor B2B-gebruikers van bepaalde organisaties](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve apps beschikbaar in Azure AD app-galerie

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden

In April 2018, hebben we deze 13 nieuwe apps met Federatie ondersteuning aan onze app-galerie toegevoegd:

Criterium HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [geheim Server (On-Premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dynamische signaal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [organigram Nu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Prestatiemeter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), rek, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>GRANT B2B-gebruikers in Azure AD-toegang tot uw on-premises toepassingen (preview-versie)

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C

Als een organisatie die gebruikmaakt van functionaliteit voor samenwerking in Azure Active Directory (Azure AD) B2B uitnodigen van gastgebruikers van partnerorganisaties koppelt aan uw Azure AD, kunt u nu deze B2B-gebruikers toegang bieden tot on-premises toepassingen. Deze on-premises toepassingen kunnen verificatie op basis van SAML of geïntegreerde Windows-verificatie (IWA) gebruiken met Kerberos-beperkte delegatie (KCD).

Zie voor meer informatie, [Grant B2B-gebruikers in Azure AD de toegang tot uw on-premises toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Zelfstudies over integratie van eenmalige aanmelding ophalen uit de Azure-Marketplace

**Type:** Gewijzigde functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Integratie met toepassing van derden

Als een toepassing die wordt vermeld in de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) ondersteunt SAML gebaseerde eenmalige aanmelding, te klikken op **nu downloaden** biedt u de zelfstudie voor integratie van die zijn gekoppeld aan deze toepassing. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Snellere prestaties van Azure AD automatisch gebruikers inrichten voor SaaS-toepassingen

**Type:** Gewijzigde functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
Voorheen klanten die gebruikmaken van de inrichting van connectors voor SaaS-toepassingen (bijvoorbeeld Salesforce, ServiceNow en vak) Azure Active Directory-gebruiker kunnen traag als hun Azure AD-tenants die meer dan 100.000 gecombineerde gebruikers en groepen, en ze zijn toewijzingen van gebruikers en groepen gebruiken om te bepalen welke gebruikers moeten worden ingericht.

Op 2 April 2018, zijn belangrijke prestatieverbeteringen geïmplementeerd op de Azure AD-inrichtingsservice die aanzienlijk verminderen de hoeveelheid tijd die nodig is om uit te voeren van de initiële synchronisatie tussen Azure Active Directory- en doel SaaS-toepassingen.

Als gevolg hiervan, veel klanten had initiële synchronisaties met apps die het aantal dagen duurde of nooit uitgevoerd, kunt u nu binnen een paar minuten of uren zijn voltooid.

Zie voor meer informatie, [wat er gebeurt tijdens het inrichten?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self-service voor wachtwoord opnieuw instellen van Windows 10-vergrendelingsscherm voor hybride Azure AD verbonden computers

**Type:** Gewijzigde functie  
**Categorie van de service:** Selfservice voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie
 
De functie voor Windows 10 SSPR nu ondersteuning voor machines die zijn toegevoegd aan hybrid Azure AD join is bijgewerkt. Deze functie is beschikbaar in Windows 10 RS4 kunnen gebruikers hun wachtwoord van het vergrendelingsscherm van een Windows 10-computer. Deze functie kunnen gebruikmaken van gebruikers die zijn ingeschakeld en geregistreerd voor selfservice voor wachtwoord opnieuw instellen.

Zie voor meer informatie, [Azure AD-wachtwoord opnieuw instellen vanuit het aanmeldingsscherm](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Maart 2018
 
### <a name="certificate-expire-notification"></a>Certificaat verloopt melding

**Type:** Vast  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding
 
Azure AD verzendt een melding wanneer een certificaat voor een galerie of toepassing buiten de galerie is bijna verlopen. 

Sommige gebruikers is niet ontvangen van meldingen voor bedrijfstoepassingen die zijn geconfigureerd voor SAML gebaseerde eenmalige aanmelding. Dit probleem is opgelost. Azure AD stuurt de melding voor certificaten dat verloopt binnen 7, 30 en 60 dagen. U bent kunnen zien van dit evenement dat in de auditlogboeken. 

Zie voor meer informatie:

- [Certificaten beheren voor federatieve eenmalige aanmelding in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Controleactiviteitenrapporten in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Twitter- en GitHub-id-providers in Azure AD B2C

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - Consumentenidentiteitsbeheer  
**Product-mogelijkheid:** B2B/B2C
 
U kunt nu Twitter of GitHub toevoegen als een id-provider in Azure AD B2C. Twitter wordt verplaatst van de openbare preview-versie voor algemene beschikbaarheid. GitHub is in openbare preview-versie wordt uitgebracht.

Zie voor meer informatie, [wat is Azure AD B2B-samenwerking?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Beperken van toegang via de browser met behulp van Intune Managed Browser voor iOS en Android met Azure AD-toepassing gebaseerde voorwaardelijke toegang

**Type:** Nieuwe functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming
 
**Nu in openbare preview!**

**Intune Managed Browser eenmalige aanmelding:** Uw werknemers kunnen eenmalige aanmelding in systeemeigen clients (zoals Microsoft Outlook) en de Intune Managed Browser gebruiken voor alle Azure AD verbonden apps.

**Intune Managed Browser-ondersteuning voor voorwaardelijke toegang:** U kunt nu vereisen dat werknemers kunnen met behulp van beleid voor voorwaardelijke toegang op basis van een toepassing Intune Managed browser gebruiken.

Meer informatie over hoe dit in onze [blogbericht](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Zie voor meer informatie:

- [Voorwaardelijke toegang op basis van een toepassing instellen](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Beheerde-browserbeleid configureren](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>App-Proxy-Cmdlets in algemene beschikbaarheid van Powershell-Module

**Type:** Nieuwe functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Access Control
 
Ondersteuning voor toepassingsproxy-cmdlets is nu in de Powershell-Module voor algemene beschikbaarheid. Dit vereist dat u op de hoogte te blijven van Powershell-modules - als u meer dan een jaar achter, sommige cmdlets worden mogelijk niet meer werken. 

Zie voor meer informatie, [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 systeemeigen clients worden ondersteund door een naadloze eenmalige aanmelding met een niet-interactieve protocol

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Gebruiker met behulp van Office 365 systeemeigen clients (versie 16.0.8730.xxxx en hoger) ophalen van een op de achtergrond aanmelding met naadloze eenmalige aanmelding. Deze ondersteuning wordt geleverd door de toevoeging een niet-interactieve protocol (WS-Trust) naar Azure AD.

Zie voor meer informatie, [hoe aanmelding op een systeemeigen client met naadloze eenmalige aanmelding werk?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Gebruikers krijgen een op de achtergrond aanmelding, met naadloze eenmalige aanmelding, als een toepassing aanmeldingsaanvragen naar Azure AD-tenant-eindpunten verzendt

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Gebruikers krijgen een op de achtergrond aanmelding, met naadloze eenmalige aanmelding, als een toepassing (bijvoorbeeld `https://contoso.sharepoint.com`) aanmeldingsaanvragen naar eindpunten van Azure AD-tenant - dat wil zeggen, verzendt `https://login.microsoftonline.com/contoso.com/<..>` of `https://login.microsoftonline.com/<tenant_ID>/<..>` : in plaats van algemene Azure AD-eindpunt (`https://login.microsoftonline.com/common/<...>`).

Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Moet slechts één Azure AD-URL, in plaats van twee URL's eerder, toevoegen aan gebruikers Intranet zone-instellingen voor de implementatie van naadloze eenmalige aanmelding

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Voor implementatie naadloze eenmalige aanmelding in uw gebruikers, moet u slechts één Azure AD-URL met het Intranet van de gebruikers van de zones met behulp van Groepsbeleid in Active Directory toevoegen: `https://autologon.microsoftazuread-sso.com`. Voorheen moesten klanten om toe te voegen twee URL's.

Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve Apps beschikbaar in Azure AD app-galerie

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden

In maart 2018, hebben we deze 15 nieuwe apps met Federatie ondersteuning aan onze app-galerie toegevoegd:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), -assistent door FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech digitale Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>PIM voor Azure-Resources is algemeen beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Als u van Azure AD Privileged Identity Management voor directory-rollen gebruikmaakt, kunt u nu gebruiken de tijdelijke toegang en de mogelijkheden van de toewijzing van PIM voor Azure-Resource-rollen, zoals abonnementen, resourcegroepen, virtuele Machines en een andere bron die wordt ondersteund door Azure Resource Manager. Meervoudige verificatie afdwingen bij het activeren van rollen Just-In-Time en plannen van activeringen in combinatie met windows goedgekeurde wijzigen. Deze versie wordt bovendien niet beschikbaar tijdens de openbare preview, met inbegrip van een bijgewerkte gebruikersinterface, goedkeuringswerkstromen en de mogelijkheid om verlopen rollen vernieuwen uit te breiden rollen verloopt binnenkort verbeteringen toegevoegd.

Zie voor meer informatie, [PIM voor Azure-resources (Preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Toevoegen van optionele Claims voor uw apps-tokens (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Uw Azure AD-app kunt nu aanvraag aangepaste of optioneel claims in JWTs of SAML-tokens.  Dit zijn de claims over de gebruiker of de tenant die niet zijn opgenomen in het token wordt vanwege beperkingen grootte of toepasselijkheid standaard.  Dit is momenteel in openbare preview voor Azure AD-apps op de v1.0 en v2.0-eindpunten.  Zie de documentatie voor meer informatie over welke claims kunnen worden toegevoegd en hoe u uw toepassingsmanifest om aan te vragen deze te bewerken.  

Zie voor meer informatie, [optioneel claims in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD biedt ondersteuning voor PKCE voor veiliger OAuth-stromen

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Azure AD-documenten zijn bijgewerkt voor ondersteuning voor PKCE, waardoor meer beveiligde communicatie tijdens de stroom voor het verlenen van OAuth 2.0-autorisatiecode opmerking.  Zowel S256 als tekst zonder opmaak code_challenges worden ondersteund op de v1.0 en v2.0-eindpunten. 

Zie voor meer informatie, [aanvragen van een autorisatiecode](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Ondersteuning voor het inrichten van alle gebruiker kenmerkwaarden beschikbaar in de Workday Get_Workers-API

**Type:** Nieuwe functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
De openbare preview van inkomende inrichten van Workday naar Active Directory en Azure AD nu ondersteunt de mogelijkheid om op te halen en de inrichting van alle waarden van kenmerken beschikbaar in de Workday Get_Workers-API. Dit biedt ondersteuning voor honderden extra standard wordt toegevoegd en aangepaste kenmerken die verzonden met de eerste versie van de Workday inbound provisioning connector.

Zie voor meer informatie: [De lijst met gebruikerskenmerken Workday aanpassen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Groepslidmaatschap wijzigen van dynamisch naar statisch, en vice versa

**Type:** Nieuwe functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking
 
Het is mogelijk om te wijzigen hoe lidmaatschap in een groep wordt beheerd. Dit is handig als u behouden van de dezelfde naam en de ID in het systeem wilt, zodat alle bestaande verwijzingen naar de groep nog steeds geldig zijn. het maken van een nieuwe groep zorgde deze verwijzingen worden bijgewerkt.
Het Azure AD-beheercentrum ter ondersteuning van deze functionaliteit is bijgewerkt. Klanten kunnen bestaande groepen nu converteren van een dynamisch lidmaatschap toegewezen lidmaatschap en vice versa. De bestaande PowerShell-cmdlets zijn ook nog steeds beschikbaar.

Zie voor meer informatie, [dynamisch lidmaatschap wijzigen in statisch en vice versa](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Verbeterde afmelding gedrag met naadloze eenmalige aanmelding

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Eerder, zelfs als gebruikers wordt expliciet afgemeld bij een toepassing die wordt beveiligd door Azure AD, ze zou worden automatisch aangemeld terug met behulp van naadloze eenmalige aanmelding als ze toegang probeert te krijgen van een Azure AD-toepassing opnieuw binnen het bedrijfsnetwerk vanaf hun apparaten toegevoegd aan een domein. Met deze wijziging, wordt Meld u af ondersteund.  Hiermee kunnen gebruikers kiezen voor de dezelfde of verschillende Azure AD-account aan te melden in met, in plaats van dat automatisch wordt aangemeld bij het gebruik van naadloze eenmalige aanmelding.

Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Application Proxy Connector versie 1.5.402.0 die zijn uitgebracht

**Type:** Gewijzigde functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming
 
De versie van deze connector wordt geleidelijk tot en met November wordt geïmplementeerd. Deze nieuwe versie van de connector bevat de volgende wijzigingen:

- De connector nu stelt domein niveau cookies in plaats daarvan subdomein niveau. Dit zorgt ervoor dat een soepeler SSO-ervaring en voorkomt u redundante verificatie wordt gevraagd.
- Ondersteuning voor gesegmenteerde codering aanvragen
- Verbeterde connector statuscontrole 
- Verschillende oplossingen voor problemen en stabiliteitsverbeteringen

Zie voor meer informatie, [over Azure AD Application Proxy connectors](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Februari 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Verbeterde navigatie voor het beheren van gebruikers en groepen

**Type:** Plan voor wijziging  
**Categorie van de service:** Mapbeheer  
**Product-mogelijkheid:** Directory

De navigatie-ervaring voor het beheren van gebruikers en groepen is gestroomlijnd. U kunt nu rechtstreeks aan de lijst met alle gebruikers, met eenvoudiger toegang tot de lijst met verwijderde gebruikers uit de directory-overzicht navigeren. U kunt ook rechtstreeks aan de lijst met alle groepen, eenvoudiger toegang tot de beheerinstellingen van de groep uit de directory-overzicht navigeren. En ook de overzichtspagina voor de directory, u kunt zoeken naar een gebruiker, groep, bedrijfstoepassing of app-registratie. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Beschikbaarheid van aanmeldingen en audit rapporten in Microsoft Azure uitgevoerd door 21Vianet (Azure China 21Vianet)

**Type:** Nieuwe functie  
**Categorie van de service:** Azure Stack  
**Product-mogelijkheid:** Controleren en rapporteren

Azure AD-activiteit log-rapporten zijn nu beschikbaar in Microsoft Azure, bediend door 21Vianet (Azure China 21Vianet) exemplaren. De volgende logboeken zijn opgenomen:

- **Activiteitenlogboeken aanmeldingen** -omvat alle aanmeldingen logboeken die zijn gekoppeld aan uw tenant.

- **Selfservice voor wachtwoord controleren logboeken** -omvat alle de SSPR-auditlogboeken.

- **Controle van Directory-beheer registreert** -omvat alle logboeken voor directory audit-gerelateerde zoals gebruiker management, App-beheer en anderen.

Met deze logboeken krijgt u inzicht in hoe uw omgeving presteert. Met de gegevens kunt u het volgende doen:

- Bepalen hoe uw apps en services worden gebruikt door uw gebruikers.

- Oplossen van problemen te voorkomen dat uw gebruikers van het ontvangen van hun werk afmaken.

Zie voor meer informatie over het gebruik van deze rapporten [Azure Active Directory-rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>'Rapportlezer'-rol (niet-beheerdersrol) gebruiken om Azure AD-activiteitenrapporten weer te geven

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Controleren en rapporteren

Als onderdeel van feedback van klanten om in te schakelen van niet-beheerdersrollen toegang hebben tot Azure AD-activiteit zich aanmeldt, is voorzien van de mogelijkheid voor gebruikers die zich in de rol "Lezer rapport" voor toegang tot aanmeldingen en Audit-activiteit in de Azure-portal, evenals met onze Graph API's. 

Voor meer informatie over het gebruik van deze rapporten, [Azure Active Directory-rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>EmployeeID-claim die beschikbaar zijn als gebruikerskenmerk en gebruikers-id

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding
 
U kunt configureren **EmployeeID** als de gebruikers-id en het gebruikerskenmerk voor lidgebruikers en B2B-gasten in SAML gebaseerde aanmelding toepassingen vanuit de gebruikersinterface van de Enterprise-toepassing.

Zie voor meer informatie, [in het SAML-token voor bedrijfstoepassingen in Azure Active Directory wordt uitgegeven claims aanpassen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Vereenvoudigd Toepassingsbeheer met jokertekens in Azure AD-toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Gebruikersverificatie
 
Implementatie van toepassingen te vereenvoudigen en reduceren uw administratieve overhead, ondersteunen we nu de mogelijkheid voor het publiceren van toepassingen die gebruikmaken van jokertekens. U kunt voor het publiceren van een jokertekentoepassing, volgt u de standard-toepassing publiceren stroom maar een jokerteken gebruiken in de interne en externe URL's.

Zie voor meer informatie, [Wildcard-toepassingen in de Azure Active Directory-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Er zijn nieuwe cmdlets voor de ondersteuning van de configuratie van de toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Platform

De nieuwste versie van de Preview van AzureAD PowerShell-module bevat nieuwe cmdlets waarmee klanten het configureren van Application Proxy-toepassingen met behulp van PowerShell.

De nieuwe-cmdlets zijn: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Er zijn nieuwe cmdlets voor de ondersteuning van de configuratie van groepen

**Type:** Nieuwe functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Platform

De nieuwste versie van de AzureAD PowerShell-module bevat cmdlets voor het beheren van groepen in Azure AD. Deze cmdlets waren eerder beschikbaar in de AzureADPreview-module en zijn nu toegevoegd aan de module AzureAD

De groep-cmdlets die nu release voor algemene beschikbaarheid zijn zijn: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Er is een nieuwe versie van Azure AD Connect beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** AD Sync  
**Product-mogelijkheid:** Platform
 
Azure AD Connect is de gewenste hulpprogramma voor het synchroniseren van gegevens tussen Azure AD en lokale gegevensbronnen, met inbegrip van Windows Server Active Directory en LDAP.

>[!Important]
>Deze versie introduceert schema en de synchronisatie regel wijzigingen. De Azure AD Connect-synchronisatieservice wordt een volledige importeerbewerking en een volledige synchronisatie stappen geactiveerd na een upgrade. Zie voor meer informatie over het wijzigen van dit gedrag [het uitstellen van volledige synchronisatie na de upgrade](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Deze release bevat de volgende updates en wijzigingen:

**Opgeloste problemen**

- Los tijdvenster op achtergrondtaken voor pagina partities filteren wanneer u overschakelt naar de volgende pagina.

- Een opgelost waardoor toegangsfout tijdens de aangepaste actie ConfigDB.

- Een opgelost om te herstellen van de time-out van de sql-verbinding.

- Een bug opgelost waarbij certificaten met jokertekens SAN controle van vereisten mislukt.

- Een opgelost die ervoor zorgt miiserver.exe crashes tijdens het exporteren van AAD-connector dat.

- Een bug opgelost waar een onjuist wachtwoord poging uitgevoerd veroorzaakt de AAD DC aangemeld verbinding maken met de wizard configuratie te wijzigen

**Nieuwe functies en verbeteringen**
 
- Toepassingstelemetrie - beheerders kunnen overschakelen naar deze klasse van gegevens in-of uitschakelen.

- Statusgegevens van Azure AD - beheerders moeten Ga naar de health-portal voor het beheren van de health-instellingen. Zodra de service-beleid is gewijzigd, wordt de agents lezen en deze toepassen.

- Apparaat terugschrijven Configuratieacties en een voortgangsbalk voor de initialisatie van de pagina toegevoegd.

- Verbeterde algemene diagnostische gegevens met HTML-rapport en de volledige gegevens verzamelen in een ZIP-tekst / HTML-rapport.

- Verbeterde betrouwbaarheid van Automatische upgrade en extra telemetrie om te controleren of de status van de server kan worden bepaald toegevoegd.

- Machtigingen die beschikbaar zijn beperkt tot beschermde accounts in AD Connector-account. Voor nieuwe installaties van de wizard beperkt de machtigingen die accounts met bevoegdheden hebben op de MSOL-account na het maken van de MSOL-account. De wijzigingen die invloed hebben op installaties van snelle en aangepaste installaties met account automatisch maken.

- Het installatieprogramma niet verplicht SA-bevoegdheden op een schone installatie van AADConnect gewijzigd.

- Nieuwe hulpprogramma voor het oplossen van synchronisatieproblemen met voor een specifiek object. Op dit moment controleert het hulpprogramma voor de volgende zaken:

    - UserPrincipalName komt niet overeen tussen gesynchroniseerde gebruikersobject en het gebruikersaccount in Azure AD-Tenant.
  
    - Als het object is uitgesloten voor synchronisatie vanwege domein filteren
  
    - Als het object is uitgesloten voor synchronisatie vanwege een organisatie-eenheid (OE) filteren

- Nieuwe hulpprogramma voor het synchroniseren van de huidige wachtwoord-hash die zijn opgeslagen in de on-premises Active Directory voor een specifiek gebruikersaccount. Het hulpprogramma is niet vereist voor een wachtwoordwijziging. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Toepassingen ondersteunende Intune App Protection-beleid toegevoegd voor gebruik met Azure AD-toepassing gebaseerde voorwaardelijke toegang

**Type:** Gewijzigde functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming

We hebt meer toepassingen die ondersteuning bieden voor voorwaardelijke toegang op basis van een toepassing toegevoegd. Nu krijgt u toegang tot Office 365 en andere Azure AD verbonden cloudapps met behulp van deze goedgekeurde client-apps.

De volgende toepassingen worden toegevoegd aan het einde van februari:

- Microsoft Power BI

- Microsoft Launcher

- Facturering van Microsoft

Zie voor meer informatie:

- [Vereiste voor goedgekeurde client-app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Bijgewerkte gebruiksvoorwaarden voor de mobiele ervaring 

**Type:** Gewijzigde functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving

Wanneer de gebruiksvoorwaarden worden weergegeven, kunt u nu klikken **hebt u problemen met weergeven? Klik hier**. Op deze koppeling te klikken, opent de gebruiksvoorwaarden zelf op uw apparaat. Ongeacht de tekengrootte in het document of het schermformaat van het apparaat, kunt u inzoomen en het document lezen. 

---
 
## <a name="january-2018"></a>Januari 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve Apps beschikbaar in Azure AD app-galerie 

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden

In januari 2018 zijn de volgende nieuwe apps met ondersteuning voor identiteitsfederatie in de app-galerie toegevoegd:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy beheersoftware](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk Directory, Gefedereerd en [kwaliteit NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Meld u aan met extra risico gedetecteerd

**Type:** Nieuwe functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming

Het inzicht u voor een gedetecteerde risicogebeurtenis krijgt is gekoppeld aan uw Azure AD-abonnement. Met de Azure AD Premium P2-editie krijgt u de meest gedetailleerde informatie over alle onderliggende detecties.

Met de Azure AD Premium P1-editie worden detecties die niet bij uw licentie inbegrepen zijn weergegeven als de risicogebeurtenis aanmelden met extra risico gedetecteerd.

Zie [Risicogebeurtenissen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) voor meer informatie.
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Office 365-toepassingen uit de toegangsdeelvensters van de eindgebruiker verbergen

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn apps  
**Product-mogelijkheid:** Eenmalige aanmelding

U kunt nu beter beheren hoe Office 365-toepassingen weergegeven op de toegangsdeelvensters van uw gebruikers via een nieuwe gebruikersinstelling. Deze optie is nuttig voor het verminderen van het aantal apps in de toegangsdeelvensters van een gebruiker als u liever alleen Office-apps weergeven in de Office-portal. De instelling bevindt zich in de **gebruikersinstellingen** en is met het label, **gebruikers zien alleen Office 365-apps in de Office 365-beheerportal**.

Zie voor meer informatie, [een toepassing gebruikerservaring in Azure Active Directory verbergen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Naadloze Meld u aan bij de apps die zijn ingeschakeld voor eenmalige aanmelding wachtwoord rechtstreeks vanuit de URL van app 

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn apps  
**Product-mogelijkheid:** Eenmalige aanmelding

De Apps in mijn browser-extensie is nu beschikbaar via een handig hulpmiddel waarmee u beschikt over de mijn Apps van eenmalige aanmelding over mogelijkheid als snelkoppeling in uw browser. Na het installeren ziet van de gebruiker een pictogram wafel in hun browser waarmee ze snel toegang tot apps. Gebruikers kunnen nu profiteren van:

- De mogelijkheid rechtstreeks aanmelden bij wachtwoord-eenmalige aanmelding op basis van apps van de aanmeldingspagina van de app
- Een app met behulp van de functie snelle zoekactie starten
- Snelkoppelingen naar onlangs gebruikte apps van de extensie
- De extensie is beschikbaar voor Microsoft Edge, Chrome en Firefox.
 
Zie voor meer informatie, [mijn Apps beveiligde aanmelding extensie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Beheer van Azure AD-ervaring in de klassieke Azure Portal is buiten gebruik gesteld

**Type:** Afgeschaft   
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Directory

Vanaf 8 januari 2018, het beheer van Azure AD-ervaring in de klassieke Azure portal is buiten gebruik gesteld. Dit heeft plaatsgevonden in combinatie met het buiten gebruik stellen van de klassieke Azure portal zelf. In de toekomst, moet u de [Azure AD-beheercentrum](https://aad.portal.azure.com) voor alle uw portal-gebaseerd beheer van Azure AD.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>De PhoneFactor-web-portal is buiten gebruik gesteld

**Type:** Afgeschaft  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Directory
 
Vanaf 8 januari 2018 de PhoneFactor-web-portal is buiten gebruik gesteld. Deze portal is gebruikt voor het beheer van MFA-server, maar deze functies zijn verplaatst naar de Azure-portal op portal.azure.com. 

De configuratie van MFA bevindt zich in: **Azure Active Directory \> MFA-Server**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Afschaffen van Azure AD-rapporten

**Type:** Afgeschaft  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli  


Met de algemene beschikbaarheid van de nieuwe Azure Active Directory Administration console en de nieuwe API's nu beschikbaar voor zowel de activiteit en de beveiliging van rapporten, het rapport API's onder '/ rapporten'-eindpunt is buiten gebruik gesteld vanaf het einde van 31 December 2017.

**Wat is er beschikbaar?**

Als onderdeel van de overgang naar de nieuwe beheerconsole, 2 nieuwe API's beschikbaar gemaakt voor het ophalen van Azure AD-activiteitenlogboeken. De nieuwe set API's biedt uitgebreidere filteren en sorteren van functionaliteit naast het leveren van uitgebreide controle- en aanmeldingsactiviteiten. De gegevens die eerder beschikbaar tot en met de beveiligingsrapporten kan nu worden geopend via de Identity Protection risicogebeurtenissen API in Microsoft Graph.

Zie voor meer informatie:

- [Aan de slag met de Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Aan de slag met Azure Active Directory Identity Protection en Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>December 2017

### <a name="terms-of-use-in-the-access-panel"></a>Gebruiksvoorwaarden in het toegangsvenster

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 
U kunt nu gaat u naar het toegangsvenster en weergeven van de gebruiksvoorwaarden die u eerder hebt geaccepteerd.

Volg deze stappen:

1. Ga naar de [MyApps-portal](https://myapps.microsoft.com), en meld u aan.

2. Selecteer uw naam in de rechterbovenhoek en selecteer vervolgens **profiel** in de lijst. 

3. Op uw **profiel**, selecteer **gebruiksvoorwaarden controleren**. 

4. Nu kunt u de gebruiksvoorwaarden bekijken die u hebt geaccepteerd. 

Zie voor meer informatie de [Azure AD gebruiksrechtovereenkomst functie gebruiken (preview)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Nieuwe Azure Active Directory-aanmeldingservaring

**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Gebruikersverificatie
 
De Azure AD en Microsoft-account-identiteitssysteem gebruikersinterfaces zijn opnieuw ontworpen zodat ze beschikken over een consistent uiterlijk geven. Bovendien verzamelt de Azure AD-aanmeldingspagina de naam van de gebruiker eerst, gevolgd door de referentie in een tweede scherm.

Zie voor meer informatie, [de nieuwe Azure Active Directory-aanmeldingservaring is nu in openbare preview-versie](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Minder aanmeldingsprompts: Een nieuwe "aangemeld blijven'-ervaring voor aanmelding bij Azure AD

**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Gebruikersverificatie
 
De **aangemeld blijven** selectievakje op de aanmeldingspagina van Azure AD is vervangen door een nieuwe opdrachtprompt die wordt weergegeven nadat u kunnen verifiëren. 

Als u reageren **Ja** deze prompt, de service biedt u een permanente vernieuwingstoken. Dit gedrag is hetzelfde als wanneer u hebt geselecteerd de **aangemeld blijven** het selectievakje in de oude ervaring. Voor federatieve tenants ziet dit bericht nadat u met de federatieve service verifiëren.

Zie voor meer informatie, [minder prompts voor aanmelden: De nieuwe 'aangemeld blijven'-ervaring voor Azure AD is beschikbaar als preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Configuratie om te vereisen dat de gebruiksvoorwaarden worden uitgebreid alvorens deze te accepteren toevoegen

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 
Een optie voor beheerders moeten hun gebruikers om uit te breiden de gebruiksvoorwaarden voorafgaand aan de voorwaarden te accepteren.

Selecteer een **op** of **uit** gebruikers moeten de gebruiksvoorwaarden uitbreiden. De **op** instelling verplicht gebruikers om weer te geven van de gebruiksvoorwaarden alvorens deze te accepteren.

Zie voor meer informatie de [Azure AD gebruiksrechtovereenkomst functie gebruiken (preview)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Scoped activering voor in aanmerking komende roltoewijzingen

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Roltoewijzingen in aanmerking komende Azure-resource met minder autonomie dan de oorspronkelijke toewijzing standaardwaarden activeren kunt u binnen het bereik activering. Een voorbeeld is als u als eigenaar van een abonnement in uw tenant toegewezen bent. Met een bereik-activering, kunt u de rol van eigenaar voor maximaal vijf resources die zich in het abonnement (zoals resourcegroepen en virtuele machines) activeren. Bereik van de activering kan de mogelijkheid van de uitvoering van ongewenste wijzigingen om essentiële Azure-resources te beperken.

Zie voor meer informatie, [wat is Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Nieuwe federatieve apps in de Azure AD-app-galerie

**Type:** Nieuwe functie  
**Categorie van de service:** Enterprise-apps  
**Product-mogelijkheid:** Integratie met toepassing van derden

In December 2017, hebben we de ondersteuning van deze nieuwe apps met Federatie naar onze app-galerie toegevoegd:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager [EFI digitale winkel](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [INSTALLATIEKOPIE werkt](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD-integratie](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO voor Bamboe resolutie GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO voor Bitbucket resolutie GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD-integratie.

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Goedkeuringswerkstromen voor Azure AD-maprollen

**Type:** Gewijzigde functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Werkstroom voor goedkeuring voor Azure AD-maprollen is algemeen beschikbaar.

Met de werkstroom voor goedkeuring, bevoegde rol beheerders kunnen vereisen dat leden van een in aanmerking komende-rol op aanvraag voor rolactivering voordat ze de bevoorrechte rol kunnen gebruiken. Meerdere gebruikers en groepen kunnen worden gedelegeerd goedkeuring verantwoordelijkheden. Leden van een in aanmerking komende rol ontvangen meldingen wanneer goedkeuring is voltooid en hun rol actief is.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Pass through-verificatie: Skype voor bedrijven-ondersteuning

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie

Nu Pass through-verificatie ondersteunt gebruikersaanmeldingen tot Skype voor bedrijven-clienttoepassingen die ondersteuning bieden voor moderne verificatie, waaronder online en hybride topologieën. 

Zie voor meer informatie, [Skype voor bedrijven-topologieën met moderne verificatie ondersteund](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Updates voor Azure AD Privileged Identity Management voor Azure RBAC (preview)

**Type:** Gewijzigde functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Met het vernieuwen van de openbare preview van Azure AD Privileged Identity Management (PIM) voor op rollen gebaseerd toegangsbeheer (RBAC) kunt u nu:

* Just Enough Administration gebruiken.
* Goedkeuring van de resource-rollen activeren.
* Plan een toekomstige activering van een rol die moet worden goedgekeurd voor beide Azure AD en Azure RBAC-rollen.
 
Zie voor meer informatie, [Privileged Identity Management voor Azure-resources (preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>November 2017
 
### <a name="access-control-service-retirement"></a>Access Control service buiten gebruik stellen

**Type:** Plan voor wijziging  
**Categorie van de service:** Access Control-service  
**Product-mogelijkheid:** Access Control-service 

Azure Active Directory Access Control (ook wel bekend als de Access Control service) wordt in latere 2018 beëindigd. Meer informatie, waaronder gedetailleerde plan en hulp bij de migratie op hoog niveau zijn, beschikbaar in de komende weken. U kunt opmerkingen achterlaten op deze pagina met vragen over de Access Control-service en een teamlid zal deze beantwoorden.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Browsertoegang beperken tot de Intune Managed Browser 

**Type:** Plan voor wijziging  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

U kunt browsertoegang tot Office 365 en andere Azure AD verbonden cloud-apps beperken met behulp van de Intune Managed Browser als een goedgekeurde app. 

U kunt nu de volgende voorwaarde voor voorwaardelijke toegang op basis van een toepassing configureren:

**Client-apps:** Browser

**Wat is het effect van de wijziging?**

Vandaag de dag toegang geblokkeerd wanneer u deze voorwaarde gebruiken. Wanneer de Preview-versie beschikbaar is, wordt alle toegang tot het gebruik van de toepassing van de beheerde browser vereist. 

Zoeken naar deze mogelijkheid en meer informatie in toekomstige blogs en release-opmerkingen. 

Zie voor meer informatie, [voorwaardelijke toegang in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde apps voor op Azure AD-app gebaseerde voorwaardelijke toegang

**Type:** Plan voor wijziging  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

De volgende apps zijn in de lijst met [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

Zie voor meer informatie:

- [Vereiste voor goedgekeurde client-app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Gebruiksvoorwaarden van ondersteuning voor meerdere talen

**Type:** Nieuwe functie    
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving

Beheerders kunnen nu nieuwe gebruiksvoorwaarden die meerdere PDF-documenten bevatten maken. U kunt deze PDF-documenten met een bijbehorende taal kunt labelen. Gebruikers worden weergegeven van het PDF-bestand met de overeenkomende taal op basis van hun voorkeuren. Als er geen overeenkomst is, wordt de standaardtaal weergegeven.

---
 
### <a name="real-time-password-writeback-client-status"></a>Status van de client realtime wachtwoord terugschrijven

**Type:** Nieuwe functie  
**Categorie van de service:** Self-service voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie

U kunt nu de status van uw on-premises wachtwoord-Write-backclient bekijken. Deze optie is beschikbaar in de **On-premises integratie** sectie van de [wachtwoordherstel](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) pagina. 

Als er problemen met de verbinding met uw on-premises Write-backclient zijn, ziet u een foutmelding krijgen dat beschikt u over:

- Informatie over waarom u geen verbinding met uw on-premises Write-backclient.
- Een koppeling naar de documentatie die u helpt bij het oplossen van het probleem. 

Zie voor meer informatie, [on-premises integratie](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD op Apps gebaseerde voorwaardelijke toegang 
 
**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

U kunt toegang nu beperken tot Office 365 en andere Azure AD verbonden cloud-apps naar [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) die ondersteuning bieden voor Intune beveiligingsbeleid voor apps met behulp van [voorwaardelijke toegang van Azure AD-app op basis van](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Intune beveiligingsbeleid voor apps worden gebruikt voor het configureren en beveiligen van bedrijfsgegevens op deze clienttoepassingen.

Door te combineren [op basis van app](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) met [apparaatgebaseerde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) beleid voor voorwaardelijke toegang hebt u de flexibiliteit om gegevens voor persoonlijke en bedrijfsapparaten te beveiligen.

De volgende voorwaarden en besturingselementen zijn nu beschikbaar voor gebruik met Apps gebaseerde voorwaardelijke toegang:

**Ondersteund platform voorwaarde**

- iOS
- Android

**Voorwaarde voor client-apps**

- Mobiele apps en bureaubladclients

**Toegangsbeheer**

- Goedgekeurde client-apps vereisen

Zie voor meer informatie, [voorwaardelijke toegang van Azure AD-app op basis van](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure AD-apparaten in de Azure-portal beheren

**Type:** Nieuwe functie  
**Categorie van de service:** Device Registration service en beheer  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

Nu kunt u uw apparaten die zijn verbonden met Azure AD en de activiteiten met betrekking tot apparaat op één plek. Er is een nieuwe beheerervaring voor het beheren van uw apparaat-id's en instellingen in de Azure portal. In deze release kunt u het volgende doen:

- Bekijk al uw apparaten die beschikbaar voor voorwaardelijke toegang in Azure AD zijn.
- Eigenschappen, waaronder uw hybride Azure AD gekoppelde apparaten.
- BitLocker-sleutels voor uw Azure AD join-apparaten zoeken, beheren van uw apparaat met Intune en meer.
- Apparaat met betrekking tot Azure AD-instellingen beheren.

Zie voor meer informatie, [apparaten beheren met behulp van de Azure-portal](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Ondersteuning voor macOS als een platform voor apparaten voor voorwaardelijke toegang van Azure AD 

**Type:** Nieuwe functie    
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming 

U kunt nu opnemen of uitsluiten macOS als een voorwaarde voor het platform van apparaat in uw Azure AD-beleid voor voorwaardelijke toegang. Met de toevoeging van macOS om de ondersteunde apparaatplatformen, kunt u het volgende doen:

- **Registreren en macOS-apparaten beheren met behulp van Intune.** Net als bij andere platformen, zoals iOS en Android, een bedrijf portal toepassing beschikbaar is voor macOS geïntegreerde inschrijvingen doen. De nieuwe bedrijfsportal-app voor macOS kunt u een apparaat met Intune inschrijven en Registreer het bij Azure AD.
- **Zorg ervoor dat op macOS-apparaten voldoen aan uw organisatie-nalevingsbeleid dat is gedefinieerd in Intune.** In Intune in Azure portal kunt kunt u nu instellen nalevingsbeleid voor macOS-apparaten. 
- **Toegang tot toepassingen in Azure AD om alleen compatibele macOS-apparaten te beperken.** Voorwaardelijk beleid ontwerpen heeft macOS als een afzonderlijk apparaat platform-optie. Nu kunt u macOS-specifieke voorwaardelijk toegangsbeleid voor de betreffende toepassing instellen in Azure maken.

Zie voor meer informatie:

- [Een apparaatnalevingsbeleid maken voor macOS-apparaten in Intune](https://aka.ms/macoscompliancepolicy)
- [Voorwaardelijke toegang in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Network Policy Server-extensie voor Azure multi-factor Authentication 

**Type:** Nieuwe functie    
**Categorie van de service:**  Multi-Factor Authentication  
**Product-mogelijkheid:** Gebruikersverificatie

De Network Policy Server-extensie voor Azure multi-factor Authentication wordt cloud-gebaseerde multi-factor Authentication-mogelijkheden toegevoegd aan uw infrastructuur voor verificatie met behulp van uw bestaande servers. Met de extensie voor Network Policy Server, kunt u telefonische oproepen, SMS-bericht of verificatie via de telefoon-app kunt toevoegen aan uw bestaande verificatiestroom. U hoeft niet te installeren, configureren en onderhouden van nieuwe servers. 

Deze extensie is bedoeld voor organisaties die verbindingen virtual private network beveiligen willen zonder de Azure multi-factor Authentication-Server te implementeren. De Network Policy Server-extensie als een adapter tussen RADIUS- en cloud-gebaseerde Azure multi-factor Authentication fungeert voor een tweede factor-verificatie voor federatieve of gesynchroniseerde gebruikers.

Zie voor meer informatie, [uw bestaande infrastructuur van de Network Policy Server integreren met Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Herstellen of verwijderde gebruikers permanent verwijderen

**Type:** Nieuwe functie    
**Categorie van de service:** Gebruikersbeheer  
**Product-mogelijkheid:** Directory 

In het beheercentrum van Azure AD kunt u nu:

- Een verwijderde gebruiker herstellen. 
- Een gebruiker permanent verwijdert.

**Om het te proberen:**

1. Selecteer in het Azure AD-beheercentrum [alle gebruikers](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) in de **beheren** sectie. 

2. Uit de **weergeven** in de lijst met **onlangs verwijderde gebruikers**. 

3. Selecteer een of meer onlangs verwijderde gebruikers, en vervolgens ofwel herstellen of deze definitief verwijderen.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde apps voor op Azure AD-app gebaseerde voorwaardelijke toegang
 
**Type:** Gewijzigde functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

De volgende apps zijn toegevoegd aan de lijst met [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 

Zie voor meer informatie:

- [Vereiste voor goedgekeurde client-app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Gebruik 'Of' tussen de besturingselementen in een beleid voor voorwaardelijke toegang 

**Type:** Gewijzigde functie    
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming
 
Nu kunt u gebruiken ' of ' (een van de geselecteerde besturingselementen vereisen) voor besturingselementen voor voorwaardelijke toegang. U kunt deze functie gebruiken om beleid te maken met 'of' tussen de besturingselementen voor toegang. Bijvoorbeeld, kunt u deze functie om een beleid waarvoor een gebruiker zich aanmeldt met behulp van multi-factor Authentication 'of' op een compatibel apparaat te maken.

Zie voor meer informatie, [besturingselementen in Azure AD voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-events"></a>Aggregatie van realtime risicogebeurtenissen

**Type:** Gewijzigde functie    
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

In Azure AD Identity Protection worden nu alle realtime risicogebeurtenissen die afkomstig van hetzelfde IP-adres op een bepaalde dag zijn samengevoegd voor elk type risicogebeurtenis. Deze wijziging beperkt het volume van de risicogebeurtenissen die worden weergegeven zonder wijziging in de gebruikersbeveiliging.

De onderliggende realtime detectie werkt telkens wanneer de gebruiker zich aanmeldt. Als u een aanmeldingsrisico beveiligingsbeleid instellen voor multi-factor Authentication of de toegang blokkeert hebt, is het nog steeds geactiveerd tijdens elke riskante aanmelding.
 
---
 
## <a name="october-2017"></a>Oktober 2017

### <a name="deprecate-azure-ad-reports"></a>Afschaffen van Azure AD-rapporten

**Type:** Plan voor wijziging  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli  

De Azure-portal beschikt u over:

- Een nieuwe Azure AD-beheerconsole.
- Nieuwe API's voor rapporten van activiteit en beveiliging.
 
Vanwege deze nieuwe mogelijkheden, het rapport API's onder het eindpunt/Reports zijn buiten gebruik gesteld op 10 December 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Detectie van het veld voor automatische aanmelding

**Type:** Vast   
**Categorie van de service:** Mijn apps  
**Product-mogelijkheid:** Eenmalige aanmelding  

Azure AD biedt ondersteuning voor detectie van het veld voor automatische aanmelding voor toepassingen die een gebruiker HTML-veld naam en het wachtwoord weergeven. Deze stappen worden beschreven in [automatisch vastleggen aanmeldingsvelden voor een toepassing](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). U vindt deze mogelijkheid door toe te voegen een *niet in de galerij* toepassing voor de **bedrijfstoepassingen** pagina in de [Azure-portal](https://aad.portal.azure.com). Bovendien kunt u de **Single Sign-on** modus op deze nieuwe toepassing **wachtwoord gebaseerde Single Sign-on**, Geef een web-URL en sla vervolgens op de pagina.
 
Deze functionaliteit is vanwege een serviceprobleem tijdelijk uitgeschakeld. Het probleem is opgelost en de detectie van het veld voor automatische aanmelding weer beschikbaar is.

---

### <a name="new-multi-factor-authentication-features"></a>Nieuwe functies van multi-factor Authentication

**Type:** Nieuwe functie  
**Categorie van de service:** Multi-Factor Authentication  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming  

Multi-factor authentication (MFA) is een essentieel onderdeel van uw organisatie te beschermen. Als u meer geavanceerde referenties en de ervaring biedt een naadloze ervaring, zijn de volgende functies toegevoegd: 

- Resultaten van de basis van meerdere factoren uitdaging zijn rechtstreeks geïntegreerd in het Azure AD aanmelden rapport, waaronder programmatische toegang tot de MFA-resultaten.
- De configuratie van MFA dieper is geïntegreerd in de configuratie van de Azure AD-ervaring in Azure portal.

Met deze preview-versie, MFA-beheer en rapportage een geïntegreerde deel uitmaken van de belangrijkste Azure AD-configuratie-ervaring. U kunt nu de portal beheerfunctionaliteit MFA in de Azure AD-ervaring beheren.

Zie voor meer informatie, [verwijzing voor het melden van MFA in de Azure-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Gebruiksvoorwaarden

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving  

U kunt Azure AD gebruiksrechtovereenkomst gebruiken om gegevens te presenteren, zoals relevante disclaimers voor juridische vereisten of nalevingsvereisten voor gebruikers.

U kunt Azure AD gebruiksrechtovereenkomst gebruiken in de volgende scenario's:

- Algemene gebruiksrechtovereenkomst voor alle gebruikers in uw organisatie
- Specifieke gebruiksrechtovereenkomst op basis van kenmerken van een gebruiker (bijvoorbeeld artsen versus verpleegkundigen) of binnenlandse versus werknemers, uitgevoerd door dynamische groepen
- Specifieke gebruiksrechtovereenkomst gebruiken voor toegang tot indrukwekkende business-apps, zoals Salesforce

Zie voor meer informatie, [Azure AD gebruiksrechtovereenkomst](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="enhancements-to-privileged-identity-management"></a>Verbeteringen voor Privileged Identity Management

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management  

U kunt met Azure AD Privileged Identity Management kunt beheren, controleren en toegang tot Azure-resources (preview) binnen uw organisatie te controleren:

- Abonnementen
- Resourcegroepen
- Virtuele machines 

Alle resources in Azure portal die gebruikmaken van de functionaliteit voor Azure RBAC kunnen profiteren van alle beveiligings- en functionaliteit voor het beheer die Azure AD Privileged Identity Management te bieden heeft.

Zie voor meer informatie, [Privileged Identity Management voor Azure-resources](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Toegangsbeoordelingen

**Type:** Nieuwe functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Naleving  

Organisaties kunnen toegangsbeoordelingen (preview) gebruiken op efficiënte wijze groepslidmaatschappen en toegang tot bedrijfstoepassingen beheren: 

- U kunt toegang voor gastgebruikers opnieuw certificeren door gebruik te maken van toegangsbeoordelingen van hun toegang tot toepassingen en lidmaatschappen van groepen. Revisoren kunnen efficiënt kunnen beslissen of het om toe te staan van gasten toegang op basis van de inzichten geleverd door de toegangsbeoordelingen blijvende.
- Met toegangsbeoordelingen kunt u de toegang van werknemers voor toepassingen en groepslidmaatschappen opnieuw certificeren.

U kunt de controles van toegangsbeoordelingen verzamelen in programma's die relevant zijn voor uw organisatie, om beoordelingen bij te houden voor naleving of risicogevoelige toepassingen.

Zie voor meer informatie, [Azure AD-toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Toepassingen van derden van mijn Apps en het startprogramma voor Office 365 verbergen

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn apps  
**Product-mogelijkheid:** Eenmalige aanmelding  

Nu kunt u beter apps beheren die worden weergegeven op uw gebruikers portals door middel van een nieuwe **app verbergen** eigenschap. U kunt apps om in gevallen waar de app-tegels voor back-end-services of dubbele tegels en overbodige items gebruikers-app voor ruimtevaartuigen weergegeven te verbergen. De wisselknop is in de **eigenschappen** sectie van de app van derden en heet **zichtbaar voor gebruiker?** U kunt ook een app via een programma via PowerShell verbergen. 

Zie voor meer informatie, [verbergen van een toepassing van derden uit de ervaring van een gebruiker in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Wat is er beschikbaar?**

 Als onderdeel van de overgang naar de nieuwe beheerconsole, twee nieuwe API's voor het ophalen van Azure AD-activiteit logboeken beschikbaar zijn. De nieuwe set API's biedt uitgebreidere filteren en sorteren van functionaliteit naast het leveren van uitgebreide controle- en aanmeldingsactiviteiten. De gegevens die eerder beschikbaar tot en met de beveiligingsrapporten nu zijn toegankelijk via de API voor Identity Protection risico gebeurtenissen in Microsoft Graph.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-identity-manager"></a>Hotfix voor Identity Manager

**Type:** Gewijzigde functie  
**Categorie van de service:** Identity Manager  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli  

Een hotfix totaliseren-pakket (build 4.4.1642.0) is beschikbaar vanaf 25 September 2017 voor Identity Manager 2016 Service Pack 1. Dit pakket totaliseren:

- Lost problemen op en voegt verbeteringen toe.
- Is een cumulatieve update die wordt vervangen door alle Identity Manager 2016 Service Pack 1-updates maximaal build 4.4.1459.0 voor Identity Manager 2016. 
- Vereist dat u beschikt over Identity Manager 2016 4.4.1302.0 bouwen. 

Zie voor meer informatie, [hotfixpakket (build 4.4.1642.0) is beschikbaar voor Identity Manager 2016 Service Pack 1](https://support.microsoft.com/help/4021562). 

---