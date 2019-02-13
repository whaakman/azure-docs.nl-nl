---
title: Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM | Microsoft Docs
description: Meer informatie over het goedkeuren of weigeren van aanvragen voor de Azure-resource in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: c176055098cd1a24fec6b4bc4a1265c1062ac82b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56218003"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM

Met Azure AD Privileged Identity Management (PIM), kunt u rollen om te vereisen van goedkeuring voor activering configureren en kies een of meer gebruikers of groepen als gedelegeerde fiatteurs. Volg de stappen in dit artikel voor het goedkeuren of weigeren van aanvragen voor Azure-resourcerollen.

## <a name="view-pending-requests"></a>De aanvragen in de wachtrij weergeven

Als een gedelegeerde fiatteur ontvangt u een e-mailmelding wanneer een aanvraag om de rol Azure-resource in afwachting van uw goedkeuring is. U kunt deze aanvragen in behandeling weergeven in PIM.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **aanvragen goedkeuren**.

    ![Azure-resources - aanvragen goedkeuren](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    In de **aanvragen voor rolactiveringen** sectie ziet u een lijst met aanvragen moeten worden goedgekeurd.

## <a name="approve-requests"></a>Aanvragen goedkeuren

1. Zoek en klik op de aanvraag die u wilt goedkeuren. Een goedkeuring deelvenster wordt weergegeven.

    ![Deelvenster aanvragen goedkeuren](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. In de **reden** typt u een reden op.

1. Klik op **goedkeuren**.

    Er wordt een melding weergegeven met uw goedkeuring wordt vereist.

    ![Melding goed te keuren](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

## <a name="deny-requests"></a>Aanvragen weigeren

1. Zoek en klik op de aanvraag die u wilt weigeren. Een goedkeuring deelvenster wordt weergegeven.

    ![Deelvenster aanvragen goedkeuren](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. In de **reden** typt u een reden op.

1. Klik op **weigeren**.

    Er wordt een melding weergegeven met uw DOS-aanval.

## <a name="workflow-notifications"></a>Werkstroommeldingen

Hier volgt informatie over werkstroommeldingen:

- Alle leden van de Fiatteurlijst krijgt een melding via e-mail wanneer een aanvraag voor een rol in afwachting van de beoordeling is. E-mailmeldingen bevatten een directe koppeling naar de aanvraag, waarin de fiatteur kan goedkeuren of weigeren.
- Aanvragen worden opgelost door de eerste lid van de lijst die goedkeurt of weigert.
- Wanneer een goedkeurder op de aanvraag reageert, zijn alle leden van de Fiatteurlijst een melding van de actie.
- Resource-beheerders worden gewaarschuwd wanneer een goedgekeurde lid actief in hun rol.

>[!Note]
>De resourcebeheerder van een die ervan overtuigd is dat een goedgekeurde lid niet actief mag zijn, kunt de toewijzing van actieve rollen in PIM verwijderen. Hoewel resource beheerders zijn niet op de hoogte gesteld van aanvragen in behandeling, tenzij ze lid van de Fiatteurlijst zijn, kunnen ze weergeven en annuleren in behandeling zijnde aanvragen van alle gebruikers weergeven in behandeling zijnde aanvragen in PIM. 

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM verlengen of vernieuwen](pim-resource-roles-renew-extend.md)
- [E-mailmeldingen in PIM](pim-email-notifications.md)
- [Goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in PIM](azure-ad-pim-approval-workflow.md)
