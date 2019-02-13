---
title: B2B-samenwerking voor hybride organisaties - Azure Active Directory | Microsoft Docs
description: Uw partners toegang geven tot zowel on-premises en cloudresources met Azure AD B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a95b9bc6637526ef0d975815cc88dd084360397
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56196906"
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Azure Active Directory B2B-samenwerking voor hybride organisaties

Azure Active Directory (Azure AD) B2B-samenwerking kunt eenvoudig uw externe partners toegang geven tot apps en resources in uw organisatie. Dit geldt zelfs in een hybride configuratie waarbij u zowel on-premises en cloudbronnen hebben. Het maakt niet uit als u momenteel externe partneraccounts beheren lokaal in uw on-premises identiteitssysteem, of als u de externe accounts in de cloud als Azure AD B2B-gebruikers beheren. U kunt nu deze gebruikers toegang verlenen tot bronnen in één van beide locaties met dezelfde referenties aanmelden voor beide omgevingen.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>GRANT B2B-gebruikers in Azure AD-toegang tot uw on-premises toepassingen

Als uw organisatie gebruikmaakt van functionaliteit voor Azure AD B2B-samenwerking uitnodigen van gastgebruikers van partnerorganisaties koppelt aan uw Azure AD, kunt u nu deze B2B-gebruikers toegang hebben tot on-premises apps opgeven.

Voor apps die gebruikmaken van verificatie op basis van SAML, kunt u deze apps beschikbaar maken voor B2B-gebruikers via de Azure-portal, Azure AD-toepassingsproxy gebruiken voor verificatie.

Voor apps die gebruikmaken van geïntegreerde Windows-verificatie (IWA) met Kerberos-beperkte delegatie (KCD), gebruikt u ook Azure AD-Proxy voor verificatie. Voor autorisatie om te werken, is een gebruikersobject echter vereist in de on-premises Windows Server Active Directory. Er zijn twee methoden die u gebruiken kunt om lokale gebruiker-objecten die staan voor de gebruikers van uw B2B-gasten te maken.

- U kunt Microsoft Identity Manager (MIM) 2016 SP1 en de MIM-beheeragent gebruiken voor Microsoft Graph.
- U kunt een PowerShell-script gebruiken. (Deze oplossing is MIM niet vereist.)

Zie voor meer informatie over het implementeren van deze oplossingen [Grant B2B-gebruikers in Azure AD de toegang tot uw on-premises toepassingen](hybrid-cloud-to-on-premises.md).

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Verleen lokaal beheerde partneraccounts toegang krijgen bronnen in de cloud tot

Voordat u Azure AD hebben organisaties met on-premises identiteitssystemen traditioneel partneraccounts in hun on-premises directory beheerd. Als u die een organisatie bent, wilt u om ervoor te zorgen dat uw partners blijven toegang houden als u uw apps en andere resources naar de cloud verplaatsen. In het ideale geval wilt u deze gebruikers op dezelfde set referenties voor toegang tot zowel cloud en on-premises resources. 

We nu aanbieding methoden waar u Azure AD Connect gebruiken kunt om te synchroniseren van deze lokale accounts naar de cloud als 'gastgebruikers', waar de accounts werken net zoals Azure AD B2B-gebruikers.

U kunt ter bescherming van gegevens van uw bedrijf, toegang tot alleen de juiste resources beheren en configureren van autorisatiebeleidsregels die deze gastgebruikers anders dan bij uw werknemers te behandelen.

Zie voor meer informatie voor implementatie, [verlenen partner lokaal beheerde accounts toegang krijgen tot bronnen in de cloud met behulp van Azure AD B2B-samenwerking](hybrid-on-premises-to-cloud.md).
 
## <a name="next-steps"></a>Volgende stappen

- [GRANT B2B-gebruikers in Azure AD-toegang tot uw on-premises toepassingen](hybrid-cloud-to-on-premises.md)
- [Verleen partner lokaal beheerde accounts toegang bronnen in de cloud met behulp van Azure AD B2B-samenwerking](hybrid-on-premises-to-cloud.md)


