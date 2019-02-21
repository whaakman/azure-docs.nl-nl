---
title: Beheerders van de klassieke Azure-abonnement | Microsoft Docs
description: Beschrijft hoe toevoegen of wijzigen van de rollen Azure Medebeheerder en servicebeheerder en accountbeheerder weergeven.
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
ms.openlocfilehash: f4838f688210f9017d691bd6a463217a75fc90d4
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447511"
---
# <a name="azure-classic-subscription-administrators"></a>Beheerders van de klassieke Azure-abonnement

Microsoft raadt aan dat u de toegang tot Azure-resources met behulp van op rollen gebaseerd toegangsbeheer (RBAC) beheren. Echter, als u nog steeds het klassieke implementatiemodel gebruikt, moet u gebruik van de rol van een klassiek abonnement beheerder: Servicebeheerder en Medebeheerder. Zie voor meer informatie, [Azure Resource Manager en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).

Dit artikel wordt beschreven hoe u wilt toevoegen of wijzigen van de rollen Medebeheerder en servicebeheerder en om weer te geven van de accountbeheerder.

## <a name="add-a-co-administrator"></a>Toevoegen van een CO-beheerder

> [!TIP]
> U hoeft alleen het toevoegen van een CO-beheerder als de gebruiker nodig heeft voor het klassieke Azure-implementaties beheren met behulp van [PowerShell-Module voor Azure Service Management](https://docs.microsoft.com/en-us/powershell/module/servicemanagement/azure). Als de gebruiker wordt alleen de Azure portal gebruikt om de klassieke resources te beheren, hoeft u niet om toe te voegen van de klassieke administrator voor de gebruiker.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Service-beheerder.

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en een abonnement te selecteren.

    CO-beheerders kunnen alleen worden toegewezen op het abonnementsbereik.

1. Klik op **Toegangsbeheer (IAM)**.

1. Klik op de **klassieke administrators** tabblad.

    ![Schermafbeelding van klassieke administrators wordt geopend](./media/classic-administrators/classic-administrators.png)

1. Klik op **toevoegen** > **CO-beheerder toevoegen** om het deelvenster van de CO-beheerders toevoegen te openen.

    Als de optie toevoegen CO-beheerder is uitgeschakeld, bent u niet gemachtigd.

1. Selecteer de gebruiker die u wilt toevoegen en klik op **toevoegen**.

    ![Schermafbeelding van CO-beheerder wordt toegevoegd](./media/classic-administrators/add-coadmin.png)

### <a name="adding-a-guest-user-as-a-co-administrator"></a>Een gastgebruiker toevoegen als een CO-beheerder

[Gastgebruikers](../active-directory/b2b/b2b-quickstart-add-guest-users-portal.md) die de CO-beheerder zijn toegewezen rol ziet mogelijk enkele verschillen in vergelijking met gebruikers met de rol CO-beheerder. Houd rekening met het volgende scenario:

- Gebruiker A met een Azure AD-werk- of School-account is een Service-beheerder voor een Azure-abonnement.
- Gebruiker B heeft een Microsoft-account.
- Gebruiker A wijst de rol CO-beheerder voor gebruiker b
- Gebruiker B kan bijna alles doen, maar kan niet registreren van toepassingen of gebruikers in de Azure AD-directory opzoeken.

U zou verwachten dat de gebruiker B kan alles beheren. De reden voor dit verschil is dat het Microsoft-account wordt toegevoegd aan het abonnement als gastgebruiker in plaats van een gebruiker lid. Gastgebruikers hebben verschillende standaardmachtigingen in Azure AD in vergelijking met gebruikers. Bijvoorbeeld lidgebruikers andere gebruikers in Azure AD kunnen lezen en gastgebruikers kunnen niet. Lidgebruikers nieuwe service-principals in Azure AD kunnen registreren en gastgebruikers kunnen niet. Als een gastgebruiker kunnen deze taken uit te voeren moet, een mogelijke oplossing bestaat uit het toewijzen van de specifieke Azure AD-beheerdersrollen de gastgebruiker moet. Bijvoorbeeld, in het voorgaande scenario kan u toewijzen de [Adreslijstlezers](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) rol andere gebruikers maken en toewijzen de [toepassingsontwikkelaar](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) rol mogelijk te maken van service-principals. Zie voor meer informatie over lid en gastgebruikers en hun machtigingen [wat zijn de standaardmachtigingen van de gebruiker in Azure Active Directory?](../active-directory/fundamentals/users-default-permissions.md).

Houd er rekening mee dat de [ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md) zijn anders dan de [Azure AD-beheerdersrollen](../active-directory/users-groups-roles/directory-assign-admin-roles.md). De ingebouwde rollen verlenen geen toegang aan Azure AD. Zie voor meer informatie, [inzicht in de verschillende rollen](../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="remove-a-co-administrator"></a>Verwijderen van een CO-beheerder

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een Service-beheerder.

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en een abonnement te selecteren.

1. Klik op **Toegangsbeheer (IAM)**.

1. Klik op de **klassieke administrators** tabblad.

1. Voeg een vinkje naast de CO-beheerder u wilt verwijderen.

1. Klik op **verwijderen**.

1. In het vak van het bericht dat wordt weergegeven, klikt u op **Ja**.

    ![Schermafbeelding van CO-beheerder wordt verwijderd](./media/classic-administrators/remove-coadmin.png)

## <a name="change-the-service-administrator"></a>De servicebeheerder wijzigen

Alleen de accountbeheerder kan de Service-beheerder voor een abonnement wijzigen. Wanneer u zich aanmeldt voor een Azure-abonnement, is de servicebeheerder standaard hetzelfde als de accountbeheerder. De gebruiker met de rol beheerder van de Account heeft geen toegang tot de Azure-portal. De gebruiker met de rol servicebeheerder heeft volledige toegang tot de Azure-portal. Als de servicebeheerder en accountbeheerder dezelfde gebruiker zijn en u de servicebeheerder in een andere gebruiker wijzigen, verliest u de accountbeheerder bent toegang tot Azure portal. De accountbeheerder kan echter altijd Accountcentrum gebruiken om te wijzigen van de servicebeheerder terug naar zichzelf.

Er zijn twee manieren om te wijzigen van de Service-beheerder. U kunt wijzigen de **Azure-portal** of **Accountcentrum**.

### <a name="azure-portal"></a>Azure Portal

1. Zorg ervoor dat uw scenario wordt ondersteund door het controleren van de [beperkingen voor het wijzigen van servicebeheerders](#limits).

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als de accountbeheerder.

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en een abonnement te selecteren.

1. Klik op **Eigenschappen**.

    ![Schermopname van de accountbeheerder](./media/classic-administrators/account-admin.png)

1. Klik aan de bovenkant **servicebeheerder** om de Service-beheerder deelvenster te openen.

    Als de knop servicebeheerder is uitgeschakeld, bent u niet gemachtigd. Alleen de gebruiker die de accountbeheerder kan de servicebeheerder wijzigen.

1. Selecteer een nieuwe Service-beheerder en klik vervolgens op **opslaan**.

### <a name="account-center"></a>Accountcentrum

1. Zorg ervoor dat uw scenario wordt ondersteund door het controleren van de [beperkingen voor het wijzigen van servicebeheerders](#limits).

1. Aanmelden bij [Accountcentrum](https://account.windowsazure.com/subscriptions) als de accountbeheerder.

1. Klik op een abonnement.

1. Aan de rechterkant, klikt u op **abonnementsgegevens bewerken**.

    ![Schermopname van de knop bewerken abonnement in het Accountcentrum](./media/classic-administrators/editsub.png)

1. In de **SERVICEBEHEERDER** voert u het e-mailadres van de nieuwe Service-beheerder.

    ![Schermafbeelding met van het vak voor het wijzigen van het e-mailadres voor de Service-beheerder](./media/classic-administrators/change-service-admin.png)

1. Klik op het vinkje om op te slaan van de wijziging.

### <a name="limitations-for-changing-the-service-administrator"></a>Beperkingen voor het wijzigen van de Service-beheerder

Elk abonnement is gekoppeld aan een Azure AD-directory. Als u wilt zoeken in de map die het abonnement gekoppeld is, opent u **abonnementen** in Azure portal en selecteer vervolgens een abonnement op de map.

Als u bent aangemeld met een werk- of schoolaccount, kunt u andere accounts in uw organisatie als servicebeheerder toevoegen. Bijvoorbeeld, abby@contoso.com kunt toevoegen bob@contoso.com als servicebeheerder, maar kan toevoegen john@notcontoso.com tenzij john@notcontoso.com aanwezig is in de map contoso.com. Gebruikers die zijn aangemeld met werk of schoolaccounts kunnen doorgaan met het Microsoft-accountgebruikers toevoegen als servicebeheerder.

  | Aanmeldingsmethode | Microsoft-accountgebruiker als een servicebeheerder toevoegen? | Werk-of schoolaccount in dezelfde organisatie als servicebeheerder toevoegen? | Werk-of schoolaccount in de andere organisatie als servicebeheerder toevoegen? |
  | --- | --- | --- | --- |
  |  Microsoft-account |Ja |Nee |Nee |
  |  Werk- of schoolaccount |Ja |Ja |Nee |

## <a name="view-the-account-administrator"></a>De accountbeheerder weergeven

De accountbeheerder is de gebruiker die in eerste instantie geregistreerd voor het Azure-abonnement en is verantwoordelijk als de eigenaar van de facturering van het abonnement. De accountbeheerder voor een abonnement, Zie [eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md).

Volg deze stappen om de accountbeheerder weer te geven.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Open [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en een abonnement te selecteren.

1. Klik op **Eigenschappen**.

    De accountbeheerder voor het abonnement wordt weergegeven in de **accountbeheerder** vak.

    ![Schermopname van de accountbeheerder](./media/classic-administrators/account-admin.png)

## <a name="next-steps"></a>Volgende stappen

* [Inzicht in de verschillende rollen](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Toegang tot Azure-resources met behulp van RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md)
