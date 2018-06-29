---
title: Wat is nieuw? Release-opmerkingen voor Azure AD | Microsoft Docs
description: Ontdek wat er nieuw bij Azure Active Directory (Azure AD), zoals de meest recente release-opmerkingen, bekende problemen, oplossingen voor problemen, afgeschafte functionaliteit en toekomstige wijzigingen.
services: active-directory
author: eross-msft
manager: mtillman
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: d86a2b702eac4fc976fd59ca24fd7122a76005b4
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061674"
---
# <a name="whats-new-in-azure-active-directory"></a>Wat is er nieuw in Azure Active Directory?

> De hoogte blijven van wat is er nieuw in Azure Active Directory (Azure AD) met een abonnement op de [ ![RSS](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us) [feed](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us).

Azure AD ontvangt verbeteringen voortdurend. Om te blijven up-to-date blijven met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

-   De meest recente versies
-   Bekende problemen
-   Opgeloste fouten
-   Afgeschafte functionaliteit
-   Plannen voor wijzigingen

Deze pagina maandelijks wordt bijgewerkt, dus regelmatig bezoeken.

## <a name="june-2018"></a>2018 juni

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Wijzigen van de aankondiging: beveiliging oplossing voor de gegevensstroom gedelegeerd autorisatie voor apps met behulp van Azure AD activiteit logboeken API

**Type:** wijzigingen  
**Servicecategorie:** bewaking en rapportage  
**Product mogelijkheid:** rapportage

Als gevolg van de naleving van onze sterkere beveiliging, we een wijziging aanbrengen in de machtigingen voor apps die gebruikmaken van een stroom gedelegeerd autorisatie voor toegang tot hadden [Azure AD activiteit logboeken-API's](https://aka.ms/aadreportsapi). Deze wijziging wordt uitgevoerd door **26 juni 2018**.

Als een van uw apps in Azure AD activiteit Log-API's gebruikt, volg deze stappen om te controleren of dat de app niet verbreken nadat de wijziging gebeurt.

**Uw app-machtigingen bijwerken**

1. Aanmelden bij de Azure portal, selecteer **Azure Active Directory**, en selecteer vervolgens **App registraties**.
2. Selecteer de app die gebruikmaakt van de Azure AD activiteit logboeken API, selecteer **instellingen**, selecteer **vereist machtigingen**, en selecteer vervolgens de **Windows Azure Active Directory** API.
3. In de **overgedragen machtigingen** gebied van de **toegang** blade, schakel het selectievakje naast **lezen directory** gegevens en selecteer vervolgens **Opslaan**.
4. Selecteer **machtigingen verlenen**, en selecteer vervolgens **Ja**.
    
    >[!Note]
    >U moet een globale beheerder machtigen om de app.

Zie voor meer informatie de [machtigingen verlenen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) gebied van de vereisten voor toegang tot het artikel voor rapportage-API van Azure AD.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>Verbinding maken met Azure AD-services voor PCI DSS naleving TLS-instellingen configureren

**Type:** nieuwe functie  
**Servicecategorie:** nieuwe functie  
**Product mogelijkheid:** Platform

Transport Layer Security (TLS) is een protocol dat privacy en gegevensintegriteit tussen de twee communicerende toepassingen biedt en is het meest geïmplementeerde beveiligingsprotocol op dit moment gebruikt.

De [PCI Security Standards Council](https://www.pcisecuritystandards.org/) heeft vastgesteld dat oudere versies van TLS- en Secure Sockets Layer (SSL) moeten worden uitgeschakeld voor het inschakelen van nieuwe en veiliger app protocollen, met het starten van de naleving op **30 juni 2018**. Deze wijziging betekent dat als u verbinding met Azure AD-services maken en PCI DSS-compatibiliteit vereist, moet u TLS 1.0 uitschakelen. Er zijn meerdere versies van TLS beschikbaar, maar TLS 1.2 is de meest recente versie die beschikbaar zijn voor Azure Active Directory-Services. Het is raadzaam verplaatsen rechtstreeks naar TLS 1.2 voor combinaties van zowel de client of de server en de browser of de server.

Verouderde browsers ondersteunen mogelijk niet nieuwere versies van TLS, zoals het TLS 1.2. Welke versies van TLS worden ondersteund door uw browser, Ga naar de [Qualys SSL Labs](https://www.ssllabs.com/) site en op **testen van uw browser**. We raden u upgraden naar de nieuwste versie van uw webbrowser en bij voorkeur inschakelen alleen TLS 1.2.

**TLS 1.2 inschakelen door de browser**

- **Microsoft Edge en Internet Explorer (beide zijn ingesteld met behulp van Internet Explorer)**

    1. Open Internet Explorer, selecteer **extra** > **Internetopties** > **Geavanceerd**.
    2. In de **beveiliging** gebied, selecteer **gebruik van TLS 1.2**, en selecteer vervolgens **OK**.
    3. Alle browservensters sluiten en opnieuw starten van Internet Explorer. 

- **Google Chrome**

    1. Open Google Chrome, type *chrome://settings/* in de adresbalk en druk op **Enter**.
    2. Vouw de **Geavanceerd** opties, gaat u naar de **System** gebied en selecteer **proxy-instellingen openen**.
    3. In de **Interneteigenschappen** de optie de **Geavanceerd** tabblad, gaat u naar de **beveiliging** gebied, selecteer **gebruik van TLS 1.2**, en selecteer vervolgens  **OK**.
    4. Alle browservensters sluiten en opnieuw starten van Google Chrome.

- **Mozilla Firefox**

    1. Open Firefox, type *over: config* in de adresbalk en druk vervolgens op **Enter**.
    2. Zoeken naar de term *TLS*, en selecteer vervolgens de **security.tls.version.max** vermelding.
    3. De waarde instelt op **3** om af te dwingen van de browser om te gebruiken om versie TLS 1.2 en selecteer vervolgens **OK**.

        >[!NOTE]
        >Firefox versie 60,0 biedt ondersteuning voor TLS 1.3, zodat u kunt ook de security.tls.version.max waarde instellen op **4**.

    4. Alle browservensters sluiten en opnieuw starten van Mozilla Firefox.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - juni 2018

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** integratie van derden
 
In juni 2018 toegevoegd dat deze 15 nieuwe apps met Federatie aan onze app-galerie kunnen ondersteunen:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [vereffenen muziek](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1.1 Token LOB-App ingeschakeld](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ Back-up eindpunt](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh netwerken](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho een](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [ SharePoint on-premises](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [voorzien CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Zie voor meer informatie over de apps [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de galerie van Azure AD app [uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD-wachtwoordbeveiliging is beschikbaar in public preview

**Type:** nieuwe functie  
**Servicecategorie:** Identity Protection  
**Product mogelijkheid:** gebruikersverificatie

Azure AD-wachtwoord Protection gebruiken om u te helpen voorkomen gemakkelijk geraden wachtwoorden van uw omgeving. Deze wachtwoorden elimineren helpt omdat het risico van inbreuk op basis van een wachtwoord spuitnevel type aanval.

In het bijzonder kunt wachtwoordbeveiliging van Azure AD u:

- Beveiligen van uw organisatie accounts in beide Azure AD en Windows Server Active Directory (AD). 
- Uw gebruikers van het gebruik van wachtwoorden voor een lijst met meer dan 500 van de meest gebruikte wachtwoorden en meer dan 1 miljoen teken vervanging variaties van deze wachtwoorden stopt.
- Beheren van Azure AD wachtwoordbeveiliging vanaf één locatie in de Azure AD-portal voor beide Azure AD en on-premises Windows Server AD.

Zie voor meer informatie over Azure AD-wachtwoordbeveiliging [elimineren onjuiste wachtwoorden in uw organisatie](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Nieuwe 'alle gasten' voorwaardelijke toegang beleidssjabloon gemaakt tijdens het maken van de gebruiksrechtovereenkomst (ToU)

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** Governance

Tijdens het maken van uw gebruiksvoorwaarden (ToU), wordt ook een nieuwe beleidssjabloon van voorwaardelijke toegang gemaakt voor 'alle gasten' en 'alle apps'. Deze nieuwe beleidssjabloon geldt de zojuist gemaakte gebruiksvoorwaarden, stroomlijnen het maken en het afdwingen van proces voor gasten.

Zie voor meer informatie [Azure Active Directory-voorwaarden van de functie gebruiken](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-tou).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Nieuwe sjabloon die 'aangepaste' voorwaardelijk beleid gemaakt tijdens het maken van de gebruiksrechtovereenkomst (ToU)

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** Governance

Tijdens het maken van uw gebruiksvoorwaarden (ToU), wordt ook een nieuwe 'aangepaste' voorwaardelijk beleidssjabloon gemaakt. Deze nieuwe beleidssjabloon kunt u de gebruiksvoorwaarden en onmiddellijk gaat u naar de blade van voorwaardelijk beleid maken, zonder handmatig Navigeer via de portal.

Zie voor meer informatie [Azure Active Directory-voorwaarden van de functie gebruiken](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-tou).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Nieuwe en uitgebreide richtlijnen over het implementeren van Azure multi-factor Authentication

**Type:** nieuwe functie  
**Servicecategorie:** andere  
**Product mogelijkheid:** identiteit beveiliging en bescherming
 
We hebt nieuwe Stapsgewijze instructies over het implementeren van Azure multi-factor Authentication (MFA) in uw organisatie uitgebracht.

Als u wilt weergeven in de Implementatiehandleiding voor MFA, gaat u naar de [identiteit implementatiehandleidingen](https://aka.ms/DeploymentPlans) opslagplaats op GitHub. Als u feedback over de implementatiehandleidingen, wilt gebruiken de [implementatie plannen feedbackformulier](https:aka.ms/deploymentplanfeedback). Als u vragen over de implementatiehandleidingen hebt, contact met ons op [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD app management-rollen openbare preview zijn overgedragen

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps **Product mogelijkheid:** toegangsbeheer

Beheerders kunt nu app beheertaken delegeren zonder de rol globale beheerder toe te wijzen. De nieuwe functies en mogelijkheden zijn:

- **Nieuwe standaard Azure AD-beheerdersrollen:**

    - **De beheerder van de toepassing.** De mogelijkheid voor het beheren van alle aspecten van alle apps, waaronder registratie, SSO-instellingen, app-toewijzingen en licentieverlening, App-proxy-instellingen en toestemming verleent (behalve aan Azure AD-resources).

    - **Beheerder van de cloud-toepassing.** Verleent alle van de mogelijkheden van de beheerder van de toepassing, met uitzondering van toepassingsproxy omdat het niet mogelijk om lokale toegang.

    - **De ontwikkelaar van de toepassing.** Geeft de mogelijkheid voor het maken van app-registraties, zelfs als de **toestaan dat gebruikers om apps te registreren** optie is uitgeschakeld.

- **Eigendom (per-app-registratie en per enterprise app instelt, vergelijkbaar met het proces voor het eigendom van groep:**
 
    - **De eigenaar van de registratie van de App.** Geeft de mogelijkheid voor het beheren van alle aspecten van in eigendom van app-registratie, met inbegrip van het app-manifest en het toevoegen van extra eigenaren.

    - **Enterprise-App-eigenaar.** De mogelijkheid voor het beheren van veel aspecten van in eigendom van zakelijke apps, waaronder instellingen voor eenmalige aanmelding, app-toewijzingen en toestemming verleent (behalve aan Azure AD-resources).

Zie voor meer informatie over openbare preview, de [Azure AD overgedragen Toepassingsbeheer rollen met openbare preview zijn!](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) blog. Zie voor meer informatie over de rollen en machtigingen [beheerdersrollen toewijzen in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Mei 2018

### <a name="expressroute-support-changes"></a>Wijzigingen van ExpressRoute-ondersteuning

**Type:** wijzigingen  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** Platform  

Software als een Service aanbieden, zoals Azure Active Directory (Azure AD) zijn ontworpen voor best door te gaan rechtstreeks via Internet, zonder dat ExpressRoute of andere persoonlijke VPN-tunnels. Als gevolg hiervan op **1 augustus 2018**, ExpressRoute ondersteuning voor Azure AD-services met behulp van openbare Azure-peering en Azure-community's in het Microsoft-peering wordt gestopt. Alle services die beïnvloed door deze wijziging wellicht opgevallen dat Azure AD-verkeer geleidelijk ze kunnen overstappen van ExpressRoute met Internet.

Terwijl we onze support wilt wijzigen we ook weten dat er zijn nog steeds situaties waarin u mogelijk wilt gebruiken een speciale reeks circuits voor uw verificatieverkeer. Als gevolg hiervan blijven Azure AD per tenant IP-bereikbeperkingen met behulp van ExpressRoute- en -services al op de Microsoft-peering met de community 'Andere Online Office 365-services' ondersteunen. Als uw services worden beïnvloed, maar u ExpressRoute nodig hebt, moet u het volgende doen:

- **Als u van openbare Azure-peering gebruikmaakt.** Verplaatsen naar het Microsoft-peering en zich aanmelden voor de **andere Office 365 onlineservices (12076:5100)** community. Zie voor meer informatie over het verplaatsen van openbare Azure-peering voor het Microsoft-peering de [verplaatsen van een openbare peering voor het Microsoft-peering](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) artikel.

- **Als u van Microsoft-peering gebruikmaakt.** Aanmelden voor de **andere Office 365 Online-service (12076:5100)** community. Zie voor meer informatie over vereisten voor routering, de [ondersteuning voor BGP-community's sectie](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) van het ExpressRoute-routering vereisten-artikel.

Als u toegewezen circuits gebruiken blijven moet, moet u contact opnemen met uw Microsoft-Account-team over het ophalen van de autorisatie voor het gebruik van de **andere Office 365 Online-service (12076:5100)** community. De MS Office beheerde controleteam controleert of u deze circuits moet en zorg ervoor dat u begrijpt wat de technische gevolgen te houden. Niet-geautoriseerde abonnementen die bij het maken van routefilters voor Office 365 ontvangt een foutbericht weergegeven. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph-API's voor administratieve scenario's voor gebruiksvoorwaarden

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** functionaliteit voor ontwikkelaars
 
Microsoft Graph-API's toegevoegd voor de beheerbewerking van Azure AD gebruiksvoorwaarden. U zijn kunt maken, bijwerken en verwijderen van het object gebruiksvoorwaarden.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Eindpunt van Azure AD met meerdere tenants toevoegen als een id-provider in Azure AD B2C

**Type:** nieuwe functie  
**Servicecategorie:** B2C - identiteitsbeheer van consumenten  
**Product mogelijkheid:** B2B/B2C
 
Gebruik van aangepast beleid, kunt u het algemene Azure AD-eindpunt toevoegen als een id-provider in Azure AD B2C. Hiermee kunt u een aanspreekpunt voor alle Azure AD-gebruikers die bij uw toepassingen aanmeldt zich hebt. Zie voor meer informatie [Azure Active Directory B2C: toestaan dat gebruikers zich aanmelden bij een multitenant Azure AD-id-provider met behulp van aangepaste beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Interne URL's gebruiken voor toegang tot apps vanaf elke locatie met onze extensie My Apps Secure Sign-in en de Azure AD-toepassingsproxy

**Type:** nieuwe functie  
**Servicecategorie:** mijn Apps  
**Product mogelijkheid:** eenmalige aanmelding
 
Gebruikers kunnen nu toegang tot toepassingen via interne URL's zelfs buiten uw bedrijfsnetwerk met behulp van de extensie mijn Apps beveiligen aanmelden voor Azure AD. Dit werkt met elke toepassing die u hebt gepubliceerd met behulp van Azure AD-toepassingsproxy in elke browser die ook het toegangsvenster Browseruitbreiding geïnstalleerd is. De URL omleidingsfunctionaliteit automatisch ingeschakeld wanneer een gebruiker zich in de uitbreiding. De extensie is beschikbaar voor downloaden op [rand](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367), en [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Verouderde verificaties weergeven via logboeken met aanmeldingsactiviteiten

**Type:** nieuwe functie  
**Servicecategorie:** rapportage  
**Product mogelijkheid:** bewaking en rapportage
 
Dankzij de introductie van de **Clientapp** veld in de aanmeldingsactiviteiten zich aanmeldt, klanten kunnen nu Zie gebruikers die gebruikmaken van verouderde verificaties. Klanten moeten toegang kunnen krijgen tot deze gegevens met behulp van de aanmeldingen MS Graph API of via de aanmeldingspagina wordt activiteit geregistreerd in Azure AD-portal waar u kunt de **Client-App** besturingselement kunt u filteren op verouderde verificaties. Raadpleeg de documentatie voor meer informatie.

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory - gegevens in Europa voor Europese klanten

**Type:** nieuwe functie  
**Servicecategorie:** andere  
**Product mogelijkheid:** GoLocal

Klanten in Europa vereisen van hun gegevens blijven in Europa en niet buiten de Europese datacenters voor vergadering privacy en Europese wetgeving gerepliceerd. Dit [artikel](https://go.microsoft.com/fwlink/?linkid=872328) bevat de specifieke details over welke gegevens van identiteit wordt opgeslagen in Europa en bieden ook details over de informatie die wordt opgeslagen buiten Europese datacenters. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Nieuwe SaaS-app-integraties voor het inrichten van gebruikers - mei 2018

**Type:** nieuwe functie  
**Servicecategorie:** Apps inrichten  
**Product mogelijkheid:** integratie van derden
 
Azure AD kunt u het maken, het onderhoud en het verwijderen van gebruikers-id's in de SaaS-toepassingen zoals Dropbox, Salesforce en ServiceNow automatiseren. Voor mei 2018, hebben we gebruikers inrichten ondersteuning voor de volgende toepassingen in de galerie van Azure AD-app toegevoegd:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Basis OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Zie voor een lijst van alle toepassingen die ondersteuning bieden voor gebruikers inrichten in de galerie van Azure AD [ https://aka.ms/appstutorial ](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Azure AD-toegangsbeoordelingen van groepen en app-toegang biedt nu terugkerende beoordelingen

**Type:** nieuwe functie  
**Servicecategorie:** toegang beoordelingen  
**Product mogelijkheid:** Governance
 
Toegang beoordeling van de groepen en apps is nu algemeen beschikbaar als onderdeel van Azure AD Premium-P2.  Beheerders zich toegang beoordelingen van groepslidmaatschappen en toewijzingen van de toepassing automatisch herhaald met regelmatige tussenpozen, zoals maandelijkse of driemaandelijkse configureren.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD-activiteitenlogboeken (aanmeldingen en audit) zijn nu beschikbaar via MS Graph

**Type:** nieuwe functie  
**Servicecategorie:** rapportage  
**Product mogelijkheid:** bewaking en rapportage
 
Azure AD activiteitenlogboeken, waaronder, aanmeldingen en controlelogboeken, zijn nu beschikbaar via MS Graph. We hebben twee eindpunten via MS Graph voor toegang tot deze logboeken weergegeven. Bekijk onze [documenten](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) voor programmatische toegang tot Azure AD rapportage-API's aan de slag. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Verbeteringen aan de B2B-inwisselervaring en een organisatie verlaten

**Type:** nieuwe functie  
**Servicecategorie:** B2B  
**Product mogelijkheid:** B2B/B2C

**Alleen bij tijd inwisseling:** zodra u een resource delen met een gastgebruiker met behulp van B2B-API: u hoeft niet te verzenden van een uitnodiging voor een speciaal e-mailadres. In de meeste gevallen is de gastgebruiker hebben toegang tot de resource en via de inwisselcode ervaring in de tijd worden uitgevoerd. Geen invloed meer vanwege gemiste e-mailberichten. Niet meer vragen uw gastgebruikers 'Hebt u op op de koppeling inwisseling die u door het systeem worden gestuurd?'. Dit betekent dat zodra de uitnodiging manager maakt gebruik van gesimuleerde Productieorder – bewolkt bijlagen canonieke dezelfde URL voor alle gebruikers – interne en externe – in een status van inwisseling kunnen hebben.

**Moderne inwisseling ervaring:** niet meer gesplitst scherm inwisseling landingspagina. Gebruikers zien een moderne toestemming ervaring met de uitnodiging organisatie privacyverklaring, net als voor apps van derden.

**Gastgebruikers kunnen laten de org:** zodra een gebruiker-relatie met een organisatie afgelopen is, kunnen ze zelf bedienen hij de organisatie verlaat. Het aanroepen van een beheerder de uitnodiging org 'worden verwijderd', niet meer verhogen ondersteuningstickets niet meer.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - mei 2018

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** integratie van derden
 
In mei 2018 toegevoegd dat deze 18 nieuwe apps met Federatie aan onze app-galerie kunnen ondersteunen:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), [Infogix Data3Sixty Govern](), [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial), [まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [Arc Publishing - SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Zie voor meer informatie over de apps [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de galerie van Azure AD app [uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Nieuwe stapsgewijze implementatiehandleidingen voor Azure Active Directory

**Type:** nieuwe functie  
**Servicecategorie:** andere  
**Product mogelijkheid:** Directory
 
Nieuwe, stapsgewijze richtlijnen over het implementeren van Azure Active Directory (Azure AD), inclusief selfservice voor wachtwoordherstel (SSPR) eenmalige aanmelding (SSO), voorwaardelijke toegang (CA), App-proxy, gebruikers inrichten, Active Directory Federation Services (ADFS) naar Pass through-verificatie (PTA), en ADFS naar wachtwoordhashsynchronisatie (PBS).

Als u wilt weergeven in de implementatiehandleidingen, gaat u naar de [identiteit implementatiehandleidingen](https://aka.ms/DeploymentPlans) opslagplaats op GitHub. Als u feedback over de implementatiehandleidingen, wilt gebruiken de [implementatie plannen feedbackformulier](https:aka.ms/deploymentplanfeedback). Als u vragen over de implementatiehandleidingen hebt, contact met ons op [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="enterprise-applications-search---load-more-apps"></a>Bedrijfstoepassingen zoeken - Meer apps laden

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** eenmalige aanmelding
 
Problemen bij het vinden van uw toepassingen / service principals? Toegevoegd de mogelijkheid om te laden meer toepassingen in uw bedrijfstoepassingen lijst met alle toepassingen. Standaard weergeven we 20 toepassingen. U kunt nu op klikt, **laden meer** om extra toepassingen weer te geven. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>De versie van AADConnect een openbare preview van de integratie met PingFederate bevat, mogelijk belangrijke beveiligingsupdates, veel oplossingen voor problemen en nieuwe uitermate nieuwe hulpprogramma's voor probleemoplossing. 

**Type:** gewijzigde functie  
**Servicecategorie:** AD Connect  
**Product mogelijkheid:** Identity Lifecycle Management
 
De versie van AADConnect een openbare preview van de integratie met PingFederate bevat, mogelijk belangrijke beveiligingsupdates, veel oplossingen voor problemen en nieuwe uitermate nieuwe hulpprogramma's voor probleemoplossing. U vindt de release-opmerkingen [hier](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD-toegang en -beoordelingen: automatisch toepassen

**Type:** gewijzigde functie  
**Servicecategorie:** toegang beoordelingen  
**Product mogelijkheid:** Governance

Toegang controleert van groepen en -apps worden nu algemeen beschikbaar als onderdeel van Azure AD Premium-P2. Een beheerder kan configureren zodat automatisch de revisor wijzigingen toepassen aan die groep of een app, zoals het controleren van de toegang is voltooid. De beheerder kan ook opgeven wat er gebeurt met permanente toegang van de gebruiker als revisoren niet reageren, toegang verwijderen, toegang houden of systeemaanbevelingen op te nemen. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>ID-tokens kunnen niet meer worden geretourneerd met de response_mode query voor nieuwe apps. 

**Type:** gewijzigde functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie
 
Apps die zijn gemaakt op of na 25 April 2018 niet langer mogelijk om aan te vragen een **id_token** met behulp van de **query** response_mode.  Hiermee beschikt u over Azure AD-inline met de specificaties OIDC en helpt de kwetsbaarheid van uw apps te verminderen.  Apps die zijn gemaakt voordat 25 April 2018 niet is geblokkeerd met behulp van de **query** response_mode met een response_type van **id_token**.  De volgende fout is geretourneerd, wanneer u een id_token aanvraagt bij AAD, is **AADSTS70007: 'query' is geen ondersteunde waarde van 'response_mode' bij het aanvragen van een token**.

De **fragment** en **form_post** response_modes blijven werken - wanneer maken objecten in de nieuwe toepassing (bijvoorbeeld voor toepassingsproxy van gebruik), zorg ervoor dat gebruik van een van deze response_modes voordat ze maakt een nieuwe toepassing.  
 
---
 
## <a name="april-2018"></a>April 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C-toegangstokens zijn algemeen beschikbaar

**Type:** nieuwe functie  
**Servicecategorie:** B2C - identiteitsbeheer van consumenten  
**Product mogelijkheid:** B2B/B2C 

U kunt nu toegang tot Web-API's beveiligd door Azure AD B2C met toegangstokens. De functie wordt verplaatst van de openbare preview naar algemene beschikbaarheid. De UI-mogelijkheden voor het configureren van Azure AD B2C-toepassingen en web-API's is verbeterd en andere kleine verbeteringen aangebracht.
 
Zie voor meer informatie [Azure AD B2C: aanvragende toegangstokens](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Configuratie van eenmalige aanmelding voor toepassingen op basis van SAML testen

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** eenmalige aanmelding

Wanneer u eenmalige aanmelding op basis van SAML-toepassingen configureert, kun je je voor het testen van de integratie op de configuratiepagina. Als er een fout tijdens het aanmelden optreden, kunt u de fout in de ervaring voor de test en Azure AD levert u stappen voor het oplossen van dit specifieke probleem.

Zie voor meer informatie:

- [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
- [Fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>De gebruiksvoorwaarden van Azure AD bevatten nu rapportages per gebruiker

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** naleving
 
Beheerders kunnen nu selecteren van een bepaalde gebruiksvoorwaarden en zien alle gebruikers die toestemming hebt gegeven voor gebruiksvoorwaarden en wat datum/tijd deze plaatsgevonden heeft.

Zie voor meer informatie de [gebruiksvoorwaarden Azure AD-functie](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: riskante IP-adressen voor de AD FS-functie voor beveiligingsvergrendeling voor extranet 

**Type:** nieuwe functie  
**Servicecategorie:** andere  
**Product mogelijkheid:** bewaking en rapportage

Connect Health nu ondersteunt de mogelijkheid voor het detecteren van IP adressen die groter is dan een drempel voor mislukte U/P aanmeldingen op basis van per uur of dagelijks. De mogelijkheden van deze functie zijn:

- Uitgebreide rapport met IP-adres en het aantal mislukte aanmeldingen gegenereerd op basis van elk uur/dagelijks met aanpasbare drempelwaarde.
- Waarschuwingen op basis van een e-mailadres weergegeven wanneer een specifiek IP-adres is groter dan de drempelwaarde van mislukte U/P aanmeldingen op basis van elk uur/dag.
- Een downloadoptie om een gedetailleerde analyse van de gegevens

Zie voor meer informatie [riskante IP-rapport](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Eenvoudige app-configuratie met een metagegevensbestand of URL

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** eenmalige aanmelding

Op de pagina Enterprise-toepassingen kunnen beheerders een SAML-metagegevensbestand voor het configureren van SAML op basis van eenmalige aanmelding voor AAD-galerie en niet toepassing uploaden.

Bovendien kunt u Azure AD-toepassing federatieve metagegevens-URL voor het configureren van eenmalige aanmelding met de doeltoepassing.

Zie voor meer informatie [configureren van eenmalige aanmelding tot toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>De gebruiksvoorwaarden van Azure AD zijn nu algemeen beschikbaar

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** naleving
 

Azure AD gebruiksvoorwaarden hebt verplaatst van de openbare preview naar algemeen beschikbaar.

Zie voor meer informatie de [gebruiksvoorwaarden Azure AD-functie](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Uitnodigingen voor B2B-gebruikers van bepaalde organisaties toestaan of blokkeren

**Type:** nieuwe functie  
**Servicecategorie:** B2B  
**Product mogelijkheid:** B2B/B2C
 

Nu kunt u opgeven welke partners die u wilt delen en samenwerken met Azure AD B2B-samenwerking. Om dit te doen, kunt u de lijst van specifieke maken toestaan of weigeren van domeinen. Wanneer een domein is geblokkeerd met behulp van deze mogelijkheden, kunnen werknemers niet meer uitnodigingen verzenden naar mensen in dat domein.

Dit helpt u bij het beheren van toegang tot uw resources tijdens het inschakelen van een vlotte gebruikerservaring goedgekeurde gebruikers.

Deze functie B2B-samenwerking is beschikbaar voor alle klanten van Azure Active Directory en kan worden gebruikt in combinatie met Azure AD Premium-functies zoals voorwaardelijke toegang en identity protection voor gedetailleerde controle over wanneer en hoe de externe zakelijke gebruikers zich aanmelden in en toegang te krijgen.

Zie voor meer informatie [toestaan of blokkeren uitnodigingen voor B2B gebruikers van specifieke organisaties](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve apps beschikbaar in de galerie van Azure AD-app

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** integratie van derden

In April 2018 toegevoegd dat deze 13 nieuwe apps met Federatie aan onze app-galerie kunnen ondersteunen:

Criterium HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (On-Premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dynamische signaal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [organigram Nu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Prestatiemeter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), rek, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Zie voor meer informatie over de apps [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de galerie van Azure AD app [uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>B2B-gebruikers in Azure AD toegang verlenen tot uw on-premises toepassingen (openbare preview)

**Type:** nieuwe functie  
**Servicecategorie:** B2B  
**Product mogelijkheid:** B2B/B2C

Als een organisatie die gebruikmaakt van Azure Active Directory (Azure AD) B2B-samenwerkingsmogelijkheden om uit te nodigen gastgebruikers van partnerorganisaties aan uw Azure AD, kunt u nu bieden deze B2B-gebruikers toegang tot lokale apps. Deze apps lokale kunt SAML-gebaseerde authenticatie of geïntegreerde Windows-verificatie (IWA) met Kerberos-beperkte delegatie (KCD) gebruiken.

Zie voor meer informatie [Grant B2B-gebruikers in Azure AD toegang tot uw on-premises toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Zelfstudies voor SSO-integratie downloaden van de Azure Marketplace

**Type:** gewijzigde functie  
**Servicecategorie:** andere  
**Product mogelijkheid:** integratie van derden

Als een toepassing die wordt vermeld in de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) ondersteunt SAML-gebaseerde eenmalige aanmelding, te klikken op **nu downloaden** biedt u de integratie-zelfstudie die is gekoppeld aan die toepassing. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Betere prestaties bij automatische inrichting van gebruikers voor SaaS-toepassingen in Azure AD

**Type:** gewijzigde functie  
**Servicecategorie:** Apps inrichten  
**Product mogelijkheid:** integratie van derden
 
Voorheen klanten die gebruikmaken van de inrichting van connectors voor SaaS-toepassingen (bijvoorbeeld Salesforce, ServiceNow en vak) Azure Active Directory-gebruiker kunnen traag als hun Azure AD-tenants meer dan 100.000 gecombineerde gebruikers bevat en groepen en ze gebruikten toewijzingen van gebruikers en groepen om te bepalen welke gebruikers moeten worden ingericht.

Op 2 April 2018, zijn belangrijke prestatieverbeteringen geïmplementeerd voor de inrichting Azure AD-service die aanzienlijk verminderen de hoeveelheid tijd die nodig zijn voor het uitvoeren van de initiële synchronisatie tussen Azure Active Directory en doel SaaS-toepassingen.

Als gevolg hiervan veel klanten die had eerste synchronisaties voor apps die het aantal dagen duurde of nooit voltooid, worden binnen een paar minuten of uren nu voltooid.

Zie voor meer informatie [wat er gebeurt tijdens het inrichten?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Selfservice voor wachtwoord opnieuw instellen vanuit het vergrendelingsscherm van Windows 10 voor machines die zijn toegevoegd aan hybride Azure AD

**Type:** gewijzigde functie  
**Servicecategorie:** wachtwoord opnieuw instellen in selfservice  
**Product mogelijkheid:** gebruikersverificatie
 
We hebt bieden ondersteuning voor computers die zijn hybride die lid zijn van Azure AD voor de functie Windows 10 SSPR bijgewerkt. Deze functie is beschikbaar in Windows 10 RS4 kunnen gebruikers hun wachtwoord van het vergrendelingsscherm van een Windows 10-machine. Deze functie kunnen gebruikmaken van gebruikers die zijn ingeschakeld en geregistreerd voor selfservice voor wachtwoordherstel.

Zie voor meer informatie [Azure AD-wachtwoord opnieuw instellen van het aanmeldingsscherm](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).
 
---

## <a name="march-2018"></a>2018 maart
 
### <a name="certificate-expire-notification"></a>Melding voor het verlopen van het certificaat

**Type:** vast  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** eenmalige aanmelding
 
Azure AD verzendt een melding wanneer een certificaat voor een galerie of niet-galerie van toepassing is verlopen. 

Sommige gebruikers heeft geen meldingen voor enterprise-toepassingen die zijn geconfigureerd voor op basis van SAML eenmalige aanmelding ontvangen. Dit probleem is opgelost. Azure AD verzendt melding voor certificaten dat verloopt binnen 7, 30 en 60 dagen. U zijn kunt zien in de controlelogboeken gebeurtenis. 

Zie voor meer informatie:

- [Certificaten beheren voor federatieve eenmalige aanmelding bij Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Controlerapporten van activiteit in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Twitter- en GitHub-id-providers in Azure AD B2C

**Type:** nieuwe functie  
**Servicecategorie:** B2C - identiteitsbeheer van consumenten  
**Product mogelijkheid:** B2B/B2C
 
U kunt nu Twitter of GitHub toevoegen als een id-provider in Azure AD B2C. Twitter wordt verplaatst van de openbare preview naar algemene beschikbaarheid. GitHub wordt vrijgegeven openbare preview.

Zie voor meer informatie [wat is Azure AD B2B-samenwerking?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Browsertoegang beperken via Intune Managed Browser met voorwaardelijke Azure AD-toegang op basis van de toepassing voor iOS- en Android

**Type:** nieuwe functie  
**Servicecategorie:** voorwaardelijke toegang  
**Product mogelijkheid:** identiteit beveiliging en gegevensbescherming
 
**Nu in public preview!**

**Intune Managed Browser SSO:** uw werknemers kunt eenmalige aanmelding in systeemeigen clients (zoals Microsoft Outlook) en de Intune Managed Browser voor alle Azure AD verbonden apps.

**Intune Managed Browser voorwaardelijke toegang ondersteuning:** werknemers gebruikmaken van de Intune Managed browser met behulp van beleid voor voorwaardelijke toegang op basis van een toepassing kunt u nu vereisen.

Meer informatie over dit op onze [blogbericht](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Zie voor meer informatie:

- [Voorwaardelijke toegang op basis van een toepassing instellen](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

- [Beheerde-browserbeleid configureren](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>App Proxy-cmdlets in Powershell GA-module

**Type:** nieuwe functie  
**Servicecategorie:** toepassingsproxy  
**Product mogelijkheid:** toegangsbeheer
 
Ondersteuning voor toepassingsproxy-cmdlets is nu in de Powershell-Module GA! Dit vereist dat u op de hoogte te blijven van Powershell-modules - als u meer dan een jaar achter, bepaalde cmdlets worden mogelijk niet meer werken. 

Zie voor meer informatie [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Systeemeigen Office 365-clients worden ondersteund door naadloos SSO via een niet-interactief protocol

**Type:** nieuwe functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie
 
Gebruiker met behulp van Office 365 systeemeigen clients (versie 16.0.8730.xxxx en hoger) een achtergrond-on-ervaring met naadloze eenmalige aanmelding. Deze ondersteuning wordt geboden door toevoeging een niet-interactieve protocol (WS-Trust) naar Azure AD.

Zie voor meer informatie [hoe aanmelden op een native client met naadloze eenmalige aanmelding werk?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Gebruikers krijgen een achtergrond sign-on-ervaring naadloze aanmelding bij als een toepassing aanmeldingsaanvragen naar Azure AD-tenant-eindpunten verzendt

**Type:** nieuwe functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie
 
Gebruikers krijgen een achtergrond sign-on-ervaring naadloze aanmelding bij als een toepassing (bijvoorbeeld `https://contoso.sharepoint.com`) aanmeldingsaanvragen dat wil zeggen, verzendt naar Azure AD-tenant-eindpunten - `https://login.microsoftonline.com/contoso.com/<..>` of `https://login.microsoftonline.com/<tenant_ID>/<..>` - in plaats van Azure AD gemeenschappelijk eindpunt (`https://login.microsoftonline.com/common/<...>`).

Zie voor meer informatie [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Er hoeft slechts één Azure AD-URL te worden toegevoegd aan de intranetzone-instellingen van gebruikers om naadloze SSO te implementeren, in plaats van zoals voorheen twee URL's

**Type:** nieuwe functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie
 
Als u wilt implementeren naadloze eenmalige aanmelding voor uw gebruikers, moet u slechts één Azure AD-URL met de gebruikers Intranet zone-instellingen met behulp van Groepsbeleid in Active Directory toevoegen: `https://autologon.microsoftazuread-sso.com`. Voorheen moest klanten twee URL's toevoegen.

Zie voor meer informatie [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** integratie van derden

In maart 2018 toegevoegd dat deze 15 nieuwe apps met Federatie aan onze app-galerie kunnen ondersteunen:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), -assistent door FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech digitale Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Zie voor meer informatie over de apps [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de galerie van Azure AD app [uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>PIM voor Azure-resources is algemeen beschikbaar

**Type:** nieuwe functie  
**Servicecategorie:** Privileged Identity Management  
**Product mogelijkheid:** Privileged Identity Management
 
Als u van Azure AD Privileged Identity Management voor directory-functies gebruikmaakt, kunt u nu gebruiken de tijdsgebonden toegang en de mogelijkheden van de toewijzing van PIM voor Azure Resource-functies, zoals abonnementen, resourcegroepen, virtuele Machines en een andere bron ondersteund door Azure Resource Manager. Multi-factor Authentication afdwingen bij het activeren van rollen Just-In-Time en activeringen in coördinatie met goedgekeurde wijziging windows plannen. Deze versie wordt bovendien niet beschikbaar tijdens de openbare preview, met inbegrip van een bijgewerkte gebruikersinterface, werkstromen voor goedkeuring en de mogelijkheid voor het uitbreiden van de rollen die binnenkort verlopen en vernieuwen van verlopen rollen verbeteringen toegevoegd.

Zie voor meer informatie [PIM voor Azure-resources (Preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Optionele claims toevoegen aan uw apps-tokens (openbare preview-versie)

**Type:** nieuwe functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie
 
Uw Azure AD-app kan nu aangepaste of optionele aanvraagclaims in JWTs of SAML tokens.  Dit zijn de claims over de gebruiker of de tenant die niet zijn opgenomen in het token, als gevolg van grootte of toepasselijkheid beperkingen standaard.  Dit is momenteel in public preview voor Azure AD-apps op de eindpunten v1.0 en v2.0.  Zie de documentatie voor informatie over welke claims kunnen worden toegevoegd en het bewerken van uw toepassingsmanifest om aan te vragen deze.  

Zie voor meer informatie [optioneel claims in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD ondersteunt PKCE voor veiligere OAuth-stromen

**Type:** nieuwe functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie
 
Azure AD-documenten zijn bijgewerkt om ondersteuning voor PKCE, waardoor de meer beveiligde communicatie terwijl de autorisatiecode van OAuth 2.0 grant opmerking.  Zowel S256 als tekst zonder opmaak code_challenges worden ondersteund op de eindpunten v1.0 en v2.0. 

Zie voor meer informatie [aanvragen van een autorisatiecode](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Ondersteuning voor het inrichten van alle beschikbare waarden voor gebruikerskenmerken in de API Workday Get_Workers

**Type:** nieuwe functie  
**Servicecategorie:** Apps inrichten  
**Product mogelijkheid:** integratie van derden
 
De openbare preview van inkomende inrichten vanuit Workday naar Active Directory en Azure AD nu ondersteunt de mogelijkheid om op te halen en het inrichten van alle beschikbaar in de API van Workday Get_Workers kenmerkwaarden. Hiermee voegt u ondersteuning voor extra standard honderden en aangepaste kenmerken buiten degene die worden geleverd bij de eerste versie van de werkdag inkomende connector inrichten.

Zie voor meer informatie: [aanpassen van de lijst met gebruikerskenmerken Workday](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Groepslidmaatschap wijzigen van dynamisch in statisch en omgekeerd

**Type:** nieuwe functie  
**Servicecategorie:** groepsbeheer  
**Product mogelijkheid:** samenwerking
 
Het is mogelijk om te wijzigen hoe lidmaatschap wordt beheerd in een groep. Dit is handig als u de naam en de ID in het systeem behouden wilt, zodat alle bestaande verwijzingen naar de groep nog steeds geldig zijn zijn. maken van een nieuwe groep zou moeten worden bijgewerkt die verwijzingen.
We hebben het Azure AD-beheercentrum om deze functionaliteit bijgewerkt. Klanten kunnen bestaande groepen nu converteren van dynamisch lidmaatschap naar toegewezen lidmaatschap en vice versa. De bestaande PowerShell-cmdlets zijn ook nog steeds beschikbaar.

Zie voor meer informatie [dynamisch lidmaatschap wijzigen in een statisch en vice versa](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Verbeterd afmeldgedrag met naadloze SSO

**Type:** gewijzigde functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie
 
Eerder, zelfs als gebruikers wordt expliciet afgemeld bij een toepassing die wordt beveiligd door Azure AD, ze zou worden automatisch aangemeld terug met behulp van naadloze eenmalige aanmelding als ze toegang probeert te krijgen van een Azure AD-toepassing opnieuw aan binnen hun corpnet vanaf hun apparaten verbonden met het domein. Met deze wijziging wordt afmelden ondersteund.  Hiermee kunnen gebruikers ervoor kiezen de dezelfde of verschillende Azure AD-account voor aanmelden terug, in plaats van wordt automatisch aangemeld met behulp van naadloze eenmalige aanmelding.

Zie voor meer informatie [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Application Proxy-connector versie 1.5.402.0 is uitgebracht

**Type:** gewijzigde functie  
**Servicecategorie:** toepassingsproxy  
**Product mogelijkheid:** identiteit beveiliging en gegevensbescherming
 
De versie van deze connector geleidelijk in tegenstelling tot en met November. Deze nieuwe versie van de connector bevat de volgende wijzigingen:

- De connector nu stelt domein niveau cookies in plaats daarvan subdomein niveau. Dit zorgt ervoor dat een soepeler SSO-ervaring en redundante verificatie prompts voorkomt.
- Ondersteuning voor gesegmenteerde codering aanvragen
- Verbeterde connector statuscontrole 
- Verschillende oplossingen voor problemen en stabiliteitsverbeteringen

Zie voor meer informatie [inzicht in Azure AD-toepassingsproxy connectors](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>2018 februari
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Verbeterde navigatie voor het beheren van gebruikers en groepen

**Type:** wijzigingen  
**Servicecategorie:** Directory Management  
**Product mogelijkheid:** Directory

De navigatie-ervaring voor het beheren van gebruikers en groepen is gestroomlijnd. U kunt nu navigeren vanuit een overzicht van de directory rechtstreeks aan de lijst met alle gebruikers gemakkelijker toegang aan de lijst met verwijderde gebruikers. U kunt ook rechtstreeks aan de lijst van alle groepen, met gemakkelijker toegang tot de groepsinstellingen uit het overzicht directory navigeren. En ook op de pagina overzicht directory u kunt zoeken naar een gebruiker, groep, bedrijfstoepassing of app-registratie. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Beschikbaarheid van aanmeldingen en audit rapporten in Microsoft Azure beheerd door 21Vianet (Azure China 21Vianet)

**Type:** nieuwe functie  
**Servicecategorie:** Azure Stack  
**Product mogelijkheid:** bewaking en rapportage

Azure AD-activiteit log-rapporten zijn nu beschikbaar in Microsoft Azure beheerd door 21Vianet (Azure China 21Vianet) exemplaren. De volgende logboeken zijn opgenomen:

- **Aanmeldingen activiteitenlogboeken** -omvat alle de aanmeldingen logboeken die zijn gekoppeld aan uw tenant.

- **Selfservice voor wachtwoord Audit logboeken** -bevat de logboeken voor de audit SSPR.

- **Controle van Directory-beheer registreert** -omvat alle de map incidentbeheer controlelogboeken zoals gebruiker management, App-beheer en andere gebruikers.

Met deze logboeken krijgt u inzicht in hoe uw omgeving doet. Met de gegevens kunt u het volgende doen:

- Bepalen hoe uw apps en services worden gebruikt door uw gebruikers.

- Los problemen te voorkomen dat uw gebruikers toegang krijgen tot hun werk.

Zie voor meer informatie over het gebruik van deze rapporten [rapportage van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Gebruik van 'rapportlezer'-rol (niet-beheerders rol) Azure AD activiteitsrapporten weer te geven

**Type:** nieuwe functie  
**Servicecategorie:** rapportage  
**Product mogelijkheid:** bewaking en rapportage

Als onderdeel van feedback van klanten om in te schakelen van niet-beheerders functies toegang hebben tot Azure AD-activiteit zich aanmeldt, hebben we de mogelijkheid voor gebruikers die in de rol 'rapportlezer' aanmeldingen met toegang tot en controle-activiteit in de Azure-portal, evenals met onze Graph-API's zijn ingeschakeld. 

Voor meer informatie het gebruik van deze rapporten, Zie [rapportage van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Werknemer-id claim beschikbaar als gebruikerskenmerk en gebruikers-id

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** eenmalige aanmelding
 
U kunt configureren **werknemer-id** als de gebruikers-id en gebruikerskenmerk voor gebruikers en lid B2B gasten in SAML-aanmelding toepassingen uit de bedrijfstoepassing gebruikersinterface.

Zie voor meer informatie [uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure Active Directory aanpassen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Vereenvoudigde Toepassingsbeheer gebruik van jokertekens in Azure AD-toepassingsproxy

**Type:** nieuwe functie  
**Servicecategorie:** toepassingsproxy  
**Product mogelijkheid:** gebruikersverificatie
 
Voor de implementatie van de toepassing te vereenvoudigen en reduceren uw administratieve overhead, ondersteunen we nu de mogelijkheid voor het publiceren van toepassingen die gebruikmaken van jokertekens. U kunt een jokerteken als toepassing wilt publiceren, volgt u de standaardtoepassing publishing stroom maar een jokerteken gebruikt in de interne en externe URL's.

Zie voor meer informatie [jokertekens toepassingen in de Azure Active Directory-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Nieuwe cmdlets voor de ondersteuning van de configuratie van Application Proxy

**Type:** nieuwe functie  
**Servicecategorie:** toepassingsproxy  
**Product mogelijkheid:** Platform

De meest recente versie van de module AzureAD PowerShell Preview bevat nieuwe cmdlets waarmee klanten om Application Proxy toepassingen met behulp van PowerShell te configureren.

De nieuwe cmdlets zijn: 

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
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Nieuwe cmdlets voor de ondersteuning van de configuratie van groepen

**Type:** nieuwe functie  
**Servicecategorie:** toepassingsproxy  
**Product mogelijkheid:** Platform

De nieuwste versie van de AzureAD PowerShell-module bevat cmdlets voor het beheren van groepen in Azure AD. Deze cmdlets eerder beschikbaar waren in de module AzureADPreview en nu zijn toegevoegd aan de module AzureAD

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

**Type:** nieuwe functie  
**Servicecategorie:** AD Sync  
**Product mogelijkheid:** Platform
 
Azure AD Connect is de aanbevolen hulpprogramma om te synchroniseren van gegevens tussen Azure AD en lokale gegevensbronnen, met inbegrip van Windows Server Active Directory en LDAP.

>[!Important]
>Deze build schema en sync introduceert wijzigingen regel. De Azure AD Connect-synchronisatieservice activeert een volledige Import en een volledige synchronisatie stappen na een upgrade. Zie voor meer informatie over het wijzigen van dit gedrag [het uitstellen van de volledige synchronisatie na de upgrade](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Deze release heeft de volgende updates en wijzigingen:

**Opgeloste problemen**

- Los op achtergrondtaken voor partitie filteren pagina tijdvenster wanneer u overschakelt naar de volgende pagina.

- Een fout die toegangsfout tijdens de aangepaste actie ConfigDB veroorzaakt vast.

- Heeft een fout bij het herstellen van de time-out van de sql-verbinding.

- Een fout vastgesteld waar certificaten met jokertekens SAN controle van vereisten mislukt.

- Heeft een fout die ervoor zorgt miiserver.exe crashes tijdens het exporteren van AAD-connector dat.

- Een probleem opgelost waar een onjuist wachtwoord poging DC aangemeld wanneer uitgevoerd AAD veroorzaakt verbinding maken met de wizard configuratie wijzigen

**Nieuwe functies en verbeteringen**
 
- Toepassingstelemetrie - beheerders kunnen schakelen deze klasse van gegevens in-of uitschakelen.

- Statusgegevens van Azure AD - beheerders moeten Ga naar de health-portal voor het beheren van instellingen voor de status. Zodra de service-beleid is gewijzigd, wordt de agents lezen en wordt deze toepassen.

- Apparaat terugschrijven Configuratieacties en een voortgangsbalk zien voor de initialisatie van de pagina toegevoegd.

- Verbeterde algemene diagnostische gegevens met de HTML-rapport en volledige gegevens verzamelen in een ZIP-tekst / HTML-rapport.

- Verbeterde betrouwbaarheid van Automatische upgrade en extra telemetrie om te controleren of de status van de server kan worden bepaald toegevoegd.

- Machtigingen die beschikbaar zijn beperkt tot beschermde accounts op AD-Connector-account. Voor nieuwe installaties van de wizard beperkt de machtigingen die accounts met bevoegdheden hebben op het account MSOL na het maken van het account MSOL. De wijzigingen die invloed hebben op installaties van snelle en aangepaste installaties met account automatisch maken.

- Het installatieprogramma niet verplicht stellen SA-bevoegdheden op een schone installatie van AADConnect wordt gewijzigd.

- Nieuwe hulpprogramma voor het oplossen van synchronisatieproblemen voor een specifiek object. Het hulpprogramma controleert op dit moment wordt de volgende zaken:

    - UserPrincipalName komt niet overeen tussen gesynchroniseerde gebruikersobject en het gebruikersaccount in Azure AD-Tenant.
  
    - Als het object is gefilterd van synchronisatie vanwege domein filteren
  
    - Als het object van synchronisatie vanwege organisatie-eenheid (OE) voor het filteren is gefilterd

- Nieuwe hulpprogramma voor het synchroniseren van de huidige wachtwoordhash opgeslagen in de lokale Active Directory voor een specifiek gebruikersaccount. Het hulpprogramma is niet vereist voor wachtwoord wijzigen. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Toepassingen ondersteunende Intune App Protection-beleid wordt toegevoegd voor gebruik met voorwaardelijke toegang van Azure AD op basis van een toepassing

**Type:** gewijzigde functie  
**Servicecategorie:** voorwaardelijke toegang  
**Product mogelijkheid:** identiteit beveiliging en gegevensbescherming

We hebben hebt meer toepassingen die ondersteuning bieden voor voorwaardelijke toegang op basis van een toepassing toegevoegd. U krijgt nu toegang tot Office 365 en andere Azure AD verbonden cloud-apps met behulp van deze goedgekeurde ClientApps.

De volgende toepassingen worden toegevoegd aan het einde van februari:

- Microsoft Power BI

- Microsoft Launcher

- Microsoft facturering

Zie voor meer informatie:

- [Goedgekeurde app vereiste](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD app gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Gebruiksvoorwaarden bijwerken naar mobiele ervaring 

**Type:** gewijzigde functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** naleving

Wanneer u de gebruiksvoorwaarden worden weergegeven, kunt u nu klikken **met problemen bij het bekijken? Klik hier**. Op deze koppeling klikt, opent de gebruiksvoorwaarden systeemeigen op uw apparaat. Ongeacht de grootte van het lettertype in het document of de schermgrootte van apparaat, kunt u inzoomen en lezen van het document indien nodig. 

---
 
## <a name="january-2018"></a>2018 januari
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app 

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** integratie van derden

In januari 2018, zijn de volgende nieuwe apps met ondersteuning voor identiteitsfederatie toegevoegd in de app-galerie:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy beheersoftware](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk federatieve Directory, en [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Zie voor meer informatie over de apps [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de galerie van Azure AD app [uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Meld u aan met het extra risico gedetecteerd

**Type:** nieuwe functie  
**Servicecategorie:** Identity Protection  
**Product mogelijkheid:** identiteit beveiliging en gegevensbescherming

Het voor een risicogebeurtenis gedetecteerde dat u inzicht is gekoppeld aan uw abonnement Azure AD. Met de Azure AD Premium P2-editie, moet u de meest gedetailleerde informatie over alle onderliggende detecties ophalen.

Met de editie Azure AD Premium-P1 weergegeven detecties die niet wordt gedekt door uw licentie als de risicogebeurtenis aanmelden met extra risico gedetecteerd.

Zie [Risicogebeurtenissen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) voor meer informatie.
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Office 365-toepassingen van de eindgebruiker toegang panelen verbergen

**Type:** nieuwe functie  
**Servicecategorie:** mijn Apps  
**Product mogelijkheid:** eenmalige aanmelding

U kunt nu beter beheren hoe Office 365-toepassingen weergegeven op uw gebruikers toegang panelen via een nieuwe gebruikersinstelling. Deze optie is handig voor het aantal apps in een gebruiker toegang panelen te verminderen, als u liever alleen Office-apps worden weergegeven in de Office-portal. De instelling bevindt zich in de **gebruikersinstellingen** en is met het label, **kunnen gebruikers alleen Office 365-apps in de Office 365-beheerportal zien**.

Zie voor meer informatie [verbergen van een toepassing van de gebruikerservaring in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Naadloze Meld u aan bij de apps die zijn ingeschakeld voor eenmalige aanmelding wachtwoord rechtstreeks vanuit de app-URL 

**Type:** nieuwe functie  
**Servicecategorie:** mijn Apps  
**Product mogelijkheid:** eenmalige aanmelding

De Browseruitbreiding van mijn Apps is nu beschikbaar via een handig hulpmiddel waarmee u eenmalige aanmelding in de mijn Apps van mogelijkheden als een snelkoppeling in uw browser. Na de installatie, ziet de gebruiker van een pictogram waffle in hun browser waarmee ze snel toegang krijgt tot apps. Gebruikers kunnen nu profiteren van:

- De mogelijkheid rechtstreeks aanmelden bij wachtwoord SSO gebaseerde apps uit de aanmeldingspagina van de app
- Start een app met de functie Snel zoeken
- Snelkoppelingen naar de laatst gebruikte apps van de extensie
- De extensie is beschikbaar voor de rand en Chrome, Firefox.
 
Zie voor meer informatie [mijn Apps beveiligen aanmelden extensie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure AD-beheer ervaring in de klassieke Azure-Portal is buiten gebruik gesteld

**Type:** afgeschaft   
**Servicecategorie:** Azure AD  
**Product mogelijkheid:** Directory

Vanaf 8 januari 2018, het beheer van Azure AD ervaring in de klassieke Azure portal is buiten gebruik gesteld. Dit heeft plaatsgevonden in combinatie met de buiten gebruik stellen van de klassieke Azure portal zelf. In de toekomst, moet u de [Azure AD-beheercentrum](https://aad.portal.azure.com) voor alle uw portal-beheer op basis van Azure AD.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>De PhoneFactor-webportal is buiten gebruik gesteld

**Type:** afgeschaft  
**Servicecategorie:** Azure AD  
**Product mogelijkheid:** Directory
 
Vanaf 8 januari 2018, de PhoneFactor-webportal is buiten gebruik gesteld. Deze portal is gebruikt voor het beheer van MFA-server, maar deze functies zijn verplaatst naar de Azure portal op portal.azure.com. 

De configuratie van MFA bevindt zich op: **Azure Active Directory \> MFA-Server**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Afschaffen Azure AD-rapporten

**Type:** afgeschaft  
**Servicecategorie:** rapportage  
**Product mogelijkheid:** Identity Lifecycle Management  


Met de algemene beschikbaarheid van de nieuwe Azure Active Directory Administration console en de nieuwe API's nu beschikbaar voor zowel beveiliging als activiteit rapporten, het rapport API's onder '/ reports'-eindpunt is buiten gebruik gesteld vanaf het einde van 31 December 2017.

**Wat is er beschikbaar?**

Als onderdeel van de overgang naar de nieuwe beheerconsole, hebben we 2 nieuwe API's beschikbaar zijn voor het ophalen van Azure AD-activiteitenlogboeken aangebracht. De nieuwe set API's biedt uitgebreidere filteren en sorteren op de functionaliteit naast het bieden van uitgebreidere audit en aanmelden activiteiten. De gegevens die eerder beschikbaar via de beveiligingsrapporten zijn nu toegankelijk via de risicogebeurtenissen Identity Protection API in Microsoft Graph.

Zie voor meer informatie:

- [Aan de slag met de Azure Active Directory rapportage-API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Aan de slag met Azure Active Directory: Identity Protection en Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>December 2017

### <a name="terms-of-use-in-the-access-panel"></a>Gebruiksvoorwaarden in het toegangsvenster

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** naleving
 
U kunt nu gaat u naar het Toegangspaneel en weergeven van de gebruiksvoorwaarden die u eerder hebt geaccepteerd.

Volg deze stappen:

1. Ga naar de [MyApps portal](https://myapps.microsoft.com), en meld u aan.

2. Selecteer uw naam in de rechterbovenhoek en selecteer vervolgens **profiel** uit de lijst. 

3. Op uw **profiel**, selecteer **gebruiksvoorwaarden controleren**. 

4. Nu u de gebruiksvoorwaarden kunt bekijken die u hebt geaccepteerd. 

Zie voor meer informatie de [Azure AD-voorwaarden van de functie gebruiken (preview)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Nieuwe Azure AD-aanmeldingservaring aanpast

**Type:** nieuwe functie  
**Servicecategorie:** Azure AD  
**Product mogelijkheid:** gebruikersverificatie
 
De Azure AD en Microsoft identity-accountsysteem UI zijn opnieuw ontworpen zodat ze een consistent uiterlijk hebben. Bovendien verzamelt de aanmeldingspagina van Azure AD de naam van de gebruiker eerst, gevolgd door de referentie in een tweede scherm.

Zie voor meer informatie [de nieuwe Azure AD-aanmeldingservaring aanpast bevindt zich in de openbare preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Minder prompts aanmelden: een nieuwe 'aangemeld blijven' ervaring voor aanmelding bij Azure AD

**Type:** nieuwe functie  
**Servicecategorie:** Azure AD  
**Product mogelijkheid:** gebruikersverificatie
 
De **aangemeld blijven** selectievakje op de aanmeldingspagina van Azure AD is vervangen door een nieuwe prompt die wordt weergegeven nadat u verifiëren. 

Als u reageert **Ja** deze prompt de service biedt u een permanente vernieuwingstoken. Dit gedrag is hetzelfde als wanneer u hebt geselecteerd de **aangemeld blijven** selectievakje in de oude ervaring. Voor federatieve tenants ziet deze prompt nadat u met de federatieve service verifiëren.

Zie voor meer informatie [minder prompts aanmelden: de nieuwe ervaring 'aangemeld blijven' voor Azure AD is een Preview-versie](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Configuratie om ervoor te zorgen de gebruiksvoorwaarden worden uitgebreid voor het accepteren van toevoegen

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** naleving
 
Een optie voor beheerders moet hun gebruikers uit te breiden de gebruiksvoorwaarden voorafgaand aan de voorwaarden accepteren.

Selecteer een **op** of **uit** gebruikers moeten de gebruiksvoorwaarden uitbreiden. De **op** instelling moet gebruikers om weer te geven van de gebruiksrechtovereenkomst voordat ze worden geaccepteerd.

Zie voor meer informatie de [Azure AD-voorwaarden van de functie gebruiken (preview)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Bereik activering voor in aanmerking komende roltoewijzingen

**Type:** nieuwe functie  
**Servicecategorie:** Privileged Identity Management  
**Product mogelijkheid:** Privileged Identity Management
 
Roltoewijzingen in aanmerking komende Azure-resource met minder autonomie dan de oorspronkelijke toewijzing standaardinstellingen activeren kunt u binnen het bereik activering. Een voorbeeld is als u als eigenaar van een abonnement in uw tenant krijgt. Met een bereik-activering, kunt u de rol van eigenaar voor maximaal vijf bronnen die zich bevinden in het abonnement (zoals resourcegroepen en virtuele machines) activeren. De activering scoping mogelijk beperkt u de kans van de uitvoering van ongewenste wijzigingen in kritieke Azure-resources.

Zie voor meer informatie [wat is Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Nieuwe federatieve apps in de galerie van Azure AD-app

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke apps  
**Product mogelijkheid:** integratie van derden

In December 2017 toegevoegd ondersteuning voor deze nieuwe apps met Federatie met onze app-galerie:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD integration](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD Integration.

Zie voor meer informatie over de apps [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de galerie van Azure AD app [uw toepassing vermelden in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Werkstromen voor goedkeuring voor Azure AD-directory-functies

**Type:** gewijzigde functie  
**Servicecategorie:** Privileged Identity Management  
**Product mogelijkheid:** Privileged Identity Management
 
Werkstroom voor goedkeuring voor Azure AD-directory-functies is algemeen beschikbaar.

Met de werkstroom voor het goedkeuren van in aanmerking komende rolleden activering kunnen aanvragen rol op bevoegde rol beheerders kunnen vereisen voordat de bevoorrechte rol kan worden gebruikt. Meerdere gebruikers en groepen kunnen worden gedelegeerd goedkeuring verantwoordelijkheden. Leden van een in aanmerking komende rol ontvangen meldingen wanneer goedkeuring is voltooid en hun rol actief is.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Pass through-verificatie: Skype voor bedrijven

**Type:** gewijzigde functie  
**Servicecategorie:** Authenticaties (aanmeldingen)  
**Product mogelijkheid:** gebruikersverificatie

Nu Pass through-verificatie ondersteunt gebruikersaanmeldingen tot Skype voor bedrijven-clienttoepassingen die ondersteuning bieden voor moderne verificatie, waaronder online en hybride topologieën. 

Zie voor meer informatie [Skype voor bedrijven-topologieën ondersteund met moderne verificatie](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Updates voor Azure AD Privileged Identity Management voor Azure RBAC (preview)

**Type:** gewijzigde functie  
**Servicecategorie:** Privileged Identity Management  
**Product mogelijkheid:** Privileged Identity Management
 
Met het vernieuwen van de openbare preview van Azure AD Privileged Identity Management (PIM) voor gebaseerd toegangsbeheer (RBAC) kunt u nu:

* Just Enough Administration gebruiken.
* Goedkeuring vereist voor het resourcerollen activeren.
* Plannen van een rol die moet worden goedgekeurd voor beide Azure AD en Azure RBAC-rollen een toekomstige geactiveerd.
 
Zie voor meer informatie [Privileged Identity Management voor Azure-resources (preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>November 2017
 
### <a name="access-control-service-retirement"></a>Access Control service buiten gebruik stellen

**Type:** wijzigingen  
**Servicecategorie:** Access Control-service  
**Product mogelijkheid:** Access Control-service 

Azure Active Directory-toegangsbeheer (ook wel bekend als de Access Control-service) wordt in latere 2018 buiten gebruik worden gesteld. Meer informatie een gedetailleerde planning en op hoog niveau migratie-instructies bevat worden vermeld in de volgende enkele weken. U kunt opmerkingen op deze pagina verlaten met eventuele vragen over de Access Control-service en een teamlid kan deze worden beantwoord.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>De browsertoegang tot de Intune Managed Browser beperken 

**Type:** wijzigingen  
**Servicecategorie:** voorwaardelijke toegang  
**Product mogelijkheid:** identiteit beveiliging en bescherming

U kunt browsertoegang tot Office 365 en andere Azure AD verbonden cloud-apps beperken met behulp van de Intune Managed Browser als een goedgekeurde app. 

U kunt nu de volgende voorwaarde voor voorwaardelijke toegang op basis van een toepassing configureren:

**Client-apps:** Browser

**Wat is het effect van de wijziging?**

Vandaag de dag toegang geblokkeerd wanneer u deze voorwaarde gebruiken. Wanneer de Preview-versie beschikbaar is, wordt alle toegang tot het gebruik van de toepassing van de beheerde browser vereisen. 

Zoek naar deze functionaliteit en meer informatie in toekomstige blogs en release-opmerkingen. 

Zie voor meer informatie [voorwaardelijke toegang in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde client-apps voor voorwaardelijke toegang van Azure AD op basis van een app

**Type:** wijzigingen  
**Servicecategorie:** voorwaardelijke toegang  
**Product mogelijkheid:** identiteit beveiliging en bescherming

De volgende apps zijn op de lijst met [client-apps goedgekeurd](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://microsoft.com/en-us/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

Zie voor meer informatie:

- [Goedgekeurde app vereiste](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD app gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Gebruiksvoorwaarden van ondersteuning voor meerdere talen

**Type:** nieuwe functie    
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** naleving

Beheerders kunnen nu nieuwe gebruiksvoorwaarden die meerdere PDF-documenten bevatten maken. U kunt deze PDF-documenten met een overeenkomstige taal labelen. Gebruikers met de overeenkomstige taal op basis van hun voorkeuren het PDF-bestand weergegeven. Als er geen overeenkomst, wordt de standaardtaal weergegeven.

---
 

### <a name="real-time-password-writeback-client-status"></a>Realtime wachtwoord terugschrijven clientstatus

**Type:** nieuwe functie  
**Servicecategorie:** selfservice voor wachtwoordherstel  
**Product mogelijkheid:** gebruikersverificatie

U kunt nu de status van uw lokale wachtwoord terugschrijven client bekijken. Deze optie is beschikbaar in de **On-premises integratie** sectie van de [wachtwoordherstel](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) pagina. 

Als er problemen met de verbinding met uw lokale Write-back-client zijn, ziet u een foutbericht weergegeven dat beschikt u over:

- Informatie over waarom u geen verbinding met uw lokale Write-back-client.
- Een koppeling naar de documentatie die u helpt bij het oplossen van het probleem. 

Zie voor meer informatie [on-premises integratie](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD app gebaseerde voorwaardelijke toegang 
 
**Type:** nieuwe functie  
**Servicecategorie:** Azure AD  
**Product mogelijkheid:** identiteit beveiliging en bescherming

Nu kunt u toegangsbeperkingen voor Office 365 en andere Azure AD verbonden cloud-apps naar [client-apps goedgekeurd](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) die ondersteuning bieden voor beveiligingsbeleid voor Intune-app met behulp van [voorwaardelijke toegang van Azure AD op basis van een app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam). Intune app beveiliging beleidsregels worden gebruikt om te configureren en beveiligen van bedrijfsgegevens op deze clienttoepassingen.

Door te combineren [op basis van een app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) met [op basis van apparaten](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) beleid voor voorwaardelijke toegang hebt u de flexibiliteit om gegevens voor persoonlijke en zakelijke apparaten te beveiligen.

De volgende voorwaarden en besturingselementen zijn nu beschikbaar voor gebruik met voorwaardelijke toegang op basis van een app:

**Ondersteund platform voorwaarde**

- iOS
- Android

**Voorwaarde voor client-apps**

- Mobiele apps en bureaubladclients

**Toegangsbeheer**

- Goedgekeurde client-apps vereisen

Zie voor meer informatie [voorwaardelijke toegang van Azure AD op basis van een app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure AD-apparaten in de Azure Portal beheren

**Type:** nieuwe functie  
**Servicecategorie:** apparaatregistratie en beheer  
**Product mogelijkheid:** identiteit beveiliging en bescherming

Nu kunt u uw apparaten die zijn verbonden met Azure AD en de activiteiten die betrekking hebben op apparaten op één plek. Er is een nieuwe beheerervaring voor het beheren van uw apparaat-id's en instellingen in de Azure-portal. U kunt in deze release:

- Alle apparaten die beschikbaar voor voorwaardelijke toegang in Azure AD zijn weergeven.
- Eigenschappen, waaronder uw hybride Azure weergeven die lid zijn van AD-apparaten.
- BitLocker-sleutels voor uw Azure AD-die lid zijn van apparaten zoeken, beheren van uw apparaat met Intune en meer.
- Apparaat-gerelateerde Azure AD-instellingen beheren.

Zie voor meer informatie [apparaten beheren met behulp van de Azure-portal](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Ondersteuning voor Mac OS als een apparaatplatform voor voorwaardelijke toegang van Azure AD 

**Type:** nieuwe functie    
**Servicecategorie:** voorwaardelijke toegang  
**Product mogelijkheid:** identiteit beveiliging en bescherming 

U kunt nu opnemen of uitsluiten Mac OS als een voorwaarde voor het platform van apparaten in uw Azure AD-beleid voor voorwaardelijke toegang. U kunt met de toevoeging van Mac OS aan de ondersteunde platforms voor apparaten:

- **Registreren en Mac OS-apparaten beheren met Intune.** Net als bij andere platforms, zoals iOS en Android, een portaltoepassing bedrijf is beschikbaar voor Mac OS unified inschrijvingen doen. U kunt de nieuwe bedrijfsportal-app voor Mac OS gebruiken op een apparaat inschrijven bij Intune en registreren met Azure AD.
- **Zorg ervoor dat Mac OS apparaten voldoen aan de beleidsregels voor naleving van uw organisatie gedefinieerd in Intune.** In Intune in de Azure portal kunt kunt u nu instellen nalevingsbeleid voor apparaten met Mac OS. 
- **Toegang tot toepassingen in Azure AD om alleen compatibele Mac OS-apparaten te beperken.** Voorwaardelijk beleid ontwerpen heeft Mac OS als een afzonderlijk apparaat platform-optie. Nu kunt u Mac OS-specifieke voorwaardelijk toegangsbeleid voor de beoogde applicatie instellen in Azure schrijven.

Zie voor meer informatie:

- [Een apparaatnalevingsbeleid maken voor macOS-apparaten in Intune](https://aka.ms/macoscompliancepolicy)
- [Voorwaardelijke toegang in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Network Policy Server-extensie voor Azure multi-factor Authentication 

**Type:** nieuwe functie    
**Servicecategorie:** multi-factor authentication  
**Product mogelijkheid:** gebruikersverificatie

De Network Policy Server-extensie voor Azure multi-factor Authentication wordt cloud-gebaseerde multi-factorauthenticatie mogelijkheden toegevoegd aan uw verificatie-infrastructuur met behulp van uw bestaande servers. U kunt met de extensie Network Policy Server telefoongesprek, tekstbericht of verificatie via de telefoon-app toevoegen aan uw bestaande verificatiestroom. U hoeft niet te installeren, configureren en onderhouden van nieuwe servers. 

Deze uitbreiding is gemaakt voor organisaties die beveiligen van VPN-verbindingen willen zonder de Azure multi-factor Authentication-Server implementeren. Network Policy Server extensie als een adapter tussen RADIUS- en cloud-gebaseerde Azure multi-factor Authentication fungeert voor een tweede factor van verificatie voor federatieve of gesynchroniseerde gebruikers.


Zie voor meer informatie [uw bestaande infrastructuur van Network Policy Server integreren met Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Herstellen of verwijderde gebruikers permanent verwijderen

**Type:** nieuwe functie    
**Servicecategorie:** Gebruikersbeheer  
**Product mogelijkheid:** Directory 


In het Azure AD-beheercentrum kunt u nu:

- Een verwijderde gebruiker herstellen. 
- Een gebruiker permanent te verwijderen.


**Uitproberen:**

1. Selecteer in het Azure AD-beheercentrum, [alle gebruikers](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) in de **beheren** sectie. 

2. Van de **weergeven** selecteert **onlangs verwijderd gebruikers**. 

3. Selecteer een of meer van de onlangs verwijderde gebruikers en ofwel herstel ze dan of deze permanent verwijderen.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde client-apps voor voorwaardelijke toegang van Azure AD op basis van een app

 
**Type:** gewijzigde functie  
**Servicecategorie:** voorwaardelijke toegang  
**Product mogelijkheid:** identiteit beveiliging en bescherming


De volgende apps zijn toegevoegd aan de lijst met [client-apps goedgekeurd](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 


Zie voor meer informatie:

- [Goedgekeurde app vereiste](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD app gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Gebruik 'Of' tussen besturingselementen in een beleid voor voorwaardelijke toegang 


**Type:** gewijzigde functie    
**Servicecategorie:** voorwaardelijke toegang  
**Product mogelijkheid:** identiteit beveiliging en bescherming

 
U nu kunt gebruiken ' of ' (een van de geselecteerde besturingselementen vereisen) voor besturingselementen van voorwaardelijke toegang. U kunt deze functie gebruiken om beleid te maken met 'of' tussen toegangsbeheer. Bijvoorbeeld, kunt u deze functie voor het maken van een beleid dat een gebruiker aan te melden met behulp van multi-factor authentication 'of' op een compatibel apparaat vereist.

Zie voor meer informatie [besturingselementen in voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).

 
---

### <a name="aggregation-of-real-time-risk-events"></a>Aggregatie van realtime risico 's


**Type:** gewijzigde functie    
**Servicecategorie:** Identity protection  
**Product mogelijkheid:** identiteit beveiliging en bescherming


In Azure AD Identity Protection worden nu alle realtime risicogebeurtenissen die afkomstig van hetzelfde IP-adres op een bepaalde dag zijn samengevoegd voor elk type risico gebeurtenis. Deze wijziging beperkt de omvang van de risicogebeurtenissen die worden weergegeven zonder wijziging in de gebruikersbeveiliging.

De onderliggende realtime detectie werkt elke keer dat de gebruiker zich aanmeldt. Als er een aanmeldingspagina risico beveiligingsbeleid instellen voor multi-factor authentication of de toegang blokkeert, is het nog steeds geactiveerd tijdens elke riskant aanmelden.

 
---
 




## <a name="october-2017"></a>Oktober 2017


### <a name="deprecate-azure-ad-reports"></a>Afschaffen Azure AD-rapporten


**Type:** wijzigingen  
**Servicecategorie:** rapportage  
**Product mogelijkheid:** Identity Lifecycle Management  



De Azure-portal beschikt u over:

- Een nieuwe Azure AD-beheerconsole.
- Nieuwe API's voor de activiteit en beveiliging rapporten.
 
Als gevolg van deze nieuwe functies zijn het rapport API's onder het eindpunt/Reports op 10 December 2017 is ingetrokken. 

---

### <a name="automatic-sign-in-field-detection"></a>Detectie van het veld voor automatische aanmelding


**Type:** vast   
**Servicecategorie:** mijn Apps  
**Product mogelijkheid:** eenmalige aanmelding  



Azure AD biedt ondersteuning voor automatische aanmelding veld detectie voor toepassingen die een gebruiker HTML-veld naam en het wachtwoord genereren. Deze stappen zijn gedocumenteerd in [automatisch vastleggen velden aanmelden voor een toepassing](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). U vindt deze mogelijkheid door toe te voegen een *Non-galerie* toepassing op de **bedrijfstoepassingen** pagina in de [Azure-portal](http://aad.portal.azure.com). Bovendien kunt u de **Single Sign-on** modus op deze nieuwe toepassing **op basis van wachtwoorden Single Sign-on**, voer een URL en sla de pagina.
 
Deze functionaliteit is vanwege een serviceprobleem tijdelijk uitgeschakeld. Het probleem is opgelost en de detectie automatisch aanmelden veld weer beschikbaar is.

---

### <a name="new-multi-factor-authentication-features"></a>Nieuwe functies van multi-factor authentication-server


**Type:** nieuwe functie  
**Servicecategorie:** multi-factor authentication  
**Product mogelijkheid:** identiteit beveiliging en bescherming  



Multi-factor authentication (MFA) is een essentieel onderdeel van het beveiligen van uw organisatie. Als u meer geavanceerde referenties en de ervaring meer naadloze, zijn de volgende functies toegevoegd: 

- Resultaten van meerdere factoren uitdaging zijn rechtstreeks geïntegreerd in de Azure AD-in rapport, waaronder programmatisch toegang biedt tot MFA resultaten.
- De MFA-configuratie is meer nauw geïntegreerd in de configuratie van Azure AD optreden in de Azure-portal.

Met deze openbare preview MFA beheer- en rapportageopties een geïntegreerde deel uitmaken van de ervaring van de core Azure AD-configuratie. U kunt nu de portal beheerfunctionaliteit MFA binnen de ervaring van de Azure AD beheren.

Zie voor meer informatie [verwijzing voor het melden van MFA in de Azure portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 


---

### <a name="terms-of-use"></a>Gebruiksvoorwaarden



**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** naleving  



U kunt Azure AD gebruiksvoorwaarden met informatie zoals de relevante vrijwaringen voor juridische of naleving vereisten voor gebruikers.

U kunt Azure AD gebruiksvoorwaarden gebruiken in de volgende scenario's:

- Algemene gebruiksvoorwaarden voor alle gebruikers in uw organisatie
- Specifieke gebruiksvoorwaarden op basis van een gebruiker kenmerken (bijvoorbeeld arts versus verpleegkundigen) of binnenlandse versus internationale werknemers, uitgevoerd door dynamische groepen
- Gebruiksvoorwaarden specifieke voor toegang tot hoge impact business-apps, zoals Salesforce

Zie voor meer informatie [Azure AD gebruiksvoorwaarden](https://docs.microsoft.com/azure/active-directory/active-directory-tou).


---

### <a name="enhancements-to-privileged-identity-management"></a>Verbeteringen in Privileged Identity Management


**Type:** nieuwe functie  
**Servicecategorie:** Privileged Identity Management  
**Product mogelijkheid:** Privileged Identity Management  


U kunt met Azure AD Privileged Identity Management, beheren en toegang tot Azure-resources (preview) binnen uw organisatie te controleren:

- Abonnementen
- Resourcegroepen
- Virtuele machines 

Alle resources binnen de Azure-portal met de Azure RBAC-functionaliteit kunnen profiteren van de beveiliging en de levenscyclus van beheermogelijkheden voor Azure AD Privileged Identity Management te bieden heeft.

Zie voor meer informatie [Privileged Identity Management voor Azure-resources](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).


---

### <a name="access-reviews"></a>Toegangsbeoordelingen


**Type:** nieuwe functie  
**Servicecategorie:** beoordelingen openen  
**Product mogelijkheid:** naleving  



Organisaties kunnen toegang beoordelingen (preview) gebruiken voor het efficiënt groepslidmaatschappen en toegang tot zakelijke toepassingen beheren: 

- U kunt toegang voor gastgebruikers opnieuw certificeren door gebruik te maken van toegangsbeoordelingen van hun toegang tot toepassingen en lidmaatschappen van groepen. Revisoren bepalen efficiënt of gasten toestaan toegang op basis van de inzichten die is geleverd door de beoordelingen toegang overgenomen.
- Met toegangsbeoordelingen kunt u de toegang van werknemers voor toepassingen en groepslidmaatschappen opnieuw certificeren.

U kunt de controles van toegangsbeoordelingen verzamelen in programma's die relevant zijn voor uw organisatie, om beoordelingen bij te houden voor naleving of risicogevoelige toepassingen.

Zie voor meer informatie [beoordeelt de toegang van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Toepassingen van derden van mijn Apps en de Office 365 app linksboven verbergen



**Type:** nieuwe functie  
**Servicecategorie:** mijn Apps  
**Product mogelijkheid:** eenmalige aanmelding  



Nu kunt u beter apps beheren die worden weergegeven voor uw gebruikers portals via een nieuwe **app verbergen** eigenschap. U kunt apps om in gevallen waarin app-tegels voor back-end-services of dubbele tegels en overzichtelijker gebruikers app voor ruimtevaartuigen weergegeven te verbergen. De wisselknop is in de **eigenschappen** sectie van de app van derden en label **zichtbaar voor gebruiker?** U kunt ook een app via een programma via PowerShell verbergen. 

Zie voor meer informatie [verbergen van een toepassing van derden van de gebruikerservaring in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Wat is er beschikbaar?**

 Als onderdeel van de overgang naar de nieuwe beheerconsole twee nieuwe API's voor het ophalen van Azure AD-activiteit logboeken beschikbaar zijn. De nieuwe set API's biedt uitgebreidere filteren en sorteren op de functionaliteit naast het bieden van uitgebreidere audit en aanmelden activiteiten. De eerder beschikbaar via de beveiligingsrapporten nu gegevens zijn toegankelijk via de API Identity Protection risico gebeurtenissen in Microsoft Graph.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-identity-manager"></a>Hotfix voor Identity Manager


**Type:** gewijzigde functie  
**Servicecategorie:** Identity Manager  
**Product mogelijkheid:** Identity lifecycle management  



Een totaliseren hotfixpakket (build 4.4.1642.0) is beschikbaar vanaf 25 September 2017 voor Identity Manager 2016 Service Pack 1. Dit updatepakket:

- Problemen opgelost en verbeteringen toegevoegd.
- Is een cumulatieve update die wordt vervangen door alle Identity Manager 2016 Service Pack 1-updates maximaal build 4.4.1459.0 voor Identity Manager 2016. 
- Moet u Identity Manager 2016 bouwen 4.4.1302.0 hebben. 

Zie voor meer informatie [hotfixpakket (build 4.4.1642.0) is beschikbaar voor Identity Manager 2016 Service Pack 1](https://support.microsoft.com/help/4021562). 

---
