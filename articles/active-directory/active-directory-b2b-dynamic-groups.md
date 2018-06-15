---
title: Dynamische groepen en Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Laat zien hoe u Azure AD-dynamische groepen met Azure Active Directory B2B-samenwerking
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 12/14/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 751502c2be84e9454c507f09a47b609d003ce2c5
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33927848"
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamische groepen en Azure Active Directory B2B-samenwerking

## <a name="what-are-dynamic-groups"></a>Wat zijn dynamische groepen?
Dynamische configuratie van de beveiligingsgroep voor Azure Active Directory (Azure AD) is beschikbaar in [de Azure-portal](https://portal.azure.com). Beheerders kunnen de regels voor het vullen van groepen die zijn gemaakt in Azure AD op basis van gebruikerskenmerken (zoals userType, afdeling of land) instellen. Leden kunnen automatisch worden toegevoegd aan of verwijderd uit een beveiligingsgroep op basis van hun kenmerken. Deze groepen krijgt u toegang tot toepassingen of cloudbronnen (SharePoint-sites, documenten) en licenties toewijzen aan leden. Meer informatie over dynamische groepen in [groepen in Azure Active Directory-specifieke](active-directory-accessmanagement-dedicated-groups.md).

De juiste [Azure AD Premium-P1 of P2 licentieverlening](https://azure.microsoft.com/pricing/details/active-directory/) is vereist voor dynamische groepen maken en gebruiken. Meer informatie in het artikel [op kenmerken gebaseerde regels maken voor dynamisch lidmaatschap in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Wat zijn de ingebouwde dynamische groepen?
De **alle gebruikers** kunnen dynamische groep tenantbeheerders voor het maken van een groep met alle gebruikers in de tenant met een enkele klik. Standaard de **alle gebruikers** groep bevat alle gebruikers in de directory, inclusief leden en gastbesturingssystemen.
Binnen het nieuwe Azure Active Directory-beheerportal kunt u kiezen om in te schakelen de **alle gebruikers** groep in de weergave instellingen.

![Geeft inschakelen de groep alle gebruikers is ingesteld op Ja](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

## <a name="hardening-the-all-users-dynamic-group"></a>De dynamische groep met alle gebruikers beperken
Standaard de **alle gebruikers** groep bevat uw B2B-samenwerking (Gast) gebruikers ook. U kunt verder te beveiligen, uw **alle gebruikers** groeperen met behulp van een regel te verwijderen van gastgebruikers. De volgende afbeelding ziet u de **alle gebruikers** groep Gasten uitsluiten is gewijzigd.

![Toont regel waarbij gebruikerstype niet gelijk is aan Gast](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

Ook het wellicht handig voor het maken van een nieuwe dynamische groep met alleen gastgebruikers zodat u kunt beleidsregels (zoals Azure AD voorwaardelijk toegangsbeleid) toe.
Wat zo'n groep als volgt uitzien:

![Toont regel waarbij gebruikerstype gelijk is aan Gast](media/active-directory-b2b-dynamic-groups/only-guest-users.png)

## <a name="next-steps"></a>Volgende stappen

- [Gebruikerseigenschappen B2B-samenwerking](active-directory-b2b-user-properties.md)
- [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
- [Voorwaardelijke toegang voor gebruikers voor B2B-samenwerking](active-directory-b2b-mfa-instructions.md)

