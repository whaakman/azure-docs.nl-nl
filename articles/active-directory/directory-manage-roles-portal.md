---
title: De leden van een beheerdersrol weergeven in Azure Active Directory | Microsoft Docs
description: U kunt nu zien en beheren van leden van de rol van een Azure AD-beheerder in de portal. Voor diegenen die vaak roltoewijzingen beheren.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/02/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 39f7d69482845af48b1dce8c2e51f4acf77bf4fb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448057"
---
# <a name="view-all-members-of-an-administrator-role-in-azure-active-directory"></a>Alle leden van een beheerdersrol weergeven in Azure Active Directory

U kunt nu zien en beheren van alle leden van de beheerdersrollen in de Azure Active Directory-portal. Als u vaak roltoewijzingen beheren, kunt u deze ervaring wordt waarschijnlijk liever. En als u zich ooit afgevraagd "Wat de heid moeten deze rollen echt doen?", ziet u een gedetailleerde lijst met machtigingen voor elk van de Azure AD-beheerdersrollen.

Het is gemakkelijk om te zien van uw eigen machtigingen ook. Klik op **uw rol** krijg snel toegang tot de pagina van de gebruiker voor een lijst van uw actieve toegewezen rollen. Klik op het weglatingsteken aan de rechterkant van elke rij de gedetailleerde beschrijving van de rol te openen.

![lijst met rollen in Azure AD-portal](./media/directory-manage-roles-portal/role-list.png)

Selecteer de hele rij om de lijst met toegewezen leden weer te geven. U kunt selecteren **beheren in PIM** voor aanvullende beheermogelijkheden. Privileged Role Administrators "Permanent" kunt wijzigen (altijd actief in de rol) toewijzingen aan 'In aanmerking komende' (in de rol alleen als er verhoogde). Als u geen PIM hebt, kunt u nog steeds selecteren **beheren in PIM** om u te registreren voor een proefversie. Privileged Identity Management is vereist een [Azure AD Premium P2-licentieabonnement](./privileged-identity-management/subscription-requirements.md).

![lijst met leden van de rol van beheerder](./media/directory-manage-roles-portal/member-list.png)

Als u een globale beheerder of een beheerder met bevoorrechte rol bent, kunt u eenvoudig toevoegen of verwijderen leden, de lijst filteren, of Selecteer een lid gaat u naar de pagina van de gebruiker om te zien van hun actieve rollen toegewezen. 

## <a name="role-details-in-the-portal"></a>Details van de rol in de portal

Wanneer u de leden van een rol bekijkt, selecteert u **beschrijving** om te zien van de volledige lijst met machtigingen die zijn verleend door de roltoewijzing. De pagina bevat koppelingen naar relevante documentatie aan u te helpen bij het beheren van directory-rollen.

![lijst met machtigingen voor de rol van beheerder](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Volgende stappen

* U kunt delen met ons op de [forum van Azure AD-beheerdersrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Zie voor meer informatie over de functies en de toewijzing van de rol beheerder [beheerdersrollen toewijzen](users-groups-roles/directory-assign-admin-roles.md).
* Zie voor gedetailleerde bevoegdheden voor standaard een [vergelijking van de standaard Gast en lid gebruikersmachtigingen](./fundamentals/users-default-permissions.md).
