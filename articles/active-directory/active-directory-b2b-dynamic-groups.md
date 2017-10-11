---
title: Dynamische groepen en Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Azure Active Directory B2B-samenwerking kan met Azure AD-dynamische groepen worden gebruikt
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: 5818c41610c8c5df89abcb0dcd058bcbe9579ce7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamische groepen en Azure Active Directory B2B-samenwerking

## <a name="what-are-dynamic-groups"></a>Wat zijn dynamische groepen?
Dynamische configuratie van de beveiligingsgroep voor Azure Active Directory (Azure AD) is beschikbaar in [de Azure-portal](https://portal.azure.com). Beheerders kunnen de regels voor het vullen van groepen die zijn gemaakt in Azure Active Directory op basis van gebruikerskenmerken (zoals userType, afdeling of land) instellen. Leden kunnen automatisch worden toegevoegd aan of verwijderd uit een beveiligingsgroep op basis van hun kenmerken. Deze groepen krijgt u toegang tot toepassingen of cloudbronnen (SharePoint-sites, documenten) en licenties toewijzen aan leden. Meer informatie over dynamische groepen in [groepen in Azure Active Directory-specifieke](active-directory-accessmanagement-dedicated-groups.md).

De juiste [Azure AD Premium-P1 of P2 licentieverlening](https://azure.microsoft.com/pricing/details/active-directory/) is vereist voor dynamische groepen maken en gebruiken. Meer informatie in het artikel [op kenmerken gebaseerde regels maken voor dynamisch lidmaatschap in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Wat zijn de ingebouwde dynamische groepen?
De **alle gebruikers** kunnen dynamische groep tenantbeheerders voor het maken van een groep met alle gebruikers in de tenant met een enkele klik. Standaard de **alle gebruikers** groep bevat alle gebruikers in de directory, inclusief leden en gastbesturingssystemen.
Binnen het nieuwe Azure Active Directory-beheerportal kunt u kiezen om in te schakelen de **alle gebruikers** groep in de weergave instellingen.

![ingebouwde groepen](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-the-all-users-dynamic-group"></a>De dynamische groep met alle gebruikers beperken
Standaard de **alle gebruikers** groep bevat uw B2B-samenwerking (Gast) gebruikers ook. U kunt verder te beveiligen, uw **alle gebruikers** groeperen met behulp van een regel te verwijderen van gastgebruikers. De volgende afbeelding ziet u de **alle gebruikers** groep Gasten uitsluiten is gewijzigd.

![groep alle gebruikers inschakelen](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

Ook het wellicht handig voor het maken van een nieuwe dynamische groep met alleen gastgebruikers zodat u kunt beleidsregels (zoals Azure AD voorwaardelijk toegangsbeleid) toe.
Wat zo'n groep als volgt uitzien:

![gastgebruikers uitsluiten](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Gebruikerseigenschappen B2B-samenwerking](active-directory-b2b-user-properties.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
* [B2B-samenwerking uitnodigingen delegeren](active-directory-b2b-delegate-invitations.md)
* [B2B-samenwerking code en PowerShell-voorbeelden](active-directory-b2b-code-samples.md)
* [SaaS-apps voor B2B-samenwerking configureren](active-directory-b2b-configure-saas-apps.md)
* [B2B-samenwerking gebruikerstokens](active-directory-b2b-user-token.md)
* [Gebruikersclaims voor B2B-samenwerking toewijzing](active-directory-b2b-claims-mapping.md)
* [Office 365 extern delen](active-directory-b2b-o365-external-user.md)
* [Huidige beperkingen voor B2B-samenwerking](active-directory-b2b-current-limitations.md)
