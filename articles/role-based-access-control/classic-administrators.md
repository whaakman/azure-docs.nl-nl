---
title: Administrators van het klassieke Azure-abonnement | Microsoft Docs
description: Hierin wordt beschreven hoe u de rollen Azure mede beheerder en service beheerder toevoegt of wijzigt, en hoe u de account beheerder kunt weer geven.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/19/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c2096b6c5ddb72c8ab5c5d3203a05c94db51f6c5
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444339"
---
# <a name="azure-classic-subscription-administrators"></a>Beheerders van klassieke Azure-abonnementen

Micro soft raadt u aan om de toegang tot Azure-resources te beheren met op rollen gebaseerd toegangs beheer (RBAC). Als u nog steeds gebruikmaakt van het klassieke implementatie model, moet u echter een klassieke rol voor abonnements beheerders gebruiken: Service beheerder en mede beheerder. Zie [Azure Resource Manager vs. klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md)voor meer informatie.

In dit artikel wordt beschreven hoe u de rollen mede beheerder en service beheerder kunt toevoegen of wijzigen, en hoe u de account beheerder kunt weer geven.

## <a name="add-a-co-administrator"></a>Een mede beheerder toevoegen

> [!TIP]
> U hoeft alleen een mede beheerder toe te voegen als de gebruiker klassieke Azure-implementaties moet beheren met behulp van de [Azure Service Management Power shell-module](https://docs.microsoft.com/powershell/module/servicemanagement/azure). Als de gebruiker alleen de Azure Portal gebruikt voor het beheren van de klassieke resources, hoeft u de klassieke beheerder niet toe te voegen aan de gebruiker.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als service beheerder.

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en selecteer een abonnement.

    Mede beheerders kunnen alleen worden toegewezen bij het abonnements bereik.

1. Klik op **toegangsbeheer (IAM)** .

1. Klik op het tabblad **klassieke beheerders** .

    ![Scherm afbeelding waarmee klassieke beheerders worden geopend](./media/classic-administrators/classic-administrators.png)

1. Klik op **toevoegen** > **mede beheerder** toevoegen om het deel venster CO-Administrators toevoegen te openen.

    Als de optie co-beheerder toevoegen is uitgeschakeld, bent u niet gemachtigd.

1. Selecteer de gebruiker die u wilt toevoegen en klik op **toevoegen**.

    ![Scherm afbeelding waarmee co-beheerder wordt toegevoegd](./media/classic-administrators/add-coadmin.png)

### <a name="adding-a-guest-user-as-a-co-administrator"></a>Een gast gebruiker toevoegen als co-beheerder

[Gast gebruikers](../active-directory/b2b/b2b-quickstart-add-guest-users-portal.md) aan wie de rol mede beheerder is toegewezen, kunnen enkele verschillen zien ten opzichte van gebruikers die lid zijn van de rol mede beheerder. Houd rekening met het volgende scenario:

- Gebruiker A met een werk-of school account voor Azure AD is een service beheerder voor een Azure-abonnement.
- Gebruiker B heeft een Microsoft-account.
- Gebruiker A wijst de rol mede beheerder toe aan gebruiker B.
- Gebruiker B kan vrijwel alles doen, maar kan geen toepassingen registreren of gebruikers opzoeken in de Azure AD-adres lijst.

U verwacht dat gebruiker B alles kan beheren. De reden hiervoor is dat de Microsoft-account wordt toegevoegd aan het abonnement als gast gebruiker in plaats van een lid van een gebruiker. Gast gebruikers hebben een andere standaard machtiging in azure AD, vergeleken met gebruikers van leden. Gebruikers kunnen bijvoorbeeld andere gebruikers in azure AD en gast gebruikers lezen. Gebruikers van leden kunnen nieuwe service-principals in azure AD en gast gebruikers registreren. Als een gast gebruiker deze taken moet kunnen uitvoeren, is een mogelijke oplossing om de specifieke Azure AD-beheerders rollen toe te wijzen die de gast gebruiker nodig heeft. In het vorige scenario kunt u bijvoorbeeld de rol van [Directory lezers](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) toewijzen om andere gebruikers te lezen en de rol [toepassings ontwikkelaar](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) toe te wijzen om service-principals te kunnen maken. Zie [Wat zijn de standaard machtigingen voor gebruikers in azure Active Directory?](../active-directory/fundamentals/users-default-permissions.md)voor meer informatie over lid-en gast gebruikers en de bijbehorende machtigingen.

Houd er rekening mee dat de [ingebouwde rollen voor Azure-resources verschillen van](../role-based-access-control/built-in-roles.md) de [Azure AD-beheerders rollen](../active-directory/users-groups-roles/directory-assign-admin-roles.md). De ingebouwde rollen verlenen geen toegang tot Azure AD. Zie [inzicht krijgen in de verschillende rollen](../role-based-access-control/rbac-and-directory-admin-roles.md)voor meer informatie.

## <a name="remove-a-co-administrator"></a>Een mede beheerder verwijderen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als service beheerder.

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en selecteer een abonnement.

1. Klik op **toegangsbeheer (IAM)** .

1. Klik op het tabblad **klassieke beheerders** .

1. Voeg een vinkje toe naast de co-beheerder die u wilt verwijderen.

1. Klik op **Verwijderen**.

1. Klik op **Ja**in het bericht venster dat wordt weer gegeven.

    ![Scherm afbeelding waarmee co-beheerder wordt verwijderd](./media/classic-administrators/remove-coadmin.png)

## <a name="change-the-service-administrator"></a>De servicebeheerder wijzigen

Alleen de account beheerder kan de service beheerder voor een abonnement wijzigen. Wanneer u zich aanmeldt voor een Azure-abonnement, is de service beheerder standaard hetzelfde als de account beheerder. De gebruiker met de rol account beheerder heeft geen toegang tot de Azure Portal. De gebruiker met de rol van service beheerder heeft volledige toegang tot de Azure Portal. Als de account beheerder en service beheerder dezelfde gebruiker zijn en u de service beheerder wijzigt in een andere gebruiker, verliest de account beheerder de toegang tot Azure Portal. De account beheerder kan echter altijd het account centrum gebruiken om de service beheerder weer op zichzelf te zetten.

Er zijn twee manieren om de servicebeheerder te wijzigen. U kunt wijzigingen aanbrengen in de **Azure Portal** of het **account centrum**.

### <a name="azure-portal"></a>Azure Portal

1. Zorg ervoor dat uw scenario wordt ondersteund door de beperkingen voor het wijzigen van service beheerders te controleren.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als account beheerder.

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en selecteer een abonnement.

1. Klik op **Eigenschappen**.

    ![Scherm opname van de account beheerder](./media/classic-administrators/account-admin.png)

1. Klik bovenaan op **service beheerder** om het deel venster Service beheerder te openen.

    Als de knop Service beheer is uitgeschakeld, bent u niet gemachtigd. Alleen de gebruiker die de account beheerder is, kan de service beheerder wijzigen.

1. Selecteer een nieuwe service beheerder en klik vervolgens op **Opslaan**.

### <a name="account-center"></a>Accountcentrum

1. Zorg ervoor dat uw scenario wordt ondersteund door de beperkingen voor het wijzigen van service beheerders te controleren.

1. Meld u als account beheerder aan bij het [account centrum](https://account.windowsazure.com/subscriptions) .

1. Klik op een abonnement.

1. Klik aan de rechter kant op **abonnements gegevens bewerken**.

    ![Scherm afbeelding met de knop abonnement bewerken in het account centrum](./media/classic-administrators/editsub.png)

1. Voer in het vak **service beheerder** het e-mail adres van de nieuwe service beheerder in.

    ![Scherm opname van het vak voor het wijzigen van het e-mail adres van de service beheerder](./media/classic-administrators/change-service-admin.png)

1. Klik op het vinkje om de wijziging op te slaan.

### <a name="limitations-for-changing-the-service-administrator"></a>Beperkingen voor het wijzigen van de service beheerder

Elk abonnement is gekoppeld aan een Azure AD-adres lijst. Als u wilt zoeken naar de map waaraan het abonnement is gekoppeld, opent u **abonnementen** in de Azure Portal en selecteert u vervolgens een abonnement om de map weer te geven.

Als u bent aangemeld met een werk-of school account, kunt u andere accounts in uw organisatie toevoegen als service beheerder. Bijvoorbeeld, abby@contoso.com kan als service bob@contoso.com beheerder worden toegevoegd, maar kan alleen john@notcontoso.com john@notcontoso.com worden toegevoegd als de map contoso.com een aanwezigheid heeft. Gebruikers die zijn aangemeld met werk-of school accounts, kunnen Microsoft-account gebruikers blijven toevoegen als service beheerder. Er kan slechts één service beheerder per Azure-abonnement zijn.

  | Aanmeldings methode | Microsoft-account gebruiker toevoegen als een service beheerder? | Werk-of school account toevoegen in dezelfde organisatie als een service beheerder? | Werk-of school account toevoegen in een andere organisatie als service beheerder? |
  | --- | --- | --- | --- |
  |  Microsoft-account |Ja |Nee |Nee |
  |  Werk- of schoolaccount |Ja |Ja |Nee |

## <a name="view-the-account-administrator"></a>De account beheerder weer geven

De account beheerder is de gebruiker die zich voor het eerst aanmeldt voor het Azure-abonnement en is verantwoordelijk als de facturerings eigenaar van het abonnement. Zie [eigendom van een Azure-abonnement overdragen aan een ander account](../billing/billing-subscription-transfer.md)om de account beheerder van een abonnement te wijzigen.

Volg deze stappen om de account beheerder weer te geven.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en selecteer een abonnement.

1. Klik op **Eigenschappen**.

    De account beheerder van het abonnement wordt weer gegeven in het vak **account beheerder** .

    ![Scherm opname van de account beheerder](./media/classic-administrators/account-admin.png)

## <a name="next-steps"></a>Volgende stappen

* [Inzicht in de verschillende rollen](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Toegang tot Azure-resources beheren met op rollen gebaseerd toegangsbeheer en de Azure-portal](../role-based-access-control/role-assignments-portal.md)
* [Azure-abonnementsbeheerders toevoegen of wijzigen](../billing/billing-add-change-azure-subscription-administrator.md)
