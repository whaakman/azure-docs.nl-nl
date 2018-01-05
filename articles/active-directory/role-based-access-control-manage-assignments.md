---
title: Azure-resource toegangstoewijzingen weergeven | Microsoft Docs
description: Weergeven en beheren van alle toewijzingen van rollen gebaseerd toegangsbeheer voor een gebruiker of groep in de Azure portal
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: 51b158ebfeea52e726213edd042277779f52124b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal"></a>Toegang tot toewijzingen weergeven voor gebruikers en groepen in de Azure portal
> [!div class="op_single_selector"]
> * [Toegang beheren per gebruiker of groep](role-based-access-control-manage-assignments.md)
> * [Toegang beheren per resource](role-based-access-control-configure.md)

Met op rollen gebaseerde toegangsbeheer (RBAC) in de Azure Active Directory (Azure AD), kunt u toegang tot uw Azure-resources te beheren. 

Toegang met RBAC toegewezen is fijnmazig omdat er zijn twee manieren kunt u de machtigingen beperken:

* **Bereik:** RBAC-roltoewijzingen zijn gericht op een bepaald abonnement, resourcegroep of resource. Een gebruiker toegang krijgen tot een enkele bron heeft geen toegang tot alle andere resources in hetzelfde abonnement.
* **Rol:** binnen het bereik van de toewijzing toegang wordt teruggebracht nog verder door een rol toewijst. Rollen kunnen worden op hoog niveau, zoals eigenaar of specifieke, zoals de lezer van de virtuele machine.

Rollen kunnen alleen binnen het abonnement, resourcegroep of resource die het bereik voor de toewijzing worden toegewezen. Maar u kunt de toegang tot toewijzingen voor een bepaalde gebruiker of groep weergeven op één plaats. U kunt maximaal 2000 roltoewijzingen in elk abonnement hebben. 

Meer informatie over het [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](role-based-access-control-configure.md).

## <a name="view-access-assignments"></a>De toewijzingen toegang weergeven
Als u de toegang tot toewijzingen voor een enkele gebruiker of groep opzoeken, te starten in Azure Active Directory in de [Azure-portal](http://portal.azure.com).

1. Selecteer **Azure Active Directory**. Als deze optie niet zichtbaar in de navigatie-lijst is, selecteert u **meer Services** en schuif vervolgens omlaag naar zoeken **Azure Active Directory**.
2. Selecteer **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers** of **alle groepen**. In dit voorbeeld richten we op afzonderlijke gebruikers.
    ![Beheren van gebruikers en groepen in Azure Active Directory - schermafbeelding](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. De gebruiker zoeken op naam of gebruikersnaam.
4. Selecteer **Azure-resources** op de blade van de gebruiker. Alle toegangstoewijzingen voor de betreffende gebruiker worden nu weergegeven.

### <a name="read-permissions-to-view-assignments"></a>Leesmachtigingen voor het weergeven van toewijzingen
Deze pagina bevat alleen de toegangstoewijzingen dat u gemachtigd bent om te lezen. Bijvoorbeeld, u leestoegang hebben tot een abonnement en gaat u naar de blade van Azure-resources om te controleren van een gebruiker toewijzingen. U kunt zien haar toegangstoewijzingen voor een abonnement, maar niet zien dat zij ook toegangstoewijzingen op abonnement B. heeft

## <a name="delete-access-assignments"></a>Toegangstoewijzingen verwijderen
U kunt via deze blade toegangstoewijzingen die rechtstreeks aan een gebruiker of groep zijn toegewezen verwijderen. Als de toewijzing van de toegang is overgenomen van een bovenliggende groep, moet u gaat u naar de bron- of -abonnement en de toewijzing er beheren.

1. Selecteer in de lijst van alle toegangstoewijzingen van de voor een gebruiker of groep die u wilt verwijderen.
2. Selecteer **verwijderen** en vervolgens **Ja** om te bevestigen.
    ![Verwijder toegang toewijzing - schermafbeelding](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="next-steps"></a>Volgende stappen

* Aan de slag met toegangsbeheer op basis van rollen voor [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](role-based-access-control-configure.md)
* Zie de [Ingebouwde RBAC-rollen](role-based-access-built-in-roles.md)

