---
title: Een proefversie in de Azure Marketplace inschakelen met behulp van Azure Active Directory | Azure
description: Een proefversie van type aanbieding met behulp van Azure Active Directory in de Azure Marketplace en AppSource voor uitgevers van app- en service inschakelen.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/04/2018
ms.author: ellacroi
ms.openlocfilehash: c5b7b4967c1acef733d366e651d50706db42aace
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160464"
---
# <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Een proefversie van aanbieding met behulp van Azure Active Directory inschakelen

Azure Active Directory (Azure AD) is een identiteitsservice in de cloud waarmee verificatie werken met een Microsoft- of schoolaccount met behulp van industriestandaard-frameworks. Azure AD biedt ondersteuning voor OAuth en OpenID Connect-verificatie. De [Azure Marketplace](https://azuremarketplace.microsoft.com) Azure AD gebruikt om u en uw klanten te verifiëren.

Zie voor meer informatie over Azure AD, [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

Nadat een klant uw aanbieding in de Marketplace proefversie selecteert, wordt uw klant omgeleid naar uw proefversie omgeving. In uw omgeving proefversie, kunt u uw klant instellen zonder extra stappen voor aanmelden. Uw app of de aanbieding ontvangt een token uit Azure AD tijdens de verificatie. Het token bevat waardevolle gebruikersgegevens die wordt gebruikt voor het maken van een gebruikersaccount in uw app of de aanbieding. U kunt automatiseren van klanten en vergroot de kans van de conversie.

Zie voor meer informatie over het token dat wordt verzonden vanuit Azure AD tijdens de verificatie, [voorbeeld tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Azure AD gebruiken voor het inschakelen van verificatie in uw app of de proefversie van één klik. Azure AD biedt u de volgende voordelen: 
*   Stroomlijn de klantervaring in de Marketplace naar proefversie.
*   Het uiterlijk van een ervaring in het product, onderhouden, zelfs wanneer de gebruiker wordt omgeleid vanuit de Marketplace aan uw domein of de proefversie-omgeving.
*   Vermindert het risico van stopzetting van omleiding, omdat er geen extra stappen aanmelden.
*   Minder barrières voor implementatie voor het vullen van de grote van Azure AD-gebruikers.

## <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Controleer of uw Azure AD-integratie in de Marketplace: apps voor meerdere Tenants
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

## <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Controleer of uw Azure AD-integratie in de Marketplace: apps met één tenant
Azure AD gebruiken voor de ondersteuning van een van de volgende opties voor uw oplossing met één tenant: 
*   Gebruikers toevoegen aan uw directory als gastgebruikers met behulp van Azure Active Directory B2B (Azure AD B2B).
    
    Zie voor meer informatie over Azure AD B2B [wat is Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Handmatig instellen proefversies voor klanten met behulp van de contactpersoon Me optie publiceren.
*   Ontwikkel een proefrit per klant.
*   Bouw een voorbeeld van multitenant-demo-app die gebruikmaakt van eenmalige aanmelding.

## <a name="next-steps"></a>Volgende stappen
*   Controleer de [Azure Marketplace en AppSource publicatiehandleiding voor](./marketplace-publishers-guide.md).
