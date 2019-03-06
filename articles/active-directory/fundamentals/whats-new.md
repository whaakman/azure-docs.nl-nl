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
ms.date: 02/28/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c34cd59a5f4636eeca395fde49e45beb1a49752
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57408884"
---
# <a name="whats-new-in-azure-active-directory"></a>Wat is er nieuw in Azure Active Directory?

>Blijf op de hoogte over wanneer u terug naar deze pagina voor updates kopiëren en plakken van deze URL: `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` in uw ![RSS-pictogram](./media/whats-new/feed-icon-16x16.png) reader-feed.

Azure AD ontvangt verbeteringen regelmatig. Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

- De meest recente versies
- Bekende problemen
- Opgeloste fouten
- Afgeschafte functies
- Plannen voor wijzigingen

Deze pagina wordt maandelijks bijgewerkt, dus regelmatig bezoekt. Als u naar items die ouder dan zes maanden zoekt zijn, kunt u vinden in de [archief voor de wat is er nieuw in Azure Active Directory](whats-new-archive.md).

---
## <a name="february-2019"></a>Februari 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Configureerbare Azure AD-SAML-tokenversleuteling (openbare preview) 

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding
 
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
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In januari 2019, hebben we deze 27 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT VINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [ IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), Machtiging klikken, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [seismische ](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [Delen een droom](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [webMethods integratie Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [kennis overal LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [Organisatie-eenheid Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope gegevens](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [Netop Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [PureCloud door Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [ClickUp productiviteit Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="enhanced-combined-mfasspr-registration"></a>Verbeterde gecombineerde MFA/SSPR-registratie

**Type:** Gewijzigde functie  
**Categorie van de service:** Selfservice voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie
 
In reactie op feedback van klanten, hebben we de gecombineerde MFA/SSPR-registratie preview-ervaring, uitgebreid uw gebruikers helpen bij het sneller hun beveiligingsgegevens registreren voor MFA en SSPR. 

**Als u wilt inschakelen op de verbeterde ervaring voor uw gebruikers vandaag, de volgende stappen uit:**

1. Als een globale beheerder of Gebruikerbeheerder, moet u zich aanmelden bij de Azure-portal en gaat u naar **Azure Active Directory > Instellingen > instellingen voor toegang tot deelvenster preview-functies beheren**. 

2. In de **gebruikers dat toegang heeft tot de preview-functies voor het registreren en beheren van beveiligingsgegevens: vernieuwen** optie, kiest u om in te schakelen op de functies voor een **geselecteerde groep gebruikers** of voor **alle gebruikers** .

Het volgende aantal weken duurt, zullen we de mogelijkheid om in te schakelen op de oude gecombineerde MFA/SSPR-registratie preview-ervaring voor tenants die nog niet ingeschakeld hebt op verwijderen.

**Als u wilt zien als het besturingselement voor uw tenant, worden verwijderd, de volgende stappen uit:**

1. Als een globale beheerder of Gebruikerbeheerder, moet u zich aanmelden bij de Azure-portal en gaat u naar **Azure Active Directory > Instellingen > instellingen voor toegang tot deelvenster preview-functies beheren**.  

2.  Als de **gebruikers die de preview-functies gebruiken kunnen voor het registreren en beheren van beveiligingsgegevens** optie is ingesteld op **geen**, de optie wordt verwijderd uit uw tenant.

Preview-ervaring voor gebruikers, ongeacht of u eerder hebt ingeschakeld op de oude gecombineerde MFA/SSPR-registratie of niet, de oude ervaring worden uitgeschakeld op een later tijdstip. Vanwege die sterk het is raadzaam dat u naar de nieuwe, verbeterde ervaring zo snel mogelijk verplaatsen.

Zie voor meer informatie over de verbeterde registratie-ervaring, de [Cool verbeteringen in de Azure AD MFA gecombineerd en registratie-ervaring voor wachtwoordherstel](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271).

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Bijgewerkte beleid beheerervaring voor gebruikersstromen

**Type:** Gewijzigde functie  
**Categorie van de service:** B2C - Consumentenidentiteitsbeheer  
**Product-mogelijkheid:** B2B/B2C

We hebben het beleid maken en beheren van het proces voor het gebruikersstromen (voorheen bekend als, ingebouwde beleidsregels) eenvoudiger bijgewerkt. Deze nieuwe ervaring is nu de standaard voor al uw Azure AD-tenants.

U kunt aanvullende feedback en suggesties geven met behulp van de glimlach of Boog omlaag pictogrammen in de **Stuur ons feedback** gebied aan de bovenkant van het scherm.

Zie voor meer informatie over de nieuwe beheerervaring van beleid, de [JavaScript-aanpassingen en veel meer nieuwe functies van Azure AD B2C heeft nu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blog.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Kies specifieke pagina element versies geleverd door Azure AD B2C

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - Consumentenidentiteitsbeheer  
**Product-mogelijkheid:** B2B/B2C

U kunt nu een specifieke versie van de pagina-elementen die is geleverd door Azure AD B2C. Als u een specifieke versie selecteert, kunt u uw wijzigingen testen voordat ze op een pagina weergegeven en u kunt voorspelbaar gedrag. Bovendien kunt u nu zich om af te dwingen van specifieke paginaversies om toe te staan van JavaScript-aanpassingen. Als u wilt deze functie inschakelen, gaat u naar de **eigenschappen** pagina in uw gebruikersstromen.

Zie voor meer informatie over het kiezen van specifieke versies van pagina-elementen, de [JavaScript-aanpassingen en veel meer nieuwe functies van Azure AD B2C heeft nu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) blog.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>Vereisten voor wachtwoorden kunnen worden geconfigureerd door eindgebruikers voor B2C (GA)

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - Consumentenidentiteitsbeheer  
**Product-mogelijkheid:** B2B/B2C

U kunt nu specifiek instellen van uw organisatie wachtwoordcomplexiteit voor uw eindgebruikers, in plaats van gebruik te maken van uw eigen Azure AD-wachtwoordbeleid. Uit de **eigenschappen** blade van uw stromen (voorheen bekend als de ingebouwde beleidsregels), kunt u een wachtwoordcomplexiteit van **eenvoudige** of **sterke**, of u kunt Maak een **aangepaste** set vereisten.

Zie voor meer informatie over de vereiste configuratie van wachtwoord complexiteit [complexiteitsvereisten voldoen voor wachtwoorden configureren in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Nieuwe standaardsjablonen voor aangepaste huisstijl authenticatie-ervaringen

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - Consumentenidentiteitsbeheer  
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

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Nieuwe cookie-instellingen Azure AD-toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Toegangsbeheer

We hebben drie nieuwe cookie-instellingen, beschikbaar voor uw apps die worden gepubliceerd via toepassingsproxy geïntroduceerd:

- **Gebruik alleen HTTP-cookie.** Stelt de **HTTPOnly** vlag aan de Application Proxy toegangs- en sessiebeleid cookies. Deze instelling inschakelt, biedt extra voordelen, zoals het helpen om te voorkomen dat kopiëren of wijzigen van cookies met behulp van client-side '-scripts. We raden u deze vlag inschakelt (Kies **Ja**) voor de extra voordelen.

- **Veilige cookie gebruiken.** Stelt de **Secure** vlag aan de Application Proxy toegangs- en sessiebeleid cookies. Deze instelling inschakelt, biedt de voordelen van de extra beveiliging, door ervoor te zorgen dat cookies worden alleen verzonden via TLS beveiligde kanalen, zoals HTTPS. We raden u deze vlag inschakelt (Kies **Ja**) voor de extra voordelen.

- **Permanente cookie gebruiken.** Hiermee voorkomt dat toegang cookies verlopen wanneer de webbrowser wordt gesloten. Deze cookies laatste gedurende de levensduur van het toegangstoken. Cookies worden echter opnieuw ingesteld als de vervaltijd is bereikt of als de gebruiker de cookie voor het handmatig verwijdert. We raden u aan de standaardinstelling **Nee**, alleen de instelling voor oudere apps die geen cookies tussen processen delen inschakelen.

Zie voor meer informatie over de nieuwe cookies [Cookie-instellingen voor toegang tot on-premises toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Er zijn nieuwe federatieve apps beschikbaar in de app-galerie van Azure AD: januari 2019

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In januari 2019, hebben we deze 35 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Talent palet](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco overkoepelende](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Beheerder van Internet](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [vervaldatum herinnering](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Viewer](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [term](https://app.verb.net/login), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Pavaso digitale sluit](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [ Cloud Service PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO-systeem](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [Are voor Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [K2 voor Office 365](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn veerboot ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Er zijn nieuwe uitbreidingen voor Azure AD Identity Protection (openbare preview)

**Type:** Gewijzigde functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming

We zijn trots te kunnen aankondigen dat er de volgende verbeteringen is toegevoegd aan de aanbieding van Azure AD Identity Protection preview-versie, met inbegrip van:

- Een bijgewerkte en meer geïntegreerde gebruikersinterface

- Aanvullende API's

- Verbeterde risico-evaluatie van machine Learning

- Uitlijning van de gehele product voor riskante gebruikers en riskante aanmeldingen

Zie voor meer informatie over de verbeteringen voor [wat is Azure Active Directory Identity Protection (vernieuwd)?](https://aka.ms/IdentityProtectionDocs) voor meer informatie en deel uw ideeën tot en met de stappen in het product.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>Nieuwe functie App-vergrendeling voor de Microsoft Authenticator-app op iOS- en Android-apparaten

**Type:** Nieuwe functie  
**Categorie van de service:** Microsoft Authenticator-App  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming

Uw wachtwoordcodes eenmalig, app-gegevens en app-instellingen om veiliger te houden, kunt u de App-Lock-functie in de Microsoft Authenticator-app inschakelen. Inschakelen van App-vergrendeling betekent dat u waarschijnlijk gevraagd verifiëren met behulp van uw PINCODE of biometrische telkens wanneer u de Microsoft Authenticator-app opent.

Zie voor meer informatie de [Microsoft Authenticator-app Veelgestelde vragen over](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Er zijn verbeterde exportmogelijkheden in Azure AD Privileged Identity Management (PIM)

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management

Privileged Identity Management (PIM)-beheerders kunnen nu alle actieve en in aanmerking komende roltoewijzingen voor een specifieke bron, waaronder roltoewijzingen voor alle onderliggende resources exporteren. Voorheen was het moeilijk voor beheerders om een volledige lijst van roltoewijzingen voor een abonnement en ze had roltoewijzingen voor elke specifieke resource te exporteren.

Zie voor meer informatie, [activiteit en audit geschiedenis weergeven voor Azure-resource-rollen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

## <a name="novemberdecember-2018"></a>November/December 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Gebruikers die zijn verwijderd uit synchronisatiebereik worden niet langer naar alleen-cloudaccounts overgeschakeld

**Type:** Vast  
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
**Product-mogelijkheid:** Controleren en rapporteren

We zijn momenteel log-streams voor zowel de controle en meld u via Azure Monitor, publiceren, zodat u de logboekbestanden naadloos met uw SIEM-hulpprogramma's of met Log Analytics integreren kunt. Op basis van uw feedback en ter voorbereiding van de aankondiging van de algemene beschikbaarheid van deze functie, doorvoeren we de volgende wijzigingen om onze schema. Deze wijzigingen in het schema en de bijbehorende documentatie-updates wordt uitgevoerd op de eerste week van januari.

#### <a name="new-fields-in-the-audit-schema"></a>Nieuwe velden in het schema controleren
We voegen een nieuwe toe **bewerkingstype** veld voor het type bewerking uitgevoerd op de resource. Bijvoorbeeld, **toevoegen**, **Update**, of **verwijderen**.

#### <a name="changed-fields-in-the-audit-schema"></a>Gewijzigde velden in het schema controleren
De volgende velden wijzigt in het controle-schema:

|Veldnaam|Wat is gewijzigd|Oude waarden|Nieuwe waarden|
|----------|------------|----------|----------|
|Categorie|Dit is de **servicenaam** veld. Het is nu de **Audit categorieën** veld. **Servicenaam** is gewijzigd in de **loggedByService** veld.|<ul><li>Account inrichten</li><li>Hoofddirectory</li><li>Self-service voor wachtwoord opnieuw instellen</li></ul>|<ul><li>Gebruikersbeheer</li><li>Groepsbeheer</li><li>App-beheer</li></ul>|
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

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Identity Protection-verbeteringen voor het Machine Learning-model en de risicoscore-engine

**Type:** Gewijzigde functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Risicoscores

Verbeteringen in de Identity Protection-gerelateerde gebruiker en meld u risico-evaluatie-engine kunnen helpen om gebruiker risico nauwkeurigheid en dekking te verbeteren. Beheerders kunnen u ziet dat risiconiveau van de gebruiker niet meer rechtstreeks is gekoppeld aan het risiconiveau van specifieke detecties, en of er een toename van het aantal en het niveau van riskante aanmelding-gebeurtenissen.

Detecties van risico's worden nu geëvalueerd door de bewaakte machine learning-model, welke gebruikers risico's berekent met behulp van een patroon van detecties en extra functies van de aanmeldingen van de gebruiker. Op basis van dit model, kan de beheerder van de gebruikers met een hoog risicoscores, zoeken, zelfs als detecties die zijn gekoppeld aan deze gebruiker van laag of Gemiddeld risico zijn. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Beheerders kunnen hun eigen wachtwoord opnieuw instellen met de Microsoft Authenticator-app (openbare preview)

**Type:** Gewijzigde functie  
**Categorie van de service:** Selfservice voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie

Azure AD-beheerders kunnen nu opnieuw instellen voor hun eigen wachtwoord met behulp van de Microsoft Authenticator-app-meldingen of een code van een mobiele verificator-app of de hardware-token. Om hun eigen wachtwoord opnieuw instellen, is beheerders nu mogelijk gebruik van twee van de volgende methoden:

- Microsoft Authenticator-app-melding

- Andere mobiele verificator-app / Hardware token code

- Email

- Telefoonoproep

- Sms-bericht

Zie voor meer informatie over het gebruik van de Microsoft Authenticator-app opnieuw instellen van wachtwoorden [Azure AD Self-service voor wachtwoord opnieuw instellen - mobiele app en SSPR (Preview)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr-preview)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Nieuwe rol van Azure AD-cloudapparaatbeheerder (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Apparaatregistratie en -beheer  
**Product-mogelijkheid:** Toegangsbeheer

Beheerders kunnen gebruikers toewijzen aan de nieuwe rol in de Cloud-Apparaatbeheerder cloud apparaat beheerderstaken uit te voeren. Gebruikers die zijn toegewezen de beheerdersrol van Cloud-apparaat kunnen inschakelen, uitschakelen en verwijderen van apparaten in Azure AD, samen met kunnen Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal worden gelezen.

Zie voor meer informatie over rollen en machtigingen, [beheerdersrollen toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Apparaten beheren met de nieuwe activiteitstijdstempel in Azure AD (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Apparaatregistratie en -beheer  
**Product-mogelijkheid:** Levenscyclusbeheer voor apparaten

We realiseren ons dat na verloop van tijd moet u vernieuwen en is voor uw organisatie apparaten buiten gebruik stellen in Azure AD, om te voorkomen dat verouderde apparaten die zijn vastgelopen om in uw omgeving. Om te helpen met dit proces, werkt nu Azure AD uw apparaten met een nieuwe activiteit timestamp, helpen u bij het beheren van de levenscyclus van uw apparaat.

Zie voor meer informatie over het verkrijgen en gebruiken van deze timestamp [How To: De verouderde apparaten beheren in Azure AD](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Beheerders kunnen gebruikers verplichten om gebruiksvoorwaarden op elk apparaat te accepteren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer
 
Beheerders kunnen nu inschakelen op de **vereisen dat gebruikers accepteren op elk apparaat** optie uw gebruikers moeten accepteer de gebruiksvoorwaarden op elk apparaat dat wordt gebruikt op uw tenant.

Zie voor meer informatie de [Per apparaat en de gebruiksvoorwaarden gedeelte van de Azure Active Directory-voorwaarden van functie gebruiken](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Beheerders kunnen gebruiksvoorwaarden zo configureren dat deze verlopen op basis van een terugkerend schema

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer
 

Beheerders kunnen nu inschakelen op de **verlopen toestemmingen** optie om te maken van een gebruiksrechtovereenkomst verlopen voor alle gebruikers op basis van de opgegeven terugkerende planning. Het schema mag per jaar, bi per jaar, kwartaal of per maand. Nadat de gebruiksvoorwaarden is verlopen, moeten gebruikers accepteren.

Zie voor meer informatie de [voorwaarden toevoegen van gedeelte van de Azure Active Directory-voorwaarden van functie gebruiken](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Beheerders kunnen een gebruiksrechtovereenkomst verloopt op basis van de planning van elke gebruiker configureren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

Beheerders kunnen nu opgeven voor een duur van die gebruiker moet de gebruiksvoorwaarden accepteren. Beheerders kunnen bijvoorbeeld opgeven dat een gebruiksrechtovereenkomst om de 90 dagen door gebruikers moeten accepteren.

Zie voor meer informatie de [voorwaarden toevoegen van gedeelte van de Azure Active Directory-voorwaarden van functie gebruiken](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use).
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Nieuwe e-mailberichten van Azure AD Privileged Identity Management (PIM) voor Azure Active Directory-rollen

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

### <a name="group-based-licensing-is-now-generally-available"></a>Licentieverlening op basis van groepen is nu algemeen beschikbaar

**Type:** Gewijzigde functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Directory

Groepslicenties is uit de openbare preview en is nu algemeen beschikbaar. Als onderdeel van deze algemene versie is uitgebracht, we deze functie beter schaalbaar hebt gemaakt en de mogelijkheid om te opnieuw verwerken licentieverlening toewijzingen op basis van een groep voor één gebruiker en de mogelijkheid om met Groepslicenties met Office 365 E3/A3-licenties hebt toegevoegd.

Zie voor meer informatie over Groepslicenties [wat is licentieverlening in Azure Active Directory op basis van groep?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Er zijn nieuwe federatieve apps beschikbaar in de app-galerie van Azure AD: november 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In November 2018, hebben we deze 26 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[CoreStack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [grijs Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [eHour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [DriveDollar](https://www.drivedollar.com/Account/Login), [Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [oneindige Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [Alaya](https://alayagood.com/en/demo/), [ HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy voor bedrijven centrale 365](https://accounting.zenegy.com/), [Everbridge lid Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [Plex Apps: Klassieke Test](https://test.plexonline.com/signon), [Plex Apps – klassiek](https://www.plexonline.com/signon), [Plex Apps - UX Test](https://test.cloud.plex.com/sso), [Plex Apps – UX](https://cloud.plex.com/sso), [Plex Apps – IAM](https://accounts.plex.com/), [KNUTSELEN - kinderopvang Records, aanwezigheid en financiële volgsysteem](https://getcrafts.ca/craftsregistration) 

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

## <a name="october-2018"></a>Oktober 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure Log Analytics (openbare preview) nu beschikbaar in Azure AD-logboeken

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Controleren en rapporteren

We zijn trots te kunnen aankondigen dat u nu uw Azure AD-logboeken naar Azure Log Analytics doorsturen kunt. Deze functie meest gevraagde helpt u nog beter toegang geven tot analytics voor uw bedrijf, bewerkingen, en beveiliging, evenals een manier om u te helpen bij het beheren van uw infrastructuur. Zie voor meer informatie de [Azure Active Directory activiteitenlogboeken in Azure Log Analytics nu beschikbaar](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) blog.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Nieuwe federatieve apps beschikbaar in de galerie met Azure AD-apps - oktober 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In oktober 2018, hebben we deze 14 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Mijn punten Award](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), kiesvenster, [ON24 virtuele omgeving](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler drie](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot besturingselement](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>E-mailmeldingen voor Azure AD Domain Services

**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD Domain Services  
**Product-mogelijkheid:** Azure AD Domain Services

Azure AD Domain Services biedt waarschuwingen in Azure portal over onjuiste configuraties of problemen met uw beheerde domein. Deze waarschuwingen bevatten stapsgewijze handleidingen, zodat u de problemen kunt oplossen kunt zonder dat contact opnemen met ondersteuning.

Met ingang van oktober, zal het mogelijk om aan te passen van de meldingsinstellingen voor uw beheerde domein dus wanneer er nieuwe waarschuwingen optreden, een e-mailbericht is verzonden naar een aangewezen groep mensen, hoeft u niet voortdurend controleren van de portal voor updates.

Zie voor meer informatie, [meldingsinstellingen in Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD-portal biedt ondersteuning voor de ForceDelete-domein-API voor het verwijderen van aangepaste domeinen 

**Type:** Gewijzigde functie  
**Categorie van de service:** Mapbeheer  
**Product-mogelijkheid:** Directory

We zijn trots aan u kunt nu het domein ForceDelete API gebruiken om te verwijderen van uw aangepaste domeinnamen asynchroon naam van referenties, zoals gebruikers, groepen en apps van uw aangepaste domeinnaam (contoso.com) terug naar de naam van de initiële standaard-domein ( Contoso.onmicrosoft.com).

Deze wijziging kunt u snel uw aangepaste domeinnamen als uw organisatie de naam niet meer gebruikt, of te verwijderen als u nodig hebt voor het gebruik van de domeinnaam met een andere Azure AD.

Zie voor meer informatie, [verwijderen van een aangepaste domeinnaam](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>September 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Bijgewerkte machtigingen voor beheerdersrollen voor dynamische groepen

**Type:** Vast  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

We hebben een probleem is opgelost, zodat de specifieke beheerdersrollen nu kunnen maken en bijwerken van dynamisch-lidmaatschapregels, zonder dat de eigenaar van de groep.

De rollen zijn:

- Globale beheerder of onderneming Writer

- Intune-servicebeheerder

- Beheerder van gebruikersaccounts

Zie voor meer informatie, [een dynamische groep maken en de status controleren](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Configuratie-instellingen voor vereenvoudigde eenmalige aanmelding voor sommige apps van derden

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding

We realiseren ons dat instellen van eenmalige aanmelding (SSO) voor Software als een Service (SaaS)-apps kunnen lastig zijn vanwege de unieke aard van de configuratie van apps. We hebben een vereenvoudigde configuratie-ervaring voor het automatisch vullen van de SSO-configuratie-instellingen voor de volgende externe SaaS-apps gemaakt:

- Zendesk

- ArcGis Online

- Jamf Pro

Als u wilt gaan met behulp van deze ervaring met één klik, gaat u naar de **Azure-portal** > **SSO-configuratie** pagina voor de app. Zie voor meer informatie, [SaaS-toepassing-integratie met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Azure Active Directory - Pagina Waar zijn uw gegevens opgeslagen?

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** GoLocal

Selecteer de regio van uw bedrijf uit de **Azure Active Directory - waar bevindt uw gegevens zich** pagina om te bekijken welke Azure-datacenter ook uw Azure AD-gegevens in rust voor alle Azure AD-services nieuwste. U kunt de gegevens filteren op specifieke Azure AD-services voor de regio van uw bedrijf.

Voor toegang tot deze functie en voor meer informatie, Zie [Azure Active Directory - waar bevindt uw gegevens zich](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Nieuw implementatieplan beschikbaar voor het deelvenster Mijn apps-toegang

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn apps  
**Product-mogelijkheid:** Eenmalige aanmelding

Bekijk de nieuwe implementatieplan die beschikbaar is voor het toegangsvenster voor mijn Apps (https://aka.ms/deploymentplans).
Het deelvenster Mijn Apps toegang biedt gebruikers met één plek om te zoeken en toegang tot hun apps. De portal bevat ook gebruikers met selfservice-mogelijkheden, zoals aanvragen van toegang tot apps en -groepen of beheren van toegang tot deze resources namens anderen.

Zie voor meer informatie, [wat is de portal mijn Apps?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Nieuw tabblad Probleemoplossing en ondersteuning op de pagina met logboeken van aanmeldingen in Azure Portal

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Controleren en rapporteren

De nieuwe **probleemoplossing en ondersteuning** tabblad op de **aanmeldingen** pagina van de Azure portal, is bedoeld voor beheerders en ondersteuningstechnici oplossen van problemen met betrekking tot Azure AD-aanmeldingen. Deze nieuwe tabblad bevat de foutcode, foutbericht en herstelaanbevelingen (indien aanwezig) om u te helpen bij het oplossen van het probleem. Als u niet het probleem op te lossen, ook geven we u een nieuwe manier om u te maken van een ondersteuning ticket met de **naar Klembord kopiëren** ondervindt, die vult de **aanvraag-ID** en **datum (UTC)** velden voor het logboekbestand in uw ondersteuningsticket.  

![Logboeken aanmelden met het nieuwe tabblad](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Verbeterde ondersteuning voor aangepaste uitbreidingseigenschappen gebruikt om regels voor dynamisch lidmaatschap te maken

**Type:** Gewijzigde functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

Met deze update, u kunt nu klikt u op de **aangepaste extensie-eigenschappen ophalen** koppelen vanuit de opbouwfunctie voor dynamische gebruiker groep regel, Voer uw unieke app-ID en de volledige lijst met aangepaste extensie-eigenschappen om te gebruiken bij het maken van een dynamisch te ontvangen het lidmaatschapsregel voor gebruikers. Deze lijst kan ook worden vernieuwd om op te halen van alle nieuwe aangepaste extensie-eigenschappen voor die app.

Zie voor meer informatie over het gebruik van aangepaste extensie-eigenschappen voor de dynamisch-lidmaatschapregels [extensie-eigenschappen en aangepaste extensie-eigenschappen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde apps voor op Azure AD-app gebaseerde voorwaardelijke toegang

**Type:** Plan voor wijziging  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

De volgende apps zijn in de lijst met [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement):

- Microsoft To-Do

- Microsoft Stream

Zie voor meer informatie:

- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Nieuwe ondersteuning voor selfservice voor wachtwoordherstel op het vergrendelingsscherm van Windows 7/8/8.1

**Type:** Nieuwe functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie

Na het instellen van deze nieuwe functie, uw gebruikers een koppeling om hun wachtwoord opnieuw in te zien de **vergrendeling** scherm van een apparaat met Windows 7, Windows 8 of Windows 8.1. Door te klikken op de koppeling, wordt de gebruiker geleid door de dezelfde stroom voor wachtwoord opnieuw instellen via de webbrowser.

Zie voor meer informatie, [inschakelen voor wachtwoord opnieuw instellen van Windows 7, 8 en 8.1](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>U ziet dat wijzigen: Autorisatiecodes wordt niet langer beschikbaar voor hergebruik 

**Type:** Plan voor wijziging  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie

Vanaf 15 November 2018, Azure AD wordt niet meer accepteren van eerder gebruikte verificatie codes voor apps. Deze wijziging in de beveiliging zorgt ervoor dat Azure AD in overeenstemming met de OAuth-specificatie brengen en worden afgedwongen op de v1- en v2-eindpunten.

Als uw app wordt gebruikgemaakt van autorisatiecodes om op te halen van tokens voor meerdere bronnen, raden wij u gebruik van de code om op te halen van een vernieuwingstoken en gebruikt vervolgens die vernieuwingstoken om te verkrijgen van aanvullende tokens voor andere resources. Autorisatiecodes kunnen slechts eenmaal worden gebruikt, maar vernieuwen van tokens kunnen meerdere keren worden gebruikt in meerdere resources. Een app waarmee wordt geprobeerd om een verificatiecode op te geven tijdens de OAuth-codestroom opnieuw te gebruiken krijgt een foutmelding invalid_grant zijn.

Zie voor deze en andere wijzigingen met betrekking tot de protocollen, [de volledige lijst met wat is er nieuw voor de verificatie](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Er zijn nieuwe federatieve apps beschikbaar in de app-galerie voor Azure AD - september 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In September 2018, hebben we deze 16 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet werving Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [ JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), SSO voor Azure, SurveyMonkey Enlyft [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Ondersteuning voor aanvullende claimstransformatiemethoden

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding

We hebben nieuwe claim transformatie methoden, ToLower() en ToUpper(), die kan worden toegepast op SAML-tokens uit de SAML-gebaseerde geïntroduceerd **configuratie voor eenmalige aanmelding** pagina.

Zie voor meer informatie, [over het aanpassen van uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Bijgewerkte configuratie-UI voor SAML-apps (preview-versie)

**Type:** Gewijzigde functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Eenmalige aanmelding

Als onderdeel van onze bijgewerkt op basis van SAML appconfiguratie-UI krijgt u het:

- Een bijgewerkte scenario-ervaring voor het configureren van uw apps op basis van SAML.

- Meer zichtbaarheid over wat er ontbreekt of is onjuist in uw configuratie.

- De mogelijkheid om toe te voegen meerdere e-mailadressen voor melding over verlopen certificaat.

- Nieuwe claim transformatie methoden, ToLower() en ToUpper() en meer.

- Een manier voor het uploaden van uw eigen token handtekeningcertificaat voor apps in uw onderneming.

- Een manier om in te stellen de NameID-indeling voor SAML-apps, en een manier om in te stellen de NameID-waarde als uitbreidingen van de Directory.

Als u wilt inschakelen op deze bijgewerkte weergave, klikt u op de **proberen onze nieuwe ervaring voor** koppeling vanaf de bovenkant van de **Single Sign-On** pagina. Zie [Zelfstudie: Configureer SAML gebaseerde eenmalige aanmelding voor een toepassing met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).

---

## <a name="august-2018"></a>Augustus 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Wijzigingen in Azure Active Directory-IP-adresbereiken

**Type:** Plan voor wijziging  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Platform

We introduceren grotere IP-adresbereiken naar Azure AD, wat betekent dat als u Azure AD-IP-adresbereiken voor uw firewalls, routers of Netwerkbeveiligingsgroepen hebt geconfigureerd, moet u deze bijwerken. We doorvoeren deze update zodat u uw firewall, router of Network Security groepen IP-bereik configuraties opnieuw wijzigen hoeft wanneer Azure AD nieuwe eindpunten toevoegt. 

Netwerkverkeer wordt verplaatst naar deze nieuwe bereiken in de volgende twee maanden. Als u wilt doorgaan met de service niet wordt onderbroken, moet u deze bijgewerkte waarden toevoegen aan uw IP-adressen voor 10 September 2018:

- 20.190.128.0/18 

- 40.126.0.0/18 

Het is raadzaam de oude IP-adresbereiken niet verwijderen tot al uw netwerkverkeer is verplaatst naar de nieuwe bereiken. Zie voor updates over de overstap en voor meer informatie over wanneer u de oude bereiken kunt verwijderen, [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>U ziet dat wijzigen: Autorisatiecodes wordt niet langer beschikbaar voor hergebruik 

**Type:** Plan voor wijziging  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie

Vanaf 15 November 2018, Azure AD wordt niet meer accepteren van eerder gebruikte verificatie codes voor apps. Deze wijziging in de beveiliging zorgt ervoor dat Azure AD in overeenstemming met de OAuth-specificatie brengen en worden afgedwongen op de v1- en v2-eindpunten.

Als uw app wordt gebruikgemaakt van autorisatiecodes om op te halen van tokens voor meerdere bronnen, raden wij u gebruik van de code om op te halen van een vernieuwingstoken en gebruikt vervolgens die vernieuwingstoken om te verkrijgen van aanvullende tokens voor andere resources. Autorisatiecodes kunnen slechts eenmaal worden gebruikt, maar vernieuwen van tokens kunnen meerdere keren worden gebruikt in meerdere resources. Een app waarmee wordt geprobeerd om een verificatiecode op te geven tijdens de OAuth-codestroom opnieuw te gebruiken krijgt een foutmelding invalid_grant zijn.

Zie voor deze en andere wijzigingen met betrekking tot de protocollen, [de volledige lijst met wat is er nieuw voor de verificatie](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Geconvergeerd beveiligingsinformatiebeheer voor selfservice voor wachtwoordherstel en Multi-Factor Authentication

**Type:** Nieuwe functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie
 
Deze nieuwe functie helpt mensen hun beveiligingsgegevens (zoals, telefoonnummer, mobiele app, enzovoort) beheren voor SSPR en MFA in een enkele locatie en ervaring; in vergelijking met op eerder, waar dit is gedaan in twee verschillende locaties.

Deze ervaring geconvergeerde werkt ook voor mensen met behulp van SSPR of MFA. Bovendien, als uw organisatie niet van MFA of SSPR-registratie afdwingen, kunt mensen nog steeds registreren MFA of SSPR info beveiligingsmethoden toegestaan door uw organisatie van de portal mijn Apps.

Dit is een opt-in voor openbare preview-versie. Beheerders kunnen inschakelen op de nieuwe ervaring (indien gewenst) voor een geselecteerde groep of voor alle gebruikers in een tenant. Zie voor meer informatie over de geconvergeerde ervaring, de [geconvergeerd ervaring blog](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Nieuwe instelling Alleen HTTP-cookies in apps voor de Azure AD-toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Toegangsbeheer

Er is een nieuwe instelling, **HTTP-Only Cookies** in uw apps met Application Proxy. Deze instelling biedt extra beveiliging door de vlag HTTPOnly opnemen in de HTTP-antwoordheader voor beide Application Proxy toegangs- en sessiebeleid cookies, toegang aan de cookie van een client-side-script stoppen en verder te voorkomen dat bewerkingen zoals kopiëren of het wijzigen van de cookie. Hoewel deze vlag nog niet eerder zijn gebruikt, zijn uw cookies altijd versleuteld en verzonden met behulp van een SSL-verbinding om u te helpen beschermen tegen verkeerde wijzigingen.

Deze instelling is niet compatibel is met apps met behulp van ActiveX-besturingselementen, zoals Extern bureaublad. Als u bent in dit geval is, wordt u aangeraden dat u deze instelling uitschakelen.

Zie voor meer informatie over de instelling van de Cookies HTTP-Only [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Privileged Identity Management (PIM) voor Azure-resources ondersteunt resourcestypen voor beheergroepen

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Instellingen voor het activeren en de toewijzing van Just-In-Time kunnen nu worden toegepast op beheergroep-resourcetypen, net zoals u al voor abonnementen, resourcegroepen en Resources (zoals virtuele machines, App Services en meer doet). Bovendien kan iedereen met een rol waarmee de toegang als beheerder voor een beheergroep detecteren en beheren van die resource in PIM.

Zie voor meer informatie over de PIM- en Azure-resources, [detecteren en beheren van Azure-resources met behulp van Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Toegang tot toepassingen (preview-versie) biedt sneller toegang tot de Azure AD-portal

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Vandaag de dag bij het activeren van een rol met PIM, deze kan meer dan 10 minuten duren voordat de machtigingen voor het van kracht. Als u kiest voor toegang tot toepassingen, dat zich momenteel in openbare preview, beheerders hebben toegang tot de Azure AD-portal zodra de activeringsaanvraag is voltooid.

Toegang tot de toepassing ondersteunt momenteel alleen de Azure AD portal-ervaring en de Azure-resources. Zie voor meer informatie over PIM en de toepassing toegang [wat is Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Er zijn nieuwe federatieve apps beschikbaar in de app-galerie voor Azure AD - augustus 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In augustus 2018, hebben we deze 16 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline is losgekoppeld](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [saus Labs - mobiele en Web testen](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta netwerken Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [manier waarop wij doen](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [ProMaster (door Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - onkosten rapporten](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [ Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Systeemeigen Tableau-ondersteuning is nu beschikbaar in Azure AD-toepassingsproxy

**Type:** Gewijzigde functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Toegangsbeheer

Met de update van de OpenID Connect voor het verlenen van OAuth 2.0-Code-protocol voor het protocol van onze vooraf-verificatie hebt u niet langer geen aanvullende instellingen voor het gebruik van Tableau met Application Proxy. Deze wijziging protocol helpt ook bij de toepassingsproxy betere ondersteuning bieden voor moderne apps met behulp van alleen HTTP-omleidingen, die vaak worden ondersteund in JavaScript en HTML-codes.

Zie voor meer informatie over onze systeemeigen ondersteuning voor Tableau [Azure AD Application Proxy nu met systeemeigen ondersteuning voor Tableau](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Nieuwe ondersteuning om Google toe te voegen als een id-provider voor B2B-gastgebruikers in Azure Active Directory (preview-versie)

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C

Door het instellen van Federatie met Google in uw organisatie, kunt u uitgenodigde Gmail gebruikers zich laten uw gedeelde apps en resources met behulp van hun bestaande Google-account, zonder te hoeven maken van een persoonlijk Microsoft-Account (MSA's) of een Azure AD-account.

Dit is een opt-in voor openbare preview-versie. Zie voor meer informatie over Google federation [Google toevoegen als een id-provider voor B2B-gastgebruikers](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---
