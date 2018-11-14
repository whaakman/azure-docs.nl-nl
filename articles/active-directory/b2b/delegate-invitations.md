---
title: Uitnodigingen voor Azure Active Directory B2B-samenwerking delegeren | Microsoft Docs
description: Azure Active Directory B2B-samenwerking gebruikerseigenschappen kunnen worden geconfigureerd
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: fd00fb8da3cf36518f9e28e827e59fd7ff45d687
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622207"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Uitnodigingen voor Azure Active Directory B2B-samenwerking delegeren

Met business-to-business (B2B)-samenwerking van Azure Active Directory (Azure AD) hoeft u niet te worden van een globale beheerder zijn om uitnodigingen te verzenden. U kunt in plaats daarvan gebruik van beleid en delegeren uitnodigingen voor gebruikers wiens rollen toestaat om uitnodigingen te verzenden. Een belangrijke nieuwe manier om te delegeren uitnodigingen voor gasten is via de rol Gastuitnodiging.

## <a name="guest-inviter-role"></a>De rol Gastuitnodiging
We kunnen de gebruiker toewijzen aan de rol Gastuitnodiging om uitnodigingen te verzenden. U hebt geen lid van de rol globale beheerder zijn om uitnodigingen te verzenden. Standaard aanroepen normale gebruikers ook de uitnodiging-API, tenzij een globale beheerder uitgeschakeld uitnodigingen voor reguliere gebruikers. Een gebruiker kan ook aanroepen voor de API met behulp van de Azure portal of PowerShell.

Hier volgt een voorbeeld waarin wordt uitgelegd hoe u PowerShell gebruiken voor het toevoegen van een gebruiker aan de rol Gastuitnodiging:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Bepalen wie kan uitnodigingen versturen

Ga naar Azure Active Directory > Instellingen > instellingen voor externe samenwerking beheren

![externalusers](https://user-images.githubusercontent.com/13383753/45905128-2c47f680-bda4-11e8-955d-6219c67935e0.PNG)

Met Azure AD B2B-samenwerking kunt tenantbeheerder de volgende uitnodiging beleidsregels instellen:

- Uitnodigingen uitschakelen
- Alleen beheerders en gebruikers in de rol Gastuitnodiging kunnen uitnodigingen versturen
- Beheerders, de rol Gastuitnodiging en leden kunnen uitnodigingen versturen
- Alle gebruikers, inclusief gasten, kunnen uitnodigingen versturen

Tenants zijn standaard ingesteld op #4. (U kunnen B2B-gebruikers uitnodigen voor alle gebruikers, inclusief gasten,.)

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [B2B-samenwerking gastgebruikers toevoegen zonder uitnodiging](add-user-without-invite.md)
- [B2B-samenwerking gebruiker toe te voegen aan een rol](add-guest-to-role.md)


