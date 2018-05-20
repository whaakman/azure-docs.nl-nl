---
title: B2B-samenwerking voor hybride organisaties - Azure Active Directory | Microsoft Docs
description: Partners toegang geven tot zowel on-premises en cloudresources met Azure AD B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b9a74a4e44fefd389a309b776e50d76d362ee9d8
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Azure Active Directory B2B-samenwerking voor hybride organisaties

Azure Active Directory (Azure AD) B2B-samenwerking kunt u uw externe partners toegang geven tot apps en resources in uw organisatie. Dit geldt zelfs in een hybride configuratie waar u zowel on-premises en cloudresources hebt. Het maakt niet uit als u momenteel beheren externe partner accounts lokaal in uw on-premises identity-systeem, of als u de externe accounts in de cloud als Azure AD B2B-gebruikers beheren. U kunt nu deze gebruikers toegang verlenen tot bronnen in beide locaties met dezelfde referenties aanmelden voor beide omgevingen.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>GRANT B2B-gebruikers in Azure AD toegang tot uw lokale-apps

Als uw organisatie Azure AD B2B-samenwerkingsmogelijkheden gebruikt om uit te nodigen gastgebruikers van partnerorganisaties aan uw Azure AD, kunt u nu deze B2B gebruikerstoegang tot lokale apps opgeven.

Voor apps die gebruikmaken van verificatie op basis van SAML, kunt u deze apps beschikbaar voor gebruikers van de B2B via de Azure-portal, Azure AD-toepassingsproxy gebruiken voor verificatie.

Voor apps die gebruikmaken van geïntegreerde Windows-verificatie met Kerberos-beperkte delegatie (KCD), ook u Azure AD-Proxy voor verificatie. Voor autorisatie werkt de gebruikersobject is vereist in de lokale Windows Server Active Directory. Er zijn twee methoden die u gebruiken kunt voor het maken van lokale gebruiker-objecten die uw B2B gastgebruikers vertegenwoordigen.

- U kunt Microsoft Identity Manager (MIM) 2016 SP1 en de MIM-beheeragent gebruiken voor Microsoft Graph.
- U kunt een PowerShell-script gebruiken. (Deze oplossing vereist geen MIM.)

Zie voor meer informatie over het implementeren van deze oplossingen [Grant B2B-gebruikers in Azure AD toegang tot uw on-premises toepassingen](hybrid-cloud-to-on-premises.md).

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Verleen partner lokaal beheerde accounts toegang krijgen bronnen in de cloud tot

Voordat u Azure AD hebben organisaties met on-premises identiteitssystemen oudsher partner accounts in hun on-premises directory beheerd. Als u dergelijke een organisatie, u om ervoor te zorgen dat uw partners toegang hebben als u uw apps en andere resources naar de cloud verplaatsen blijven. In het ideale geval wilt u deze gebruikers op dezelfde set referenties voor toegang tot zowel cloud als on-premises resources. 

We nu aanbieding methoden waar u Azure AD Connect gebruiken kunt voor het synchroniseren van deze lokale accounts naar de cloud als 'gastgebruikers', waar de accounts zich gedragen net zoals Azure AD B2B-gebruikers.

Ter bescherming van gegevens van uw bedrijf, kunt u toegang tot alleen de juiste resources en autorisatie beleidsregels configureren waarmee deze gastgebruikers anders dan bij uw werknemers behandelen.

Zie voor meer informatie implementatie [Grant partner lokaal beheerde accounts toegang krijgen tot bronnen in de cloud met Azure AD B2B-samenwerking](hybrid-on-premises-to-cloud.md).
 
## <a name="next-steps"></a>Volgende stappen

- [Gebruikers in Azure AD GRANT B2B toegang tot uw on-premises toepassingen](hybrid-cloud-to-on-premises.md)
- [Verleen partner lokaal beheerde serviceaccounts toegang bronnen in de cloud met Azure AD B2B-samenwerking](hybrid-on-premises-to-cloud.md)


