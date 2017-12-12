---
title: Uitnodigingen voor Azure Active Directory B2B-samenwerking delegeren | Microsoft Docs
description: Azure Active Directory B2B-samenwerking gebruikerseigenschappen kunnen worden geconfigureerd
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 08d06153ab64021d94cd3fdc8ac13839bb4c6908
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Gemachtigde uitnodigingen voor Azure Active Directory B2B-samenwerking

Samenwerking van Azure Active Directory (Azure AD)-business-to-business (B2B), moet u beschikt niet over een globale beheerder uitnodigingen verzenden. In plaats daarvan kunt u beleidsregels gebruiken en uitnodigingen voor gebruikers waarvan rollen kunnen worden uitnodiging delegeren. Er is een belangrijke nieuwe manier om te delegeren Gast gebruiker uitnodigingen via de functie Gast uitnodiging antwoorden.

## <a name="guest-inviter-role"></a>Rol van Gast uitnodiging antwoorden
We kan de gebruiker toewijzen aan de rol van Gast uitnodiging antwoorden uitnodigingen verzenden. U hoeft niet te worden lid van de rol globale beheerder uitnodigingen verzenden. Standaard aanroepen gewone gebruikers ook de uitnodiging-API als een globale beheerder uitgeschakeld uitnodigingen voor reguliere gebruikers. Een gebruiker kan ook de met de Azure-portal of PowerShell API aanroepen.

Hier volgt een voorbeeld ziet u hoe u een gebruiker toevoegen aan de rol van Gast uitnodiging antwoorden met PowerShell:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Beheren wie kunt uitnodigen

![Beheren hoe om uit te nodigen](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

Een tenantbeheerder kan de volgende beleidsregels voor uitnodiging ingesteld met Azure AD B2B-samenwerking:

- Uitnodigingen uitschakelen
- Alleen beheerders en gebruikers in de rol van Gast uitnodiging antwoorden kunnen uitnodigen
- Beheerders, de rol van Gast uitnodiging antwoorden en leden kunnen uitnodigen
- Alle gebruikers bevat, inclusief gasten, kunnen uitnodigen

Tenants zijn standaard ingesteld op #4. (U kunnen B2B gebruikers uitnodigen voor alle gebruikers, met inbegrip van gastgebruikers.)

## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Gebruikerseigenschappen B2B-samenwerking](active-directory-b2b-user-properties.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
* [Dynamische groepen en B2B-samenwerking](active-directory-b2b-dynamic-groups.md)
* [B2B-samenwerking code en PowerShell-voorbeelden](active-directory-b2b-code-samples.md)
* [SaaS-apps voor B2B-samenwerking configureren](active-directory-b2b-configure-saas-apps.md)
* [B2B-samenwerking gebruikerstokens](active-directory-b2b-user-token.md)
* [Gebruikersclaims voor B2B-samenwerking toewijzing](active-directory-b2b-claims-mapping.md)
* [Office 365 extern delen](active-directory-b2b-o365-external-user.md)
* [Huidige beperkingen voor B2B-samenwerking](active-directory-b2b-current-limitations.md)
