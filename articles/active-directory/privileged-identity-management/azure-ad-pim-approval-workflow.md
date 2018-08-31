---
title: Goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in PIM | Microsoft Docs
description: Meer informatie over het goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: bc60c0dbe6c609a08907feb243fe514badfe3e4e
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288072"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in PIM

Met Azure AD Privileged Identity Management (PIM), kunt u rollen om te vereisen van goedkeuring voor activering configureren en kies een of meer gebruikers of groepen als gedelegeerde fiatteurs.

## <a name="view-pending-requests"></a>De aanvragen in de wachtrij weergeven

Als een gedelegeerde fiatteur ontvangt u e-mailmeldingen wanneer een aanvraag voor de goedkeuring in behandeling is. U kunt deze aanvragen in behandeling weergeven in PIM.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-maprollen**.

1. Klik op **aanvragen goedkeuren**.

    ![Rollen in PIM-Azure AD directory - rollen](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    U ziet een lijst met aanvragen moeten worden goedgekeurd.

## <a name="approve-requests"></a>Aanvragen goedkeuren

1. Selecteer de aanvragen die u wilt goedkeuren en klik vervolgens op **goedkeuren** geselecteerde aanvragen deelvenster opent u de goedkeuren.

    ![Lijst met aanvragen PIM goedkeuren](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. In de **reden voor goedkeuring** typt u een reden op.

    ![PIM goedkeuren geselecteerd aanvragen](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Klik op **goedkeuren**.

    Het statussymbool wordt bijgewerkt met uw goedkeuring.

    ![PIM goedkeuren geselecteerd aanvragen](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Aanvragen weigeren

1. Selecteer de aanvragen die u wilt weigeren en klik vervolgens op **weigeren** geselecteerde aanvragen deelvenster opent u de weigeren.

    ![Lijst met aanvragen PIM goedkeuren](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. In de **reden voor weigering** typt u een reden op.

    ![PIM weigeren geselecteerd aanvragen](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Klik op **weigeren**.

    Het statussymbool wordt bijgewerkt met uw DOS-aanval.

## <a name="next-steps"></a>Volgende stappen

- [Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM](pim-resource-roles-approval-workflow.md)
- [E-mailmeldingen in PIM](pim-email-notifications.md)
