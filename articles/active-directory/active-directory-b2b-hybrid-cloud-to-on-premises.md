---
title: Gebruikers in Azure AD GRANT B2B toegang tot uw on-premises toepassingen | Microsoft Docs
description: Laat zien hoe cloud B2B-gebruikerstoegang geven tot on-premises apps met Azure AD B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/20/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 028bbb28c7091db3c3ebea321ca2e167b999949d
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
ms.locfileid: "34068793"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Gebruikers in Azure AD GRANT B2B toegang tot uw on-premises toepassingen

Als een organisatie die gebruikmaakt van Azure Active Directory (Azure AD) B2B-samenwerkingsmogelijkheden om uit te nodigen gastgebruikers van partnerorganisaties aan uw Azure AD, kunt u nu bieden deze B2B-gebruikers toegang tot lokale apps. Deze apps lokale kunt SAML-gebaseerde authenticatie of geïntegreerde Windows-verificatie (IWA) met Kerberos-beperkte delegatie (KCD) gebruiken.

## <a name="access-to-saml-apps"></a>Toegang tot SAML-apps

Als uw lokale app gebruikmaakt van verificatie op basis van SAML, kunt u deze apps gemakkelijk beschikbaar maken voor uw Azure AD B2B-samenwerking gebruikers via de Azure portal.

U moet het volgende doen:

- De SAML-app integreren met behulp van de toepassingssjabloon niet galerie, zoals beschreven in [configureren van eenmalige aanmelding tot toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie](active-directory-saas-custom-apps.md). Zorg ervoor dat u gebruiken voor de **aanmeldings-URL** waarde.
-  Azure AD-toepassingsproxy gebruiken voor het publiceren van de app lokaal met **Azure Active Directory** geconfigureerd als de verificatiebron. Zie voor instructies [toepassingen publiceren met Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md). 

   Wanneer u configureert de **interne Url** instellen, de aanmeldings-URL die u hebt opgegeven in de niet gallery-toepassingssjabloon gebruiken. Op deze manier kunnen gebruikers toegang tot de app vanuit buiten de grens van de organisatie. Toepassingsproxy voert het SAML eenmalige aanmelding voor de lokale app.
 
   ![Toont de interne URL van lokale app-instellingen en -verificatie](media/active-directory-b2b-hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>Toegang tot IWA en KCD apps

U kunt B2B gebruikers toegang tot on-premises toepassingen die zijn beveiligd met geïntegreerde Windows-verificatie en Kerberos-beperkte overdracht, moet u de volgende onderdelen:

- **Verificatie via Azure AD-toepassingsproxy**. B2B-gebruikers moeten toegang hebben tot de on-premises toepassing. Om dit te doen, moet u de app lokaal via de Azure AD-toepassingsproxy publiceren. Zie voor meer informatie [aan de slag met Application Proxy en installeer de connector](manage-apps/application-proxy-enable.md) en [toepassingen publiceren met Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md).
- **Autorisatie via een B2B-gebruikersobject in de on-premises directory**. De toepassing moet kunnen verlenen toegang tot de juiste resources en gebruiker toegangscontroles uitvoeren. IWA en KCD moet een gebruikersobject in de lokale Windows Server Active Directory om deze autorisatie te voltooien. Zoals beschreven in [hoe eenmalige aanmelding met KCD works](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works), Application Proxy moet dit gebruikersobject voor de gebruiker imiteren en het verkrijgen van een Kerberos-token naar de app. 

   Er zijn twee methoden beschikbaar waarmee u kunt de Gast gebruikersobjecten die vereist voor autorisatie in de on-premises directory zijn maken voor het scenario B2B-gebruiker:

   - Microsoft Identity Manager (MIM) en de [MIM-beheeragent voor Microsoft Graph](#create-b2b-guest-user-objects-through-mim-preview). 
   - [Een PowerShell-script](#create-b2b-guest-user-objects-through-a-script-preview). Met het script is een meer lichtgewicht oplossing waarvoor MIM niet. 

Het volgende diagram biedt een overzicht van hoe Azure AD-toepassingsproxy en het genereren van het gebruikersobject B2B in de on-premises directory werk samen B2B gebruikers om toegang te verlenen tot uw lokale IWA en KCD apps. Genummerde stappen worden beschreven in de details hieronder het diagram.

![Diagram van oplossingen voor MIM- en B2B-script](media/active-directory-b2b-hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Een gebruiker vanuit een andere organisatie (de Fabrikam-tenant) wordt verzocht de Contoso-tenant.
2.  Een gast user-object is gemaakt in de Contoso-tenant (bijvoorbeeld een gebruikersobject met de UPN-guest_fabrikam.com#EXT#@contoso.onmicrosoft.com).
3.  De Gast Fabrikam is geïmporteerd van Contoso via MIM of via het B2B PowerShell-script.
4.  Een weergave of 'footprint' van het gebruikersobject van Fabrikam gast (Gast EXT #) wordt in de on-premises adreslijst, Contoso.com, via MIM of via het B2B PowerShell-script gemaakt.
5.  De Gast gebruiker toegang tot de on-premises toepassing, app.contoso.com.
6.  De verificatieaanvraag is geautoriseerd via toepassingsproxy, met behulp van Kerberos-beperkte overdracht. 
7.  Omdat het gebruikersobject Gast lokaal bestaat, wordt de verificatie is geslaagd.

### <a name="lifecycle-management-policies"></a>De levenscyclus van management-beleid

U kunt de lokale B2B gebruikersobjecten via lifecycle management-beleid kunt beheren. Bijvoorbeeld:

- U kunt multi-factor authentication (MFA)-beleid voor de gastgebruiker instellen zodat MFA tijdens de toepassingsproxy-verificatie wordt gebruikt. Zie voor meer informatie [voorwaardelijke toegang voor gebruikers voor B2B-samenwerking](active-directory-b2b-mfa-instructions.md).
- Eventuele sponsoring, toegang beoordelingen, account verificaties, enzovoort worden uitgevoerd op de cloud B2B gebruiker van toepassing is op de on-premises gebruikers. Bijvoorbeeld, als de cloudgebruiker via uw lifecycle management-beleid wordt verwijderd, wordt de on-premises gebruiker ook verwijderd door MIM Sync of Azure AD Connect sync. Zie voor meer informatie [gasttoegang beheren met Azure AD access beoordeelt](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim-preview"></a>Maken van objecten uit B2B Gast gebruiker via MIM (Preview)

Zie voor meer informatie over het gebruik van MIM 2016 Service Pack 1 en de MIM-beheeragent voor Microsoft Graph maken van de Gast gebruikersobjecten in de on-premises directory [samenwerking van Azure AD-business-to-business (B2B) met Microsoft Identity Manager (MIM) 2016 SP1 met Azure-toepassingsproxy (openbare Preview)](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario).

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>B2B Gast gebruikersobjecten via een script (Preview) maken

Er is een PowerShell-voorbeeldscript beschikbaar waarmee u kunt als een beginpunt voor het maken van de Gast gebruikersobjecten in uw lokale Active Directory.

U kunt het script en het Leesmij-bestand downloaden de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=51495). Kies de **Script en Leesmij-bestand voor het ophalen van Azure AD B2B gebruikers op de prem.zip** bestand.

Controleer voordat u het script gebruiken, of u wordt aangeraden de vereisten en overwegingen in het bijbehorende Leesmij-bestand. Begrijp ook, dat het alleen beschikbaar als een voorbeeld van het script wordt gemaakt. Uw ontwikkelteam of een partner aanpassen en bekijk het script voordat u deze uitvoeren op.

## <a name="license-considerations"></a>Licentie-overwegingen

Zorg ervoor dat u de juiste licenties voor clienttoegang (CAL's) voor externe gebruikers die toegang lokale apps tot hebt. Zie voor meer informatie de sectie 'Externe Connectors' [licenties voor clienttoegang en licenties Management](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx). Raadpleeg uw Microsoft-vertegenwoordiger of plaatselijke softwareleverancier met betrekking tot uw specifieke behoeften voor licentieverlening.

## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory B2B-samenwerking voor hybride organisaties](active-directory-b2b-hybrid-organizations.md)

- Zie voor een overzicht van Azure AD Connect [uw on-premises adreslijsten integreren met Azure Active Directory](connect/active-directory-aadconnect.md).

