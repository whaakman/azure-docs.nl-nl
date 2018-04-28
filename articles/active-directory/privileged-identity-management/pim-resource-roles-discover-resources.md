---
title: Detecteren en beheren van Azure-resources met behulp van Privileged Identity Management | Microsoft Docs
description: Beschrijft hoe Azure-resources beveiligen met PIM.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 51a10ea164e8bd7650ad2823281d9ed6a4c91915
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="discover-and-manage-azure-resources-by-using-privileged-identity-management"></a>Detecteren en beheren van Azure-resources met behulp van Privileged Identity Management

Informatie over het detecteren en Azure-resources te beheren wanneer u Privileged Identity Management (PIM) in Azure Active Directory (Azure AD gebruikt). Deze informatie kan nuttig zijn voor organisaties die al gebruikmaken van PIM beheerder bronnen beveiligen, en abonnementseigenaren die op zoek zijn naar de productieresources beveiligen zijn.

Wanneer u eerst PIM voor Azure-resources instellen, moet u om te detecteren en selecteer resources met PIM moeten worden beveiligd. Er is geen limiet aan het aantal resources die u met PIM beheren kunt. We raden echter beginnen met de meest kritieke (productie)-resources.

> [!NOTE]
> U kunt alleen Zoek en selecteer abonnementresources beheren met behulp van PIM. Wanneer u een abonnement in PIM beheert, kunt u ook onderliggende resources in het abonnement te beheren.

## <a name="discover-resources"></a>Detecteren van bronnen

In de Azure portal, gaat u naar de **Privileged Identity Management** deelvenster. In het menu links in de **beheren** sectie **Azure-resources**.

![De 'Privileged Identity Management - Azure-resources' deelvenster](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Als dit de eerste keer met PIM voor Azure-resources, voer detectie om resources te beheren te zoeken. In de **bronnen detecteren** deelvenster, selecteer de **bronnen detecteren** knop starten van de ervaring voor de detectie.

![Het deelvenster 'Bronnen detecteren'](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Als een andere resource of directory-beheerder in uw organisatie al een Azure-resource beheren is met behulp van PIM of hebt u een in aanmerking komende roltoewijzing voor een resource, het bericht wordt weergegeven door de lijstweergave **detecteren van bronnen of activeren een in aanmerking komende roltoewijzing om door te gaan**. 

![De knop 'Bronnen detecteren' in de ' Privileged Identity Manager - Azure-resources ' deelvenster](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Wanneer u selecteert de **bronnen detecteren** knop, of in het menu bovenaan of in het midden van het deelvenster, verschijnt er een lijst met abonnementen die u kunt beheren. Abonnementen die zijn gemarkeerd al worden beveiligd door PIM.

> [!NOTE]
> Het abonnement kan niet onbeheerd om te voorkomen dat een andere resourcebeheerder PIM-instellingen, worden verwijderd nadat een abonnement is ingesteld om te worden beheerd.

![De 'Azure-resources - detectie"deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

In de **RESOURCE** kolom, plaatst u de muisaanwijzer op een abonnement dat u wilt beveiligen met PIM. Selecteer het selectievakje links van de naam van de resource. U kunt meerdere abonnementen per keer selecteren.

![De lijst met resources in de 'Azure-resources - detectie"deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Selecteer voor het initiëren van het voorbereidingsproces, in het bovenste menu **resource beheren**.

![De knop 'Resource beheren' in de 'Azure-resources - detectie"deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

De geselecteerde bronnen worden nu beheerd door de PIM. Als u wilt sluiten in de rechterbovenhoek van het scherm detectie selecteren **X**. Om te beginnen met het beheer van PIM-instellingen en toewijzen leden, in het menu aan de bovenkant van de **Privileged Identity Management - Azure-resources** deelvenster, selecteer de **vernieuwen** knop.

![De knop 'Vernieuwen' in het bovenste menu van de 'Privileged Identity Management - Azure-resources' deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Volgende stappen

- [Rolinstellingen configureren](pim-resource-roles-configure-role-settings.md)
- [Toewijzen van rollen in PIM](pim-resource-roles-assign-roles.md)
