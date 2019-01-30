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
ms.date: 01/29/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.openlocfilehash: 0847096674d446940ed3fed3390457e3d2e894bf
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55248449"
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
## <a name="january-2019"></a>Januari 2019

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Nieuwe Azure AD-toepassingsproxy cookie-instellingen

**Type:** Nieuwe functie  
**Categorie van de service:** App-proxy  
**Product-mogelijkheid:** Toegangsbeheer

We hebben drie nieuwe cookie-instellingen, beschikbaar voor uw apps die worden gepubliceerd via toepassingsproxy geïntroduceerd:

- **Gebruik alleen HTTP-cookie.** Stelt de **HTTPOnly** vlag aan de Application Proxy toegangs- en sessiebeleid cookies. Deze instelling inschakelt, biedt extra voordelen, zoals het helpen om te voorkomen dat kopiëren of wijzigen van cookies met behulp van client-side '-scripts. We raden u deze vlag inschakelt (Kies **Ja**) voor de extra voordelen.

- **Veilige cookie gebruiken.** Stelt de **Secure** vlag aan de Application Proxy toegangs- en sessiebeleid cookies. Deze instelling inschakelt, biedt de voordelen van de extra beveiliging, door ervoor te zorgen dat cookies worden alleen verzonden via TLS beveiligde kanalen, zoals HTTPS. We raden u deze vlag inschakelt (Kies **Ja**) voor de extra voordelen.

- **Permanente cookie gebruiken.** Hiermee voorkomt dat toegang cookies verlopen wanneer de webbrowser wordt gesloten. Deze cookies laatste gedurende de levensduur van het toegangstoken. Cookies worden echter opnieuw ingesteld als de vervaltijd is bereikt of als de gebruiker de cookie voor het handmatig verwijdert. We raden u aan de standaardinstelling **Nee**, alleen de instelling voor oudere apps die geen cookies tussen processen delen inschakelen.

Zie voor meer informatie over de nieuwe cookies [Cookie-instellingen voor toegang tot on-premises toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - januari 2019

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In januari 2019, hebben we deze 35 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Talent palet](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco overkoepelende](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Beheerder van Internet](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [vervaldatum herinnering](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Viewer](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [term](https://app.verb.net/login), [MazeMap koppeling](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Pavaso digitale sluit](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [ GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [Cloud Service PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [ CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO-systeem](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [are voor Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [K2 voor Office 365](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [ iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn veerboot ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [CaféX Portal (vergaderingen) ](https://docs.microsoft.com/azure/active-directory/saas-apps/caf%c3%a9xportal%28meetings%29-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)        

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Verbeteringen van Azure AD Identity Protection (preview-versie)

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

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>Nieuwe App Lock-functie voor de Microsoft Authenticator-app op iOS en Android-apparaten

**Type:** Nieuwe functie  
**Categorie van de service:** Microsoft Authenticator-App  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming

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

- Telefoonoproep

- Sms-bericht

Zie voor meer informatie over het gebruik van de Microsoft Authenticator-app opnieuw instellen van wachtwoorden [Azure AD Self-service voor wachtwoord opnieuw instellen - mobiele app en SSPR (Preview)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr-preview)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Nieuwe Cloud-Apparaatbeheerder voor Azure AD-rol (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Apparaatregistratie en -beheer  
**Product-mogelijkheid:** Toegangsbeheer

Beheerders kunnen gebruikers toewijzen aan de nieuwe rol in de Cloud-Apparaatbeheerder cloud apparaat beheerderstaken uit te voeren. Gebruikers die zijn toegewezen de beheerdersrol van Cloud-apparaat kunnen inschakelen, uitschakelen en verwijderen van apparaten in Azure AD, samen met kunnen Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal worden gelezen.

Zie voor meer informatie over rollen en machtigingen, [beheerdersrollen toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Uw apparaten beheren met de tijdstempel van de nieuwe activiteit in Azure AD (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Apparaatregistratie en -beheer  
**Product-mogelijkheid:** Levenscyclusbeheer voor apparaten

We realiseren ons dat na verloop van tijd moet u vernieuwen en is voor uw organisatie apparaten buiten gebruik stellen in Azure AD, om te voorkomen dat verouderde apparaten die zijn vastgelopen om in uw omgeving. Om te helpen met dit proces, werkt nu Azure AD uw apparaten met een nieuwe activiteit timestamp, helpen u bij het beheren van de levenscyclus van uw apparaat.

Zie voor meer informatie over het verkrijgen en gebruiken van deze timestamp [How To: De verouderde apparaten beheren in Azure AD](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Beheerders kunnen vereisen dat gebruikers de gebruiksvoorwaarden op elk apparaat accepteren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer
 
Beheerders kunnen nu inschakelen op de **vereisen dat gebruikers accepteren op elk apparaat** optie uw gebruikers moeten accepteer de gebruiksvoorwaarden op elk apparaat dat wordt gebruikt op uw tenant.

Zie voor meer informatie de [Per apparaat en de gebruiksvoorwaarden gedeelte van de Azure Active Directory-voorwaarden van functie gebruiken](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Beheerders kunnen een gebruiksrechtovereenkomst verloopt op basis van een terugkerend schema configureren

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

## <a name="july-2018"></a>Juli 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Verbeteringen voor e-mailmeldingen van Azure Active Directory

**Type:** Gewijzigde functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli
 
Azure Active Directory (Azure AD) e-mailberichten functie nu het ontwerp van een bijgewerkte, evenals wijzigingen in het e-mailadres afzender en de weergavenaam van de afzender, wanneer verzonden vanuit de volgende services:
 
- Azure AD-Toegangsbeoordelingen
- Azure AD Connect Health (Engelstalig) 
- Azure AD-identiteitsbeveiliging 
- Azure AD Privileged Identity Management
- Enterprise-App verloopt certificaat meldingen
- Servicemeldingen voor inrichting van Enterprise-App
 
De e-mailmeldingen worden verzonden van de volgende e-mailadres en de weergavenaam:

- E-mailadres: azure-noreply@microsoft.com
- Weergavenaam: Microsoft Azure
 
Voor een voorbeeld van enkele van de nieuwe e-ontwerpen en meer informatie, Zie [e-mailmeldingen in Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD-activiteitenlogboeken zijn nu beschikbaar via Azure Monitor

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Controleren en rapporteren

De Azure AD-activiteitenlogboeken zijn nu beschikbaar in openbare preview-versie van de Azure Monitor (van Azure-platform hele monitoring-service). Azure Monitor biedt u met een langetermijnbewaarperiode en naadloze integratie, naast deze verbeteringen:

- Langetermijnretentie van uw logboekbestanden routering naar uw eigen Azure storage-account.

- Naadloze SIEM-integratie, zonder dat u hoeft te schrijven of te onderhouden aangepaste scripts.

- Naadloze integratie met uw eigen aangepaste oplossingen, analysehulpprogramma's of oplossingen van incidentbeheer.

Zie onze blog voor meer informatie over deze nieuwe mogelijkheden, [Azure AD-activiteitenlogboeken in Azure Monitor diagnostics nu in openbare preview is](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) en onze documentatie [activiteitenlogboeken voor Azure Active Directory in Azure Monitor (preview)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Er is informatie over voorwaardelijke toegang toegevoegd aan het rapport Azure AD-aanmeldingen

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Identiteitbeveiliging en -bescherming
 
Deze update kunt u zien welke beleidsregels worden geëvalueerd wanneer een gebruiker zich aanmeldt, samen met het resultaat van het beleid. Daarnaast bevat het rapport nu het type van de client-app die wordt gebruikt door de gebruiker, zodat u oudere protocolverkeer kunt identificeren. Rapport vermeldingen kunnen nu ook worden gezocht naar een correlatie-ID, die kan worden gevonden in het foutbericht van de gebruiker gerichte en kan worden gebruikt om te identificeren en oplossen van de overeenkomende aanmeldingsaanvraag.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Verouderde verificaties weergeven via logboeken met aanmeldingsactiviteiten

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Controleren en rapporteren
 
Dankzij de introductie van de **Client-App** veld in de activiteit aanmelden zich aanmeldt, klanten kunnen nu Zie gebruikers die gebruikmaken van verouderde verificaties. Klanten moeten toegang hebben tot deze gegevens met behulp van de aanmeldingen bij MS Graph API of via de aanmelding activiteitenlogboeken in Azure AD-portal waar u kunt de **Client-App** besturingselement te filteren op verouderde verificaties. Bekijk de documentatie voor meer informatie.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Er zijn nieuwe federatieve apps beschikbaar in de galerie met apps van Azure AD - juli 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Bedrijfsapps  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
In juli 2018, hebben we deze 16 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Innovatie Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [bepaalde beheerder SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC fasering, [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Screencast-O-automatische](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial) , PowerSchool Unified klas, [Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar externe ondersteuning](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial) , [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [vaardigheden Base](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Nieuwe SaaS-app-integraties voor het inrichten van gebruikers - juli 2018

**Type:** Nieuwe functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** Integratie met toepassing van derden
 
Azure AD kunt u het maken, onderhoud en verwijderen van gebruikers-id's in SaaS-toepassingen, zoals Dropbox, Salesforce, ServiceNow en automatiseren. Voor juli 2018, hebben we ondersteuning voor de volgende toepassingen in de galerie van Azure AD-app inrichten van gebruikers toegevoegd:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Zie voor een lijst van alle toepassingen die ondersteuning bieden voor het inrichten van gebruikers in de Azure AD-galerie, [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Connect Health voor synchronisatie - een eenvoudigere manier om synchronisatiefouten met zwevende en dubbele kenmerken te herstellen

**Type:** Nieuwe functie  
**Categorie van de service:** AD Connect  
**Product-mogelijkheid:** Controleren en rapporteren
 
Azure AD Connect Health introduceert herstel van self-service te markeren en synchronisatiefouten oplossen. Deze functie Hiermee lost u dubbel kenmerk synchronisatiefouten en correcties van objecten die zijn zwevende van Azure AD. Deze diagnose biedt de volgende voordelen:

- De taalinstelling van de synchronisatiefouten dubbel kenmerk, bieden specifieke oplossingen

- Van toepassing is een oplossing voor Azure AD-scenario's, het oplossen van fouten in één stap toegewezen

- Er is geen upgrade of de configuratie is in te schakelen en gebruik deze functie vereist

Zie voor meer informatie, [vaststellen en herstellen van de synchronisatiefouten dubbel kenmerk](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors)

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Visualupdates voor de aanmeldingservaring van Azure AD en van MSA

**Type:** Gewijzigde functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Gebruikersverificatie

We hebben de gebruikersinterface voor de ervaring van Microsoft online services aanmelden, zoals bijgewerkt voor Office 365 en Azure. Deze wijziging stelt de schermen, minder rommelige en eenvoudiger. Zie voor meer informatie over deze wijziging, de [komende verbeteringen in de Azure Active Directory-aanmeldingservaring](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) blog.

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Nieuwe versie van Azure AD Connect - juli 2018

**Type:** Gewijzigde functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli

De meest recente versie van Azure AD Connect bevat: 

- Oplossingen voor problemen en ondersteuning-updates 

- Algemene beschikbaarheid van de integratie Ping federeren

- Updates voor de nieuwste SQL 2012-client 

Zie voor meer informatie over deze update [Azure AD Connect: Versiegeschiedenis van release](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-tou-end-user-ui"></a>Updates voor de gebruiksvoorwaarden voor de gebruikersinterface van de eindgebruiker

**Type:** Gewijzigde functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

De acceptatie van de tekenreeks in de gebruikersinterface van de eindgebruiker gebruiksvoorwaarden worden bijgewerkt.

**Huidige tekst.** Voor toegang tot resources [tenantName], moet u de gebruiksvoorwaarden accepteren.<br>**Nieuwe tekst.** Voor toegang tot [tenantName]-resource, moet u de gebruiksvoorwaarden lezen.

**Huidige tekst:** Betekent dat u akkoord met alle van de bovenstaande gebruiksvoorwaarden gaat accepteren te kiezen.<br>**Nieuwe tekst:** Klik op accepteren om te bevestigen dat u hebt gelezen en de gebruiksvoorwaarden begrepen.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Pass-through-verificatie ondersteunt verouderde protocollen en toepassingen

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Nu Pass through-verificatie biedt ondersteuning voor verouderde protocollen en -apps. De volgende beperkingen zijn nu volledig ondersteund:

- Gebruikersaanmeldingen voor verouderde Office clienttoepassingen, Office 2010 en Office 2013, zonder moderne verificatie.

- Toegang tot de agenda te delen en beschikbaarheidsgegevens in hybride omgevingen voor Office 2010 alleen.

- Gebruikersaanmeldingen tot Skype voor bedrijven-clienttoepassingen zonder moderne verificatie.

- Gebruikersaanmeldingen naar PowerShell versie 1.0.

- Het Apple Device Enrollment Program (DEP) van Apple, met behulp van de iOS-Configuratieassistent. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Geconvergeerd beveiligingsinformatiebeheer voor selfservice voor wachtwoordherstel en Multi-Factor Authentication

**Type:** Nieuwe functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie

Deze nieuwe functie kan gebruikers hun beveiligingsgegevens (bijvoorbeeld telefoonnummer, e-mailadres, mobiele app, enzovoort) voor selfservice voor wachtwoordherstel (SSPR) en multi-factor Authentication (MFA) in één ervaring beheren. Niet meer hebben gebruikers de dezelfde beveiligingsgegevens registreren voor SSPR en MFA in twee verschillende ervaringen. Deze nieuwe ervaring is ook van toepassing op gebruikers die Self-service voor Wachtwoordherstel of MFA hebben.

Als een organisatie wordt niet van MFA of SSPR-registratie afdwingen, kunnen gebruikers zich registreren hun beveiligingsgegevens via de **mijn Apps** portal. Van daaruit kunnen gebruikers zich registreren ingeschakeld voor MFA of SSPR methoden. 

Dit is een opt-in voor openbare preview-versie. Beheerders kunnen inschakelen op de nieuwe ervaring (indien gewenst) voor een geselecteerde groep van gebruikers of alle gebruikers in een tenant.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>De app Microsoft Authenticator gebruiken om uw identiteit te verifiëren wanneer u uw wachtwoord opnieuw instelt

**Type:** Gewijzigde functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie

Deze functie kunt niet-beheerders hun identiteit te verifiëren bij het herstellen van een wachtwoord met behulp van een melding of de code van de Microsoft Authenticator (of een andere verificator-app). Nadat beheerders inschakelen dit self-service voor wachtwoord opnieuw instellen van methode, gebruikers die zich hebben geregistreerd een mobiele app via aka.ms/mfasetup of aka.ms/setupsecurityinfo kunnen hun mobiele app gebruiken als een verificatiemethode tijdens hun wachtwoord opnieuw instellen.

Mobiele app-meldingen kan alleen worden ingeschakeld als onderdeel van een beleid dat is vereist twee methoden voor uw wachtwoord opnieuw instellen.

---
