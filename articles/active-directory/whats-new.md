---
title: Wat is er nieuw? Release-opmerkingen voor Azure Active Directory | Microsoft Docs
description: Ontdek wat er nieuw is bij Azure Active Directory (Azure AD) met inbegrip van de meest recente release-opmerkingen, bekende problemen, oplossingen voor problemen, afgeschafte functionaliteit en toekomstige wijzigingen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9e2186839648ffc6492cd64cb140cc767729d189
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="whats-new-in-azure-active-directory"></a>Wat is er nieuw in Azure Active Directory?




> De hoogte blijven van wat is er nieuw in Azure Active Directory Abonneer u op dit [feed](https://docs.microsoft.com/api/search/rss?search=%22what%27s%20new%20in%20azure%20active%20directory%3F%22&locale=en-us) in uw favoriete RSS-feed reader.



We zijn voortdurend verbeteren van de Azure Active Directory. Zodat u het product up-to-date houden met de meest recente ontwikkelingen vindt in dit onderwerp u informatie over:

-   De meest recente versies 
-   Bekende problemen 
-   Oplossingen voor problemen 
-   Afgeschafte functionaliteit 
-   Plannen voor wijzigingen 

Neem terugkeren naar deze pagina regelmatig werken wij deze op maandelijkse basis.

## <a name="november-2017"></a>November 2017
 
### <a name="retiring-acs"></a>Buiten gebruik stellen van ACS



**Type:** wijzigingen  
**Servicecategorie:** ACS  
**Mogelijkheid tot het product:** toegang tot de Control-Service 


Microsoft Azure Active Directory-toegangsbeheer (ook wel bekend als Access Control Service of ACS) wordt in latere 2018 buiten gebruik worden gesteld.  Meer informatie, inclusief een gedetailleerde planning & op hoog niveau migratie-instructies worden vermeld in de volgende enkele weken. In de tussentijd reacties op deze pagina met vragen met betrekking tot ACS en lid is van ons team helpt te beantwoorden.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>De browsertoegang tot de Intune managed browser beperken 


**Type:** wijzigingen  
**Servicecategorie:** voorwaardelijke toegang  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming




Met dit gedrag kunt u zich kunt browsertoegang tot Office 365 en andere Azure AD verbonden cloud-apps met behulp van de Intune Managed Browser als een goedgekeurde app beperken. 

Deze wijziging kunt u de volgende voorwaarde voor voorwaardelijke toegang op basis van een toepassing te configureren:

**Client-apps:** Browser

**Wat is het effect van de wijziging?**

Vandaag de dag toegang geblokkeerd bij gebruik van deze voorwaarde. Wanneer de evaluatieversie van dit gedrag beschikbaar is, wordt alle toegang tot het gebruik van de toepassing van de beheerde browser vereisen. 

Zoek voor deze optie en meer in de toekomstige blogs en release-opmerkingen. 

Zie voor meer informatie [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde client-apps voor voorwaardelijke toegang van Azure AD op basis van een app

 
**Type:** wijzigingen  
**Servicecategorie:** voorwaardelijke toegang  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming




De volgende apps zijn gepland om te worden toegevoegd aan de lijst met [client-apps goedgekeurd](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)

- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Zie voor meer informatie:

- [Goedgekeurde app vereiste](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory op basis van een app voorwaardelijke toegang](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Gebruiksvoorwaarden gebruik ondersteuning voor meerdere talen



**Type:** nieuwe functie    
**Servicecategorie:** gebruiksvoorwaarden  
**Mogelijkheid tot het product:** Governance/naleving





Beheerders kunnen nu nieuwe gebruiksvoorwaarden (TOU) waarin meerdere PDF-documenten maken. U kunt deze PDF-documenten met een overeenkomstige taal labelen. Gebruikers die vallen binnen het bereik weergegeven het PDF-bestand met de overeenkomstige taal op basis van hun voorkeuren. Als er geen overeenkomst, wordt de standaardtaal weergegeven.


---
 

### <a name="realtime-password-writeback-client-status"></a>Realtime wachtwoord terugschrijven clientstatus



**Type:** nieuwe functie  
**Servicecategorie:** SSPR  
**Mogelijkheid tot het product:** gebruikersverificatie


 

U kunt nu de status van uw lokale wachtwoord terugschrijven client bekijken. Deze optie is beschikbaar in de **On-premises integratie** sectie van de  **[wachtwoordherstel](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)**  pagina. 

Als er problemen met de verbinding met uw lokale Write-back-client zijn, ziet u een foutbericht weergegeven dat beschikt u over:

- Informatie over waarom u geen verbinding met uw lokale Write-back-client 
- Een koppeling naar de documentatie die u helpt bij het oplossen van het probleem. 


Zie voor meer informatie [On-premises integratie](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD app gebaseerde voorwaardelijke toegang 



 
**Type:** nieuwe functie  
**Servicecategorie:** Azure AD  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming





U kunt nu toegang tot Office 365 en andere Azure AD verbonden cloud-apps te beperken [client-apps goedgekeurd](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) die ondersteuning bieden voor Intune App Protection beleidsregels met behulp van [voorwaardelijke toegang van Azure AD op basis van een app](active-directory-conditional-access-mam.md). Intune app beveiliging beleidsregels worden gebruikt om te configureren en beveiligen van bedrijfsgegevens op deze clienttoepassingen.

Door te combineren [op basis van een app](active-directory-conditional-access-mam.md) met [op basis van apparaten](active-directory-conditional-access-policy-connected-applications.md) beleid voor voorwaardelijke toegang hebt u de flexibiliteit om gegevens voor persoonlijke en zakelijke apparaten te beveiligen.

De volgende voorwaarden en besturingselementen zijn nu beschikbaar voor gebruik met voorwaardelijke toegang op basis van een app:

**Ondersteund platform voorwaarde**

- iOS
- Android

**Voorwaarde voor client-apps**

- Mobiele apps en bureaublad-clients

**Toegangsbeheer**

- Goedgekeurde clientapp vereist


Zie voor meer informatie [voorwaardelijke toegang voor Azure Active Directory op basis van een app](active-directory-conditional-access-mam.md).

 
---

### <a name="managing-azure-ad-devices-in-the-azure-portal"></a>Het beheer van Azure AD-apparaten in de Azure portal



**Type:** nieuwe functie  
**Servicecategorie:** apparaatregistratie en beheer  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming

 



U kunt nu uw apparaten die zijn verbonden met Azure AD zoeken en de activiteiten die betrekking hebben op apparaten op één plek. Er is een nieuwe beheerervaring voor het beheren van uw apparaat-id's en instellingen in de Azure-portal. In deze release kunt u het volgende doen:

- Alle apparaten die beschikbaar voor voorwaardelijke toegang in Azure AD zijn weergeven

- Eigenschappen weergeven, met inbegrip van uw hybride Azure AD die lid zijn van apparaten

- BitLocker-sleutels voor uw Azure AD-die lid zijn van apparaten zoeken, beheren van uw apparaat bij Intune en meer.

- Apparaat-gerelateerde Azure AD-instellingen beheren


Zie voor meer informatie [apparaten beheert met de Azure-portal](device-management-azure-portal.md).



 
---

### <a name="support-for-macos-as-device-platform-for-azure-ad-conditional-access"></a>Ondersteuning voor Mac OS als apparaatplatform voor voorwaardelijke toegang van Azure AD 



**Type:** nieuwe functie    
**Servicecategorie:** voorwaardelijke toegang  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming 
 

U kunt nu opnemen of uitsluiten Mac OS als apparaat platform voorwaarde in uw beleid voor voorwaardelijke toegang van Azure AD. U kunt met de toevoeging van Mac OS aan de ondersteunde platforms voor apparaten:

- **Mac OS-apparaten met Intune registreren en beheren** -net als bij andere platforms, zoals iOS en Android, een portaltoepassing bedrijf is beschikbaar voor Mac OS unified inschrijvingen doen. De nieuwe bedrijfsportal-app voor Mac OS kunt u een apparaat inschrijven bij Intune en registreren met Azure AD.
 
- **Zorg ervoor dat Mac OS apparaten voldoen aan de beleidsregels voor naleving van uw organisatie gedefinieerd in Intune** -In de Intune in Azure portal, kunt u nu instellen nalevingsbeleid voor apparaten met Mac OS. 
  
- **Toegang tot toepassingen in Azure AD om alleen compatibele Mac OS-apparaten te beperken** -Mac OS als een afzonderlijk apparaat platform optie voorwaardelijk beleid maken heeft. Deze optie kunt u Mac OS specifieke voorwaardelijk toegangsbeleid voor de doeltoepassing is ingesteld in Azure maken.

Zie voor meer informatie:

- [Maken van een nalevingsbeleid voor apparaten voor Mac OS-apparaten met Intune](https://aka.ms/macoscompliancepolicy)
- [Voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="nps-extension-for-azure-mfa"></a>NPS-extensie voor Azure MFA 


**Type:** nieuwe functie    
**Servicecategorie:** MFA  
**Mogelijkheid tot het product:** gebruikersverificatie




De Network Policy Server (NPS)-extensie voor Azure MFA biedt een voor de verificatie-infrastructuur met behulp van uw bestaande servers cloud-gebaseerde MFA mogelijkheden. Met de extensie NPS kunt u telefoongesprek, tekstbericht of verificatie via de telefoon-app toevoegen aan uw bestaande authenticatiestroom zonder te installeren, configureren en onderhouden van nieuwe servers. 

Deze uitbreiding is gemaakt voor organisaties die beveiligen van VPN-verbindingen willen zonder de Azure MFA-Server implementeren. De NPS-extensie fungeert als een adapter tussen RADIUS- en cloud-gebaseerde Azure MFA voor een tweede factor van verificatie voor federatieve of gebruikers gesynchroniseerde.


Zie voor meer informatie [uw bestaande NPS-infrastructuur integreren met Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Herstellen of verwijderde gebruikers permanent verwijderen


**Type:** nieuwe functie    
**Servicecategorie:** Gebruikersbeheer  
**Mogelijkheid tot het product:** Directory 



In het Azure AD-beheercentrum kunt u nu:

- Een verwijderde gebruiker herstellen 
- Een gebruiker permanent te verwijderen 


**Uitproberen:**

1. Selecteer in het Azure AD-beheercentrum, [ **alle gebruikers** ](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) in de **beheren** sectie. 

2. Van de **weergeven** selecteert **onlangs verwijderd gebruikers**. 

4. Selecteer een of meer van de onlangs verwijderde gebruikers en ofwel herstel ze dan, of deze permanent verwijderen.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde client-apps voor voorwaardelijke toegang van Azure AD op basis van een app

 
**Type:** gewijzigde functie  
**Servicecategorie:** voorwaardelijke toegang  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming


De volgende apps zijn toegevoegd aan de lijst met [client-apps goedgekeurd](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- Microsoft Planner

- Microsoft Azure Information Protection 


Zie voor meer informatie:

- [Goedgekeurde app vereiste](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory op basis van een app voorwaardelijke toegang](active-directory-conditional-access-mam.md)


---

### <a name="ability-to-or-between-controls-in-a-conditional-access-policy"></a>Mogelijkheid om te 'of' tussen besturingselementen in een beleid voor voorwaardelijke toegang 


**Type:** gewijzigde functie    
**Servicecategorie:** voorwaardelijke toegang  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming

 
De mogelijkheid om te 'Of' (een van de geselecteerde besturingselementen vereisen) besturingselementen voor voorwaardelijke toegang is vrijgegeven. Deze functie kunt u voor het maken van beleid met een **of** tussen toegangsbeheer. Bijvoorbeeld, u kunt deze functie een beleid maken dat vereist dat een gebruiker zich aanmelden met multi-factor authentication **of** op een apparaat dat compatibel is.

Zie voor meer informatie [besturingselementen in voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-controls.md).

 
---

### <a name="aggregation-of-realtime-risk-events"></a>Aggregatie van realtime risico 's


**Type:** gewijzigde functie    
**Servicecategorie:** Identity Protection  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming


Ter verbetering van uw beheerervaring in Azure AD Identity Protection worden nu alle realtime risicogebeurtenissen die afkomstig zijn uit hetzelfde IP-adres zijn op een bepaalde dag samengevoegd voor elk type risico gebeurtenis. Deze wijziging beperkt de omvang van de risicogebeurtenissen die worden weergegeven zonder wijziging van de gebruikersbeveiliging.

De onderliggende realtime-detectie werkt elke keer dat de gebruiker zich aanmeldt. Als er een aanmeldingspagina risico beveiliging beleid instellen voor MFA of de toegang blokkeert, is het nog steeds geactiveerd tijdens elke riskant aanmelden.

 
---
 




## <a name="october-2017"></a>Oktober 2017


### <a name="deprecating-azure-ad-reports"></a>Azure AD-rapporten bestandstypen


**Type:** wijzigingen  
**Servicecategorie:** rapportage  
**Mogelijkheid tot het product:** Identity Lifecycle Management  



De Azure-portal beschikt u over:

- Een nieuwe Azure Active Directory-beheerconsole 
- Nieuwe API's voor rapporten activiteit en beveiliging
 
Als gevolg van deze nieuwe functies, het rapport API's onder de **/reports** eindpunt wordt buiten gebruik worden gesteld op 10 December 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Detectie van het veld voor automatische aanmelding


**Type:** vast   
**Servicecategorie:** mijn Apps  
**Mogelijkheid tot het product:** eenmalige aanmelding  



Azure Active Directory ondersteunt automatische aanmelding veld detectie voor toepassingen die een HTML-gebruikersnaam en wachtwoord veld weergeven.  Deze stappen zijn gedocumenteerd in [automatisch vastleggen velden aanmelden voor een toepassing](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). U vindt deze mogelijkheid door toe te voegen een *Non-galerie* toepassing op de **bedrijfstoepassingen** pagina in de [Azure-portal](http://aad.portal.azure.com). Bovendien kunt u de **Single Sign-on** modus op deze nieuwe toepassing **op basis van wachtwoorden Single Sign-on**, voert u een URL en vervolgens de pagina op te slaan.
 
Deze functionaliteit is vanwege een serviceprobleem tijdelijk uitgeschakeld voor een bepaalde periode. Het probleem is opgelost en de detectie automatisch aanmelden veld weer beschikbaar is.

---

### <a name="new-mfa-features"></a>Nieuwe MFA-functies


**Type:** nieuwe functie  
**Servicecategorie:** MFA  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming  



Multi-factor authentication (MFA) is een essentieel onderdeel van het beveiligen van uw organisatie. Als u meer geavanceerde referenties en de ervaring meer naadloze, zijn de volgende functies toegevoegd: 

- Integratie van meerdere factoren uitdaging resultaten rechtstreeks in de Azure AD-in rapport, inclusief programmatisch toegang biedt tot MFA resultaten

- Betere integratie van de configuratie van MFA in de configuratie van Azure AD optreden in de Azure-portal

Met deze openbare preview MFA beheer- en rapportageopties een geïntegreerde deel uitmaken van de ervaring van de core Azure AD-configuratie. Samenvoegen van beide functies kunt u voor het beheren van de portal beheerfunctionaliteit MFA in de Azure AD-ervaring.

Zie voor meer informatie [verwijzing voor multi-factor authentication rapportage in de Azure-portal](active-directory-reporting-activity-sign-ins-mfa.md) 


---

### <a name="introducing-terms-of-use"></a>Inleiding tot gebruiksvoorwaarden



**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Mogelijkheid tot het product:** Governance  



Azure AD-gebruiksvoorwaarden bieden u een eenvoudige methode om gegevens te presenteren aan eindgebruikers. Dit zorgt ervoor dat gebruikers relevante afwijzingen voor juridische of naleving vereisten bekijken.

U kunt Azure AD gebruiksvoorwaarden gebruiken in de volgende scenario's:

- Algemene gebruiksrechtovereenkomst voor alle gebruikers in uw organisatie. 

- Specifieke gebruiksvoorwaarden op basis van een gebruiker kenmerken (ex. arts tegenover verpleegkundigen of nationale tegenover internationale werknemers, uitgevoerd door dynamische groepen). 

- Gebruiksvoorwaarden specifieke voor toegang tot grote zakelijke invloed apps, zoals Salesforce.

Zie voor meer informatie [Azure Active Directory gebruiksvoorwaarden](active-directory-tou.md).


---

### <a name="enhancements-to-privileged-identity-management"></a>Verbeteringen bij privileged identity management


**Type:** nieuwe functie  
**Servicecategorie:** PIM  
**Mogelijkheid tot het product:** Privileged Identity Management  


Met Azure Active Directory Privileged Identity Management (PIM), kunt u nu beheren, beheren en te controleren, toegang tot Azure-Resources (Preview) binnen uw organisatie:

- Abonnementen
- Resourcegroepen
- Virtuele machines. 

Alle resources binnen de Azure-portal die gebruikmaken van de functionaliteit van Azure rollen gebaseerd toegangsbeheer (RBAC) kunnen profiteren van alle beveiligingsupdates en mogelijkheden voor het beheer van de levenscyclus van die Azure AD PIM te bieden heeft.

Zie voor meer informatie [PIM voor Azure-resources](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="introducing-access-reviews"></a>Introductie van de toegang controleert


**Type:** nieuwe functie  
**Servicecategorie:** beoordelingen openen  
**Mogelijkheid tot het product:** Governance  



Toegang beoordelingen (preview) kunnen organisaties efficiënt groepslidmaatschap beheren en toegang tot zakelijke toepassingen: 

- U kunt opnieuw certificeren gasttoegang voor gebruikers met toegang beoordelingen van toegang tot toepassingen en lidmaatschappen van groepen. De inzichten die is geleverd door de beoordelingen toegang inschakelen revisoren efficiënt te bepalen of gasten moeten voortdurend toegang hebben.

- U kunt werknemerstoegang tot toepassingen en groepslidmaatschappen opnieuw certificeren met toegangsbeoordelingen.

U kunt de controles van toegangsbeoordelingen verzamelen in programma's die relevant zijn voor uw organisatie, om beoordelingen bij te houden voor naleving of risicogevoelige toepassingen.

Zie voor meer informatie [beoordeelt de toegang van Azure AD](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hiding-third-party-applications-from-my-apps-and-the-office-365-launcher"></a>Toepassingen van derden van mijn Apps en de Office 365-launcher verbergen



**Type:** nieuwe functie  
**Servicecategorie:** mijn Apps  
**Mogelijkheid tot het product:** eenmalige aanmelding  



U kunt nu beter beheer van apps die worden weergegeven op de portals gebruiker via een nieuwe **app verbergen** eigenschap. Apps verbergen helpt met gevallen waarbij de app-tegels voor back-end-services of dubbele tegels, verschijnen en dat de gebruiker de app voor ruimtevaartuigen terechtkomen. De wisselknop bevindt zich op het eigenschappengedeelte van de app van derden en heet **zichtbaar voor gebruiker?** U kunt ook een app via een programma via PowerShell verbergen. 

Zie voor meer informatie [verbergen van een toepassing van derden van de gebruikerservaring in Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Wat is er beschikbaar?**

 Als onderdeel van de overgang naar de nieuwe beheerconsole zijn 2 nieuwe API's voor het ophalen van Azure AD-activiteitenlogboeken beschikbaar. De nieuwe set API's biedt uitgebreidere filteren en sorteren op de functionaliteit naast het bieden van uitgebreidere audit en aanmelden activiteiten. De gegevens die eerder beschikbaar via de beveiligingsrapporten zijn nu toegankelijk via de risicogebeurtenissen Identity Protection API in Microsoft Graph.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-microsoft-identity-manager"></a>Hotfix voor Microsoft Identity Manager


**Type:** gewijzigde functie  
**Servicecategorie:** Microsoft Identity Manager  
**Mogelijkheid tot het product:** Identity Lifecycle Management  



Een hotfixpakket (build 4.4.1642.0) is beschikbaar vanaf 25 September 2017 voor Microsoft Identity Manager (MIM) 2016 2016 servicepack 1 (SP1). Dit updatepakket:

- Problemen opgelost en verbeteringen toegevoegd
- Is een cumulatieve update die wordt vervangen door alle MIM 2016 SP1 updates tot build 4.4.1459.0 voor Microsoft Identity Manager 2016. 
- Moet u hebben **Microsoft Identity Manager 2016 4.4.1302.0 bouwen.** 

Zie voor meer informatie [hotfixpakket (build 4.4.1642.0) is beschikbaar voor Microsoft Identity Manager 2016 SP1](https://support.microsoft.com/en-us/help/4021562). 

---
