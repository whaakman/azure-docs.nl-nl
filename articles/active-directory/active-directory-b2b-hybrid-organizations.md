---
title: Azure Active Directory B2B-samenwerking voor hybride organisaties | Microsoft Docs
description: Partners bieden toegang tot lokale en cloudresources met Azure AD B2B-samenwerking
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 12/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 2e690eeea6a9f7e1cc10830a913774daa3c66689
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Azure Active Directory B2B-samenwerking voor hybride organisaties

In een hybride organisatie, waar u beide hebt on-premises en cloudresources, kunt u geeft externe partners toegang tot lokale en cloud-bronnen. Voor toegang tot lokale bronnen, kunt u lokaal partner accounts beheren in uw on-premises Active Directory-omgeving. Partners zich aanmelden met hun accountpartner referenties voor toegang tot bronnen van uw organisatie. Partners om toegang te verlenen tot cloudbronnen met behulp van deze zelfde referenties, kunt u nu verbinding maken met Azure Active Directory (Azure AD) gebruiken om te synchroniseren van de partner-accounts naar de cloud als Azure AD B2B-gebruikers (dat wil zeggen, de gebruikers met UserType = Gast).

## <a name="identify-unique-attributes-for-usertype"></a>De unieke kenmerken voor UserType identificeren

Voordat u de synchronisatie van het UserType-kenmerk inschakelt, moet u eerst bepalen hoe het UserType-kenmerk worden afgeleid van de lokale Active Directory. Met andere woorden, welke parameters in uw on-premises omgeving uniek zijn voor uw externe deelnemers? Een parameter die deze externe deelnemers van leden van uw eigen organisatie onderscheidt bepalen.

Er zijn twee algemene benaderingen voor deze naar:

- Een niet-gebruikte lokale Active Directory-kenmerk (bijvoorbeeld extensionAttribute1) om te gebruiken als het bronkenmerk aanwijzen. 
- U kunt ook de waarde voor UserType-kenmerk worden afgeleid van andere eigenschappen. Bijvoorbeeld, u wilt synchroniseren van alle gebruikers als Gast als de lokale Active Directory-UserPrincipalName kenmerk met het domein eindigt  *@partners.fabrikam123.org* .
 
Zie voor gedetailleerde kenmerk vereisten [synchronisatie van UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Azure AD Connect sync-gebruikers naar de cloud configureren

Nadat u het kenmerk unieke hebt geïdentificeerd, kunt u Azure AD Connect voor deze gebruikers synchroniseren met de cloud als Azure AD B2B-gebruikers configureren (dat wil zeggen, de gebruikers met UserType = Gast). Deze gebruikers zijn uit een oogpunt van de autorisatie te onderscheiden van B2B-gebruikers die zijn gemaakt door het proces van Azure AD B2B-samenwerking uitnodiging.

Zie voor instructies voor implementatie [synchronisatie van UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure AD B2B-samenwerking [wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- Zie voor een overzicht van Azure AD Connect [uw on-premises adreslijsten integreren met Azure Active Directory](connect/active-directory-aadconnect.md).

