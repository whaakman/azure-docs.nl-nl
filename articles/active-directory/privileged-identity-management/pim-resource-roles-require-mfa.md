---
title: Azure multi-factor Authentication afdwingen bij het Azure-resources met behulp van Privileged Identity Management | Microsoft Docs
description: Dit document beschrijft het inschakelen van multi-factor authentication voor PIM-resources.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 0c50a17b651fc1fb5d49915da5da4a37d40a0d3e
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260060"
---
# <a name="enforce-azure-multi-factor-authentication-in-azure-resources-by-using-privileged-identity-management"></a>Azure multi-factor Authentication afdwingen bij het Azure-resources met behulp van Privileged Identity Management

Privileged Identity Management (PIM) voor Azure-resource-functies kunt resource-beheerders en beheerders van de identiteit ter bescherming van essentiële Azure-infrastructuur met tijdsgebonden lidmaatschap en just-in-time-toegang. Daarnaast biedt PIM optionele afdwinging van Azure multi-factor Authentication voor twee verschillende scenario's.

## <a name="require-multi-factor-authentication-to-activate"></a>Meervoudige authenticatie activeren

Resource-beheerders kunnen leden van een rol in aanmerking komende Azure multi-factor Authentication uitvoeren voordat ze kunnen activeren vereisen. Dit proces zorgt ervoor dat de gebruiker die de activering is wie hij of zij beweren te zijn met redelijke zekerheid vraagt. Afdwingen van deze optie worden beveiligd op kritieke bronnen in situaties wanneer het gebruikersaccount waarmee is geknoeid. 

Selecteer een resource in de lijst met beheerde bronnen voor het afdwingen van deze vereiste. Van de [Overzichtsdashboard](pim-resource-roles-overview-dashboards.md), selecteer een rol in de lijst met rollen op het deel van de rechterbenedenhoek van het scherm.

Bovendien kunt u downloaden in de instellingen voor ofwel de **rollen** of **rolinstellingen** tabbladen in het linkerdeelvenster.

>[!Note]
>Als de opties in het linkerdeelvenster lichter gekleurd weergegeven worden en u een banner aan de bovenkant van de pagina die aangegeven "U moet in aanmerking komende rollen die kunnen worden geactiveerd" ziet, bent u niet een actieve beheerder. Dit betekent dat u moet [activeren](pim-resource-roles-activate-your-roles.md) voordat u doorgaat.

!['Functies' en 'rolinstellingen' tabbladen ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Selecteren om weer te geven van een rol lidmaatschap, **rolinstellingen** van de balk aan de bovenkant van het scherm openen de **rol instelling detail**.

De om rolinstellingen te wijzigen, selecteert u de **bewerken** bovenaan op de knop.

In de sectie onder **activeren**, schakel het selectievakje in om te **meervoudige authenticatie vereisen activering**. Selecteer vervolgens **Opslaan**.

![Multi-Factor Authentication bij activering vereisen](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Multi-factor Authentication vereisen voor toewijzing

In sommige gevallen kan de resourcebeheerder van een mogelijk wilt toewijzen van een lid aan een rol gedurende een korte periode (bijvoorbeeld één dag). In dit geval hoeft ze niet de toegewezen leden activering kunnen aanvragen. In dit scenario kan geen PIM multi-factor Authentication afdwingen wanneer het lid maakt gebruik van hun roltoewijzing, omdat ze al actief is in de rol vanaf het moment dat ze worden toegewezen.

Om ervoor te zorgen dat de resourcebeheerder van de die voldoen aan de toewijzing is wie hij of zij beweren te zijn, kunt u multi-factor Authentication afdwingen voor toewijzing.

Vink het selectievakje uit hetzelfde rol instelling details scherm **meervoudige authenticatie vereisen rechtstreekse toewijzing**.

![Multi-factor Authentication vereisen op rechtstreekse toewijzing](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Volgende stappen

[Goedkeuring vereist voor het activeren](pim-resource-roles-approval-workflow.md)

[Het controlelogboek gebruiken](pim-resource-roles-use-the-audit-log.md)



