---
title: Toegang beheren met RBAC en Azure Portal | Microsoft Docs
description: Lees hier hoe u de toegang voor gebruikers, groepen en toepassingen beheert met behulp van op rollen gebaseerde toegangsbeheer (RBAC) en Azure Portal. U vindt hier instructies voor het weergeven van toegang, het verlenen van toegang en het intrekken van toegang.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 6544503353f085a9dd9b0fe7207bbf8d4e0bc9d0
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435614"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Toegang beheren met RBAC en Azure Portal

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot resources in Azure beheert. In dit artikel wordt beschreven hoe u de toegang voor gebruikers, groepen en toepassingen beheert met behulp van RBAC en Azure Portal.

## <a name="list-roles"></a>Rollen opvragen

Een roldefinitie is een verzameling machtigingen die u gebruikt voor roltoewijzingen. Azure heeft meer dan 60 [ingebouwde rollen](built-in-roles.md).

1. Kies in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

1. Kies uw abonnement.

1. Kies **Toegangsbeheer (IAM)**.

   ![De optie Rollen](./media/role-assignments-portal/list-subscription-access-control.png)

1. Kies **Rollen** om een lijst te zien met alle ingebouwde en aangepaste rollen.

   ![De optie Rollen](./media/role-assignments-portal/roles-option.png)

   U ziet het aantal gebruikers en groepen dat is zijn toegewezen aan elke rol.

   ![Lijst met rollen](./media/role-assignments-portal/roles-list.png)

## <a name="list-access"></a>Toegang opvragen

Toegang kan alleen efficiënt worden beheerd als u weet wie er toegang heeft, wat voor machtigingen gebruikers hebben en op welk niveau. U kunt deze informatie bekijken door de roltoewijzingen weer te geven.

### <a name="list-role-assignments-for-a-subscription"></a>Roltoewijzingen weergeven voor een abonnement

1. Kies in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

1. Kies uw abonnement.

1. Kies **Toegangsbeheer (IAM)**.

    Op de blade Toegangsbeheer (IAM), ook wel identiteits- en toegangsbeheer genoemd, kunt u zien wie er toegang heeft tot dit abonnement en wat hun rol is.

    ![De blade Toegangsbeheer (IAM)](./media/role-assignments-portal/subscription-access-control.png)

    Beheerders en co-beheerders van klassieke abonnementen worden beschouwd als eigenaar van het abonnement in het RBAC-model.


### <a name="list-role-assignments-for-a-resource-group"></a>Roltoewijzingen voor een resourcegroep opvragen

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies een resourcegroep en kies vervolgens **Toegangsbeheer (IAM)**.

   Op de blade Toegangsbeheer (IAM) ziet u wie toegang heeft tot deze resourcegroep. U ziet dat voor sommige rollen het bereik is ingesteld op **Deze resource**, terwijl andere zijn **(Overgenomen)** uit een ander bereik. De toegang is specifiek toegewezen aan de resourcegroep of is overgenomen van een toewijzing aan het bovenliggende abonnement.

   ![Resourcegroepen](./media/role-assignments-portal/resource-group-access-control.png)

### <a name="list-role-assignments-for-a-user"></a>Roltoewijzingen voor een gebruiker weergeven

1. Kies **Azure Active Directory** in de navigatielijst.

1. Kies **Gebruikers** om de blade **Alle gebruikers** te openen.

   ![De blade Alle gebruikers van Azure Active Directory](./media/role-assignments-portal/aad-all-users.png)

1. Kies een gebruiker in de lijst.

1. Kies **Azure-resources** in de sectie **Beheren**.

   ![Azure resources van Azure Active Directory-gebruiker](./media/role-assignments-portal/aad-user-azure-resources.png)

   Op de blade Azure-resources kunt u de roltoewijzingen voor de geselecteerde gebruiker zien. Deze lijst bevat alleen roltoewijzingen voor resources die u gemachtigd bent om te lezen. Als de gebruiker bijvoorbeeld ook roltoewijzingen in een ander abonnement heeft die u niet mag lezen, worden die roltoewijzingen niet opgenomen in de lijst.

## <a name="grant-access"></a>Toegang verlenen

In RBAC verleent u toegang door een roltoewijzing te maken.

### <a name="create-a-role-assignment-at-a-subscription-scope"></a>Een roltoewijzing maken voor een bepaald abonnement

1. Kies in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

1. Kies uw abonnement.

1. Kies **Toegangsbeheer (IAM)** om de huidige lijst met roltoewijzingen voor het abonnement te zien.

   ![De blade Toegangsbeheer (IAM) voor een resourcegroep](./media/role-assignments-portal/grant-subscription-access-control.png)

1. Kies **Toevoegen** om het deelvenster **Machtigingen toevoegen** te openen.

   Als u niet bevoegd bent om rollen toe te wijzen, ziet u de optie **Toevoegen** niet.

   ![Het deelvenster Machtigingen toevoegen](./media/role-assignments-portal/add-permissions.png)

1. Selecteer in de vervolgkeuzelijst **Rol** een rol, zoals **Inzender voor virtuele machines**.

1. Selecteer in de lijst **Selecteren** een gebruiker, groep of toepassing. Als u de beveiligings-principal niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om te zoeken naar weergavenamen, e-mailadressen en object-id's.

1. Kies **Opslaan** om de roltoewijzing te maken.

   Na enkele ogenblikken wordt de rol op abonnementsniveau toegewezen aan de beveiligings-principal.

### <a name="create-a-role-assignment-at-a-resource-group-scope"></a>Een roltoewijzing maken voor een resourcegroep

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies een resourcegroep.

1. Kies **Toegangsbeheer (IAM)** om de huidige lijst met roltoewijzingen voor de resourcegroep te zien.

   ![De blade Toegangsbeheer (IAM) voor een resourcegroep](./media/role-assignments-portal/grant-resource-group-access-control.png)

1. Kies **Toevoegen** om het deelvenster **Machtigingen toevoegen** te openen.

   Als u niet bevoegd bent om rollen toe te wijzen, ziet u de optie **Toevoegen** niet.

   ![Het deelvenster Machtigingen toevoegen](./media/role-assignments-portal/add-permissions.png)

1. Selecteer in de vervolgkeuzelijst **Rol** een rol, zoals **Inzender voor virtuele machines**.

1. Selecteer in de lijst **Selecteren** een gebruiker, groep of toepassing. Als u de beveiligings-principal niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om te zoeken naar weergavenamen, e-mailadressen en object-id's.

1. Kies **Opslaan** om de roltoewijzing te maken.

   Na enkele ogenblikken wordt de rol op groepsniveau toegewezen aan de beveiligings-principal.

## <a name="remove-access"></a>Toegang intrekken

Als u in RBAC de toegang wilt intrekken voor een rol, verwijdert u de roltoewijzing.

### <a name="remove-a-role-assignment"></a>Roltoewijzing verwijderen

1. Open de blade **Toegangsbeheer (IAM)** voor het abonnement, de resourcegroep of de resource met de roltoewijzing die u wilt verwijderen.

1. Schakel in de lijst met roltoewijzingen het selectievakje in naast de beveiligings-principal met de roltoewijzing die u wilt verwijderen.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Kies **Verwijderen**.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment.png)

1. Kies **Ja** om te bevestigen dat u de roltoewijzing inderdaad wilt verwijderen.

Overgenomen toewijzingen kunnen niet worden verwijderd. Als u een overgenomen roltoewijzing wilt verwijderen, moet u dit te doen voor het bereik waarvoor de roltoewijzing is gemaakt. In de kolom **Bereik**, naast **Overgenomen**, vindt u een koppeling waarmee u naar de resources gaat waaraan deze rol is toegewezen. Ga naar het bereik dat hier wordt weergegeven om de roltoewijzing te verwijderen.

## <a name="other-tools-to-manage-access"></a>Andere hulpprogramma's om toegang te beheren

U kunt rollen toewijzen en toegang beheren met Azure RBAC-opdrachten met andere hulpprogramma's dan de Azure Portal. Zie de volgende koppelingen voor meer informatie:

* [Azure PowerShell](role-assignments-powershell.md)
* [Azure-CLI](role-assignments-cli.md)
* [REST API](role-assignments-rest.md)

## <a name="next-steps"></a>Volgende stappen

* [Quickstart: Grant access for a user using RBAC and the Azure portal](quickstart-assign-role-user-portal.md) (Snelstartgids: Toegang verlenen aan een gebruiker met RBAC en Azure Portal)
* [Quickstart: Grant access for a user using RBAC and Azure PowerShell](tutorial-role-assignments-user-powershell.md) (Snelstartgids: Toegang verlenen aan een gebruiker met RBAC en Azure PowerShell)
* [Ingebouwde rollen](built-in-roles.md)
