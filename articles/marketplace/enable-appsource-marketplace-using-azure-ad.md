---
title: Een Microsoft AppSource en de Azure Marketplace-aanbieding inschakelen met behulp van Azure Active Directory | Azure
description: Een type aanbieding inschakelen met behulp van Azure Active Directory in de Azure Marketplace en AppSource voor uitgevers van app- en service.
services: Azure, AppSource, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/12/2018
ms.author: qianw211
ms.openlocfilehash: d7fd09928c0a687755d216e7f10f7eac23677c63
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45986423"
---
# <a name="enable-a-microsoft-appsource-and-azure-marketplace-listing-by-using-azure-active-directory"></a>Een Microsoft AppSource en de Azure Marketplace-aanbieding inschakelen met behulp van Azure Active Directory

Microsoft Azure Active Directory (Azure AD) is een identiteitsservice in de cloud waarmee de verificatie met een Microsoft-account met behulp van industriestandaard-frameworks.  Zie voor meer informatie over Azure AD, [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="benefits-of-using-azure-active-directory"></a>Voordelen van het gebruik van Azure Active Directory

Microsoft AppSource en de Azure Marketplace klanten gebruiken in het product ervaringen om te zoeken naar de catalogus aanbieding, waardoor er om aan te melden aan het product.  U kunt uw toepassing integreren met Azure AD, engagement versnellen en optimaliseren van de gebruikerservaring. Azure AD:

- Maakt eenmalige aanmelding (SSO) voor miljoenen zakelijke gebruikers.
- Biedt consistente gebruiker aanmelding mogelijk voor toepassingen die zijn gepubliceerd door andere partners.
- Biedt schaalbare, platformoverschrijdende verificatie voor uw mobiele en cloud apps.

Zoals beschreven in de onderstaande sectie zijn bepaalde aanbiedingen vereist voor het implementeren van Azure AD om te publiceren op Marketplace.

## <a name="azure-active-directory-requirements"></a>Vereisten voor Azure Active Directory

Er zijn verschillende [aanbiedingsopties en typen bieden](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) voor Microsoft AppSource en de Azure Marketplace.  Vereisten voor Azure AD voor deze opties voor mapweergave en aanbiedingstypen worden hieronder weergegeven:

| **Type aanbieding**    | **SSO voor AAD vereist?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Contact opnemen | Proefversie | Test Drive | Handelen |
| Virtuele machine | N/A | Nee | Nee | Nee |
| Azure-Apps (oplossingssjabloon)  | N/A | N/A | N/A | N/A |
| Beheerde Apps  | N/A | N/A | N/A | Nee |
| SaaS  | Nee | Ja | Ja | Ja |
| Containers  | N/A | N/A | N/A | Nee |
| Adviesservices  | Nee | N/A | N/A | N/A |

Zie voor meer informatie over de technische vereisten voor SaaS [SaaS-toepassingen bieden Publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="integration-with-azure-active-directory"></a>Integratie met Azure Active Directory

Voor informatie over het integreren met Azure AD om eenmalige aanmelding, gaat u naar https://aka.ms/aaddev.

Zie voor meer informatie over Azure AD-eenmalige aanmelding, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)?

## <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Een proefversie van aanbieding met behulp van Azure Active Directory inschakelen

Nadat een klant uw aanbieding in de Marketplace proefversie selecteert, wordt uw klant omgeleid naar uw proefversie omgeving. In uw omgeving proefversie, kunt u uw klant instellen zonder extra stappen voor aanmelden. Uw app of de aanbieding ontvangt een token uit Azure AD tijdens de verificatie. Het token bevat waardevolle gebruikersgegevens die wordt gebruikt voor het maken van een gebruikersaccount in uw app of de aanbieding. U kunt automatiseren van klanten en vergroot de kans van de conversie.

Zie voor meer informatie over het token dat wordt verzonden vanuit Azure AD tijdens de verificatie, [voorbeeld tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Azure AD gebruiken voor het inschakelen van verificatie in uw app of de proefversie van één klik. Azure AD biedt u de volgende voordelen: 
*   Stroomlijn de klantervaring in de Marketplace naar proefversie.
*   Het uiterlijk van een ervaring in het product, onderhouden, zelfs wanneer de gebruiker wordt omgeleid vanuit de Marketplace aan uw domein of de proefversie-omgeving.
*   Vermindert het risico van stopzetting van omleiding, omdat er geen extra stappen aanmelden.
*   Minder barrières voor implementatie voor het vullen van de grote van Azure AD-gebruikers.

### <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Controleer of uw Azure AD-integratie in de Marketplace: apps voor meerdere Tenants
Azure AD gebruiken voor het ondersteunen van de volgende opties voor uw oplossing:
*   Registreer uw app in storefronts in de Marketplace.
*   De ondersteuning voor multitenancy inschakelen in Azure AD een evaluatieversie van één klik.

Zie voor meer informatie over app-registratie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

Als u niet bekend voor het gebruik van Azure AD federatieve eenmalige aanmelding (SSO), voert u deze stappen uit:
1.  Registreer uw app in de Marketplace. 
2.  Ontwikkel eenmalige aanmelding met Azure AD met behulp van OAuth 2.0 of OpenID Connect.
    *   Zie voor meer informatie over OAuth 2.0, [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
    *   Zie voor meer informatie over het Open ID Connect [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
3.  De ondersteuning voor multitenancy inschakelen in Azure AD voor een proefversie van één klik.
    
    Zie voor meer informatie over AppSource-certificering, [AppSource-certificering](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

### <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Controleer of uw Azure AD-integratie in de Marketplace: apps met één tenant
Azure AD gebruiken voor de ondersteuning van een van de volgende opties voor uw oplossing met één tenant: 
*   Gebruikers toevoegen aan uw directory als gastgebruikers met behulp van Azure Active Directory B2B (Azure AD B2B). Zie voor meer informatie over Azure AD B2B [wat is Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Handmatig instellen proefversies voor klanten met behulp van de contactpersoon Me optie publiceren.
*   Ontwikkel een proefrit per klant.
*   Bouw een voorbeeld van multitenant-demo-app die gebruikmaakt van eenmalige aanmelding.

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan, 
- [Registreren](https://azuremarketplace.microsoft.com/sell) in de marketplace.

Als u bent geregistreerd en het maken van een nieuwe aanbieding of werken aan een bestaande resourcegroep
- [Meld u aan bij de Cloud Partner-Portal](https://cloudpartner.azure.com/) maken of uitvoeren van uw aanbieding.

