---
title: De activiteit en audit geschiedenis weergeven voor Azure-resource-rollen in PIM - Azure Active Directory | Microsoft Docs
description: Activiteiten bekijken en controlegeschiedenis voor Azure-resource-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 01/24/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6fb97c416d0e34635b8aa4c19fc6b94889f566c
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578132"
---
# <a name="view-activity-and-audit-history-for-azure-resource-roles-in-pim"></a>Activiteiten bekijken en controlegeschiedenis voor Azure-resource-rollen in PIM

Met Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kunt u activiteit, activeringen en controlegeschiedenis voor Azure-resources-rollen weergeven binnen uw organisatie. Dit omvat abonnementen, resourcegroepen en zelfs virtuele machines. Elke resource in Azure portal die gebruikmaakt van de toegang van Azure op rollen gebaseerd beheer (RBAC)-functionaliteit kunt profiteren van de beveiliging en de levenscyclus van beheermogelijkheden in PIM.

## <a name="view-activity-and-activations"></a>Activiteiten weergeven en activeringen

Als u wilt zien welke acties die een specifieke gebruiker in verschillende resources heeft, kunt u de activiteit Azure-resource die is gekoppeld aan een bepaalde activeringsperiode weergeven.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Klik op de resource die u wilt weergeven van de activiteit en activeringen voor.

1. Klik op **rollen** of **leden**.

1. Klik op een gebruiker.

    U ziet een grafische weergave van acties van de gebruiker in Azure-resources op datum. U ziet ook de recente rolactiveringen diezelfde periode van tijd.

    ![Gebruikersdetails](media/azure-pim-resource-rbac/rbac-user-details.png)

1. Klik op een specifieke rol moet worden geactiveerd om te zien van gegevens en de bijbehorende Azure-resource-activiteit die is opgetreden bij die gebruiker actief is.

    ![Selecteer de rolactivering](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="export-role-assignments-with-children"></a>De roltoewijzing met onderliggende items exporteren

U hebt een nalevingsvereiste waarin u een volledige lijst van roltoewijzingen te auditors moet opgeven. PIM kunt u de roltoewijzingen query op een specifieke bron, waaronder roltoewijzingen voor alle onderliggende resources. Voorheen was het moeilijk voor beheerders om een volledige lijst van roltoewijzingen voor een abonnement en ze had roltoewijzingen voor elke specifieke resource te exporteren. PIM gebruiken, kunt u zoeken naar alle actieve en in aanmerking komende roltoewijzingen in een abonnement met inbegrip van roltoewijzingen voor alle resourcegroepen en resources.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Klik op de resource die u wilt exporteren roltoewijzingen voor, zoals een abonnement.

1. Klik op **leden**.

1. Klik op **exporteren** om de uitvoer lidmaatschap deelvenster te openen.

    ![Lidmaatschap van deelvenster exporteren](media/azure-pim-resource-rbac/export-membership.png)

1. Klik op **exporteren van alle leden** alle roltoewijzingen in een CSV-bestand exporteren.

    ![CSV-bestand exporteren](media/azure-pim-resource-rbac/export-csv.png)

## <a name="view-resource-audit-history"></a>Resource-Controlegeschiedenis weergeven

Resourcecontrole geeft u een overzicht van alle activiteiten van de rol voor een resource.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Klik op de resource die u wilt weergeven van controlegeschiedenis voor.

1. Klik op **resourcecontrole**.

1. De geschiedenis met behulp van een vooraf gedefinieerde datum of aangepast datumbereik filteren.

    ![Resourcecontrole filteren](media/azure-pim-resource-rbac/rbac-resource-audit.png)

1. Voor **controletype**, selecteer **activeren (toegewezen + geactiveerd)**.

    ![Detail van activiteit](media/azure-pim-resource-rbac/rbac-audit-activity.png)

1. Onder **actie**, klikt u op **(activiteit)** voor een gebruiker om te zien van de gegevens van de activiteit van die gebruiker in Azure-resources.

    ![Detail van activiteit van gebruiker](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="view-my-audit"></a>Mijn controle weergeven

Mijn controle kunt u om uw persoonlijke rol activiteit weer te geven.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Klik op de resource die u wilt weergeven van controlegeschiedenis voor.

1. Klik op **mijn controle**.

1. De geschiedenis met behulp van een vooraf gedefinieerde datum of aangepast datumbereik filteren.

    ![Persoonlijke rol activiteit](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
- [Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM](pim-resource-roles-approval-workflow.md)
- [Controlegeschiedenis voor Azure AD-rollen in PIM weergeven](pim-how-to-use-audit-log.md)
