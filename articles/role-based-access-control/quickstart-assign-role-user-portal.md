---
title: 'Zelfstudie: Toegang verlenen aan een gebruiker met behulp van RBAC en de Azure-portal | Microsoft Docs'
description: Gebruik op rollen gebaseerd toegangsbeheer (RBAC) om machtigingen te verlenen aan een gebruiker door een rol toe te wijzen in de Azure-portal.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.openlocfilehash: 8caa5c3b33ac1b483429251e0c1256636c4ece1a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634469"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-the-azure-portal"></a>Zelfstudie: Toegang verlenen aan een gebruiker met behulp van RBAC en de Azure-portal

[Op rollen gebaseerde toegangsbeheer (RBAC)](overview.md) is de manier waarop u de toegang tot resources in Azure beheert. In deze zelfstudie verleent u een gebruiker toegang tot het maken en beheren van virtuele machines in een resourcegroep.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Toegang voor een gebruiker toewijzen op het niveau van een resourcegroep
> * Toegang intrekken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op http://portal.azure.com.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

1. Kies **Resourcegroepen** in de navigatielijst.

1. Selecteer **Toevoegen** om de blade **Resourcegroep** te openen.

   ![Een nieuwe resourcegroep toevoegen](./media/quickstart-assign-role-user-portal/resource-group.png)

1. Voer bij **Resourcegroepnaam** **rbac-resource-group** in.

1. Selecteer een abonnement en een locatie.

1. Kies **Maken** om de resourcegroep te maken.

1. Kies **Vernieuwen** om de lijst met resourcegroepen te vernieuwen.

   De nieuwe resourcegroep wordt weergegeven in de lijst met resourcegroepen.

   ![Resourcegroeplijst](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Toegang verlenen

In RBAC verleent u toegang door een roltoewijzing te maken.

1. Kies in de lijst met **Resourcegroepen** de nieuwe resourcegroep **rbac-resource-group**.

1. Kies **Toegangsbeheer (IAM)**.

1. Ga naar het tabblad **Roltoewijzingen** om de huidige lijst met roltoewijzingen te zien.

   ![De blade Toegangsbeheer (IAM) voor een resourcegroep](./media/quickstart-assign-role-user-portal/access-control.png)

1. Kies **Roltoewijzing toevoegen** om het deelvenster Roltoewijzing toevoegen te openen.

   Als u niet bent gemachtigd voor het toewijzen van rollen, is de optie Roltoewijzing toevoegen uitgeschakeld.

   ![Deelvenster Roltoewijzing toevoegen](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Selecteer in de vervolgkeuzelijst **Rol** **Inzender voor virtuele machines**.

1. Selecteer in de lijst **Selecteren** uzelf of een andere gebruiker.

1. Kies **Opslaan** om de roltoewijzing te maken.

   Na enkele ogenblikken krijgt de gebruiker de rol Inzender voor virtuele machines toegewezen in het bereik van de resourcegroep rbac-resource-group.

   ![Roltoewijzing Inzender voor virtuele machines](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Toegang intrekken

Als u in RBAC de toegang wilt intrekken voor een rol, verwijdert u de roltoewijzing.

1. Plaats in de lijst met roltoewijzingen een vinkje naast de gebruiker met de rol Inzender voor virtuele machines.

1. Kies **Verwijderen**.

   ![Bericht bij verwijderen van roltoewijzing](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Kies **Ja** om te bevestigen dat u de roltoewijzing inderdaad wilt verwijderen.

## <a name="clean-up"></a>Opruimen

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies **rbac-resource-group** om de resourcegroep te openen.

1. Kies **Resourcegroep verwijderen** om de resourcegroep te verwijderen.

   ![Resourcegroep verwijderen](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Typ in de blade **Weet u zeker dat u wilt verwijderen** de naam van de resourcegroep: **rbac-resource-group**.

1. Kies **Verwijderen** om de resourcegroep te verwijderen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie - Toegang verlenen aan een gebruiker met RBAC en PowerShell](tutorial-role-assignments-user-powershell.md)

