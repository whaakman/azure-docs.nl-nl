---
title: Azure multi-factor Authentication in de Azure-resources wordt afgedwongen met behulp van Privileged Identity Management | Microsoft Docs
description: Dit document wordt beschreven hoe u multi-factor authentication voor PIM-resources inschakelen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 42c93a107c7ed58d6f7412b61627dccaf9076bb7
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617867"
---
# <a name="enforce-azure-multi-factor-authentication-in-azure-resources-by-using-privileged-identity-management"></a>Azure multi-factor Authentication in de Azure-resources wordt afgedwongen met behulp van Privileged Identity Management

Privileged Identity Management (PIM) voor Azure-resourcerollen kunnen beheerders van de resource en identiteitsbeheerders ter bescherming van essentiële Azure-infrastructuur met tijdsgebonden lidmaatschap en just-in-time-toegang. PIM biedt bovendien optioneel afdwinging van Azure multi-factor Authentication voor twee verschillende scenario's.

## <a name="require-multi-factor-authentication-to-activate"></a>Vereis multi-factor Authentication te activeren

Resource-beheerders kunnen vereisen dat in aanmerking komende leden van een rol om uit te voeren van Azure multi-factor Authentication voordat ze kunnen activeren. Dit proces zorgt ervoor dat de gebruiker die vraagt om de activering is wie ze beweren te zijn met redelijke zekerheid. Afdwingen van deze optie worden beveiligd op kritieke resources in situaties wanneer het gebruikersaccount dat mogelijk is aangetast. 

Selecteer een resource in de lijst van beheerde resources voor het afdwingen van deze vereiste. Uit de [een dashboard met](pim-resource-roles-overview-dashboards.md), selecteer een rol in de lijst met rollen in het gedeelte rechts van het scherm.

Bovendien krijgt u functie-instellingen uit de **rollen** of **rolinstellingen** tabbladen in het linkerdeelvenster.

>[!Note]
>Als de opties in het linkerdeelvenster lichter gekleurd weergegeven worden en u een banner boven aan de pagina met de mededeling 'U hebt in aanmerking komende rollen die kunnen worden geactiveerd' ziet, bent u niet een actieve beheerder. Dit betekent dat u moet [activeren](pim-resource-roles-activate-your-roles.md) voordat u doorgaat.

!['Functies' en 'rolinstellingen' tabbladen ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Als u wilt weergeven van een rol lidmaatschap, selecteer **rolinstellingen** in de balk aan de bovenkant van het scherm te openen de **rol instelling Details**.

De om rolinstellingen te wijzigen, selecteert u de **bewerken** bovenaan op de knop.

In de sectie onder **activeren**, schakel het selectievakje in om te **overeenkomende multi-factor Authentication bij activering**. Selecteer vervolgens **Opslaan**.

![Multi-Factor Authentication bij activering vereisen](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Meervoudige verificatie vereisen voor toewijzing

In sommige gevallen kan een resourcebeheerder van een mogelijk wilt toewijzen van een lid aan een rol voor korte tijd (bijvoorbeeld één dag). In dit geval hoeft ze niet de toegewezen leden aan de aanvraag voor activering. In dit scenario kan geen PIM multi-factor Authentication afdwingen wanneer het lid maakt gebruik van hun roltoewijzing, omdat ze al actief zijn in de rol vanaf het moment dat ze zijn toegewezen.

Om ervoor te zorgen dat de beheerder van de resource die voldoen aan de toewijzing is wie ze beweren te zijn, kunt u multi-factor Authentication afdwingen voor toewijzing.

Vanuit de dezelfde rol instelling detailscherm, schakel het selectievakje aan **overeenkomende multi-factor Authentication voor directe toewijzing**.

![Meervoudige verificatie vereisen voor directe toewijzing](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Volgende stappen

[Goedkeuring vereisen voor activeren](pim-resource-roles-approval-workflow.md)

[Het controlelogboek gebruiken](pim-resource-roles-use-the-audit-log.md)



