---
title: Beheer de toegang tot Azure resoruces met RBAC en Azure portal | Microsoft Docs
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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1e3057108ef179af2f4692c061091fbdf59f0af2
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342334"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>Toegang tot Azure-resources met behulp van RBAC en de Azure-portal beheren

[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) is de manier waarop dat u de toegang tot Azure-resources beheren. Dit artikel wordt beschreven hoe u de toegang voor gebruikers, groepen, service-principals en beheerde identiteiten met RBAC en de Azure-portal beheren.

## <a name="open-access-control-iam"></a>Open Access control (IAM)

De **toegangsbeheer (IAM)** blade, ook wel bekend als identiteits- en toegangsbeheer, beheer, worden weergegeven in de portal. Als u wilt bekijken of beheren van toegang in de portal, is het eerste wat dat u meestal doen open de blade toegangsbeheer (IAM) in het bereik waarin u wilt weergeven of een wijziging aanbrengt.

1. Klik in de Azure-portal op **alle services** en selecteer vervolgens het bereik of de resource die u wilt bekijken of beheren. Bijvoorbeeld, kunt u **beheergroepen**, **abonnementen**, **resourcegroepen**, of een resource.

1. Klik op de specifieke resource die u wilt bekijken of beheren.

1. Klik op **Toegangsbeheer (IAM)**.

    Hieronder ziet u een voorbeeld van de blade toegangsbeheer (IAM) voor een abonnement.

    ![Blade toegangsbeheer (IAM) voor een abonnement](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Rollen en machtigingen weergeven

Een roldefinitie is een verzameling machtigingen die u gebruikt voor roltoewijzingen. Azure heeft meer dan 70 [ingebouwde rollen voor Azure-resources](built-in-roles.md). Volg deze stappen om de rollen en machtigingen die kunnen worden uitgevoerd op de beheer- en gegevenslaag weer te geven.

1. Open **toegangsbeheer (IAM)** met een bereik, zoals de beheergroep, abonnement, resourcegroep of resource, waar u om rollen en machtigingen weer te geven.

1. Klik op de **rollen** tabblad voor een overzicht van alle ingebouwde en aangepaste rollen.

   Hier ziet u het aantal gebruikers en groepen die zijn toegewezen aan elke rol binnen dit bereik.

   ![Lijst met rollen](./media/role-assignments-portal/roles-list.png)

1. Klik op een afzonderlijke rol om te zien wie deze rol is toegewezen en ook de machtigingen voor de rol weergeven.

   ![Roltoewijzingen](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Roltoewijzingen weergeven

Toegang kan alleen efficiënt worden beheerd als u weet wie er toegang heeft, wat voor machtigingen gebruikers hebben en op welk niveau. Om de lijst met toegang voor een gebruiker, groep, service-principal of beheerde identiteit te bekijken van de roltoewijzingen.

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

1. Klik op de **roltoewijzingen** tabblad (of klik op de **weergave** knop op de tegel weergave rol toewijzingen) om de roltoewijzingen binnen dit bereik weer te geven.

   ![Access control - rol toewijzingen tabblad](./media/role-assignments-portal/access-control-role-assignments.png)

   Op het tabblad toewijzingen van rol kunt u zien wie toegang heeft binnen dit bereik. U ziet dat voor sommige rollen het bereik is ingesteld op **Deze resource**, terwijl andere zijn **(Overgenomen)** uit een ander bereik. Toegang is specifiek toegewezen aan deze resource of overgenomen van een toewijzing aan het bovenliggende bereik.

## <a name="add-a-role-assignment"></a>Een roltoewijzing toevoegen

In RBAC, als u wilt toegang verlenen u een rol toewijzen aan een gebruiker, groep, service-principal of beheerde identiteit. Volg deze stappen voor het verlenen van toegang op verschillende niveaus.

### <a name="assign-a-role-at-a-scope"></a>Een rol op een scope toewijzen

1. Open **toegangsbeheer (IAM)** met een bereik, zoals de beheergroep, abonnement, resourcegroep of resource, waar u om toegang te verlenen.

1. Klik op de **roltoewijzingen** tabblad om de roltoewijzingen binnen dit bereik.

1. Klik op **roltoewijzing toevoegen** om het deelvenster toevoegen rol toewijzing te openen.

   Als u niet bent gemachtigd voor het toewijzen van rollen, is de optie Roltoewijzing toevoegen uitgeschakeld.

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

1. Klik op **roltoewijzing toevoegen** om het deelvenster toevoegen rol toewijzing te openen.

   Als u niet bent gemachtigd voor het toewijzen van rollen, is de optie Roltoewijzing toevoegen uitgeschakeld.

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
