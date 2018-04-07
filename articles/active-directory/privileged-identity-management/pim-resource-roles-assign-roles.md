---
title: Privileged Identity Management voor Azure-Resources - rollen toewijzen | Microsoft Docs
description: Beschrijving van het toewijzen van rollen in PIM.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 9a9046afe2ee1e578333ff9d29f6fb21e95a0f22
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---assign"></a>Privileged Identity Management - resourcerollen - toewijzen

## <a name="assign-roles"></a>Rollen toewijzen

Als u wilt een gebruiker of groep toewijzen aan een rol, selecteer de rol (als bekijkt rollen), 

![](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

of klik op toevoegen uit de actiebalk (als op de weergave leden).

![](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Als een gebruiker of groep op het tabblad leden wilt toevoegen, moet u naar: 

1. Kies een rol in het menu toevoegen voordat u kunt een gebruiker of groep selecteren.

![](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Kies een gebruiker of groep in de map.

3. Kies het toewijzingstype van de juiste in de vervolgkeuzelijst. 

    - **Just In Time toewijzing:** u biedt de leden van de gebruiker of groep in aanmerking komende maar geen permanente toegang tot de functie voor een opgegeven periode of voor onbepaalde tijd (indien geconfigureerd in de functie-instellingen). 
    - **Rechtstreekse toewijzing:** hoeft niet de leden van de gebruiker of groep voor het activeren van de roltoewijzing (bekend als permanente toegang). Microsoft raadt het gebruik van rechtstreekse toewijzing op korte termijn gebruikt zoals ploegendiensten op oproep of tijd gevoelige activiteiten, waarbij toegang niet vereist wanneer de taak voltooid is.

Een selectievakje onder de vervolgkeuzelijst toewijzing kunt u opgeven als de toewijzing permanente moet (permanent in aanmerking voor het activeren van Just in Time toewijzing/permanent active voor rechtstreekse toewijzing).

![](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

>[!NOTE]
>Het is mogelijk dat het selectievakje unmodifiable als een andere beheerder de toewijzing van de maximale duur voor elk toewijzingstype is opgegeven in de functie-instellingen.

 Geef de duur van een specifieke toewijzing, schakel het selectievakje in en start wijzigen en/of datum- en tijdvelden eindigen.

![](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Roltoewijzingen beheren

Beheerders kunnen roltoewijzingen beheren door functies of leden selecteren in de linkernavigatiebalk. Functies selecteren, kunnen beheerders hun beheertaken voor een specifieke rol bereik terwijl de leden wordt weergegeven voor alle gebruikers en groepen roltoewijzingen voor de resource.

![](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Als u een rol in behandeling zijnde activering hebt, wordt een meldingsbanner wordt weergegeven boven aan de pagina wanneer lidmaatschap weer te geven.


## <a name="modify-existing-assignments"></a>Bestaande toewijzingen wijzigen

Selecteer de instellingen wijzigen van de actiebalk boven aan de pagina voor het wijzigen van bestaande toewijzingen van de gebruiker of groep detailweergave. Wijzig het toewijzingstype in NET In tijdtoewijzing of rechtstreekse toewijzing toe.

![](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
