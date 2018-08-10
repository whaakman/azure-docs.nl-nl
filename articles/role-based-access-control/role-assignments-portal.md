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
ms.date: 08/07/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 97bf33cb882d5a121b9811a8e36a1d26f9a954f8
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715366"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Toegang beheren met RBAC en Azure Portal

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot resources in Azure beheert. In dit artikel wordt beschreven hoe u de toegang voor gebruikers, groepen en toepassingen beheert met behulp van RBAC en Azure Portal.

## <a name="list-roles"></a>Rollen opvragen

Een roldefinitie is een verzameling machtigingen die u gebruikt voor roltoewijzingen. Azure heeft meer dan 70 [ingebouwde rollen](built-in-roles.md). Volg deze stappen voor het weergeven van de rollen in de portal.

1. Kies in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

1. Kies uw abonnement.

1. Kies **Toegangsbeheer (IAM)**.

   ![De optie Rollen](./media/role-assignments-portal/list-subscription-access-control.png)

1. Kies **Rollen** om een lijst te zien met alle ingebouwde en aangepaste rollen.

   ![De optie Rollen](./media/role-assignments-portal/roles-option.png)

   U ziet het aantal gebruikers en groepen dat is zijn toegewezen aan elke rol.

   ![Lijst met rollen](./media/role-assignments-portal/roles-list.png)

## <a name="list-access"></a>Toegang opvragen

Toegang kan alleen efficiënt worden beheerd als u weet wie er toegang heeft, wat voor machtigingen gebruikers hebben en op welk niveau. U kunt deze informatie bekijken door de roltoewijzingen weer te geven. Volg deze stappen voor het weergeven van de toegang voor gebruikers en de toegang op verschillende niveaus weer.

### <a name="list-role-assignments-for-a-user"></a>Roltoewijzingen voor een gebruiker weergeven

1. Kies **Azure Active Directory** in de navigatielijst.

1. Kies **Gebruikers** om de blade **Alle gebruikers** te openen.

   ![De blade Alle gebruikers van Azure Active Directory](./media/role-assignments-portal/aad-all-users.png)

1. Kies een gebruiker in de lijst.

1. Kies **Azure-resources** in de sectie **Beheren**.

   ![Azure resources van Azure Active Directory-gebruiker](./media/role-assignments-portal/aad-user-azure-resources.png)

   Op de blade Azure-resources ziet u de roltoewijzingen voor de geselecteerde gebruiker en het geselecteerde abonnement. Deze lijst bevat alleen roltoewijzingen voor resources die u gemachtigd bent om te lezen. Bijvoorbeeld, als de gebruiker ook roltoewijzingen die u niet kan worden gelezen heeft, weergegeven deze roltoewijzingen niet in de lijst.

1. Als u meerdere abonnementen hebt, kunt u de **abonnement** vervolgkeuzelijst om te zien van de roltoewijzingen in een ander abonnement.

### <a name="list-role-assignments-for-a-resource-group"></a>Roltoewijzingen voor een resourcegroep opvragen

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies een resourcegroep en kies vervolgens **Toegangsbeheer (IAM)**.

   Op de blade Access control (IAM), ook wel bekend als identiteits- en toegangsbeheer, beheer, kunt u zien wie toegang heeft tot deze resourcegroep. U ziet dat voor sommige rollen het bereik is ingesteld op **Deze resource**, terwijl andere zijn **(Overgenomen)** uit een ander bereik. De toegang is specifiek toegewezen aan de resourcegroep of is overgenomen van een toewijzing aan het bovenliggende abonnement.

   ![Resourcegroepen](./media/role-assignments-portal/resource-group-access-control.png)

### <a name="list-role-assignments-for-a-subscription"></a>Roltoewijzingen weergeven voor een abonnement

1. Kies in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

1. Kies uw abonnement.

1. Kies **Toegangsbeheer (IAM)**.

    Op de blade toegangsbeheer (IAM), kunt u zien wie toegang heeft tot dit abonnement en hun rol.

    ![Blade toegangsbeheer (IAM) voor een abonnement](./media/role-assignments-portal/subscription-access-control.png)

    Beheerders en co-beheerders van klassieke abonnementen worden beschouwd als eigenaar van het abonnement in het RBAC-model.

### <a name="list-role-assignments-for-a-management-group"></a>Lijst-roltoewijzingen voor een beheergroep

1. Kies in de Azure-portal **alle services** en vervolgens **beheergroepen**.

1. Kies uw beheergroep.

1. Kies **(details)** voor de geselecteerde beheergroep.

    ![Beheergroepen](./media/role-assignments-portal/management-groups-list.png)

1. Kies **Toegangsbeheer (IAM)**.

    Op de blade toegangsbeheer (IAM), kunt u zien wie toegang heeft tot deze beheergroep en hun rol.

    ![Blade toegangsbeheer (IAM) voor een beheergroep](./media/role-assignments-portal/management-groups-access-control.png)

## <a name="grant-access"></a>Toegang verlenen

In RBAC verleent u toegang door een roltoewijzing te maken. Volg deze stappen voor het verlenen van toegang op verschillende niveaus.

### <a name="create-a-role-assignment-at-a-resource-group-scope"></a>Een roltoewijzing maken voor een resourcegroep

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies een resourcegroep.

1. Kies **Toegangsbeheer (IAM)** om de huidige lijst met roltoewijzingen voor de resourcegroep te zien.

   ![Blade toegangsbeheer (IAM) voor een resourcegroep](./media/role-assignments-portal/grant-resource-group-access-control.png)

1. Kies **Toevoegen** om het deelvenster **Machtigingen toevoegen** te openen.

   Als u niet bevoegd bent om rollen toe te wijzen, ziet u de optie **Toevoegen** niet.

   ![Het deelvenster Machtigingen toevoegen](./media/role-assignments-portal/add-permissions.png)

1. Selecteer in de vervolgkeuzelijst **Rol** een rol, zoals **Inzender voor virtuele machines**.

1. Selecteer in de lijst **Selecteren** een gebruiker, groep of toepassing. Als u de beveiligings-principal niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om te zoeken naar weergavenamen, e-mailadressen en object-id's.

1. Kies **Opslaan** om de roltoewijzing te maken.

   Na enkele ogenblikken wordt de rol op groepsniveau toegewezen aan de beveiligings-principal.

### <a name="create-a-role-assignment-at-a-subscription-scope"></a>Een roltoewijzing maken voor een bepaald abonnement

1. Kies in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

1. Kies uw abonnement.

1. Kies **Toegangsbeheer (IAM)** om de huidige lijst met roltoewijzingen voor het abonnement te zien.

   ![Blade toegangsbeheer (IAM) voor een abonnement](./media/role-assignments-portal/grant-subscription-access-control.png)

1. Kies **Toevoegen** om het deelvenster **Machtigingen toevoegen** te openen.

   Als u niet bevoegd bent om rollen toe te wijzen, ziet u de optie **Toevoegen** niet.

   ![Het deelvenster Machtigingen toevoegen](./media/role-assignments-portal/add-permissions.png)

1. Selecteer in de vervolgkeuzelijst **Rol** een rol, zoals **Inzender voor virtuele machines**.

1. Selecteer in de lijst **Selecteren** een gebruiker, groep of toepassing. Als u de beveiligings-principal niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om te zoeken naar weergavenamen, e-mailadressen en object-id's.

1. Kies **Opslaan** om de roltoewijzing te maken.

   Na enkele ogenblikken wordt de rol op abonnementsniveau toegewezen aan de beveiligings-principal.

### <a name="create-a-role-assignment-at-a-management-group-scope"></a>Een roltoewijzing maken een beheergroepbereik

1. Kies in de Azure-portal **alle services** en vervolgens **beheergroepen**.

1. Kies uw beheergroep.

1. Kies **(details)** voor de geselecteerde beheergroep.

    ![Beheergroepen](./media/role-assignments-portal/management-groups-list.png)

1. Kies **Toegangsbeheer (IAM)** om de huidige lijst met roltoewijzingen voor het abonnement te zien.

   ![Blade toegangsbeheer (IAM) voor een beheergroep](./media/role-assignments-portal/grant-management-groups-access-control.png)

1. Kies **Toevoegen** om het deelvenster **Machtigingen toevoegen** te openen.

   Als u niet bevoegd bent om rollen toe te wijzen, ziet u de optie **Toevoegen** niet.

   ![Het deelvenster Machtigingen toevoegen](./media/role-assignments-portal/add-permissions-management-groups.png)

1. In de **rol** vervolgkeuzelijst, selecteer een rol, zoals **Management groep Inzender**.

    Zie voor informatie over de ondersteunde bewerkingen op beheergroepen voor verschillende rollen, [organiseren van uw resources met Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md#management-group-access).

1. Selecteer in de lijst **Selecteren** een gebruiker, groep of toepassing. Als u de beveiligings-principal niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om te zoeken naar weergavenamen, e-mailadressen en object-id's.

1. Kies **Opslaan** om de roltoewijzing te maken.

   Na enkele ogenblikken is de beveiligings-principal de rol op het beheerbereik van de groep toegewezen.

## <a name="remove-access"></a>Toegang intrekken

Als u in RBAC de toegang wilt intrekken voor een rol, verwijdert u de roltoewijzing. Volg deze stappen voor de toegang verwijderen.

### <a name="remove-a-role-assignment"></a>Roltoewijzing verwijderen

1. Open de **toegangsbeheer (IAM)** blade voor de beheergroep, abonnement, resourcegroep of resource waarvoor de roltoewijzing die u wilt verwijderen.

1. Schakel in de lijst met roltoewijzingen het selectievakje in naast de beveiligings-principal met de roltoewijzing die u wilt verwijderen.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Kies **Verwijderen**.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment.png)

1. Kies **Ja** om te bevestigen dat u de roltoewijzing inderdaad wilt verwijderen.

    Overgenomen toewijzingen kunnen niet worden verwijderd. Als u een overgenomen roltoewijzing wilt verwijderen, moet u dit te doen voor het bereik waarvoor de roltoewijzing is gemaakt. In de **bereik** kolom, naast **(overgenomen)** er is een koppeling die u aan het bereik waarin deze rol is toegewezen. Ga naar het bereik dat hier wordt weergegeven om de roltoewijzing te verwijderen.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Grant access for a user using RBAC and the Azure portal](quickstart-assign-role-user-portal.md) (Snelstart: Toegang verlenen aan een gebruiker met op rollen gebaseerd toegangsbeheer en Azure Portal)
* [Quickstart: Grant access for a user using RBAC and Azure PowerShell](tutorial-role-assignments-user-powershell.md) (Snelstartgids: Toegang verlenen aan een gebruiker met RBAC en Azure PowerShell)
* [Ingebouwde rollen](built-in-roles.md)
* [Resources organiseren met beheergroepen voor Azure](../azure-resource-manager/management-groups-overview.md)
