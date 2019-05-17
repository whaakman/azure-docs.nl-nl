---
title: B2B-instellingen voor externe samenwerking - Azure Active Directory inschakelen | Microsoft Docs
description: Informatie over het inschakelen van externe Active Directory B2B-samenwerking en beheren wie kunt gastgebruikers uitnodigen. De rol Gastuitnodiging gebruiken om te delegeren uitnodigingen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11dda7fc3760f468c094fb4cf4484a27895f83b9
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65812676"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>Externe B2B-samenwerking inschakelen en beheren die kunnen gasten uitnodigen

In dit artikel wordt beschreven hoe u Azure Active Directory (Azure AD) B2B-samenwerking mogelijk en bepaal wie kunnen gasten uitnodigen. Standaard kunnen alle gebruikers en gasten in uw directory gasten uitnodigen, zelfs als ze niet zijn toegewezen aan een beheerdersrol. Instellingen voor externe samenwerking kunnen u het inschakelen van Gast uitnodigingen in- of uitschakelen voor verschillende soorten gebruikers in uw organisatie. U kunt ook uitnodigingen naar afzonderlijke gebruikers delegeren door het toewijzen van rollen die kunnen worden gasten uitnodigen.

## <a name="configure-b2b-external-collaboration-settings"></a>B2B-instellingen voor externe samenwerking configureren

Met Azure AD B2B-samenwerking kunt tenantbeheerder de volgende uitnodiging beleidsregels instellen:

- Uitnodigingen uitschakelen
- Alleen beheerders en gebruikers in de rol Gastuitnodiging kunnen uitnodigingen versturen
- Beheerders, de rol Gastuitnodiging en leden kunnen uitnodigingen versturen
- Alle gebruikers, inclusief gasten, kunnen uitnodigingen versturen

Standaard kunnen alle gebruikers, inclusief gasten, gastgebruikers uitnodigen.

### <a name="to-configure-external-collaboration-settings"></a>Het configureren van instellingen voor externe samenwerking:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als tenantbeheerder.
2. Selecteer **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
3. Onder **externe gebruikers**, selecteer **instellingen voor externe samenwerking beheren**.
   > [!NOTE]
   > De **instellingen voor externe samenwerking** zijn ook toegankelijk in de **organisatie relaties** pagina. In Azure Active Directory, onder **beheren**, gaat u naar **organisatie relaties** > **instellingen**.
4. Op de **instellingen voor externe samenwerking** pagina, kies het beleid dat u wilt inschakelen.

   ![Instellingen voor externe samenwerking](./media/delegate-invitations/control-who-to-invite.png)

  - **De machtigingen van gastgebruikers zijn beperkt**: Dit beleid bepaalt de machtigingen voor gasten in uw directory. Selecteer **Ja** gasten blokkeren van bepaalde directory-taken, zoals het inventariseren van gebruikers, groepen of andere mapresources. Selecteer **Nee** geven gasten dezelfde toegang tot mapgegevens als normale gebruikers in uw directory.
   - **Beheerders en gebruikers in de rol van gastuitnodiger kunnen uitnodigingen versturen**: Als u wilt toestaan dat beheerders en gebruikers in de rol 'Gastuitnodiging' gasten uitnodigen, kunt u dit beleid instellen op **Ja**.
   - **Leden kunnen uitnodigen**: Als u wilt toestaan dat leden van de niet-beheerder van uw directory gasten uitnodigen, kunt u dit beleid instellen op **Ja**.
   - **Gasten kunnen uitnodigingen versturen**: Als u wilt toestaan dat gasten voor gasten uitnodigen, kunt u dit beleid instellen op **Ja**.
   - **E-mailbericht eenmalige wachtwoordcode voor gasten (Preview) inschakelen**: Zie voor meer informatie over de functie voor eenmalige wachtwoordcode [e eenmalige wachtwoordcode verificatie (preview)](one-time-passcode.md).
   - **Samenwerkingsbeperkingen**: Zie voor meer informatie over het toestaan of blokkeren van uitnodigingen voor specifieke domeinen [uitnodigingen toestaan of blokkeren voor B2B-gebruikers van bepaalde organisaties](allow-deny-list.md).

## <a name="assign-the-guest-inviter-role-to-a-user"></a>De rol Gastuitnodiging toewijzen aan een gebruiker

Met de rol Gastuitnodiging, kunt u afzonderlijke gebruikers bieden de mogelijkheid gasten uitnodigen zonder dat ze een globale beheerder of andere beheerdersrol toewijzen. De rol van gastuitnodiger toewijzen aan individuele gebruikers. Controleer vervolgens of u **beheerders en gebruikers in de rol van gastuitnodiger kunnen uitnodigingen versturen** naar **Ja**.

Hier volgt een voorbeeld waarin wordt uitgelegd hoe u PowerShell gebruiken voor het toevoegen van een gebruiker aan de rol Gastuitnodiging:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [B2B-samenwerking gastgebruikers toevoegen zonder uitnodiging](add-user-without-invite.md)
- [B2B-samenwerking gebruiker toe te voegen aan een rol](add-guest-to-role.md)


