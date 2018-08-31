---
title: Azure-resource-rollen in PIM toewijzen | Microsoft Docs
description: Informatie over het toewijzen van rollen voor Azure-resource in Azure AD Privileged Identity Management (PIM).
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
ms.openlocfilehash: 7019a6f97a9590d3b652584015f3077f4ed075af
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188917"
---
# <a name="assign-azure-resource-roles-in-pim"></a>Azure-resource-rollen in PIM toewijzen

Azure AD PIM kunt beheren de ingebouwde Azure-resourcerollen, evenals aangepaste rollen, inclusief (maar niet beperkt tot):

- Eigenaar
- Beheerder van gebruikerstoegang
- Inzender
- Beveiligingsbeheerder
- Beveiligingsbeheer en meer

>[!NOTE]
Gebruikers of leden van een groep die is toegewezen aan de rollen eigenaar of Administrator voor gebruikerstoegang en globale beheerders waarmee beheer van abonnementen in Azure AD zijn Resource-beheerders. Deze beheerders kunnen rollen toewijzen, configureren van instellingen voor de sitesysteemrol en toegang met behulp van PIM voor Azure-Resources beoordelen. De lijst weergeven met [ingebouwde rollen voor Azure-resources](../../role-based-access-control/built-in-roles.md).

## <a name="assign-roles"></a>Rollen toewijzen

Een gebruiker of groep toewijzen aan een rol wanneer u bekijkt de **rollen** deelvenster, selecteert u de rol, en selecteer vervolgens **gebruiker toevoegen**. 

![Deelvenster 'Functies' met de knop 'Gebruiker toevoegen'](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

U kunt ook selecteren **gebruiker toevoegen** uit de **leden** deelvenster.

![Deelvenster 'Leden' met de knop 'Gebruiker toevoegen'](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Als u van een gebruiker toevoegen wilt of groep uit de **leden** deelvenster, moet u naar: 

1. Kies een rol van de **Selecteer een rol** deelvenster voordat u kunt een gebruiker of groep selecteren.

   ![Deelvenster 'Selecteer een rol'](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Kies een gebruiker of groep in de map.

3. Het juiste toewijzingstype kiezen uit de vervolgkeuzelijst: 

   - **Just-in-time**: leden van de gebruiker of groep die in aanmerking komen, maar geen permanente toegang biedt tot de rol voor een bepaalde periode of voor onbepaalde tijd (indien geconfigureerd in de rolinstellingen). 
   - **Directe**: vereist niet de leden van de gebruiker of groep activeren van de roltoewijzing (bekend als permanente toegang). Wordt u aangeraden rechtstreekse toewijzing toe voor gebruik op korte termijn, waarbij toegang wordt niet zijn vereist wanneer de taak voltooid is. Voorbeelden zijn op de aanroep ploegen en tijdgebonden activiteiten.

4. Als de toewijzing moet permanent (permanent in aanmerking komen voor de toewijzing van een just-in-time of permanent actief voor een rechtstreekse toewijzing toe), selecteert u het onderstaande selectievakje in de **toewijzingstype** vak.

   ![Deelvenster 'Lidmaatschapsinstellingen' met het selectievakje 'Type toewijzing' en het bijbehorende selectievakje](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >Het is mogelijk dat het selectievakje unmodifiable als een andere beheerder de duur van de maximale toewijzing voor elk toewijzingstype is opgegeven in de functie-instellingen.

   Geef een duur van de specifieke toewijzing, schakel het selectievakje uit en de begin-en/of end vakken voor datum en tijd wijzigen.

   ![Deelvenster 'Lidmaatschapsinstellingen' met selectievakjes voor de begindatum, begintijd, einddatum en eindtijd](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Roltoewijzingen beheren

Beheerders kunnen roltoewijzingen beheren erin **rollen** of **leden** in het linkerdeelvenster. Selecteren **rollen** kunnen beheerders het bereik van hun beheertaken met een specifieke rol. Selecteren **leden** weergegeven van alle gebruikers en groepen-roltoewijzingen voor de resource.

![Deelvenster 'Functies'](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![Deelvenster 'Leden'](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Als u een rol in behandeling zijnde activering hebt, wordt een meldingsbanner wordt weergegeven aan de bovenkant van het deelvenster wanneer u lidmaatschap bekijkt.


## <a name="modify-existing-assignments"></a>Bestaande toewijzingen wijzigen

Als u wilt wijzigen van bestaande toewijzingen uit de detailweergave van de gebruiker of groep, selecteert u **instellingen wijzigen** in de actiebalk. Het toewijzingstype naar **Just-in-time** of **Direct**.

!['Gebruikersgegevens' deelvenster met de knop "Instellingen wijzigen"](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
