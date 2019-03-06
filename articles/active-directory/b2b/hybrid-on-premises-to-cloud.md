---
title: Lokale partner-accounts naar de cloud als gebruikers van B2B - Azure Active Directory synchroniseren | Microsoft Docs
description: Geef externe partners lokaal beheerde toegang tot lokale en cloudresources met dezelfde referenties met Azure AD B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd684b992d46edbec30a12dc3b166d6193d2eabe
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57442151"
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Verleen partner lokaal beheerde accounts toegang bronnen in de cloud met behulp van Azure AD B2B-samenwerking

Voordat u Azure Active Directory (Azure AD) hebben organisaties met on-premises identiteitssystemen traditioneel beheerde partneraccounts in hun on-premises directory. In een organisatie wanneer u apps verplaatsen naar Azure AD, start u om ervoor te zorgen dat uw partners toegang hebben tot de resources die ze nodig hebben. Deze mag niet van belang of de resources zich on-premises of in de cloud. Bovendien wilt u uw gebruikers van partnerorganisaties kunnen zich met deze referenties aanmelden voor zowel on-premises en Azure AD-resources. 

Als u accounts voor uw externe partners in uw on-premises directory maken (bijvoorbeeld, u een account maken met de naam sign-in 'wmoran' voor een externe gebruiker met de naam Wendy Moran in uw domein partners.contoso.com), kunt u deze accounts kunnen nu synchroniseren de cloud. Specifiek, kunt u Azure AD Connect synchroniseren van de partneraccounts naar de cloud als Azure AD B2B-gebruikers (dat wil zeggen, de gebruikers met UserType = Gast). Hierdoor kunnen de gebruikers van uw partner voor toegang tot cloudresources met dezelfde referenties als hun lokale accounts, zonder dat zij uitgebreidere toegang dan ze nodig hebben. 

## <a name="identify-unique-attributes-for-usertype"></a>De unieke kenmerken voor UserType identificeren

Voordat u de synchronisatie van het UserType-kenmerk inschakelt, moet u eerst bepalen hoe u het UserType-kenmerk zijn afgeleid van on-premises Active Directory. Met andere woorden, welke parameters in uw on-premises omgeving uniek zijn voor uw externe deelnemers? Een parameter die deze externe deelnemers van leden van uw eigen organisatie onderscheidt bepalen.

Er zijn twee algemene benaderingen gebruikt voor deze voor:

- Een niet-gebruikte on-premises Active Directory-kenmerk (bijvoorbeeld extensionAttribute1) om te gebruiken als het bronkenmerk aanwijzen. 
- U kunt ook de waarde voor UserType-kenmerk zijn afgeleid van andere eigenschappen. Bijvoorbeeld, u wilt synchroniseren van alle gebruikers als Gast als hun on-premises Active Directory-UserPrincipalName-kenmerk met het domein eindigt *@partners.contoso.com*.
 
Zie voor gedetailleerde kenmerk [schakelt u de synchronisatie van UserType](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Azure AD Connect om gebruikers te synchroniseren naar de cloud configureren

Nadat u het kenmerk unieke hebt geïdentificeerd, kunt u Azure AD Connect voor deze gebruikers synchroniseren met de cloud als Azure AD B2B-gebruikers configureren (dat wil zeggen, de gebruikers met UserType = Gast). Vanuit het oogpunt van een autorisatie zijn deze gebruikers te onderscheiden van B2B-gebruikers die zijn gemaakt via het uitnodigingsproces voor Azure AD B2B-samenwerking.

Zie voor de implementatie-instructies, [schakelt u de synchronisatie van UserType](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory B2B-samenwerking voor hybride organisaties](hybrid-organizations.md)
- [GRANT B2B-gebruikers in Azure AD-toegang tot uw on-premises toepassingen](hybrid-cloud-to-on-premises.md)
- Zie voor een overzicht van Azure AD Connect, [uw on-premises directory's integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

