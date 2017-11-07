---
title: Wat is er nieuw? Release-opmerkingen voor Azure Active Directory | Microsoft Docs
description: Ontdek wat er nieuw is bij Azure Active Directory (Azure AD) met inbegrip van de meest recente release-opmerkingen, bekende problemen, oplossingen voor problemen, afgeschafte functionaliteit en toekomstige wijzigingen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 0fd0ee5bb0bc3e9dcda9db291a1f9dbac6a2e7dc
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
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


## <a name="october-2017"></a>Oktober 2017

**Type:** wijzigingen  
**Servicecategorie:** rapportage  
**Mogelijkheid tot het product:** Identity Lifecycle Management  


**Azure AD-rapporten (bètaversie) bestandstypen API's onder de `https://graph.windows.net/<tenant-name>/reports/` knooppunt**

De Azure-portal beschikt u over:

- Een nieuwe Azure Active Directory-beheerconsole 
- Nieuwe API's voor rapporten activiteit en beveiliging
 
Als gevolg van deze nieuwe functies, het rapport API's onder de **/reports** eindpunt wordt buiten gebruik worden gesteld op 10 December 2017. 

---

**Type:** vast   
**Servicecategorie:** mijn Apps  
**Mogelijkheid tot het product:** eenmalige aanmelding  


Azure Active Directory ondersteunt automatische aanmelding veld detectie voor toepassingen die een HTML-gebruikersnaam en wachtwoord veld weergeven.  Deze stappen zijn gedocumenteerd in [automatisch vastleggen velden aanmelden voor een toepassing](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). U vindt deze mogelijkheid door toe te voegen een *Non-galerie* toepassing op de **bedrijfstoepassingen** pagina in de [Azure-portal](http://aad.portal.azure.com). Bovendien kunt u de **Single Sign-on** modus op deze nieuwe toepassing **op basis van wachtwoorden Single Sign-on**, voert u een URL en vervolgens de pagina op te slaan.
 
Deze functionaliteit is vanwege een serviceprobleem tijdelijk uitgeschakeld voor een bepaalde periode. Het probleem is opgelost en de detectie automatisch aanmelden veld weer beschikbaar is.

---

**Type:** nieuwe functie  
**Servicecategorie:** MFA  
**Mogelijkheid tot het product:** identiteit beveiliging en gegevensbescherming  


Multi-factor authentication (MFA) is in de wereld die er bevinden zich in een essentieel onderdeel van het beveiligen van uw organisatie. Het identity-team van Microsoft is multi-factor authentication-server als u meer geavanceerde referenties en de ervaring meer naadloze ontwikkeling. Ik ben vandaag nog twee belangrijke stappen in deze reis trots: 

- Integratie van meerdere factoren uitdaging resultaten rechtstreeks in de Azure AD-in rapport, inclusief programmatisch toegang biedt tot MFA resultaten

- Betere integratie van configuratie van MFA in de configuratie van de Azure AD core optreden in de Azure-portal

Met deze openbare preview MFA beheer- en rapportageopties een geïntegreerde deel uitmaken van de kern van het Azure AD configuratie ervaring, zodat u de portal MFA-Management-functionaliteit in de ervaring van de Azure AD beheren.

Zie voor meer informatie [verwijzing voor multi-factor authentication rapportage in de Azure-portal](active-directory-reporting-activity-sign-ins-mfa.md) 


---
**Type:** nieuwe functie  
**Servicecategorie:** gebruiksvoorwaarden  
**Mogelijkheid tot het product:** Governance  


**Azure AD-gebruiksvoorwaarden** biedt u een eenvoudige manier om gegevens te presenteren aan eindgebruikers. Dit zorgt ervoor dat gebruikers relevante afwijzingen voor juridische of naleving vereisten bekijken.

U kunt Azure AD gebruiksvoorwaarden gebruiken in de volgende scenario's:

- Algemene gebruiksvoorwaarden voor alle gebruikers in uw organisatie. 

- Specifieke gebruiksvoorwaarden op basis van een gebruiker kenmerken (ex. arts tegenover verpleegkundigen of nationale tegenover internationale werknemers, uitgevoerd door dynamische groepen). 

- Gebruiksvoorwaarden specifieke voor toegang tot grote zakelijke invloed apps, zoals Salesforce.

Zie voor meer informatie [Azure Active Directory gebruiksvoorwaarden](active-directory-tou.md).


---
**Type:** nieuwe functie  
**Servicecategorie:** PIM  
**Mogelijkheid tot het product:** Privileged Identity Management  


Met Azure Active Directory Privileged Identity Management (PIM), kunt u nu beheren, beheren en te controleren, toegang tot Azure-Resources (Preview) binnen uw organisatie. Dit omvat abonnementen en resourcegroepen zelfs virtuele machines. Alle resources binnen de Azure-portal die gebruikmaken van de functionaliteit van Azure rollen gebaseerd toegangsbeheer (RBAC) kunnen profiteren van alle betrouwbaarheid en beheermogelijkheden van de levenscyclus van die Azure AD PIM te bieden heeft en een aantal geweldige nieuwe functies die we willen meenemen naar Azure AD-rollen snel.

Zie voor meer informatie [PIM voor Azure-resources](privileged-identity-management/azure-pim-resource-rbac.md).


---
**Type:** nieuwe functie  
**Servicecategorie:** beoordelingen openen  
**Mogelijkheid tot het product:** Governance  


Toegang beoordelingen (preview) kunnen organisaties efficiënt groepslidmaatschap beheren en toegang tot zakelijke toepassingen: 

- U kunt opnieuw certificeren gasttoegang voor gebruikers met toegang beoordelingen van toegang tot toepassingen en lidmaatschappen van groepen. De inzichten die is geleverd door de beoordelingen toegang inschakelen revisoren efficiënt te bepalen of gasten moeten voortdurend toegang hebben.

- U kunt werknemerstoegang tot toepassingen en groepslidmaatschappen opnieuw certificeren met toegangsbeoordelingen.

U kunt de controles van toegangsbeoordelingen verzamelen in programma's die relevant zijn voor uw organisatie, om beoordelingen bij te houden voor naleving of risicogevoelige toepassingen.

Zie voor meer informatie [beoordeelt de toegang van Azure AD](active-directory-azure-ad-controls-access-reviews-overview.md).


---
**Type:** nieuwe functie  
**Servicecategorie:** mijn Apps  
**Mogelijkheid tot het product:** eenmalige aanmelding  


**Mogelijkheid voor het verbergen van de toepassingen van derden van mijn Apps en het startprogramma voor Office 365**

U kunt nu beter beheer van apps die worden weergegeven op de portals gebruiker via een nieuwe **app verbergen** eigenschap. Dit helpt met gevallen waarbij de app-tegels voor back-end-services of dubbele tegels, verschijnen en dat de gebruiker de app voor ruimtevaartuigen terechtkomen. De wisselknop bevindt zich op het eigenschappengedeelte van de app van derden en heet **zichtbaar voor gebruiker?**. U kunt ook een app via een programma via PowerShell verbergen. 

Zie voor meer informatie [verbergen van een toepassing van derden van de gebruikerservaring in Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Wat is er beschikbaar?**

 Als onderdeel van de overgang naar de nieuwe beheerconsole, hebben we 2 nieuwe API's voor het ophalen van Azure AD-activiteitenlogboeken beschikbaar aangebracht. De nieuwe set API's bieden uitgebreidere filteren en sorteren op de functionaliteit naast het bieden van uitgebreidere audit en aanmelden activiteiten. De gegevens die eerder beschikbaar via de beveiligingsrapporten zijn nu toegankelijk via de risicogebeurtenissen Identity Protection API in Microsoft Graph.


## <a name="september-2017"></a>September 2017

**Type:** gewijzigde functie  
**Servicecategorie:** Microsoft Identity Manager  
**Mogelijkheid tot het product:** Identity Lifecycle Management  


Een hotfixpakket (build 4.4.1642.0) is beschikbaar vanaf 25 September 2017 voor Microsoft Identity Manager (MIM) 2016 2016 servicepack 1 (SP1). Dit updatepakket:

- Problemen opgelost en verbeteringen toegevoegd
- Is een cumulatieve update die wordt vervangen door alle MIM 2016 SP1 updates tot build 4.4.1459.0 voor Microsoft Identity Manager 2016. 
- Moet u hebben **Microsoft Identity Manager 2016 4.4.1302.0 bouwen.** 

Zie voor meer informatie [hotfixpakket (build 4.4.1642.0) is beschikbaar voor Microsoft Identity Manager 2016 SP1](https://support.microsoft.com/en-us/help/4021562). 

---