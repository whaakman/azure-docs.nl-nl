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
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7bf1e437e97fdb4d929af23bd7b2a9abb49268df
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189155"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in PIM

Met Privileged Identity Management, kunt u rollen om te vereisen van goedkeuring voor activering configureren en kies een of meer gebruikers of groepen als gedelegeerde fiatteurs.

## <a name="view-pending-approvals-requests"></a>weergeven in afwachting van goedkeuring (aanvragen)

Als een gedelegeerde fiatteur ontvangt u e-mailmeldingen wanneer een aanvraag voor de goedkeuring in behandeling is. Als u wilt weergeven van deze aanvragen in de portal voor PIM, vanuit het dashboard (in de nieuwe navigatiefunctie) Selecteer het tabblad 'Goedkeuringsaanvragen in behandeling' in de linker navigatiebalk.

![](media/azure-ad-pim-approval-workflow/image023.png)

Daar ziet u een lijst met aanvragen in afwachting van goedkeuring:

![](media/azure-ad-pim-approval-workflow/image024.png)

## <a name="approve-or-deny-requests-for-role-elevation-single-andor-bulk"></a>Goedkeuren of weigeren van aanvragen om benodigde bevoegdheden rol (één en/of bulk)

Selecteer de aanvragen die u wilt goedkeuren of afwijzen en klik op de knop in de actiebalk die met uw beslissing overeenkomt:

![](media/azure-ad-pim-approval-workflow/image025.png)

## <a name="provide-justification-for-my-approvaldenial"></a>Reden opgeven wanneer ze voor Mijn goedkeuring/geweigerd

Hiermee wordt een nieuwe blade als u wilt goedkeuren of weigeren van meerdere aanvragen in één keer geopend. Voer een reden voor uw beslissing, en klik op (toestaan of weigeren) aan de onderkant of de blade:

![](media/azure-ad-pim-approval-workflow/image029.png)

Wanneer het proces voor aanvragen voltooid is, het statussymbool de beslissing weer (in dit voorbeeld wordt de beslissing is goedgekeurd):

![](media/azure-ad-pim-approval-workflow/image031.png)

## <a name="next-steps"></a>Volgende stappen

- [Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM](pim-resource-roles-approval-workflow.md)
- [E-mailmeldingen in PIM](pim-email-notifications.md)
