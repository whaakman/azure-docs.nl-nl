---
title: Azure-resources te beheren in PIM detecteren | Microsoft Docs
description: Informatie over het detecteren van Azure-resources te beheren in Azure AD Privileged Identity Management (PIM).
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
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: b5d48b3f854afaa79574e0ec13cff91f60396ac6
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190655"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Azure-resources te beheren in PIM detecteren

Informatie over het detecteren en beheren van Azure-resources, wanneer u in Azure Active Directory (Azure AD) Privileged Identity Management (PIM) hebt gebruikt. Deze informatie kan nuttig voor organisaties die al gebruikmaken van PIM om de beheerder resources te beschermen, en eigenaars van abonnementen die behoefte hebben aan voor het beveiligen van productieresources zijn.

Wanneer u eerst ingesteld PIM voor Azure-resources, moet u om te detecteren en resources te beschermen met PIM selecteren. Er is geen limiet voor het aantal resources die u met PIM kunt beheren. We raden echter aan beginnen met uw meest kritieke (productie)-resources.

> [!NOTE]
> U kunt alleen Zoek en selecteer de management-groep of abonnement resources te beheren met PIM. Wanneer u een beheergroep of een abonnement in PIM beheert, kunt u ook de onderliggende resources te beheren.

## <a name="discover-resources"></a>Services detecteren

In de Azure-portal, gaat u naar de **Privileged Identity Management** deelvenster. In het menu links in de **beheren** sectie, selecteer **Azure-resources**.

![De 'Privileged Identity Management - Azure-resources' deelvenster](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Als dit de eerste keer met PIM voor Azure-resources, moet u eerst detectie om te zoeken naar bronnen voor het beheren van uitvoeren. In de **bronnen detecteren** venster de **bronnen detecteren** knop om te starten van de discovery-ervaring.

![Het deelvenster 'Detecteren'](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Als een andere resource of directory-beheerder in uw organisatie is al een Azure-resource worden beheerd met behulp van PIM, of als u een in aanmerking komende roltoewijzing voor een resource hebt, de lijstweergave met het bericht weergegeven wordt **resources detecteren of activeren een in aanmerking komende roltoewijzing om door te gaan**. 

![De knop 'Detecteren' in de ' Privileged Identity Manager - Azure-resources ' deelvenster](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Wanneer u selecteert de **bronnen detecteren** knop, of in het menu bovenaan of in het midden van het deelvenster, verschijnt er een lijst met abonnementen die u kunt beheren. Abonnementen die zijn gemarkeerd worden al beveiligd door PIM.

> [!NOTE]
> Het abonnement kan niet worden niet-beheerde om te voorkomen dat een andere resourcebeheerder PIM-instellingen, worden verwijderd nadat een abonnement is ingesteld op beheerd.

![De 'Azure-resources - detectie"deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

In de **RESOURCE** kolom plaats de muisaanwijzer op een abonnement dat u wilt beveiligen met PIM. Selecteer vervolgens het selectievakje aan de linkerkant van de naam van de resource. U kunt meerdere abonnementen per keer selecteren.

![De lijst met resources in de 'Azure-resources - detectie"deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Selecteer voor het starten van het onboarding-proces, in het bovenste menu **resource beheren**.

![De knop 'Resource beheren' in de 'Azure-resources - detectie"deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

De geselecteerde resources worden nu beheerd door PIM. Selecteer om te sluiten van het scherm detectie in de rechterbovenhoek **X**. Om te beginnen met PIM-instellingen beheren en toewijzen leden, in het menu aan de bovenkant van de **Privileged Identity Management - Azure-resources** venster de **vernieuwen** knop.

![De knop 'Vernieuwen' in het bovenste menu van de 'Privileged Identity Management - Azure-resources' deelvenster](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
