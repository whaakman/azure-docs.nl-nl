---
title: Verleen partner lokaal beheerde accounts toegang krijgen bronnen in de cloud als gebruikers van Azure AD B2B tot | Microsoft Docs
description: Geef de externe partners lokaal beheerde toegang tot lokale en cloudresources met dezelfde referenties met Azure AD B2B-samenwerking.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 04/24/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 9abbc5576708468abdda3efeeae4f28376257e32
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Verleen partner lokaal beheerde serviceaccounts toegang bronnen in de cloud met Azure AD B2B-samenwerking

Voordat u Azure Active Directory (Azure AD) hebben organisaties met on-premises identiteitssystemen partner oudsher beheerde accounts in hun on-premises directory. In een organisatie wanneer u apps verplaatsen naar Azure AD, start u om ervoor te zorgen dat uw partners toegang tot de resources die ze nodig hebben. Deze mag niet van belang of de bronnen zich on-premises of in de cloud. Bovendien wilt u uw partner-gebruikers kunnen zich met deze referenties aanmelden voor zowel on-premises en Azure AD-resources. 

Als u accounts voor uw externe partners in uw on-premises directory maken (bijvoorbeeld, u een account maken met de naam aanmelden 'wmoran' voor een externe gebruiker met de naam Wendy Moran in uw domein partners.contoso.com), u kunt nu synchroniseren met deze accounts aan de cloud. In het bijzonder u Azure AD Connect kunt gebruiken voor het synchroniseren van de partner-accounts naar de cloud als Azure AD B2B-gebruikers (dat wil zeggen, de gebruikers met UserType = Gast). Hierdoor kunnen de gebruikers van uw partner voor toegang tot cloudbronnen met dezelfde referenties als de lokale accounts zonder dat zij uitgebreidere toegang dan ze nodig hebben. 

## <a name="identify-unique-attributes-for-usertype"></a>De unieke kenmerken voor UserType identificeren

Voordat u de synchronisatie van het UserType-kenmerk inschakelt, moet u eerst bepalen hoe het UserType-kenmerk worden afgeleid van de lokale Active Directory. Met andere woorden, welke parameters in uw on-premises omgeving uniek zijn voor uw externe deelnemers? Een parameter die deze externe deelnemers van leden van uw eigen organisatie onderscheidt bepalen.

Er zijn twee algemene benaderingen voor deze naar:

- Een niet-gebruikte lokale Active Directory-kenmerk (bijvoorbeeld extensionAttribute1) om te gebruiken als het bronkenmerk aanwijzen. 
- U kunt ook de waarde voor UserType-kenmerk worden afgeleid van andere eigenschappen. Bijvoorbeeld, u wilt synchroniseren van alle gebruikers als Gast als de lokale Active Directory-UserPrincipalName kenmerk met het domein eindigt *@partners.contoso.com*.
 
Zie voor gedetailleerde kenmerk vereisten [synchronisatie van UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Azure AD Connect sync-gebruikers naar de cloud configureren

Nadat u het kenmerk unieke hebt geïdentificeerd, kunt u Azure AD Connect voor deze gebruikers synchroniseren met de cloud als Azure AD B2B-gebruikers configureren (dat wil zeggen, de gebruikers met UserType = Gast). Deze gebruikers zijn uit een oogpunt van de autorisatie te onderscheiden van B2B-gebruikers die zijn gemaakt door het proces van Azure AD B2B-samenwerking uitnodiging.

Zie voor instructies voor implementatie [synchronisatie van UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory B2B-samenwerking voor hybride organisaties](active-directory-b2b-hybrid-organizations.md)
- [Gebruikers in Azure AD GRANT B2B toegang tot uw on-premises toepassingen](active-directory-b2b-hybrid-cloud-to-on-premises.md)
- Zie voor een overzicht van Azure AD Connect [uw on-premises adreslijsten integreren met Azure Active Directory](connect/active-directory-aadconnect.md).

