---
title: Toevoegen of wijzigen van de Azure-abonnement beheerdersrollen | Microsoft Docs
description: Beschrijft hoe u Azure CO-beheerder, servicebeheerder en accountbeheerder toevoegen of wijzigen
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: genli
ms.openlocfilehash: 97210c0d9dba9c4130b1da9ad17a257ff1d81b42
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449829"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Toevoegen of wijzigen van de beheerders van Azure-abonnement

Voor het beheren van toegang tot Azure-resources, moet u de juiste beheerdersrol hebben. In dit artikel wordt beschreven hoe u toevoegen of wijzigen van de beheerdersrol voor een gebruiker op het abonnementsniveau.

## <a name="what-administrator-role-do-i-use"></a>Welke beheerdersrol moet ik gebruiken?

Azure heeft verscheidene andere rollen. Voor het beheren van toegang tot resources, kunt u de klassiek abonnement beheerdersrollen, zoals de servicebeheerder en medebeheerder of een nieuwere autorisatiesysteem op rollen gebaseerd toegangsbeheer (RBAC) genoemd. Om te controleren of betere controle en voor het vereenvoudigen van beheer van toegang, wordt u aangeraden dat u RBAC voor alle toegang beheerbehoeften gebruikt. Indien mogelijk is het raadzaam dat u de configuratie van bestaande beleidsregels voor toegang met RBAC. Zie voor meer informatie, [wat is op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) en [inzicht in de verschillende rollen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Een RBAC-eigenaar voor een abonnement toevoegen in Azure portal 

Als u wilt toevoegen iemand als beheerder voor een Azure-abonnement, deze toewijzen de [eigenaar](../role-based-access-control/built-in-roles.md#owner) rol (een RBAC-rol) op het abonnementsbereik. De rol van eigenaar kan de resources in het abonnement dat u toegewezen en dat u beschikt niet over toegangsrechten aan andere abonnementen beheren.

1. Ga naar [ **abonnementen** in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Selecteer het abonnement dat u toegang wilt verlenen.
3. Selecteer **Toevoegen**.
   (Als de knop toevoegen ontbreekt, wordt u bent niet gemachtigd om toe te voegen van machtigingen.)
4. Selecteer **toegangsbeheer (IAM)** in de lijst.
5. In de **rol** Schakel **eigenaar**. 
6. In de **toegang toewijzen aan** Schakel **Azure AD-gebruiker, groep of toepassing**. 
7. In de **Selecteer** typt u het e-mailadres van de gebruiker die u wilt toevoegen als eigenaar. Selecteer de gebruiker en selecteer vervolgens **opslaan**.

    ![Schermafbeelding van de rol van eigenaar geselecteerd](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Dit biedt de gebruiker volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren. Als u wilt toegang op een ander bereik, zoals een resourcegroep, gaat u naar de **toegangsbeheer (IAM)** blade voor dat bereik.

## <a name="add-or-change-co-administrator"></a>Toevoegen of wijzigen van CO-beheerder

Alleen een [eigenaar](../role-based-access-control/built-in-roles.md#owner) kunnen worden toegevoegd als een CO-beheerder. Andere gebruikers met functies zoals [Inzender](../role-based-access-control/built-in-roles.md#contributor) en [lezer](../role-based-access-control/built-in-roles.md#reader) kan niet worden toegevoegd als co-beheerders.

> [!TIP]
> U hoeft alleen de eigenaar toevoegen als een CO-beheerder als de gebruiker nodig heeft voor het beheren van de klassieke Azure-implementaties. We raden u aan met het RBAC voor alle andere doeleinden.

1. Als u niet hebt gedaan, voegt u iemand toe als eigenaar van een van bovenstaande instructies te volgen.
2. **Met de rechtermuisknop op** de gebruiker de eigenaar u zojuist hebt toegevoegd en selecteer vervolgens **toevoegen als co-beheerder**. Als u niet ziet de **toevoegen als co-beheerder** optie, vernieuw de pagina of probeer het nog een internetbrowser. 

    ![Schermafbeelding van CO-beheerder wordt toegevoegd](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    De machtiging CO-beheerder verwijderen **met de rechtermuisknop op** het CO-beheerder-gebruiker en selecteer vervolgens **CO-beheerder verwijderen**.

    ![Schermafbeelding van CO-beheerder wordt verwijderd](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>De servicebeheerder voor een Azure-abonnement wijzigen

Alleen de accountbeheerder kan de Service-beheerder voor een abonnement wijzigen. Wanneer u zich aanmeldt, is de servicebeheerder standaard hetzelfde als de accountbeheerder. Als de servicebeheerder wordt gewijzigd naar een andere gebruiker, verliest de accountbeheerder toegang tot Azure-portal. De accountbeheerder kan echter altijd Accountcentrum gebruiken om te wijzigen van de servicebeheerder terug naar zichzelf.

1. Zorg ervoor dat uw scenario wordt ondersteund door het controleren van de [limieten voor het wijzigen van servicebeheerders](#limits).
1. Aanmelden bij [Accountcentrum](https://account.windowsazure.com/subscriptions) als accountbeheerder.
1. Selecteer een abonnement.
1. Selecteer aan de rechterkant **abonnementsgegevens bewerken**.

    ![Schermopname van de knop bewerken abonnement in het Accountcentrum](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. In de **SERVICEBEHEERDER** voert u het e-mailadres van de nieuwe Service-beheerder.

    ![Schermafbeelding met van het vak voor het wijzigen van het e-mailadres voor de Service-beheerder](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Beperkingen voor het wijzigen van servicebeheerders

* Elk abonnement is gekoppeld aan een Azure AD-directory. Als u wilt zoeken in de map die het abonnement gekoppeld is, gaat u naar [ **abonnementen**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecteert u een abonnement op de map.
* Als u bent aangemeld met een werk of School-account, kunt u andere accounts in uw organisatie als servicebeheerder toevoegen. Bijvoorbeeld, abby@contoso.com kunt toevoegen bob@contoso.com als Service-beheerder, maar kan toevoegen john@notcontoso.com tenzij john@notcontoso.com aanwezig is in de map contoso.com. Gebruikers die zijn aangemeld met werk of School-accounts kunnen blijven gebruikers van Microsoft-Account toevoegen als servicebeheerder.

  | Aanmeldingsmethode | Microsoft-Account gebruiker toevoegen als servicebeheerder? | Werk of School-account in dezelfde organisatie als servicebeheerder toevoegen? | Werk of School-account in de andere organisatie als servicebeheerder toevoegen? |
  | --- | --- | --- | --- |
  |  Microsoft-account |Ja |Nee |Nee |
  |  Werk of School-Account |Ja |Ja |Nee |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>De accountbeheerder voor een Azure-abonnement wijzigen

De accountbeheerder is de gebruiker die in eerste instantie geregistreerd voor het Azure-abonnement en is verantwoordelijk als de eigenaar van de facturering van het abonnement. De accountbeheerder voor een abonnement, Zie [eigendom van een Azure-abonnement naar een ander account overdragen](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Weet u niet zeker wie de accountbeheerder is?** Volg deze stappen:

1. Ga naar [ **abonnementen** in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren, en ga vervolgens naar **instellingen**.
1. Selecteer **eigenschappen**. De accountbeheerder voor het abonnement wordt weergegeven in de **accountbeheerder** vak.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Meer informatie over resource access control en Active Directory

* Zie voor meer informatie over RBAC [wat is op rollen gebaseerd toegangsbeheer (RBAC)?](../role-based-access-control/overview.md)
* Zie voor meer informatie over de rollen in Azure, [inzicht in de verschillende rollen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Zie voor meer informatie over Azure Active Directory, [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) en [beheerdersrollen toewijzen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
