---
title: Wat is nieuw? Release-opmerkingen voor Azure Active Directory | Microsoft Docs
description: Ontdek wat er nieuw bij Azure Active Directory (Azure AD), zoals de meest recente release-opmerkingen, bekende problemen, oplossingen voor problemen, afgeschafte functionaliteit en toekomstige wijzigingen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: a30b5d08377594b8ad7e10b63a23e2a9d168af9c
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="whats-new-in-azure-active-directory"></a>Wat is er nieuw in Azure Active Directory?


> Product up-to-date houden met wat is er nieuw in Azure Active Directory (Azure AD) met een abonnement op de [ ![RSS](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us) [feed](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us).



Azure AD ontvangt verbeteringen voortdurend. Om te blijven up-to-date met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

-   De meest recente versies
-   Bekende problemen
-   Oplossingen voor problemen
-   Afgeschafte functionaliteit
-   Plannen voor wijzigingen

Deze pagina maandelijks wordt bijgewerkt, dus regelmatig bezoeken.



## <a name="february-2018"></a>2018 februari
 

### <a name="improved-navigation-for-managing-users-and-groups"></a>Verbeterde navigatie voor het beheren van gebruikers en groepen

**Type:** wijzigingen  
**Servicecategorie:** Directory Management  
**Product mogelijkheid:** Directory
 

De navigatie-ervaring voor het beheren van gebruikers en groepen is gestroomlijnd. U kunt nu navigeren vanuit een overzicht van de directory rechtstreeks aan de lijst met alle gebruikers gemakkelijker toegang aan de lijst met verwijderde gebruikers. U kunt ook rechtstreeks aan de lijst van alle groepen, met gemakkelijker toegang tot de groepsinstellingen uit het overzicht directory navigeren. En ook op de pagina overzicht directory u kunt zoeken naar een gebruiker, groep, bedrijfstoepassing of app-registratie.
 

---


### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Beschikbaarheid van aanmeldingen en audit rapporten in Microsoft Azure beheerd door 21Vianet (Azure China 21Vianet)

**Type:** nieuwe functie  
**Servicecategorie:** soevereine Clouds  
**Product mogelijkheid:** bewaking en rapportage
 

Azure AD-activiteit log-rapporten zijn nu beschikbaar in Microsoft Azure beheerd door 21Vianet (Azure China 21Vianet) exemplaren. De volgende logboeken zijn opgenomen:

- **Aanmeldingen activiteitenlogboeken** -omvat alle de aanmeldingen logboeken die zijn gekoppeld aan uw tenant.

- **Selfservice voor wachtwoord Audit logboeken** -bevat de logboeken voor de audit SSPR.

- **Controleren van Directory-beheer registreert** -omvat alle gerelateerde Directorybeheer controlelogboeken zoals gebruiker management, App-beheer en anderen.

Met deze logboeken krijgt u inzicht in hoe uw omgeving doet. Met de gegevens kunt u het volgende doen:

- Bepalen hoe uw apps en services worden gebruikt door uw gebruikers.

- Los problemen te voorkomen dat uw gebruikers toegang krijgen tot hun werk.

Zie voor meer informatie over het gebruik van deze rapporten [rapportage van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).
 

---


### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Gebruik van 'rapportlezer'-rol (niet-beheerders rol) Azure AD activiteitsrapporten weer te geven

**Type:** nieuwe functie  
**Servicecategorie:** rapportage  
**Product mogelijkheid:** bewaking en rapportage
 

Als onderdeel van feedback van klanten om in te schakelen van niet-beheerders functies toegang hebben tot Azure AD-activiteit zich aanmeldt, hebben we de mogelijkheid voor gebruikers die in de rol 'rapportlezer' aanmeldingen met toegang tot en controle-activiteit in de Azure Portal, evenals met onze Graph-API's zijn ingeschakeld. 

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

Zie voor meer informatie [jokertekens toepassingen in de Azure Active Directory-toepassingsproxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-wildcard)

 

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

**Belangrijk**
 
Deze build schema en sync introduceert wijzigingen regel. De Azure AD Connect-synchronisatieservice activeert een volledige Import en een volledige synchronisatie stappen na een upgrade. Zie voor meer informatie over het wijzigen van dit gedrag [het uitstellen van de volledige synchronisatie na de upgrade](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Deze release heeft de volgende updates en wijzigingen:

**Opgeloste problemen**

- Los tijdvenster op achtergrondtaken voor pagina Paritition filteren wanneer u overschakelt naar de volgende pagina.
- Een fout die toegangsfout tijdens de aangepaste actie ConfigDB veroorzaakt vast.
- Heeft een fout bij het herstellen van de time-out van de sql-verbinding.
- Een fout vastgesteld waar certificaten met jokertekens SAN controle van vereisten mislukt.
- Heeft een fout die ervoor zorgt miiserver.exe crashes tijdens het exporteren van AAD-connector dat.
- Een fout vastgesteld welke onjuist wachtwoord poging DC aangemeld bij het uitvoeren van AAD connect wizard configuratie wijzigen

**Nieuwe functies en verbeteringen**

- Voor GDPR, zijn er vereist om aan te geven de typen van klantgegevens die worden gedeeld met Microsoft (telemetrie, status, enz.) bevatten koppelingen naar gedetailleerde online documentatie en bieden een manier om u aan uw voorkeuren wijzigen.  Dit selectievakje in voegt het volgende:
    - Delen van gegevens en privacy-melding op het opschonen installeren EULA-pagina.

    - Delen en privacy melding gegevens op de upgradepagina.

    - Een nieuwe taak aanvullende **privacyinstellingen** waarin de gebruiker de voorkeuren kunt wijzigen.
 
- Toepassingstelemetrie - beheerders kunnen schakelen deze klasse van gegevens in-of uitschakelen.

- Statusgegevens van Azure AD - beheerders moeten Ga naar de health-portal voor het beheren van instellingen voor de status. Zodra de service-beleid is gewijzigd, wordt de agents lezen en wordt deze toepassen.

- Apparaat terugschrijven Configuratieacties en een voortgangsbalk zien voor de initialisatie van de pagina toegevoegd.

- Verbeterde algemene diagnostische gegevens met de HTML-rapport en volledige gegevens verzamelen in een ZIP-tekst / HTML-rapport.

- Verbeterde betrouwbaarheid van Automatische upgrade en extra telemetrie om te controleren of de status van de server kan worden bepaald toegevoegd.

- Machtigingen die beschikbaar zijn beperkt tot beschermde accounts op AD-Connector-account. Voor nieuwe installaties van de wizard beperkt de machtigingen die accounts met bevoegdheden hebben op het account MSOL na het maken van het account MSOL. De wijzigingen die invloed hebben op installaties van snelle en aangepaste installaties met account automatisch maken.

- Het installatieprogramma niet verplicht stellen SA-bevoegdheden op een schone installatie van AADConnect wordt gewijzigd.

- Nieuwe hulpprogramma voor het oplossen van synchronisatieproblemen voor een specifiek object. Op dit moment controleert het hulpprogramma voor het volgende:

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

De volgende toepassingen worden toegevoegd aan het einde van februari 

- Microsoft PowerBI

- Microsoft Launcher

- Microsoft facturering

Zie voor meer informatie:

- [Goedgekeurde app vereiste](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD app gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

 

---
 

### <a name="terms-of-use-update-to-mobile-experience"></a>Gebruiksvoorwaarden bijwerken naar mobiele ervaring 

**Type:** gewijzigde functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** Governance
 

Wanneer u de gebruiksvoorwaarden worden weergegeven, kunt u nu klikken **met problemen bij het bekijken? Klik hier**. Op deze koppeling klikt, opent de gebruiksvoorwaarden systeemeigen op uw apparaat. Ongeacht de grootte van het lettertype in het document of de schermgrootte van apparaat, kunt u inzoomen en lezen van het document indien nodig. 
 

---
 
## <a name="january-2018"></a>2018 januari
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve Apps beschikbaar in Azure AD-App-galerie 

**Type:** nieuwe functie  
**Servicecategorie:** zakelijke Apps  
**Product mogelijkheid:** 3e integratie van derden
 

In januari 2018, zijn de volgende nieuwe apps met ondersteuning voor identiteitsfederatie toegevoegd in de App-galerie:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy beheersoftware](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk federatieve Directory](https://go.microsoft.com/fwlink/?linkid=864699) en [kwaliteit NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Zie voor een volledig overzicht van alle beschikbare zelfstudies [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial).
 

---
 


### <a name="sign-in-with-additional-risk-detected"></a>Aanmelding met de extra risico gedetecteerd

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
 

U kunt nu beter beheren hoe Office 365-toepassingen weergegeven op uw gebruikers toegang panelen via een nieuwe gebruikersinstelling. Deze optie is handig voor het beperken van de hoeveelheid apps in een gebruiker toegang panelen als u liever alleen Office-apps worden weergegeven in de Office-portal. De instelling bevindt zich in de **gebruikersinstellingen** en label **kunnen gebruikers alleen Office 365-apps in de Office 365-beheerportal zien**.
 

Zie voor meer informatie [verbergen van een toepassing van de gebruikerservaring in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 


### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Naadloze Meld u aan bij de apps die zijn ingeschakeld voor eenmalige aanmelding wachtwoord rechtstreeks vanuit de app-URL 

**Type:** nieuwe functie  
**Servicecategorie:** mijn Apps  
**Product mogelijkheid:** eenmalige aanmelding
 

De Browseruitbreiding van mijn Apps is nu beschikbaar via een handig hulpmiddel waarmee u eenmalige aanmelding in de mijn Apps van mogelijkheden als een snelkoppeling in uw browser. Nadat de installatie van de gebruiker ziet een pictogram waffle in hun browser waarmee ze snel toegang tot apps. Gebruikers kunnen nu profiteren van:

- De mogelijkheid rechtstreeks aanmelden bij wachtwoord SSO gebaseerde apps uit de app-aanmeldingspagina
- Start een app met de functie Snel zoeken
- Snelkoppelingen naar de laatst gebruikte apps van de extensie
- De extensie is beschikbaar voor Edge, Chrome en Firefox.
 
Zie voor meer informatie [mijn Apps beveiligen aanmelden extensie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure AD-beheer ervaring in de klassieke Azure-portal is buiten gebruik gesteld

**Type:** afgeschaft   
**Servicecategorie:** Azure AD  
**Product mogelijkheid:** Directory
 

Vanaf 8 januari 2018, het beheer van Azure AD ervaring in de klassieke Azure portal is buiten gebruik gesteld. Dit heeft plaatsgevonden in combinatie met de buiten gebruik stellen van de klassieke Azure portal zelf. Voortaan, moet u de [Azure AD-beheercentrum](https://aad.portal.azure.com) voor alle uw portal-beheer op basis van Azure AD.
 
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

Als onderdeel van de overgang naar de nieuwe beheerconsole, hebben we 2 nieuwe API's beschikbaar zijn voor het ophalen van Azure AD-activiteitenlogboeken aangebracht. De nieuwe set API's bieden uitgebreidere filteren en sorteren op de functionaliteit naast het bieden van uitgebreidere audit en aanmelden activiteiten. De gegevens die eerder beschikbaar via de beveiligingsrapporten zijn nu toegankelijk via de risicogebeurtenissen Identity Protection API in Microsoft Graph.

Zie voor meer informatie:

- [Aan de slag met de Azure Active Directory rapportage-API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Aan de slag met Azure Active Directory: Identity Protection en Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)


---


## <a name="december-2017"></a>December 2017
 

### <a name="terms-of-use-in-the-access-panel"></a>Gebruiksvoorwaarden in het toegangsvenster

**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** Governance/naleving
 
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
**Product mogelijkheid:** Governance/naleving
 
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
**Product mogelijkheid:** 3e integratie van derden
 
In December 2017, zijn de volgende nieuwe apps met ondersteuning voor identiteitsfederatie toegevoegd in de app-galerie:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe ervaring Manager [EFI digitale winkel](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [INSTALLATIEKOPIE werkt](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD-integratie](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO voor Bamboe door resolutie GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO voor Bitbucket door resolutie GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD-integratie.

Zie voor een volledig overzicht van alle beschikbare zelfstudies [integratie van de SaaS-toepassingen met Azure Active Directory](https://aka.ms/appstutorial).

 
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




De volgende apps zijn gepland om te worden toegevoegd aan de lijst met [client-apps goedgekeurd](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Zie voor meer informatie:

- [Goedgekeurde app vereiste](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD app gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Gebruiksvoorwaarden van ondersteuning voor meerdere talen



**Type:** nieuwe functie    
**Servicecategorie:** gebruiksvoorwaarden  
**Product mogelijkheid:** Governance/naleving





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

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure AD-apparaten in de Azure portal beheren



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
**Product mogelijkheid:** Governance/naleving  



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
**Product mogelijkheid:** Governance/naleving  



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
