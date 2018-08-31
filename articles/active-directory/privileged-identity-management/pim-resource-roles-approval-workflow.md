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
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e5cda31af5ac95e7ebe2b858b1d7355ea3f2a6bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189801"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM

Als u wilt goedkeuren of weigeren van een aanvraag, moet u lid zijn van de Fiatteurlijst zijn. 

1. Selecteer in PIM, **aanvragen goedkeuren** op het tabblad in het menu links en Ga naar de aanvraag.

   ![Deelvenster 'Aanvragen goedkeuren'](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Selecteer de aanvraag, een reden opgeven wanneer ze voor de beslissing en selecteer **goedkeuren** of **weigeren**. De aanvraag is vervolgens opgelost.

   ![Geselecteerde aanvraag met gedetailleerde informatie](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Werkstroommeldingen

Hier vindt u enige kennis over werkstroommeldingen:

- Alle leden van de Fiatteurlijst krijgt een melding via e-mail wanneer een aanvraag voor een rol in afwachting van de beoordeling is. E-mailmeldingen bevatten een directe koppeling naar de aanvraag, waarin de fiatteur kan goedkeuren of weigeren.
- Aanvragen worden opgelost door de eerste lid van de lijst die goedkeurt of weigert. 
- Wanneer een goedkeurder op de aanvraag reageert, zijn alle leden van de Fiatteurlijst een melding van de actie. 
- Resource-beheerders worden gewaarschuwd wanneer een goedgekeurde lid actief in hun rol. 

>[!Note]
>De resourcebeheerder van een die ervan overtuigd is dat een goedgekeurde lid niet actief mag zijn, kunt de toewijzing van actieve rollen in PIM verwijderen. Hoewel resource beheerders zijn niet op de hoogte gesteld van aanvragen in behandeling, tenzij ze lid van de Fiatteurlijst zijn, kunnen ze weergeven en annuleren in behandeling zijnde aanvragen van alle gebruikers weergeven in behandeling zijnde aanvragen in PIM. 

## <a name="next-steps"></a>Volgende stappen

- [Goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in PIM](azure-ad-pim-approval-workflow.md)
- [E-mailmeldingen in PIM](pim-email-notifications.md)
