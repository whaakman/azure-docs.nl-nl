---
title: Weergeven en toewijzen van beheerder machtigingen van de rol in Azure Active Directory | Microsoft Docs
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
ms.date: 09/25/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: cb2e5286eb8e910b555e221242a735f00dff4778
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182821"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Weergeven en toewijzen van beheerdersrollen in Azure Active Directory

U kunt nu zien en beheren van alle leden van de beheerdersrollen in de Azure Active Directory-portal. Als u vaak roltoewijzingen beheren, kunt u deze ervaring wordt waarschijnlijk liever. En als u zich ooit afgevraagd "Wat de heid moeten deze rollen echt doen?", ziet u een gedetailleerde lijst met machtigingen voor elk van de Azure AD-beheerdersrollen.

Het is gemakkelijk om te zien van uw eigen machtigingen ook. Klik op **uw rol** krijg snel toegang tot de pagina van de gebruiker voor een lijst van uw actieve toegewezen rollen. Klik op het weglatingsteken aan de rechterkant van elke rij de gedetailleerde beschrijving van de rol te openen.

![lijst met rollen in Azure AD-portal](./media/directory-manage-roles-portal/role-list.png)

Selecteer de rij voor een rol om de gebruikers die zijn toegewezen aan de rol weer te geven. U kunt selecteren **beheren in PIM** voor aanvullende beheermogelijkheden. Privileged Role Administrators "Permanent" kunt wijzigen (altijd actief in de rol) toewijzingen aan 'In aanmerking komende' (in de rol alleen als er verhoogde). Als u geen PIM hebt, kunt u nog steeds selecteren **beheren in PIM** om u te registreren voor een proefversie. Privileged Identity Management is vereist een [Azure AD Premium P2-licentieabonnement](../privileged-identity-management/subscription-requirements.md).

![lijst met leden van de rol van beheerder](./media/directory-manage-roles-portal/member-list.png)

Als u een globale beheerder of een beheerder met bevoorrechte rol bent, kunt u eenvoudig toevoegen of verwijderen leden, de lijst filteren, of Selecteer een lid om te zien van hun actieve rollen toegewezen krijgen.

## <a name="view-role-permissions"></a>Rolmachtigingen weergeven

Wanneer u de leden van een rol bekijkt, selecteert u **beschrijving** om te zien van de volledige lijst met machtigingen die zijn verleend door de roltoewijzing. De pagina bevat koppelingen naar relevante documentatie aan u te helpen bij het beheren van directory-rollen.

![lijst met machtigingen voor de rol van beheerder](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Volgende stappen

* U kunt delen met ons op de [forum van Azure AD-beheerdersrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Zie voor meer informatie over de functies en de toewijzing van de rol beheerder [beheerdersrollen toewijzen](directory-assign-admin-roles.md).
* Zie voor gedetailleerde bevoegdheden voor standaard een [vergelijking van de standaard Gast en lid gebruikersmachtigingen](../fundamentals/users-default-permissions.md).
