---
title: Weergave met Azure resource-rollen in PIM | Microsoft Docs
description: In de weergave die in Azure AD Privileged Identity Management (PIM) voor Azure-resourcerollen heeft.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: ce7c96d92938c4e3b4cc0b53271df48350083754
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465228"
---
# <a name="view-who-has-azure-resource-roles-in-pim"></a>Weergave met Azure resource-rollen in PIM

Met Azure Active Directory Privileged Identity Management (PIM), die u kunt beheren, controleren en toegang tot Azure-resources binnen uw organisatie controleren. Dit omvat abonnementen, resourcegroepen en zelfs virtuele machines. Elke resource in Azure portal die gebruikmaakt van de functionaliteit van Azure, op rollen gebaseerde toegang beheer (RBAC) kan profiteren van de beveiliging en de levenscyclus van mogelijkheden voor beheer in Azure AD PIM. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>PIM voor Azure-resources kan beheerders een resource

- Zien welke gebruikers en groepen zijn toegewezen rollen voor de Azure-resources beheren
- Op aanvraag, 'just-in-time' toegang tot bronnen zoals abonnementen en resourcegroepen beheren inschakelen
- Toegang tot toegewezen gebruikers/groepen met toewijzingsinstellingen voor nieuwe tijdsgebonden automatisch verlopen
- Tijdelijke toegang voor snelle taken of op een aanroep schema's toewijzen
- Multi-factor Authentication afdwingen voor toegang tot bronnen op een ingebouwde of aangepaste rol 
- Rapporten over resourceactiviteit voor resource access gecorreleerde tijdens de actieve sessie van een gebruiker ophalen
- Waarschuwingen ontvangen wanneer nieuwe gebruikers of groepen toegang tot resources worden toegewezen, en wanneer ze in aanmerking komende toewijzingen activeren

## <a name="view-activation-and-azure-resource-activity"></a>Activering en Azure-resource-activiteit bekijken

In het geval dat u nodig hebt om te zien welke verschillende resources heeft een specifieke gebruiker ondernomen acties, kunt u de Azure-resource-activiteit die is gekoppeld aan een bepaalde activeringsperiode (voor in aanmerking komende gebruikers) bekijken. Begin met het selecteren van een gebruiker vanuit de weergave van de leden of in de lijst met leden in een specifieke rol. Het resultaat wordt een grafische weergave van acties van de gebruiker op de Azure-resources op datum en de recente rolactiveringen diezelfde periode van tijd weergegeven.

![](media/azure-pim-resource-rbac/user-details.png)

Selecteren van een specifieke rol moet worden geactiveerd, ziet de details van rolactivering en de bijbehorende Azure-resource-activiteit die is opgetreden bij die gebruiker actief is.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="review-who-has-access-in-a-subscription"></a>Bekijk wie toegang in een abonnement heeft

Als u wilt controleren roltoewijzingen in uw abonnement, selecteer het tabblad leden in de linkernavigatiebalk of rollen selecteren en kiest u een specifieke rol om te controleren van leden. 

Controleer in de actiebalk bestaande toegangsbeoordelingen weergeven en selecteert u toevoegen aan het maken van een nieuwe beoordeling selecteren.

![](media/azure-pim-resource-rbac/owner.png)

[Meer informatie over toegangsbeoordelingen](pim-how-to-perform-security-review.md)

>[!NOTE]
Beoordelingen worden alleen ondersteund voor resourcetypen abonnement op dit moment.

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
- [Goedkeuren of weigeren van aanvragen voor Azure-resource-rollen in PIM](pim-resource-roles-approval-workflow.md)
- [Ingebouwde rollen in Azure](../../role-based-access-control/built-in-roles.md)
