---
title: Toegang tot Azure-resources met behulp van RBAC en de Azure-portal beheren | Microsoft Docs
description: Informatie over het beheren van toegang tot Azure-resources voor gebruikers, groepen, service-principals en beheerde identiteiten met behulp van op rollen gebaseerd toegangsbeheer (RBAC) en de Azure-portal. U vindt hier instructies voor het weergeven van toegang, het verlenen van toegang en het intrekken van toegang.
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
ms.date: 02/24/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: bb23cbc275e01eab5361504c547c020b0a29f4c3
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805287"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>Toegang tot Azure-resources met behulp van RBAC en de Azure-portal beheren

[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) is de manier waarop dat u de toegang tot Azure-resources beheren. Dit artikel wordt beschreven hoe u toegang met behulp van de Azure-portal beheren. Als u nodig hebt voor het beheren van toegang tot Azure Active Directory, Zie [weergeven en toewijzen beheerdersrollen in Azure Active Directory](../active-directory/users-groups-roles/directory-manage-roles-portal.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt toevoegen en verwijderen van roltoewijzingen, moet u het volgende hebben:

- `Microsoft.Authorization/roleAssignments/write` en `Microsoft.Authorization/roleAssignments/delete` machtigingen, zoals [Administrator voor gebruikerstoegang](built-in-roles.md#user-access-administrator) of [eigenaar](built-in-roles.md#owner)

## <a name="overview-of-access-control-iam"></a>Overzicht van Access control (IAM)

**Toegangsbeheer (IAM)** is de blade die u gebruikt om toegang tot Azure-resources te beheren. Het is ook bekend als identiteit-en toegangsbeheer en wordt weergegeven in verschillende locaties in Azure portal. Hieronder ziet u een voorbeeld van de blade toegangsbeheer (IAM) voor een abonnement.

![Blade toegangsbeheer (IAM) voor een abonnement](./media/role-assignments-portal/access-control-numbers.png)

De volgende tabel wordt beschreven wat enkele van de elementen worden gebruikt voor:

| # | Element | Wat u gebruikt voor |
| --- | --- | --- |
| 1 | Resource waarop Access control (IAM) is geopend | Identificeren van bereik (abonnement in dit voorbeeld) |
| 2 | **Voeg** knop | Roltoewijzingen toevoegen |
| 3 | **Toegang controleren** tabblad | De roltoewijzingen voor één gebruiker weergeven |
| 4 | **Roltoewijzingen** tabblad | De roltoewijzingen weergegeven bij het huidige bereik weergeven |
| 5 | **Rollen** tabblad | Alle rollen en machtigingen weergeven |

Als u de meest effectieve met de blade toegangsbeheer (IAM), is het handig als u de volgende drie vragen beantwoorden kunt wanneer u probeert om toegang te beheren:

1. **Die toegang nodig heeft?**

    Die verwijst naar een gebruiker, groep, service-principal of beheerde identiteit. Dit heet ook een *beveiligings-principal*.

1. **Welke machtigingen moeten ze?**

    Machtigingen zijn gegroepeerd tot rollen. U kunt selecteren uit een lijst met diverse rollen.

1. **Waar hebben ze toegang nodig?**

    Waar verwijst naar de set van resources die de toegang van toepassing op. Waar kan zijn een beheergroep, abonnement, resourcegroep of één resource, zoals een storage-account. Dit heet de *bereik*.

## <a name="open-access-control-iam"></a>Open Access control (IAM)

Het eerste wat dat u nodig hebt om te bepalen is waar u de blade toegangsbeheer (IAM) open. Dat hangt ervan af op welke resources u wilt beheren van toegang voor. Wilt u voor het beheren van toegang voor alles in een beheergroep, alles in een abonnement, alles in een resourcegroep of één resource?

1. Klik in de Azure-portal op **alle services** en selecteer vervolgens het bereik. Bijvoorbeeld, kunt u **beheergroepen**, **abonnementen**, **resourcegroepen**, of een resource.

1. Klik op de specifieke resource.

1. Klik op **Toegangsbeheer (IAM)**.

    Hieronder ziet u een voorbeeld van de blade toegangsbeheer (IAM) voor een abonnement. Als u geen hier access control-wijzigingen aanbrengt, zouden ze van toepassing op het hele abonnement.

    ![Blade toegangsbeheer (IAM) voor een abonnement](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Rollen en machtigingen weergeven

Een roldefinitie is een verzameling machtigingen die u gebruikt voor roltoewijzingen. Azure heeft meer dan 70 [ingebouwde rollen voor Azure-resources](built-in-roles.md). Volg deze stappen om de beschikbare rollen en machtigingen weer te geven.

1. Open **toegangsbeheer (IAM)** op een bereik.

1. Klik op de **rollen** tabblad voor een overzicht van alle ingebouwde en aangepaste rollen.

   Hier ziet u het aantal gebruikers en groepen die zijn toegewezen aan elke rol bij het huidige bereik.

   ![Lijst met rollen](./media/role-assignments-portal/roles-list.png)

1. Klik op een afzonderlijke rol om te zien wie deze rol is toegewezen en ook de machtigingen voor de rol weergeven.

   ![Roltoewijzingen](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Roltoewijzingen weergeven

Bij het beheren van toegang, u wilt weten wie er toegang heeft, wat zijn de machtigingen, en in welk bereik. Voor toegang tot de lijst voor een gebruiker, groep, service-principal of beheerde identiteit, u de roltoewijzingen van hun weergeven.

### <a name="view-role-assignments-for-a-single-user"></a>Weergave-roltoewijzingen voor één gebruiker

Volg deze stappen om de toegang voor een enkele gebruiker, groep, service-principal of beheerde identiteit bij een bepaald bereik weer te geven.

1. Open **toegangsbeheer (IAM)** met een bereik, zoals de beheergroep, abonnement, resourcegroep of resource, waar u om weer toegang te geven.

1. Klik op het tabblad **Toegang controleren**.

    ![Toegangsbeheer - Tabblad Toegang controleren](./media/role-assignments-portal/access-control-check-access.png)

1. Selecteer in de lijst **Zoeken** het type beveiligings-principal waarvoor u de toegang wilt controleren.

1. Voer in het zoekvak een tekenreeks in om de map te doorzoeken op weergavenamen, e-mailadressen of object-id's.

    ![De toegangsselectielijst controleren](./media/role-assignments-portal/check-access-select.png)

1. Klik op de beveiligings-principal om het deelvenster **Toewijzingen** te openen.

    ![Deelvenster Toewijzingen](./media/role-assignments-portal/check-access-assignments.png)

    In dit deelvenster ziet u de rollen die zijn toegewezen aan de geselecteerde beveiligings-principal en het bereik. Als er een in dit bereik toewijzingen zijn geweigerd of zijn overgenomen, worden deze weergegeven.

### <a name="view-all-role-assignments-at-a-scope"></a>Alle roltoewijzingen weergegeven bij een bereik weergeven

1. Open **toegangsbeheer (IAM)** met een bereik, zoals de beheergroep, abonnement, resourcegroep of resource, waar u om weer toegang te geven.

1. Klik op de **roltoewijzingen** tabblad om de roltoewijzingen binnen dit bereik.

   ![Access control - rol toewijzingen tabblad](./media/role-assignments-portal/access-control-role-assignments.png)

   Op het tabblad toewijzingen van rol kunt u zien wie toegang heeft binnen dit bereik. U ziet dat voor sommige rollen het bereik is ingesteld op **Deze resource**, terwijl andere zijn **(Overgenomen)** uit een ander bereik. Toegang is specifiek toegewezen aan deze resource of overgenomen van een toewijzing aan het bovenliggende bereik.

## <a name="add-a-role-assignment"></a>Een roltoewijzing toevoegen

In RBAC, als u wilt toegang verlenen u een rol toewijzen aan een gebruiker, groep, service-principal of beheerde identiteit. Volg deze stappen voor het verlenen van toegang op verschillende niveaus.

### <a name="assign-a-role-at-a-scope"></a>Een rol op een scope toewijzen

1. Open **toegangsbeheer (IAM)** met een bereik, zoals de beheergroep, abonnement, resourcegroep of resource, waar u om toegang te verlenen.

1. Klik op de **roltoewijzingen** tabblad om de roltoewijzingen binnen dit bereik.

1. Klik op **toevoegen** > **roltoewijzing toevoegen** om het deelvenster toevoegen rol toewijzing te openen.

   Als u niet bent gemachtigd voor het toewijzen van rollen, is de optie Roltoewijzing toevoegen uitgeschakeld.

   ![Menu toevoegen](./media/role-assignments-portal/add-menu.png)

   ![Deelvenster Roltoewijzing toevoegen](./media/role-assignments-portal/add-role-assignment.png)

1. Selecteer in de vervolgkeuzelijst **Rol** een rol, zoals **Inzender voor virtuele machines**.

1. In de **Selecteer** , selecteert u een gebruiker, groep, service-principal of beheerde identiteit. Als u de beveiligings-principal niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om te zoeken naar weergavenamen, e-mailadressen en object-id's.

1. Klik op **opslaan** aan de rol toe te wijzen.

   Na enkele ogenblikken is de beveiligings-principal de rol bij het geselecteerde bereik toegewezen.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Een gebruiker toewijzen als een beheerder van een abonnement

Als u een gebruiker een beheerder van een Azure-abonnement, deze toewijzen de [eigenaar](built-in-roles.md#owner) rol op het abonnementsbereik. De rol eigenaar geeft de gebruiker volledige toegang tot alle resources in het abonnement, waaronder het recht op toegang aan anderen delegeren. Deze stappen zijn hetzelfde als elke andere roltoewijzing.

1. Klik in de Azure-portal op de optie **Alle services** en vervolgens op **Abonnementen**.

1. Klik op het abonnement waar u om toegang te verlenen.

1. Klik op **Toegangsbeheer (IAM)**.

1. Klik op de **roltoewijzingen** tabblad om de roltoewijzingen voor dit abonnement.

1. Klik op **toevoegen** > **roltoewijzing toevoegen** om het deelvenster toevoegen rol toewijzing te openen.

   Als u niet bent gemachtigd voor het toewijzen van rollen, is de optie Roltoewijzing toevoegen uitgeschakeld.

   ![Menu toevoegen](./media/role-assignments-portal/add-menu.png)

   ![Deelvenster Roltoewijzing toevoegen](./media/role-assignments-portal/add-role-assignment.png)

1. In de **rol** vervolgkeuzelijst, selecteer de **eigenaar** rol.

1. In de **Selecteer** , selecteert u een gebruiker. Als u de gebruiker in de lijst niet ziet, typt u de **Selecteer** in om te zoeken naar de map voor de weergavenamen en e-mailadressen.

1. Klik op **opslaan** aan de rol toe te wijzen.

   Na enkele ogenblikken wordt de gebruiker de rol van eigenaar op het abonnementsbereik toegewezen.

## <a name="remove-role-assignments"></a>Roltoewijzingen verwijderen

Als u in RBAC de toegang wilt intrekken voor een rol, verwijdert u de roltoewijzing. Volg deze stappen voor de toegang verwijderen.

1. Open **toegangsbeheer (IAM)** met een bereik, zoals de beheergroep, abonnement, resourcegroep of resource, waar u toegang wilt verwijderen.

1. Klik op de **roltoewijzingen** tabblad om de roltoewijzingen voor dit abonnement.

1. Schakel in de lijst met roltoewijzingen het selectievakje in naast de beveiligings-principal met de roltoewijzing die u wilt verwijderen.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Klik op **verwijderen**.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment.png)

1. Klik in de software rol toewijzing bericht dat wordt weergegeven op **Ja**.

    Overgenomen toewijzingen kunnen niet worden verwijderd. Als u een overgenomen roltoewijzing wilt verwijderen, moet u dit te doen voor het bereik waarvoor de roltoewijzing is gemaakt. In de **bereik** kolom, naast **(overgenomen)** er is een koppeling die u aan het bereik waarin deze rol is toegewezen. Ga naar het bereik dat hier wordt weergegeven om de roltoewijzing te verwijderen.

   ![Bericht bij verwijderen van roltoewijzing](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie: Een gebruiker toegang tot Azure-resources met behulp van RBAC en de Azure-portal](quickstart-assign-role-user-portal.md)
* [Zelfstudie: Een gebruiker toegang tot Azure-resources met behulp van RBAC en Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [RBAC voor Azure-resources oplossen](troubleshooting.md)
* [Resources organiseren met beheergroepen voor Azure](../governance/management-groups/index.md)
