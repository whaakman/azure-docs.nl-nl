---
title: Privileged Identity Management detecteren en beheren van Azure-resources voor Azure-Resources - | Microsoft Docs
description: Beschrijft hoe Azure-resources te beveiligen.
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
ms.openlocfilehash: 78650e47ec92aa144e4ccc8c57f309240bf31ee3
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="discover-and-manage-azure-resources"></a>Detecteren en beheren van Azure-resources

Als uw organisatie al van Azure AD PIM gebruikmaakt beheerders in uw directory beveiligen of bent u de eigenaar van een abonnement wilt beveiligen productiebronnen bent u op de juiste plaats.

Wanneer u PIM voor het eerst voor Azure-resources inschakelt, moet u om te detecteren en selecteer resources met PIM moeten worden beveiligd. Er is geen limiet voor het aantal resources die u met PIM beheren kunt, maar het is raadzaam eerst wordt gestart met de meest kritieke (productie)-resources.

> [!Note]
> Alleen de abonnementresources kunnen worden doorzocht en geselecteerd voor beheer. Kiezen voor het beheren van een abonnement in PIM wordt beheer inschakelen voor alle onderliggende resources ook.

## <a name="discover-resources"></a>Detecteren van bronnen

Ga naar Azure AD PIM en Azure-resources in het gedeelte van het beheren van het menu linkernavigatievenster selecteert.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Als dit de eerste keer moet PIM voor Azure-resources met u uitvoeren van detectie om resources te beheren te zoeken.
Klik op de knop 'Bronnen detecteren' in het midden van het scherm voor het starten van de ervaring voor de detectie.

![](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Als een Azure-resource met PIM wordt al beheerd door een andere resource of directory-beheerder in uw organisatie, of u een in aanmerking komende roltoewijzing voor een resource hebt, bevat de lijstweergave het bericht: 'bronnen detecteren of een in aanmerking komende rol activeren toewijzing om door te gaan'. 

![](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Wanneer u de knop in de actiebalk of het midden van het scherm om te ontdekken resources selecteert, ziet u een lijst met beschikbare voor het beheer van abonnementen. Op dit moment ziet u de geselecteerde abonnementen geeft aan dat ze zijn beveiligd met PIM.

> [!Note]
> Om te voorkomen dat een andere resourcebeheerder PIM-instellingen worden verwijderd wanneer een abonnement wordt beheerd kan niet zonder begeleiding.

![](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

Beweeg de muisaanwijzer over een abonnement dat u wilt beveiligen met PIM en selecteer het selectievakje aan de linkerkant van de rij. U kunt meerdere abonnementen per keer selecteren.

![](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Voor het initiëren van het voorbereidingsproces proces selecteert u de knop 'Resource beheren' op de balk aan de bovenkant van het scherm.

![](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

De geselecteerde bronnen worden nu beheerd door de PIM. Sluit het detectie-scherm met 'X' in de rechterbovenhoek van de pagina en klik op Vernieuwen op de balk aan de bovenkant van het scherm beheren Azure-resources om te beginnen met het beheer van PIM-instellingen en toewijzen van leden.

![](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Volgende stappen

[Functie-instellingen configureren](pim-resource-roles-configure-role-settings.md)

[Toewijzen van rollen in PIM](pim-resource-roles-assign-roles.md)
