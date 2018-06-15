---
title: Uitnodigingen voor Azure Active Directory B2B-samenwerking delegeren | Microsoft Docs
description: Azure Active Directory B2B-samenwerking gebruikerseigenschappen kunnen worden geconfigureerd
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 0c7b0e3dd4d2ab98bc0f0bedc06424b7838fcf9e
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267409"
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

![Beheren hoe om uit te nodigen](media/delegate-invitations/control-who-to-invite.png)

Een tenantbeheerder kan de volgende beleidsregels voor uitnodiging ingesteld met Azure AD B2B-samenwerking:

- Uitnodigingen uitschakelen
- Alleen beheerders en gebruikers in de rol van Gast uitnodiging antwoorden kunnen uitnodigen
- Beheerders, de rol van Gast uitnodiging antwoorden en leden kunnen uitnodigen
- Alle gebruikers bevat, inclusief gasten, kunnen uitnodigen

Tenants zijn standaard ingesteld op #4. (U kunnen B2B gebruikers uitnodigen voor alle gebruikers, met inbegrip van gastgebruikers.)

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen over Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [B2B-samenwerking gastgebruikers zonder een uitnodiging toevoegen](add-user-without-invite.md)
- [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](add-guest-to-role.md)


