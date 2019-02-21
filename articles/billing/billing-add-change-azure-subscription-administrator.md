---
title: Toevoegen of wijzigen van de beheerders van Azure-abonnement | Microsoft Docs
description: Beschrijft hoe u wilt toevoegen of wijzigen van de beheerder van een Azure-abonnement met behulp van op rollen gebaseerd toegangsbeheer (RBAC).
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 6cc965f8e775e02e9dec9f610516739a9a2c1936
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56448004"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Toevoegen of wijzigen van de beheerders van Azure-abonnement

Om de toegang tot Azure-resources te beheren, moet u over de juiste beheerdersrol beschikken. Azure heeft een autorisatiesysteem met de naam op rollen gebaseerd toegangsbeheer (RBAC) met verschillende ingebouwde rollen die kunt u kiezen uit. U kunt deze rollen op verschillende niveaus, zoals de beheergroep, abonnement of resourcegroep toewijzen.

Microsoft raadt u toegang tot resources met behulp van RBAC te beheren. Echter, als u nog steeds met behulp van het klassieke implementatiemodel en het klassieke resources beheren met behulp van [PowerShell-Module voor Azure Service Management](https://docs.microsoft.com/en-us/powershell/module/servicemanagement/azure), moet u een klassieke administrator gebruiken. 

> [!TIP]
> Als u alleen de Azure-portal gebruiken om de klassieke resources te beheren, hoeft u niet de klassieke administrator gebruiken.

Zie voor meer informatie, [Azure Resource Manager en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md) en [beheerders van de klassieke Azure-abonnement](../role-based-access-control/classic-administrators.md).

Dit artikel wordt beschreven hoe toevoegen of wijzigen van de beheerdersrol voor een gebruiker met RBAC op het abonnementsbereik.

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Een gebruiker toewijzen als een beheerder van een abonnement

Als u een gebruiker een beheerder van een Azure-abonnement, deze toewijzen de [eigenaar](../role-based-access-control/built-in-roles.md#owner) rol (een RBAC-rol) op het abonnementsbereik. De rol eigenaar geeft de gebruiker volledige toegang tot alle resources in het abonnement, waaronder het recht op toegang aan anderen delegeren. Deze stappen zijn hetzelfde als elke andere roltoewijzing.

1. Open in de Azure-portal [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Klik op het abonnement waar u om toegang te verlenen.

1. Klik op **Toegangsbeheer (IAM)**.

1. Klik op de **roltoewijzingen** tabblad om de roltoewijzingen voor dit abonnement.

    ![Schermafbeelding van roltoewijzingen](./media/billing-add-change-azure-subscription-administrator/role-assignments.png)

1. Klik op **toevoegen** > **roltoewijzing toevoegen** openen de **roltoewijzing toevoegen** deelvenster.

    Als u geen machtigingen voor het toewijzen van rollen hebt, kunt u de optie is uitgeschakeld.

1. In de **rol** vervolgkeuzelijst, selecteer de **eigenaar** rol.

1. In de **Selecteer** , selecteert u een gebruiker. Als u de gebruiker in de lijst niet ziet, typt u de **Selecteer** in om te zoeken naar de map voor de weergavenamen en e-mailadressen.

    ![Schermafbeelding van de rol van eigenaar geselecteerd](./media/billing-add-change-azure-subscription-administrator/add-role.png)

1. Klik op **opslaan** aan de rol toe te wijzen.

    Na enkele ogenblikken wordt de gebruiker de rol van eigenaar op het abonnementsbereik toegewezen.

## <a name="next-steps"></a>Volgende stappen

* [Wat is toegangsbeheer op basis van rollen (RBAC)?](../role-based-access-control/overview.md)
* [Inzicht in de verschillende rollen](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Procedure: Koppelen of een Azure-abonnement toevoegen aan Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Rol beheerdersmachtigingen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
