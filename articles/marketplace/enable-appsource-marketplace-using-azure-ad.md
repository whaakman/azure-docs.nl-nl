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
ms.openlocfilehash: 3ad9308f3bc714ee2877627da8fdb328459b9fe4
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49351946"
---
# <a name="enable-an-appsource-and-marketplace-listing-by-using-azure-active-directory"></a>Een AppSource en Marketplace-aanbieding inschakelen met behulp van Azure Active Directory

 Azure Active Directory (Azure AD) is een identiteitsservice in de cloud waarmee de verificatie met een Microsoft-account. Azure AD maakt gebruik van industriestandaard-frameworks. [Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="azure-ad-benefits"></a>Azure AD-voordelen

Microsoft AppSource en de Azure Marketplace klanten gebruiken in het product ervaringen om te zoeken naar de aanbieding catalogussen. Deze acties vereist dat klanten zich aanmelden bij het product. Integratie van Azure AD biedt de volgende voordelen:

- Snellere betrokkenheid en een optimale klantervaring
- Eenmalige aanmelding (SSO) voor miljoenen zakelijke gebruikers
- Consistente en aanmeldingservaring voor toepassingen die zijn gepubliceerd door andere partners
- Schaalbare, platformoverschrijdende verificatie voor mobiel en cloud-apps

## <a name="offers-that-require-azure-ad"></a>Aanbiedingen waarvoor Azure AD

De verschillende [aanbiedingsopties en typen bieden](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) voor AppSource en de Azure Marketplace, verschillende vereisten voor Azure AD-implementatie hebben. Zie de volgende tabel voor meer informatie:

| **Type aanbieding**    | **Azure AD-eenmalige aanmelding vereist?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Contact opnemen | Proefversie | Test Drive | Handelen |
| Virtuele machine | N/A | Nee | Nee | Nee |
| Azure-Apps (oplossingssjabloon)  | N/A | N/A | N/A | N/A |
| Beheerde Apps  | N/A | N/A | N/A | Nee |
| SaaS  | Nee | Ja | Ja | Ja |
| Containers  | N/A | N/A | N/A | Nee |
| Adviesservices  | Nee | N/A | N/A | N/A |

Zie voor meer informatie over de technische vereisten voor SaaS [SaaS-toepassingen bieden Publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="azure-ad-integration"></a>Azure AD-integratie

- Zie voor informatie over het inschakelen van eenmalige aanmelding met Azure AD integreren in uw aanbieding, [Azure Active Directory voor ontwikkelaars]( https://aka.ms/aaddev).
- Als u meer informatie over Azure AD eenmalige aanmelding, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="enable-a-trial-listing"></a>Een proefversie aanbieding inschakelen

Geautomatiseerde klanten kunt vergroot de kans van de conversie. Als uw klant uw proefversie aanbieding selecteert en wordt omgeleid naar uw proefversie omgeving, kunt u de klant instellen zonder extra stappen voor aanmelden.

Tijdens de verificatie stuurt Azure AD een token aan uw app of de aanbieding. De gebruikersinformatie geleverd door het token wordt het maken van een gebruikersaccount in uw app of de aanbieding. Zie voor meer informatie, [voorbeeld tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Als u Azure AD met één muisklik verificatie inschakelen in uw app of proefversie aanbieding u:

- Stroomlijn de klantervaring in de Marketplace aan uw proefversie aanbieding.
- Onderhouden het uiterlijk van een ervaring in het product, zelfs wanneer de gebruiker wordt doorgestuurd vanuit de Marketplace naar uw domein of de proefversie-omgeving.
- Vermindert het risico van stopzetting wanneer gebruikers worden omgeleid omdat er geen extra stappen aanmelden.
- Minder barrières voor implementatie voor het vullen van de grote van Azure AD-gebruikers.

## <a name="verify-azure-ad-integration"></a>Controleer of de Azure AD-integratie

### <a name="multitenant-solutions"></a>Oplossingen voor meerdere tenants

Azure AD gebruiken ter ondersteuning van de volgende acties:

- Registreer uw app in een van de Marketplace-storefronts. Weergave [App-registratie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) of [AppSource-certificering](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified) voor meer informatie.
- De ondersteuning voor multitenancy inschakelen in Azure AD een evaluatieversie van één klik.

Als je bekend bent met Azure AD voor federatieve eenmalige aanmelding, voert u deze stappen uit:

1. Registreer uw app in de Marketplace.
1. Eenmalige aanmelding met Azure AD ontwikkelen met behulp van [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) of [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
1. De ondersteuning voor multitenancy inschakelen in Azure AD voor een proefversie van één klik.

### <a name="single-tenant-solutions"></a>Oplossingen voor één tenant

Azure AD gebruiken om ondersteuning voor een van de volgende acties:

- Gastgebruikers toevoegen aan uw directory via [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- Proefversies voor klanten handmatig instellen met behulp van de **Contact Me** optie publiceren.
- Ontwikkel een proefrit per klant.
- Bouw een voorbeeld van multitenant-demo-app die gebruikmaakt van eenmalige aanmelding.

## <a name="next-steps"></a>Volgende stappen

- Zorg ervoor dat u hebt [geregistreerd in de Azure Marketplace](https://azuremarketplace.microsoft.com/sell).
- Aanmelden bij [Cloud Partner-Portal](https://cloudpartner.azure.com/) maken of uitvoeren van uw aanbieding.
