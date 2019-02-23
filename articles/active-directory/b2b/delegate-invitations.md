---
title: 'Uitnodigingen voor B2B-samenwerking: Azure Active Directory delegeren | Microsoft Docs'
description: Azure Active Directory B2B-samenwerking gebruikerseigenschappen kunnen worden geconfigureerd
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18d40397f30b471699f42878a38c88efebcc6305
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674505"
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

Selecteer in Azure Active Directory, **gebruikersinstellingen**. Onder **externe gebruikers**, selecteer **instellingen voor externe samenwerking beheren**.

> [!NOTE]
> De **instellingen voor externe samenwerking** zijn ook toegankelijk in de **organisatie relaties** pagina. In Azure Active Directory, onder **beheren**, gaat u naar **organisatie relaties** > **instellingen**.

![Instellingen voor externe samenwerking](./media/delegate-invitations/control-who-to-invite.png)

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


