---
title: Dynamische groepen en Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Laat zien hoe u dynamische groepen van Azure AD met Azure Active Directory B2B-samenwerking
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: d4264a114afc04f162ae7c711b78fce506aa39d1
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54432321"
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamische groepen en Azure Active Directory B2B-samenwerking

## <a name="what-are-dynamic-groups"></a>Wat zijn dynamische groepen?
Dynamische configuratie van het lidmaatschap van beveiligingsgroep voor Azure Active Directory (Azure AD) is beschikbaar in [de Azure-portal](https://portal.azure.com). Beheerders kunnen regels voor het vullen van groepen die zijn gemaakt in Azure AD op basis van gebruikerskenmerken (zoals userType, afdeling of land/regio) instellen. Leden kunnen automatisch worden toegevoegd aan of verwijderd uit een beveiligingsgroep op basis van hun kenmerken. Deze groepen kunnen bieden toegang tot toepassingen of cloudbronnen (SharePoint-sites, documenten) en licenties toewijzen aan leden. Meer informatie over dynamische groepen in [toegewezen groepen in Azure Active Directory](../active-directory-accessmanagement-dedicated-groups.md).

De juiste [Azure AD Premium P1 of P2-licentieverlening](https://azure.microsoft.com/pricing/details/active-directory/) is vereist om te maken en gebruiken van dynamische groepen. Meer informatie in het artikel [op kenmerken gebaseerde regels voor dynamisch groepslidmaatschap maken in Azure Active Directory](../users-groups-roles/groups-dynamic-membership.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Wat zijn de ingebouwde dynamische groepen?
De **alle gebruikers** kunnen tenantbeheerders kunnen maken van een groep met alle gebruikers in de tenant met één klik dynamische groep. Standaard de **alle gebruikers** groep bevat alle gebruikers in de map, met inbegrip van leden en gasten.
Binnen de nieuwe Azure Active Directory-beheerportal, kunt u kiezen om in te schakelen de **alle gebruikers** groep in de weergave instellingen.

![Geeft inschakelen de groep alle gebruikers is ingesteld op Ja](media/use-dynamic-groups/enable-all-users-group.png)

## <a name="hardening-the-all-users-dynamic-group"></a>Beperking van de dynamische groep met alle gebruikers
Standaard de **alle gebruikers** groep bevat uw B2B-samenwerking (Gast) gebruikers ook. U kunt verder te beveiligen, uw **alle gebruikers** groeperen met behulp van een regel te verwijderen van gastgebruikers. De volgende afbeelding ziet u de **alle gebruikers** groep is gewijzigd, zodat de gasten uitsluiten.

![Toont regel waar gebruikerstype niet gelijk is aan Gast](media/use-dynamic-groups/exclude-guest-users.png)

Ook het wellicht handig voor het maken van een nieuwe dynamische groep alleen gastgebruikers, zodat u kunt beleid (zoals Azure AD voorwaardelijke toegangsbeleid) toepassen met aan hen.
Welke zo'n groep kan er als volgt uitzien:

![Toont regel waar gebruikerstype gelijk is aan Gast](media/use-dynamic-groups/only-guest-users.png)

## <a name="next-steps"></a>Volgende stappen

- [Eigenschappen van gebruikers van B2B-samenwerking](user-properties.md)
- [B2B-samenwerking gebruiker toe te voegen aan een rol](add-guest-to-role.md)
- [Voorwaardelijke toegang voor gebruikers van B2B-samenwerking](conditional-access.md)

