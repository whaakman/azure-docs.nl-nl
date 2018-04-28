---
title: Toewijzen van rollen voor Azure-resources met behulp van Privileged Identity Management | Microsoft Docs
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
ms.openlocfilehash: 501f063992d2f5c7769a5c9059b346aa2b5c2bb4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="assign-roles-for-azure-resources-by-using-privileged-identity-management"></a>Toewijzen van rollen voor Azure-resources met behulp van Privileged Identity Management

## <a name="assign-roles"></a>Rollen toewijzen

Een gebruiker of groep toewijzen aan een rol wanneer u bekijkt de **rollen** deelvenster, selecteer de rol en selecteer vervolgens **gebruiker toevoegen**. 

![Deelvenster met de knop 'Gebruiker toevoegen' 'Functies'](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

U kunt ook selecteren **gebruiker toevoegen** van de **leden** deelvenster.

![Deelvenster met de knop 'Gebruiker toevoegen' 'Leden'](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Als u bent toevoeging van een gebruiker of groep uit de **leden** deelvenster, moet u: 

1. Kies een functie uit de **Selecteer een rol** deelvenster voordat u kunt een gebruiker of groep selecteren.

   !['Selecteer een rol' deelvenster](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Kies een gebruiker of groep in de map.

3. Kies het toewijzingstype van de juiste van de vervolgkeuzelijst: 

   - **In de tijd**: biedt de leden van de gebruiker of groep in aanmerking komt, maar geen permanente toegang tot de rol voor een opgegeven periode of voor onbepaalde tijd (indien geconfigureerd in de functie-instellingen). 
   - **Directe**: vereist niet dat de gebruiker of groep leden voor het activeren van de roltoewijzing (bekend als permanente toegang). Wordt u aangeraden rechtstreekse toewijzing toe voor gebruik op korte termijn, waarbij toegang niet vereist wanneer de taak voltooid is. Voorbeelden zijn op de aanroep verschuift en tijdgebonden activiteiten.

4. Als u de toewijzing ongedaan (permanent in aanmerking komen voor de toewijzing van een just-in-time, of permanent actief is voor de toewijzing van een direct), schakel het selectievakje hieronder de **toewijzingstype** vak.

   ![Deelvenster 'Lidmaatschapsinstellingen' met het selectievakje 'Toewijzingstype' en het bijbehorende selectievakje](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >Het is mogelijk dat het selectievakje unmodifiable als een andere beheerder de toewijzing van de maximale duur voor elk toewijzingstype is opgegeven in de functie-instellingen.

   Geef de duur van een specifieke toewijzing, schakel het selectievakje en de begin-en/of datum en tijd vakken wijzigen.

   !['Lidmaatschapsinstellingen' deelvenster met de selectievakjes in voor de begindatum, begintijd, einddatum en eindtijd](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Roltoewijzingen beheren

Beheerders kunnen roltoewijzingen beheren door te kiezen uit **rollen** of **leden** in het linkerdeelvenster. Selecteren **rollen** kunnen beheerders voor het bereik van hun beheertaken voor een specifieke functie. Selecteren **leden** alle gebruikers en groepen roltoewijzingen voor de resource wordt weergegeven.

![Deelvenster 'Functies'](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![Deelvenster 'Leden'](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Als u een rol in behandeling zijnde activering hebt, wordt een meldingsbanner aan de bovenkant van het deelvenster weergegeven wanneer u lidmaatschap weergeeft.


## <a name="modify-existing-assignments"></a>Bestaande toewijzingen wijzigen

Selecteer voor het wijzigen van bestaande toewijzingen van de gebruiker of groep detailweergave **instellingen wijzigen** uit de actiebalk. Wijzigen van het toewijzingstype **Just in time** of **Direct**.

!['Gebruikersgegevens' deelvenster met de knop 'wijzigingsinstellingen'](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
