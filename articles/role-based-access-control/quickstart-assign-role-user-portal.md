---
title: 'Snelstart: Toegang verlenen aan een gebruiker met behulp van RBAC en Azure Portal | Microsoft Docs'
description: Gebruik op rollen gebaseerd toegangsbeheer (RBAC) om machtigingen te verlenen aan een gebruiker door een rol toe te wijzen in de Azure-portal.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/11/2018
ms.author: rolyon
ms.openlocfilehash: 74ecca671409b6e163bc0db29d66167d240b645c
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092519"
---
# <a name="quickstart-grant-access-for-a-user-using-rbac-and-the-azure-portal"></a>Snelstart: Toegang verlenen aan een gebruiker met behulp van RBAC en Azure Portal

Op rollen gebaseerde toegangsbeheer (RBAC) is de manier waarop u de toegang tot resources in Azure beheert. In deze snelstart verleent u een gebruiker toegang tot het maken en beheren van virtuele machines in een resource -groep.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op http://portal.azure.com.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

1. Kies **Resourcegroepen** in de navigatielijst.

1. Selecteer **Toevoegen** om de blade **Resourcegroep** te openen.

   ![Een nieuwe resourcegroep toevoegen](./media/quickstart-assign-role-user-portal/resource-group.png)

1. Voer bij **Resourcegroepnaam** **rbac-quickstart-resource-group** in.

1. Selecteer een abonnement en een locatie.

1. Kies **Maken** om de resourcegroep te maken.

1. Kies **Vernieuwen** om de lijst met resourcegroepen te vernieuwen.

   De nieuwe resourcegroep wordt weergegeven in de lijst met resourcegroepen.

   ![Resourcegroeplijst](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Toegang verlenen

In RBAC verleent u toegang door een roltoewijzing te maken.

1. Kies in de lijst met **Resourcegroepen** de nieuwe resourcegroep **rbac-quickstart-resource-group**.

1. Kies **Toegangsbeheer (IAM)** om de huidige lijst met roltoewijzingen te zien.

   ![De blade Toegangsbeheer (IAM) voor een resourcegroep](./media/quickstart-assign-role-user-portal/access-control.png)

1. Kies **Toevoegen** om het deelvenster **Machtigingen toevoegen** te openen.

   Als u niet bevoegd bent om rollen toe te wijzen, ziet u de optie **Toevoegen** niet.

   ![Het deelvenster Machtigingen toevoegen](./media/quickstart-assign-role-user-portal/add-permissions.png)

1. Selecteer in de vervolgkeuzelijst **Rol** **Inzender voor virtuele machines**.

1. Selecteer in de lijst **Selecteren** uzelf of een andere gebruiker.

1. Kies **Opslaan** om de roltoewijzing te maken.

   Na enkele ogenblikken krijgt de gebruiker de rol Inzender voor virtuele machines toegewezen in het bereik van de resourcegroep rbac-quickstart-resource-group.

   ![Roltoewijzing Inzender voor virtuele machines](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Toegang intrekken

Als u in RBAC de toegang wilt intrekken voor een rol, verwijdert u de roltoewijzing.

1. Voeg in de lijst met roltoewijzingen een vinkje toe naast de gebruiker met de rol Inzender voor virtuele machines.

1. Kies **Verwijderen**.

   ![Bericht bij verwijderen van roltoewijzing](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Kies **Ja** om te bevestigen dat u de roltoewijzing inderdaad wilt verwijderen.

## <a name="clean-up"></a>Opruimen

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies **rbac-quickstart-resource-group** om de resourcegroep te openen.

1. Kies **Resourcegroep verwijderen** om de resourcegroep te verwijderen.

   ![Resourcegroep verwijderen](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Typ op de blade **Weet u zeker dat u wilt verwijderen** de naam van de resourcegroep: **rbac-quickstart-resource-group**.

1. Kies **Verwijderen** om de resourcegroep te verwijderen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie - Toegang verlenen aan een gebruiker met RBAC en PowerShell](tutorial-role-assignments-user-powershell.md)

