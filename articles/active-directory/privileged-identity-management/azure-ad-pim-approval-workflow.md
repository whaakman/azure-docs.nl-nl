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
ms.subservice: pim
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1c4b3819494ecb4c0897edeb08e2ce45d867b626
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179069"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in PIM

Met Azure AD Privileged Identity Management (PIM), kunt u rollen om te vereisen van goedkeuring voor activering configureren en kies een of meer gebruikers of groepen als gedelegeerde fiatteurs. Volg de stappen in dit artikel voor het goedkeuren of weigeren van aanvragen voor Azure AD-maprollen.

## <a name="view-pending-requests"></a>De aanvragen in de wachtrij weergeven

Als een gedelegeerde fiatteur ontvangt u een e-mailmelding wanneer een aanvraag om de Azure AD directory rol moeten worden goedgekeurd. U kunt deze aanvragen in behandeling weergeven in PIM.

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

- [E-mailmeldingen in PIM](pim-email-notifications.md)
- [Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM](pim-resource-roles-approval-workflow.md)
