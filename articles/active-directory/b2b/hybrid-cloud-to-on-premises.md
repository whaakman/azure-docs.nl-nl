---
title: GRANT B2B-gebruikers in Azure AD de toegang tot uw on-premises toepassingen | Microsoft Docs
description: Laat zien hoe cloud B2B-gebruikerstoegang geven tot on-premises apps met Azure AD B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/20/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 9c9cde23f8f8dc557b99d2f1508c923aaa2dce91
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054499"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>GRANT B2B-gebruikers in Azure AD-toegang tot uw on-premises toepassingen

Als een organisatie die gebruikmaakt van functionaliteit voor samenwerking in Azure Active Directory (Azure AD) B2B uitnodigen van gastgebruikers van partnerorganisaties koppelt aan uw Azure AD, kunt u nu deze B2B-gebruikers toegang bieden tot on-premises toepassingen. Deze on-premises toepassingen kunnen verificatie op basis van SAML of geïntegreerde Windows-verificatie (IWA) gebruiken met Kerberos-beperkte delegatie (KCD).

## <a name="access-to-saml-apps"></a>Toegang tot de SAML-apps

Als uw on-premises app gebruikmaakt van verificatie op basis van SAML, kunt u deze apps gemakkelijk beschikbaar maken voor de gebruikers van uw Azure AD B2B-samenwerking via Azure portal.

U moet het volgende doen:

- De SAML-app integreren met behulp van de sjabloon van de toepassing buiten de galerie, zoals beschreven in [configureren van eenmalige aanmelding voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie](../active-directory-saas-custom-apps.md). Zorg ervoor dat te weten wat u gebruikt voor de **aanmeldings-URL** waarde.
-  Azure AD-toepassingsproxy gebruiken voor het publiceren van de app on-premises met **Azure Active Directory** geconfigureerd als de verificatiebron. Zie voor instructies [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](../manage-apps/application-proxy-publish-azure-portal.md). 

   Wanneer u configureert de **interne Url** instellen, gebruikt u de aanmeldings-URL die u hebt opgegeven in de sjabloon van de toepassing buiten de galerie. Op deze manier kunnen gebruikers toegang krijgen tot de app vanuit buiten een grens van de organisatie. Application Proxy voert de SAML eenmalige aanmelding voor de on-premises-app.
 
   ![Geeft de interne URL van on-premises app-instellingen en -verificatie](media/hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>Toegang tot IWA en KCD-apps

Voor B2B-gebruikers toegang tot on-premises toepassingen die zijn beveiligd met geïntegreerde Windows-verificatie en Kerberos-beperkte overdracht, moet u de volgende onderdelen:

- **Verificatie via Azure AD-toepassingsproxy**. B2B-gebruikers moeten kunnen worden geverifieerd bij de on-premises toepassing. Om dit te doen, moet u de on-premises-app via de Azure AD-toepassingsproxy publiceren. Zie voor meer informatie, [aan de slag met Application Proxy en de connector installeert](../manage-apps/application-proxy-enable.md) en [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](../manage-apps/application-proxy-publish-azure-portal.md).
- **Autorisatie via een B2B-gebruikersobject in de on-premises directory**. De toepassing moet kunnen gebruikers toegangscontroles uitvoeren en Verleen toegang tot de juiste resources. IWA en KCD moet een gebruikersobject in de on-premises Windows Server Active Directory om deze autorisatie te voltooien. Zoals beschreven in [hoe eenmalige aanmelding met KCD works](../manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works), Application Proxy moet dit gebruikersobject te imiteren van de gebruiker een Kerberos-token naar de app verkrijgen. 

   Er zijn twee methoden beschikbaar waarmee u kunt het maken van de Gast gebruiker-objecten die vereist voor verificatie in de on-premises directory zijn voor het scenario B2B-gebruiker:

   - Microsoft Identity Manager (MIM) en de [MIM-beheeragent voor Microsoft Graph](#create-b2b-guest-user-objects-through-mim-preview). 
   - [Een PowerShell-script](#create-b2b-guest-user-objects-through-a-script-preview). Met behulp van het script is een minder ingrijpende oplossing waarvoor geen MIM is vereist. 

Het volgende diagram biedt een overzicht van hoe Azure AD Application Proxy en de generatie van de B2B-gebruikersobject in de on-premises directory werken samen om B2B-gebruikers toegang verlenen tot uw on-premises IWA en KCD-toepassingen. De genummerde stappen worden in detail onder het diagram beschreven.

![Diagram van MIM en B2B-script-oplossingen](media/hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Een gebruiker vanuit een andere organisatie (de Fabrikam-tenant) wordt uitgenodigd voor de Contoso-tenant.
2.  Een gast-gebruikersobject is gemaakt in de Contoso-tenant (bijvoorbeeld een gebruikersobject met een UPN van guest_fabrikam.com#EXT#@contoso.onmicrosoft.com).
3.  De Gast van Fabrikam is geïmporteerd uit Contoso via MIM of via de B2B-PowerShell-script.
4.  Een weergave of 'footprint' van het gebruikersobject (Gast EXT #) voor de Fabrikam-Gast wordt gemaakt in de on-premises adreslijst, Contoso.com, met behulp van MIM of via de B2B-PowerShell-script.
5.  De Gast gebruiker toegang krijgt tot de on-premises toepassing, app.contoso.com.
6.  De verificatieaanvraag is toegestaan via de toepassingsproxy met behulp van Kerberos-beperkte overdracht. 
7.  Omdat het gebruikersobject Gast lokaal bestaat, wordt de verificatie is geslaagd.

### <a name="lifecycle-management-policies"></a>Levenscyclus van management-beleid

U kunt de on-premises B2B-gebruikersobjecten via lifecycle management-beleid beheren. Bijvoorbeeld:

- U kunt multi-factor authentication (MFA)-beleidsregels voor de gastgebruiker instellen zodat MFA tijdens de Application Proxy-verificatie wordt gebruikt. Zie voor meer informatie, [voorwaardelijke toegang voor gebruikers van B2B-samenwerking](conditional-access.md).
- Alle sponsoring, beoordelingen, account verificaties, enzovoort die worden uitgevoerd op de cloud B2B gebruiker van toepassing op de on-premises gebruikers. Bijvoorbeeld, als de gebruiker van de cloud via uw lifecycle management-beleid wordt verwijderd, wordt de on-premises-gebruiker ook verwijderd door MIM Sync of Azure AD Connect sync. Zie voor meer informatie, [gasttoegang beheren met Azure AD-toegang beoordeelt](../active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim-preview"></a>Gebruikersobjecten van B2B-Gast via MIM (Preview) maken

Zie voor meer informatie over hoe u MIM 2016 Service Pack 1 en de MIM-beheeragent voor Microsoft Graph gebruiken om u te maken van de Gast-gebruikersobjecten in de on-premises directory [samenwerking van Azure AD-business-to-business (B2B) met Microsoft Identity Manager (MIM) 2016 SP1 met Azure Application Proxy (openbare Preview)](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario).

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>Gebruikersobjecten van B2B-Gast via een script (Preview) maken

Er is een PowerShell-voorbeeldscript beschikbaar die u als uitgangspunt gebruiken kunt om te maken van de Gast-gebruikersobjecten in uw on-premises Active Directory.

U kunt downloaden, het script en het Leesmij-bestand van de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=51495). Kies de **Script en Leesmij-bestand voor het ophalen van Azure AD B2B gebruikers op de prem.zip** bestand.

Voordat u het script gebruikt, controleert u de vereisten en overwegingen in het bijbehorende Leesmij-bestand te controleren. Tevens, stemt ermee in dat het alleen beschikbaar als een voorbeeld van het script wordt gemaakt. Uw ontwikkelingsteam of een partner moet aanpassen en bekijk het script voordat u deze uitvoeren.

## <a name="license-considerations"></a>Licentie-overwegingen

Zorg ervoor dat u de juiste licenties voor clienttoegang (CAL's) voor externe gastgebruikers die toegang hebben tot on-premises toepassingen. Zie voor meer informatie de sectie "Externe Connectors" van [licenties voor clienttoegang en Management licenties](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx). Raadpleeg uw Microsoft-vertegenwoordiger of plaatselijke softwareleverancier met betrekking tot uw specifieke behoeften van de licentieverlening.

## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory B2B-samenwerking voor hybride organisaties](hybrid-organizations.md)

- Zie voor een overzicht van Azure AD Connect, [uw on-premises directory's integreren met Azure Active Directory](../connect/active-directory-aadconnect.md).

