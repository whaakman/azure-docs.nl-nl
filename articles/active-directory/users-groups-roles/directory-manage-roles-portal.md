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
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/06/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 267b9f7c6dcdc994345371a0884db3c1271f56bd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198160"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Weergeven en toewijzen van beheerdersrollen in Azure Active Directory

U kunt nu zien en beheren van alle leden van de beheerdersrollen in de Azure Active Directory-portal. Als u vaak roltoewijzingen beheren, kunt u deze ervaring wordt waarschijnlijk liever. En als u zich ooit afgevraagd "Wat de heid moeten deze rollen echt doen?", ziet u een gedetailleerde lijst met machtigingen voor elk van de Azure AD-beheerdersrollen.

## <a name="view-all-roles"></a>Alle rollen weergeven

Selecteer in Azure Active Directory, **rollen en beheerders** om te zien van de lijst met alle beschikbare rollen. 

Klik op het weglatingsteken aan de rechterkant van elke rij de gedetailleerde beschrijving van de rol te openen.

![lijst met rollen in Azure AD-portal](./media/directory-manage-roles-portal/role-list.png)

## <a name="view-my-roles"></a>Mijn rollen weergeven

Het is gemakkelijk om uw eigen machtigingen ook weer te geven. Selecteer **uw rol** op de **rollen en beheerders** pagina om te bekijken van de functies die momenteel aan u zijn toegewezen.

## <a name="view-assignments-for-a-role"></a>Toewijzingen voor een rol weergeven

Klik op een rol om de gebruikers die zijn toegewezen aan de rol weer te geven. U kunt selecteren **beheren in PIM** voor aanvullende beheermogelijkheden. Privileged Role Administrators "Permanent" kunt wijzigen (altijd actief in de rol) toewijzingen aan 'In aanmerking komende' (in de rol alleen als er verhoogde). Als u geen PIM hebt, kunt u nog steeds selecteren **beheren in PIM** om u te registreren voor een proefversie. Privileged Identity Management is vereist een [Azure AD Premium P2-licentieabonnement](../privileged-identity-management/subscription-requirements.md).

![lijst met leden van de rol van beheerder](./media/directory-manage-roles-portal/member-list.png)

Als u een globale beheerder of een beheerder met bevoorrechte rol bent, kunt u eenvoudig toevoegen of verwijderen leden, de lijst filteren, of Selecteer een lid om te zien van hun actieve rollen toegewezen krijgen.

## <a name="view-a-users-role-permissions"></a>Machtigingen van de rol van een gebruiker weergeven

Wanneer u de leden van een rol bekijkt, selecteert u **beschrijving** om te zien van de volledige lijst met machtigingen die zijn verleend door de roltoewijzing. De pagina bevat koppelingen naar relevante documentatie aan u te helpen bij het beheren van directory-rollen.

![lijst met machtigingen voor de rol van beheerder](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Volgende stappen

* U kunt delen met ons op de [forum van Azure AD-beheerdersrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Zie voor meer informatie over de functies en de toewijzing van de rol beheerder [beheerdersrollen toewijzen](directory-assign-admin-roles.md).
* Zie voor gedetailleerde bevoegdheden voor standaard een [vergelijking van de standaard Gast en lid gebruikersmachtigingen](../fundamentals/users-default-permissions.md).
